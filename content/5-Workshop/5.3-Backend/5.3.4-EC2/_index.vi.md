---
title : "Triển khai Backend FastAPI lên Amazon EC2"
date : 2026-07-07 
weight : 4
chapter : false
pre : " <b> 5.2.4 </b> "
---

### 1. Triển khai Backend FastAPI lên Amazon EC2
**Bước 1. Chuẩn bị triển khai**
- Kiểm tra backend đã hoạt động ổn định trên máy cục bộ.
- Xác nhận các API cơ bản hoạt động:
    - GET /
    - GET /recipes
    - GET /recipes/random
    - GET /recipes/{recipe_id}
    - GET /nutrition/{fdc_id}
- Kiểm tra requirements.txt đã chứa đầy đủ thư viện.
- Kiểm tra .gitignore không cho phép commit:
    - .env
    - .venv
    - File .pkl
- Dataset
- Chứng chỉ
- File log
- Đưa source code backend lên GitHub repository riêng tư.
- Kiểm tra Amazon RDS đang ở trạng thái Available.
- Chọn khu vực triển khai EC2 cùng khu vực với RDS

**Bước 2. Tạo Amazon EC2 Instance**

- Đăng nhập AWS Management Console.
- Truy cập dịch vụ Amazon EC2.
- Chọn Launch instance.

![endpoint diagram](/images/5-Workshop/5.3-Backend/20.jpg)

- Đặt tên cho EC2 instance.
- Chọn hệ điều hành Ubuntu Server.
- Chọn kiến trúc tương thích với các thư viện Python của dự án.

![endpoint diagram](/images/5-Workshop/5.3-Backend/21.jpg)

- Chọn instance type phù hợp với môi trường thử nghiệm.
- Tạo hoặc chọn key pair để đăng nhập SSH.
- Tải file private key về máy.
- Lưu file private key tại vị trí an toàn.

![endpoint diagram](/images/5-Workshop/5.3-Backend/22.jpg)

- Chọn VPC phù hợp với kiến trúc hệ thống.
- Cấu hình dung lượng lưu trữ phù hợp.
- Kiểm tra tổng chi phí dự kiến trước khi tạo.
- Chọn Launch instance.
- Chờ EC2 chuyển sang trạng thái Running.
- Kiểm tra status check của instance đã hoàn tất.

![endpoint diagram](/images/5-Workshop/5.3-Backend/23.jpg)

### 2. Cấu hình Security Group cho EC2
- Mở Security Group đang gắn với EC2. Thêm rule

![endpoint diagram](/images/5-Workshop/5.3-Backend/24.jpg)

- Kiểm tra lại toàn bộ inbound và outbound rules.
- Xác nhận chỉ những cổng cần thiết mới được công khai.

### 3. Gán Elastic IP
- Truy cập mục Elastic IP addresses trong EC2.
- Chọn Allocate Elastic IP address.
- Tạo một địa chỉ Elastic IP.
- Chọn Elastic IP vừa tạo.
- Chọn Associate Elastic IP address.
- Gắn Elastic IP với EC2 instance.

![endpoint diagram](/images/5-Workshop/5.3-Backend/25.jpg)

- Kiểm tra địa chỉ IP đã được liên kết.

![endpoint diagram](/images/5-Workshop/5.3-Backend/26.jpg)

- Sử dụng Elastic IP thay cho public IP động.


### 4. Kết nối SSH đến EC2

- Mở terminal tại thư mục chứa private key.
- Giới hạn quyền đọc file key
![endpoint diagram](/images/5-Workshop/5.3-Backend/27.jpg)

- Kết nối đến EC2:
![endpoint diagram](/images/5-Workshop/5.3-Backend/28.jpg)

- Xác nhận fingerprint của máy chủ trong lần kết nối đầu tiên.
Kiểm tra đăng nhập thành công.


