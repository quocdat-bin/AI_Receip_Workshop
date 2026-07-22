---
title : "Tạo và cấu hình Amazon RDS MySQL"
date : 2026-07-07 
weight : 2
chapter : false
pre : " <b> 5.2.2 </b> "
---

### 1. Tạo Amazon RDS for MySQL
Đăng nhập vào AWS Management Console.
- Truy cập dịch vụ Amazon RDS.
![endpoint diagram](/images/5-Workshop/5.3-Backend/3.jpg)
- Chọn Create database trong Amazon RDS.
![endpoint diagram](/images/5-Workshop/5.3-Backend/4.jpg)

-	Database engine: MySQL.
-	Create method: Full configuration
-	Template: Dev/Test 
![endpoint diagram](/images/5-Workshop/5.3-Backend/5.jpg)

Availability and durability: Single-AZ DB instance deployment (1 instance)
![endpoint diagram](/images/5-Workshop/5.3-Backend/6.jpg)

- Đặt tên DB instance.
- Khai báo tài khoản quản trị cơ sở dữ liệu.
- Đặt mật khẩu mạnh cho tài khoản quản trị.

![endpoint diagram](/images/5-Workshop/5.3-Backend/7.jpg)

- Chọn DB instance class db.t3.micro.
- Cấu hình dung lượng lưu trữ 20 GiB.
- Tắt hoặc giới hạn tự động mở rộng dung lượng nếu không cần thiết.
- Chọn VPC và subnet group phù hợp.
- Cấu hình quyền truy cập public trong giai đoạn phát triển nếu cần kết nối từ máy cá nhân.
- Chọn hoặc tạo Security Group cho RDS.
- Giữ cổng MySQL mặc định 3306.

![endpoint diagram](/images/5-Workshop/5.3-Backend/8.jpg)

- Kiểm tra lại toàn bộ cấu hình.
- Chọn Create database.
- Chờ DB instance chuyển sang trạng thái Available.

![endpoint diagram](/images/5-Workshop/5.3-Backend/9.jpg)

### 2. Cấu hình Security Group

- Mở phần thông tin kết nối của DB instance.
- Truy cập Security Group đang gắn với RDS.
- Mở phần Inbound rules.
- Thêm rule cho dịch vụ MySQL/Aurora.
- Chọn giao thức TCP và cổng 3306.
- Giới hạn nguồn truy cập bằng địa chỉ IP công cộng của máy phát triển.
- Lưu cấu hình Security Group.
- Cập nhật lại địa chỉ IP trong rule nếu mạng hiện tại thay đổi.
- Không duy trì quyền truy cập 0.0.0.0/0 trong môi trường chính thức.
- Sau khi triển khai backend, giới hạn kết nối RDS từ Security Group của EC2.

![endpoint diagram](/images/5-Workshop/5.3-Backend/10.jpg)

### 3. Chuẩn bị kết nối SSL
- Tải chứng chỉ CA bundle dành cho Amazon RDS.
- Lưu file với tên global-bundle.pem.
- Đặt chứng chỉ trong thư mục cục bộ an toàn.
- Không đưa chứng chỉ riêng, mật khẩu hoặc file .env lên GitHub.
- Cấu hình MySQL Client hoặc MySQL Workbench sử dụng SSL.
- Chọn chế độ xác minh danh tính máy chủ khi kết nối.
- Kiểm tra chứng chỉ có thể được chương trình đọc thành công.
### 4. Kết nối bằng MySQL Workbench
- Cài đặt MySQL Workbench trên máy phát triển.
- Mở MySQL Workbench.
- Tạo một MySQL connection mới.
- Nhập endpoint của Amazon RDS vào trường hostname.
- Nhập port 3306.
- Nhập tài khoản quản trị RDS.
- Cấu hình file global-bundle.pem trong phần SSL.

![endpoint diagram](/images/5-Workshop/5.3-Backend/11.jpg)

- Chọn Test Connection.
- Nhập mật khẩu quản trị khi được yêu cầu.

![endpoint diagram](/images/5-Workshop/5.3-Backend/12.jpg)

- Kiểm tra kết nối thành công.
- Lưu cấu hình kết nối để sử dụng cho các bước tiếp theo.

### 5. Tạo schema sử dụng cho hệ thống
**Kiểm tra danh sách database hiện có:**
- SHOW DATABASES;
- Tạo schema chính của dự án:
- CREATE DATABASE food_ai_db
- CHARACTER SET utf8mb4
- COLLATE utf8mb4_unicode_ci;
- Chọn schema vừa tạo:
- USE food_ai_db;
- Kiểm tra schema đang được sử dụng:
- SELECT DATABASE();

### 6. Tạo các bảng trên RDS
- Mở file schema.sql đã chuẩn bị.
- Chọn schema food_ai_db.
- Chạy các câu lệnh tạo bảng.
- Tạo lần lượt các bảng:
    - users
    - recipes
    - nutrition
    - favorites
    - search_history
    - chat_history
- Kiểm tra danh sách bảng:
    - SHOW TABLES;
