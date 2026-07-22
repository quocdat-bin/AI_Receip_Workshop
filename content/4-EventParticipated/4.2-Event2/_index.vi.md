---
title: "Event 2"
date: 2026-07-07
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “FCAJ COMMUNITY DAY - DATA DRIVEN, AI RISEN”

### Mục Đích Của Sự Kiện

-   Cung cấp góc nhìn thực tế về làn sóng AI đang định hình tương lai của các doanh nghiệp.
-   Hướng dẫn cách tích hợp các giải pháp hiện đại như AI đàm thoại (Voice AI), tự động hóa DevOps, và AI trong tuyển dụng (HR).
-   Chia sẻ phương pháp bảo mật kết nối nội bộ khi triển khai các hệ thống AI Agents (qua giao thức MCP) trên đám mây AWS.

### Danh Sách Diễn Giả

-   **Steve Tran** - CTO/Founder, CloudThinker
-   **Trung Vu** - CEO, Revve AI
-   **Nghi Danh** - AI Engineer, Renova Cloud
-   **Kiet Tran** - AI Engineer, AWS Student Builder Group
-   **Bao Phan & Nguyen Nguyen** - Cloud Engineers, Cloud Kinetics
-   **Truong Tran & Anh Dang** - AI Solution Sales, Noventiq
-   **Toan Nguyen** - AWS Security Builder

### Nội Dung Nổi Bật

#### Sự dịch chuyển trong định hướng nghề nghiệp:
Nhiều doanh nghiệp đang thu hẹp việc tuyển các vị trí cơ bản và chuyển sang dùng AI hoặc nhân sự Senior thành thạo trong việc vận hành AI. Với sinh viên IT, việc sớm tích lũy kinh nghiệm trong môi trường doanh nghiệp thực tế đã trở thành điều bắt buộc chứ không còn là lựa chọn.

#### DevOps Automation & Cloud Infrastructure:
Sự kiện giới thiệu công cụ DevOps Agent hỗ trợ kỹ sư truy tìm nguyên nhân sự cố (Root Cause Analysis), rút ngắn thời gian từ nhiều giờ xuống chỉ còn vài phút nhờ tự học topology hệ thống và kết xuất dữ liệu qua các giao thức bảo mật. Dù vậy, AI chỉ đưa ra đề xuất, còn con người vẫn là chốt chặn cuối cùng để thực thi quyết định.

#### Giải pháp Voice AI cho tiếng Việt:
Xử lý bài toán khan hiếm dữ liệu tiếng Việt (Low-resource language). Thay vì dựa vào mô hình Speech-to-Speech truyền thống, các chuyên gia trình bày mô hình tách thành 3 giai đoạn: Speech-to-Text -> LLM xử lý logic -> Text-to-Speech. Cách làm này giúp doanh nghiệp kiểm soát chặt chẽ nội dung mà AI phát ngôn, nhận diện giới tính/vùng miền và tích hợp Tool Calling một cách hiệu quả.

#### Ứng dụng AI trong Nhân sự (HR):
Xử lý vấn đề thiên kiến cá nhân và tình trạng bỏ lỡ ứng viên giỏi. Amazon Q được kết nối trực tiếp với các nền tảng công sở để quét CV hàng loạt, tự động tạo JD, đối chiếu năng lực ứng viên và báo cáo mức lương dự kiến mà không bị giới hạn bởi số lượng token xử lý.

#### Bảo mật kết nối MCP Private:
Để Amazon Q truy cập database nội bộ mà không đẩy dữ liệu ra Public Internet, mô hình đề xuất đặt MCP Server trong Private Subnet của VPC, cho lưu lượng đi qua VPC Endpoints và mã hóa chứng chỉ bằng AWS Certificate Manager (ACM), qua đó đảm bảo tuân thủ bảo mật một cách trọn vẹn.

### Bài học & Trải nghiệm thực tế

#### Trải nghiệm thực tế:
Được tận mắt chứng kiến những bản demo trực tiếp vô cùng ấn tượng, từ Voice Agent giải đáp thông tin MacBook qua tổng đài, DevOps AI chỉ trong chốc lát đã tự phát hiện lỗi hệ thống do DDoS gây ra, cho tới Amazon Q phân tích CV và chấm điểm ứng viên ngay tại chỗ.

#### Bài học rút ra:
-   Bản thân AI không cướp việc của con người, nhưng người biết tận dụng AI sẽ thay thế người không biết dùng.
-   Khi xây dựng AI Agent cho doanh nghiệp, bảo mật phải được đặt lên hàng đầu. Các luồng dữ liệu (Data Pipeline) cần được thiết lập theo chuẩn mã hóa, nhất là khi tương tác qua Model Context Protocol (MCP) nội bộ.
-   Đối với các hệ thống phức tạp, cần áp dụng kiến trúc Multi-Agent thay cho Single Agent nhằm giảm "ảo giác", tối ưu Context Window và dễ dàng triển khai phân quyền (Role-Based Access Control).

### Ứng dụng vào công việc 

-   **Nâng cấp Backend cho dự án:** Vận dụng mô hình VPC Connection và Private MCP Server được chia sẻ tại sự kiện để đảm bảo các yêu cầu gọi API từ hệ thống gợi ý món ăn tới Database (nơi lưu thông tin dị ứng/sở thích của khách hàng) hoàn toàn kín và không rò rỉ ra Internet.
-   **Kiểm thử và vận hành tự động:** Thử nghiệm các bộ công cụ DevOps AI Agent để tự động phân tích log mỗi khi ứng dụng báo lỗi (chẳng hạn lỗi không tải được hình ảnh món ăn), nhờ đó rút ngắn thời gian xử lý sự cố.
-   **Ứng dụng Voice AI (Gợi ý thêm):** Về sau có thể tích hợp mô hình Voice AI (Giọng nói -> Văn bản -> LLM tìm công thức -> Văn bản -> Giọng nói) vào AI Recipe Finder, giúp người dùng vừa nấu ăn vừa hỏi/đáp công thức với trợ lý ảo hoàn toàn bằng tiếng Việt mà không cần chạm vào màn hình.

#### Một số hình ảnh chứng minh tham gia sự kiện:

![Event 2](/images/4-Event/3.jpg)