### 5. Cập nhật hệ điều hành EC2
- Cập nhật danh sách package: sudo apt update
- Cài đặt các bản cập nhật cần thiết: sudo apt upgrade -y
- Cài đặt các công cụ cơ bản: sudo apt install -y git python3 python3-pip python3-venv nginx
- Kiểm tra phiên bản Python: python3 --version
- Kiểm tra Git: git --version
- Kiểm tra Nginx: nginx -v
- Kiểm tra trạng thái Nginx: sudo systemctl status nginx
- Truy cập Elastic IP trên trình duyệt.
- Xác nhận trang mặc định của Nginx hiển thị thành công.

### 6. Đưa source code Backend lên EC2
- cd /home/ubuntu
- Clone repository: git clone repository-url
- cd ai_recipe_finder
- Kiểm tra nhánh hiện tại: git branch --show-current
- Chuyển sang nhánh triển khai nếu cần.
- Kiểm tra source code: git status
- Xác nhận thư mục backend và requirements.txt đã tồn tại.

### 7. Tạo môi trường Python trên EC2
- cd /home/ubuntu/ai_recipe_finder/backend-api
- Tạo môi trường ảo: python3 -m venv .venv
- source .venv/bin/activate
- python -m pip install --upgrade pip
- Cài đặt thư viện: pip install -r requirements.txt
- pip list
- Kiểm tra tất cả ORM model:
- python -c "import app.models; print('All ORM models imported successfully')"

### 8. Cấu hình biến môi trường trên EC2
- Tạo file .env trong thư mục backend: nano .env
- Khai báo thông tin kết nối RDS:
- DB_HOST=rds-endpoint
- DB_PORT=3306
- DB_NAME=food_ai_db
- DB_USER=backend-database-user
- DB_PASSWORD=database-password
- Khai báo các cấu hình ứng dụng cần thiết.
- Khai báo danh sách CORS origin phù hợp.
- Giới hạn quyền đọc file: chmod 600 .env
- git status
- Xác nhận .env không xuất hiện trong danh sách file chuẩn bị commit.

### 9. Cho phép EC2 kết nối Amazon RDS
- Mở Security Group của Amazon RDS.
- Chọn Edit inbound rules.
- Thêm rule MySQL/Aurora:
- Type: MySQL/Aurora
- Protocol: TCP
- Port: 3306
- Source: Security Group của EC2

![endpoint diagram](/images/5-Workshop/5.3-Backend/29.jpg)

- Lưu Security Group.
- Kiểm tra RDS đang ở trạng thái Available.
- Từ EC2, kiểm tra khả năng mở kết nối đến RDS.
- Xác nhận backend kết nối đúng schema food_ai_db.
- Kiểm tra truy vấn đọc dữ liệu từ recipes và nutrition.

### 10. Chạy thử FastAPI trên EC2
- source .venv/bin/activate
- Khởi động Uvicorn để kiểm tra:
- uvicorn main:app --host 0.0.0.0 --port 8000
- Kiểm tra terminal không xuất hiện lỗi import.
- Kiểm tra backend kết nối RDS thành công.
- Nếu cổng 8000 được mở tạm thời, truy cập:
- http://<elastic-ip>:8000/
- Kiểm tra Swagger:
- http://<elastic-ip>:8000/docs
- Gọi thử API công thức và dinh dưỡng.
- Dừng tiến trình kiểm tra bằng Ctrl + C.
- Sau khi kiểm tra thành công, chuyển sang chạy bằng systemd.


### 11. Tạo dịch vụ systemd cho Backend
Tạo service: sudo nano /etc/systemd/system/recipe-backend.service
Khai báo cấu hình:

*[Unit]*
- Description=AI Recipe Finder FastAPI Backend
- After=network.target

*[Service]*
- User=ubuntu
- Group=www-data
- WorkingDirectory=/home/ubuntu/ai_recipe_finder/backend
- EnvironmentFile=/home/ubuntu/ai_recipe_finder/backend/.env
- ExecStart=/home/ubuntu/ai_recipe_finder/backend/.venv/bin/uvicorn main:app --host 127.0.0.1 --port 8000
- Restart=always
- RestartSec=5