- Kiểm tra cấu trúc từng bảng:
    - DESCRIBE users;
    - DESCRIBE recipes;
    - DESCRIBE nutrition;
    - DESCRIBE favorites;
    - DESCRIBE search_history;
    - DESCRIBE chat_history;
- Kiểm tra khóa chính, khóa ngoại và ràng buộc dữ liệu.
- Xác nhận các trường JSON tương thích với MySQL.
- Xác nhận charset của các bảng là utf8mb4.

### 7. Tạo index

- Mở file indexes.sql.
- Chọn schema food_ai_db.
- Chạy các câu lệnh tạo index.
- Tạo index tìm kiếm tên công thức.
- Tạo index hỗ trợ truy vấn danh sách yêu thích.
- Tạo index kết hợp cho lịch sử tìm kiếm theo người dùng và thời gian.
- Tạo index kết hợp cho lịch sử trò chuyện theo người dùng và thời gian.
- Tạo index cho công thức được tham chiếu trong lịch sử trò chuyện.
- Kiểm tra index của từng bảng:
    - SHOW INDEX FROM recipes;
    - SHOW INDEX FROM favorites;
    - SHOW INDEX FROM search_history;
    - SHOW INDEX FROM chat_history;
- Xác nhận không có index thừa hoặc trùng lặp.

### 8. Cấu hình kết nối cho chương trình

- Tạo file .env trong môi trường chạy backend hoặc script import.
- Khai báo các biến kết nối cần thiết, ví dụ:
    - DB_HOST=<rds-endpoint>
    - DB_PORT=3306
    - DB_NAME=food_ai_db
    - DB_USER=<database-user>
    - DB_PASSWORD=<database-password>
- Cấu hình chương trình đọc biến môi trường.
- Không hard-code endpoint, username hoặc password trong code.
- Thêm .env vào .gitignore.
- Tạo .env.example chỉ chứa tên biến và giá trị mẫu.
- Kiểm tra kết nối từ Python đến food_ai_db.
- Xác nhận chương trình có thể mở và đóng database session bình thường.

### 9. Import dữ liệu vào Amazon RDS MySQL
#### 9.1. Chuẩn bị môi trường import

- Kiểm tra endpoint, cổng 3306 và schema food_ai_db.
- Kiểm tra Security Group cho phép máy thực hiện import kết nối tới RDS.
- Kiểm tra kết nối bằng MySQL Workbench.
- Đặt hai file dữ liệu đã xử lý vào thư mục seed:
    -    backend/database/seed/
    - 	 recipes_cleaned.pkl
    - 	 nutrition_cleaned.pkl
- Đặt script import tại:
    - backend/database/scripts/import_data.py
- Tạo và kích hoạt môi trường ảo Python.
- Cài đặt các thư viện cần thiết:
    - pip install pandas sqlalchemy pymysql python-dotenv
- Kiểm tra lại dữ liệu import

#### 9.2. Đọc dữ liệu đã xử lý

- Đọc recipes_cleaned.pkl và nutrition_cleaned.pkl bằng Pandas.
- Kiểm tra số lượng bản ghi, số lượng cột, tên cột, kiểu dữ liệu., giá trị thiếu trong các trường bắt buộc, khóa chính trùng lặp.

#### 9.3. Chuẩn bị dữ liệu dinh dưỡng
- Chọn các trường cần import:
- Đối chiếu độ dài food_name và data_type với cấu trúc bảng.
- Xác nhận dữ liệu dinh dưỡng sẵn sàng để import.

#### 9.4. Import dữ liệu dinh dưỡng

- Chia dữ liệu thành các batch nhỏ.
- Mở kết nối đến Amazon RDS.
- Thực hiện insert dữ liệu vào bảng nutrition.
- Đóng kết nối sau khi hoàn thành.
- Kiểm tra tổng số bản ghi và ngẫu nhiên một số bản ghi

#### 9.5. Import dữ liệu công thức

- Chia dữ liệu công thức thành nhiều batch.
- Không gửi toàn bộ hơn 400.000 bản ghi trong một transaction.
- Mở kết nối đến Amazon RDS.
- Insert từng batch vào bảng recipes.
- Đóng kết nối sau khi hoàn thành.

#### 9.6. Kiểm tra số lượng và chất lượng dữ liệu trên RDS

- Đếm tổng số công thức:
- Đếm tổng số dữ liệu dinh dưỡng:
- Đối chiếu kết quả với báo cáo import.
- Kiểm tra số lượng bản ghi bị bỏ qua.
- Xác nhận không phát sinh bản ghi trùng khóa chính.
- Kiểm tra ngẫu nhiên dữ liệu công thức.
- Đối chiếu nguyên liệu, bước nấu, tags và các trường đếm.

#### 9.7. Kiểm tra index và hiệu năng truy vấn

- Kiểm tra index trên bảng recipes:
- Chạy thử truy vấn tìm kiếm tên món ăn:
    - SELECT recipe_id, name
    - FROM recipes
    - WHERE name LIKE 'chicken%'
    - LIMIT 20;
- Kiểm tra truy vấn lấy chi tiết công thức theo khóa chính.
- Kiểm tra truy vấn lấy dinh dưỡng theo fdc_id.
