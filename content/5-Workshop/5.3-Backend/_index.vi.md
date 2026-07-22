---
title : "Khởi Tạo Cơ Sở Dữ Liệu, TRiển Khai Backend Và Thiết Lập Amazon Cognito"
date : 2026-07-07 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---
#### Tổng quan 
Giai đoạn này tập trung vào việc xây dựng hạ tầng lưu trữ, xử lý logic và bảo mật cho ứng dụng. Quá trình triển khai bao gồm việc khởi tạo cơ sở dữ liệu trên Amazon RDS, thiết lập máy chủ backend trên Amazon EC2 và cấu hình quản lý người dùng với Amazon Cognito. 
Trong bước triển khai này, chúng ta sẽ thực hiện ba phân hệ chính để kết nối và vận hành hệ thống
- Khởi tạo Cơ sở dữ liệu (Amazon RDS) - Tạo một RDS MySQL instance được đặt trong Private Subnet. Hệ thống tiến hành chạy các script để khởi tạo bảng dữ liệu (như Users, Recipes, Collections) và thực hiện import dữ liệu công thức, dinh dưỡng đã qua xử lý vào cơ sở dữ liệu. 
- Triển khai Recipe Backend (EC2) - Khởi tạo EC2 instance (Ubuntu Server) và cấu hình môi trường ảo Python để triển khai ứng dụng FastAPI. Backend này sẽ cung cấp các Recipe API kết nối an toàn với cơ sở dữ liệu RDS, đồng thời được cấu hình chạy thông qua Nginx (Reverse Proxy) và systemd để đảm bảo hoạt động liên tục. 
- Thiết lập Amazon Cognito - Tạo User Pool để thiết lập luồng đăng nhập và đăng ký bảo mật cho người dùng. Hệ thống sử dụng token JWT để xác thực các yêu cầu API, đồng thời tích hợp với backend thông qua endpoint /auth/me nhằm đồng bộ thông tin định danh của người dùng vào bảng users trên Amazon RDS.  

#### Nội dung

- [Chuẩn bị cơ sở dữ liệu](5.3.1-Database/)
- [Tạo và cấu hình Amazon RDS MySQL](5.3.2-RDS/)
- [Xây dựng Backend API bằng FastAPI](5.3.3-API/)
- [Triển khai Backend FastAPI lên Amazon EC2](5.3.4-EC2/)
- [Cấu hình Amazon Cognito, xác thực người dùng và bảo mật API](5.3.5-Cognito/)