*[Install]*
- WantedBy=multi-user.target
- Kiểm tra lại đường dẫn repository và môi trường ảo.
- Tải lại cấu hình systemd: sudo systemctl daemon-reload
- Khởi động backend: sudo systemctl start recipe-backend
- Cho phép backend tự khởi động cùng EC2: sudo systemctl enable recipe-backend
- Kiểm tra trạng thái: sudo systemctl status recipe-backend
- Kiểm tra backend đang lắng nghe trên 127.0.0.1:8000.
- Xác nhận service ở trạng thái active (running).


### 12. Kiểm tra log của Backend

- Xem log gần nhất:
- sudo journalctl -u recipe-backend -n 100 --no-pager
- Kiểm tra lỗi

### 13. Cấu hình Nginx Reverse Proxy
- Tạo file cấu hình Nginx:
- sudo nano /etc/nginx/sites-available/recipe-backend
- Thêm cấu hình reverse proxy:

```nginx
server {
    listen 80;
    server_name <domain-or-elastic-ip>;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_connect_timeout 30s;
        proxy_read_timeout 60s;
    }
}
```
- Kích hoạt cấu hình:
- sudo ln -s /etc/nginx/sites-available/recipe-backend \
-   /etc/nginx/sites-enabled/recipe-backend
- Xóa hoặc vô hiệu hóa site mặc định nếu gây xung đột.
- Kiểm tra cú pháp: sudo nginx -t
- Khởi động lại Nginx: sudo systemctl restart nginx
- Kiểm tra trạng thái: sudo systemctl status nginx
- Truy cập backend qua:
- http://elastic-ip/
- Xác nhận request được Nginx chuyển tiếp đến FastAPI.

### 14. Cấu hình CORS cho Frontend
- Nhận Amplify base URL từ thành viên frontend.
- Cấu hình CORS origin trong biến môi trường backend.
- Ví dụ:
- CORS_ORIGINS=http://localhost:5173,https://<amplify-domain>
- Chỉ khai báo scheme và domain.
- Không thêm:
- /callback
- /login
- Khởi động lại backend sau khi sửa .env: sudo systemctl restart recipe-backend
- Kiểm tra request từ localhost.
- Kiểm tra request từ Amplify.
- Kiểm tra origin không nằm trong danh sách không nhận được CORS header.
- Phân biệt lỗi CORS với lỗi xác thực hoặc lỗi kết nối mạng.

### 15. Kiểm thử API trên môi trường EC2
- Kiểm tra endpoint trạng thái:
- GET /
- Kiểm tra danh sách công thức:
- GET /recipes
- Kiểm tra công thức ngẫu nhiên:
- GET /recipes/random
- Kiểm tra chi tiết công thức:
- GET /recipes/{recipe_id}
- Kiểm tra dữ liệu dinh dưỡng:
- GET /nutrition/167512
- Xác nhận Swagger hiển thị đúng trong môi trường cho phép.
- Xác nhận API không trả về thông tin kết nối nội bộ.

### 16. Cập nhật Backend trên EC2
- Commit và push thay đổi từ máy phát triển lên GitHub.
- Kết nối SSH đến EC2.
- Chuyển đến repository: cd /home/ubuntu/ai_recipe_finder
- Kiểm tra trạng thái trước khi cập nhật: git status
- Không chạy git pull nếu EC2 đang có thay đổi cục bộ chưa được xác định.
- Lấy source code mới: git pull origin main
- Kích hoạt môi trường ảo:
- cd backend
- source .venv/bin/activate
- Kiểm tra import model và ứng dụng.
- Khởi động lại backend: sudo systemctl restart recipe-backend
- Kiểm tra trạng thái service.
- Kiểm tra log.
- Gọi lại các API quan trọng.
- Xác nhận phiên bản mới đã được triển khai.

