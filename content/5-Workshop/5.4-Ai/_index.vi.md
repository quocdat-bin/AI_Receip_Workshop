---
title : "Triển khai AI Chatbox & Trí tuệ nhân tạo (AI Services)"
date : 2026-07-07 
weight : 5
chapter : false
pre : " <b> 5.3. </b> "
---

#### Tổng quan

Giai đoạn này tập trung vào việc thiết lập hạ tầng máy chủ và tích hợp các công nghệ Trí tuệ nhân tạo (AI) vào nền tảng. Quá trình này đảm bảo hệ thống có đủ tài nguyên để vận hành các dịch vụ cốt lõi, đồng thời kết nối thành công với các mô hình ngôn ngữ lớn nhằm mang lại khả năng tìm kiếm thông minh và giao tiếp tự nhiên cho người dùng.

Trong bước triển khai này, chúng ta sẽ thực hiện ba tác vụ chính để đưa động cơ AI vào hoạt động thực tế:

- Chuẩn bị AI Server (EC2) - Khởi tạo và cấu hình máy chủ ảo Amazon EC2. Tùy thuộc vào yêu cầu tài nguyên, hệ thống sẽ sử dụng một instance chuyên dụng cho AI hoặc thiết lập chạy chung để tối ưu hóa chi phí và hiệu suất.

- Triển khai dịch vụ bằng Docker - Tiến hành đóng gói và vận hành (build & run) các container độc lập cho ứng dụng FastAPI. Lưu lượng truy cập sẽ được phân luồng rõ ràng với Port 8001 dành cho tính năng tìm kiếm (/search) và Port 8002 dành cho tính năng trò chuyện (/chat) theo đúng sơ đồ thiết kế kiến trúc.

- Tích hợp Động cơ AI - Xây dựng các đoạn mã logic từ FastAPI để kết nối API ra các dịch vụ LLM bên ngoài (như Amazon Bedrock hoặc Google Gemini API). Đây là lõi xử lý chính giúp hệ thống thực thi lệnh tìm kiếm theo ngữ nghĩa (Semantic Search) và vận hành Chatbot tư vấn nấu ăn thông minh.
    
![overview](/images/5-Workshop/5.1-Workshop-overview/Workflow.jpg)


#### Nội dung

- [Chuẩn bị AI Server (EC2) & Hạ tầng Mạng (VPC, Security Group, Elastic IP)](5.4.1-EC2/)
- [Triển khai Dịch vụ bằng Docker & CSDL Vector (pgvector)](5.4.2-Docker/)
- [Tích hợp Động cơ AI & Logic xử lý (Semantic Search & Gemini Chatbot)](5.4.3-Chatbot/)
- [Kiểm tra và Xác thực Endpoint AI](5.4.4-CheckAi/)
