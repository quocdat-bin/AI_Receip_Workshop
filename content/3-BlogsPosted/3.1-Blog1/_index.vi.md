---
title: "Blog 1"
date: 2026-07-07
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# PHÂN TÍCH LOG AMAZON RDS BẰNG NGÔN NGỮ TỰ NHIÊN VỚI KIRO VÀ MCP

Bài blog này giới thiệu giải pháp tích hợp trợ lý AI Kiro và giao thức MCP vào quy trình phân tích log Amazon RDS, cho phép bạn truy vấn hệ thống bằng ngôn ngữ tự nhiên thay vì phải viết các câu lệnh phức tạp. Đây là một cách tiếp cận đột phá giúp tự động hóa quá trình tìm kiếm lỗi, giảm tải cho đội ngũ vận hành và tối ưu hóa hoạt động giám sát an toàn cơ sở dữ liệu trên Cloud.

**Các ý chính cần biết:**

*	Giải quyết "nỗi ám ảnh đọc log": Amazon RDS sinh ra khối lượng log khổng lồ (Error Logs, Audit Logs, Slow Query Logs...). Việc truy vết sự cố thủ công thường đòi hỏi kỹ năng viết câu query phức tạp (như trên CloudWatch Logs Insights) và cực kỳ mất thời gian trong các tình huống khẩn cấp.
*	Cơ chế hoạt động: Giải pháp kết hợp nơi lưu trữ log là Amazon CloudWatch với giao thức mã nguồn mở MCP (cloudwatch-mcp-server) và trợ lý AI Kiro để biến việc phân tích dữ liệu khô khan thành một cuộc hội thoại.
*	Truy vấn bằng ngôn ngữ tự nhiên: Bạn không cần nhớ cú pháp hệ thống. Chỉ cần ra lệnh (ví dụ: "Lọc ra các nỗ lực đăng nhập thất bại từ Audit Log trong 2 giờ qua"), hệ thống sẽ tự động gọi API, phân tích và trả về nguyên nhân cốt lõi (root cause) bằng văn bản dễ hiểu.
*	Tối ưu hóa thời gian điều tra: Ứng dụng AI giúp tự động hóa việc rút trích thông tin, rút ngắn đáng kể thời gian xử lý sự cố và giảm thiểu sai sót so với việc con người phải đọc hàng ngàn dòng log thủ công.
*	Tính ứng dụng thực tiễn: Cho thấy sự kết hợp linh hoạt và hiệu quả giữa các dịch vụ sẵn có trên Cloud với giao thức mã nguồn mở để tạo ra một luồng quản trị hệ thống thông minh và mượt mà hơn.


**Hình ảnh:**
![Blog1](/images/3-Blog/Blog1.png)

**Hướng dẫn cài đặt Kiro và CloudWatch MCP Server để phân tích Log RDS:**

Để hiện thực hóa giải pháp biến log thành cuộc hội thoại, bạn cần thiết lập kết nối giữa Amazon RDS, CloudWatch và trợ lý AI thông qua MCP (Model Context Protocol). Dưới đây là các bước thực hiện cơ bản:

**Điều kiện tiên quyết (Prerequisites):**
-	Đã cài đặt AWS CLI trên máy và cấu hình IAM User/Role có quyền truy cập đọc (Read-only) vào Amazon CloudWatch Logs.
-	Đã cài đặt Node.js (phiên bản mới nhất) hoặc Python tùy thuộc vào môi trường bạn sử dụng để chạy Kiro.
-	Hệ thống Amazon RDS đã được kích hoạt tính năng xuất log (Export logs) sang Amazon CloudWatch.

**Bước 1: Bật tính năng xuất Log từ RDS sang CloudWatch**

1.	Truy cập vào Amazon RDS Console, chọn database instance bạn muốn giám sát.
2.	Nhấp vào Modify (Sửa đổi).
3.	Cuộn xuống phần Log exports (Xuất nhật ký) và tích chọn các loại log bạn cần phân tích (ví dụ: Error log, Audit log, Slow query log).
4.	Lưu thay đổi và chờ instance cập nhật. Sau bước này, log sẽ bắt đầu được đẩy về CloudWatch Log Groups.

**Bước 2: Cài đặt trợ lý Kiro** 

Bạn có thể cài đặt Kiro CLI thông qua trình quản lý gói.
Mở terminal và chạy lệnh sau:
-	npm install -g kiro-cli

Sau khi cài đặt, hãy đảm bảo Kiro đã nhận diện được các biến môi trường cấu hình của bạn.

**Bước 3: Tích hợp AWS CloudWatch MCP Server**

MCP (Model Context Protocol) là cầu nối giúp AI hiểu được context từ CloudWatch.
1.	Tải và cài đặt MCP server cho CloudWatch từ kho lưu trữ mã nguồn mở của AWS Labs:
-	git clone https://github.com/awslabs/cloudwatch-mcp-server.git
-	cd cloudwatch-mcp-server
-	npm install
2.	Khởi chạy MCP server và cấu hình liên kết với Kiro. Bạn sẽ cần cung cấp tên miền khu vực (AWS Region) và thông tin xác thực AWS để MCP có thể gọi API GetLogEvents và StartQuery từ CloudWatch.

**Bước 4: Trải nghiệm truy vấn bằng ngôn ngữ tự nhiên**

Khi luồng kết nối Kiro -> MCP Server -> CloudWatch Logs đã sẵn sàng, bạn có thể mở giao diện dòng lệnh của Kiro và bắt đầu tương tác.
Thử nhập các câu lệnh (prompts) như:
-	"Hãy kiểm tra Audit Log của RDS instance 'db-production' và liệt kê các lần đăng nhập sai mật khẩu trong 24 giờ qua."
-	"Phân tích Slow Query Log từ 10:00 sáng đến 11:00 sáng nay và chỉ ra truy vấn SQL nào đang gây nghẽn cổ chai."
Kiro sẽ tự động dịch các yêu cầu này thành cú pháp CloudWatch Logs Insights, quét dữ liệu và trả về cho bạn báo cáo nguyên nhân cốt lõi (Root Cause Analysis) bằng văn bản hoàn chỉnh.


**Link bài viết:**  https://www.facebook.com/share/p/1EALQQhdQz/