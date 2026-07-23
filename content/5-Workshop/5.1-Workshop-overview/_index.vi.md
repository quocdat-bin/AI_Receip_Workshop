---
title : "Giới thiệu"
date : 2026-07-07 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giới thiệu về AI Recipe Finder 

Trong cuộc sống bận rộn, việc lên thực đơn hàng ngày từ những nguyên liệu còn sót lại thường mất nhiều thời gian và dễ dẫn đến lãng phí thực phẩm. Thêm vào đó, việc tự theo dõi các chỉ số dinh dưỡng (Calories, Protein, Fat, v.v.) một cách thủ công khiến người dùng gặp khó khăn trong việc duy trì chế độ ăn uống lành mạnh. Để giải quyết triệt để những vấn đề này, dự án hướng tới việc xây dựng **AI Recipe Finder** – một nền tảng gợi ý món ăn thông minh trên AWS. Hệ thống cung cấp công cụ tìm kiếm ngữ nghĩa giúp nhận diện món ăn từ nguyên liệu đầu vào, tích hợp Chatbot AI đóng vai trò như trợ lý ảo hướng dẫn nấu nướng từng bước, đồng thời tự động phân tích và tính toán dinh dưỡng. Nền tảng này không chỉ giúp người dùng cá nhân tiết kiệm thời gian và chống lãng phí, mà còn là một use-case kỹ thuật thực tế minh chứng cho sự kết hợp hiệu quả giữa Cloud Computing và Generative AI.

#### Tổng quan về workshop
Trong workshop này, toàn bộ hệ thống được thiết kế theo kiến trúc đám mây an toàn, phân tách rõ ràng các lớp dịch vụ và tối ưu hóa cho môi trường Serverless/Containerized. Luồng dữ liệu bắt đầu khi người dùng truy cập giao diện web (React SPA) được lưu trữ và triển khai tự động (CI/CD) thông qua **AWS Amplify**. Quá trình đăng nhập và xác thực danh tính được quản lý bảo mật nghiêm ngặt bởi **Amazon Cognito**. Sau khi xác thực, thay vì phơi bày backend ra internet công cộng, các yêu cầu từ Frontend được định tuyến qua cầu nối **VPC Link** để tiến vào mạng nội bộ riêng tư (VPC). Tại vùng mạng an toàn này, **EC2 Recipe API** sẽ tiếp nhận yêu cầu, xử lý logic hệ thống và tra cứu dữ liệu có cấu trúc (thông tin người dùng, công thức, lịch sử) từ cơ sở dữ liệu quan hệ **Amazon RDS (MySQL)**. Đặc biệt, đối với các tác vụ đòi hỏi Trí tuệ nhân tạo, yêu cầu sẽ được chuyển giao cho **EC2 AI Server** (hoạt động độc lập bằng các container **Docker**). Server này được thiết kế linh hoạt để giao tiếp bảo mật qua HTTPS với **Google Gemini API**, tận dụng tối đa sức mạnh xử lý ngôn ngữ tự nhiên và tính năng function calling của LLM để vận hành công cụ Semantic Search và AI Chatbot, đảm bảo phản hồi chính xác, nhanh chóng và dễ dàng kiểm soát chi phí trong quá trình triển khai.

![overview](/images/5-Workshop/5.1-Workshop-overview/Workflow.jpg)

**Bảo mật cơ bản và quản lý truy cập**

Để đảm bảo an toàn tối đa cho dữ liệu và hệ thống theo chuẩn AWS, kiến trúc của dự án áp dụng chặt chẽ các biện pháp bảo mật cốt lõi:
-	Quản lý quyền truy cập với IAM Role: Hệ thống tuân thủ nghiêm ngặt nguyên tắc quyền tối thiểu (Principle of Least Privilege) và tuyệt đối không sử dụng hard-code access key trong mã nguồn. Thay vào đó, các máy chủ EC2 (Recipe API và AI Server) được gán các IAM Role với chính sách (Policy) được thiết kế riêng biệt. Các Role này chỉ cấp đúng và đủ những quyền cần thiết để các instance có thể ghi log hệ thống lên CloudWatch hoặc tương tác an toàn với các dịch vụ AWS khác.
-	Ẩn Backend khỏi Public Internet bằng VPC Link: Điểm nhấn bảo mật của kiến trúc này là việc cách ly hoàn toàn hệ thống Backend và Database khỏi môi trường mạng công cộng. Các tài nguyên cốt lõi bao gồm EC2 Recipe API, EC2 AI Server và cơ sở dữ liệu Amazon RDS đều được đặt sâu bên trong các Private Subnet của mạng ảo (VPC) và hoàn toàn không có địa chỉ Public IP. Để giao diện Frontend có thể giao tiếp được với Backend, hệ thống sử dụng VPC Link tích hợp cùng Amazon API Gateway. VPC Link đóng vai trò như một đường hầm bảo mật riêng tư, cho phép API Gateway định tuyến trực tiếp các luồng request hợp lệ vào mạng nội bộ mà không cần phải phơi bày bất kỳ endpoint hay mở cổng (port) nào ra ngoài internet. Nhờ thiết kế này, hệ thống máy chủ và cơ sở dữ liệu được bảo vệ toàn diện khỏi các rủi ro quét lỗ hổng hay tấn công trực tiếp từ bên ngoài.


