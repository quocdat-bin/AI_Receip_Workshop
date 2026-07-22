---
title : "Chuẩn bị AI Server (EC2) & Hạ tầng Mạng (VPC, Security Group, Elastic IP)"
date : 2026-07-07
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---
### **Bước 1: Khởi tạo Máy chủ EC2 (AI Server)**
1.	Truy cập AWS Management Console chọn EC2 Dashboard chọn Launch Instance.
2.	Đặt tên Instance là embed (phục vụ chuyên biệt cho các tác vụ mã hóa Vector và AI Services).
3.	Tại mục Quick Start, chọn hệ điều hành Ubuntu Server 24.04 LTS (HVM) kiến trúc 64-bit (x86).
4.	Tại mục Configure storage, thiết lập dung lượng ổ đĩa Root Volume là 30 GiB gp3 (3000 IOPS) để đủ bộ nhớ lưu trữ mô hình AI và CSDL Vector.

![overview](/images/5-Workshop/5.4-Ai/1.jpg)
![overview](/images/5-Workshop/5.4-Ai/2.jpg)
![overview](/images/5-Workshop/5.4-Ai/3.jpg)

### **Bước 2: Cấu hình Tường lửa Security Group**
-	Tạo hoặc tùy chỉnh Security Group gắn vào Instance embed với các Inbound Rules:
    -	Port 22 (SSH): Mở kết nối SSH quản trị từ IP cá nhân.
    -	Port 8001 (Custom TCP): Mở cổng cho dịch vụ FastAPI /search (Semantic Search).
    -	Port 8002 (Custom TCP): Mở cổng cho dịch vụ FastAPI /chat (AI Chatbot).

![overview](/images/5-Workshop/5.4-Ai/4.jpg)
![overview](/images/5-Workshop/5.4-Ai/5.jpg)

### **Bước 3: Đăng ký & Gán Elastic IP (IP Cố định)**
1.	Truy cập AWS VPC Dashboard → Elastic IP addresses → Nhấn Allocate Elastic IP address.
2.	Chọn Network border group ap-southeast-1 (Singapore) và nhấn Allocate.
3.	Chọn địa chỉ IP vừa được cấp phát (ví dụ: 54.254.77.58), nhấn Associate Elastic IP address và liên kết với EC2 Instance embed (i-018c3a28a2d9e3e5c).

![overview](/images/5-Workshop/5.4-Ai/6.jpg)
![overview](/images/5-Workshop/5.4-Ai/7.jpg)

### **Bước 4: SSH Kết nối Quản trị & Kiểm tra Mã nguồn trên Server**
-	Mở Terminal/PowerShell tại máy local và thực hiện kết nối SSH bằng Private Key:
![overview](/images/5-Workshop/5.4-Ai/8.jpg)

- Kiểm tra thư mục dự án bằng lệnh ls -la
![overview](/images/5-Workshop/5.4-Ai/9.jpg)
- Đã có các file được thêm vào từ trước

### **Bước 5: Kịch bản Tối ưu hóa Chi phí Hạ tầng (Instance Resizing Strategy)**
1.	Giai đoạn Batch Embedding (Tải nặng): Dùng Instance cấu hình cao (Compute-Optimized) để tính toán vector nhanh cho hàng trăm nghìn công thức.
2.	Giai đoạn Production (Tải nhẹ): Sau khi nạp xong dữ liệu vector:
-	Trên AWS Console: Chọn Instance embed → Instance state → Stop instance.
-	Nhấn Actions → Instance settings → Change instance type → Đổi sang t3.medium (2 vCPU, 4GB RAM).
-	Nhấn Start instance để đưa máy chủ trở lại hoạt động với chi phí tối ưu.

![overview](/images/5-Workshop/5.4-Ai/10.jpg)
![overview](/images/5-Workshop/5.4-Ai/11.jpg)
![overview](/images/5-Workshop/5.4-Ai/12.jpg)