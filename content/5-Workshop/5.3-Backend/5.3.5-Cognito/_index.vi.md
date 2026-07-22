---
title : "Cấu hình Amazon Cognito, xác thực người dùng và bảo mật API"
date : 2026-07-07 
weight : 5
chapter : false
pre : " <b> 5.2.5 </b> "
---

### 1. Xác định luồng xác thực
- Sử dụng Amazon Cognito User Pool để quản lý tài khoản người dùng.
- Frontend chịu trách nhiệm:
    -	Đăng ký tài khoản.
    -	Xác minh email.
    -	Đăng nhập.
    -	Nhận token từ Cognito.
    -	Gửi access token đến Backend.
- Backend chịu trách nhiệm:
    -	Nhận access token.
    -	Kiểm tra chữ ký JWT.
    -	Kiểm tra issuer.
    -	Kiểm tra thời hạn token.
    -	Kiểm tra token_use.
    -	Kiểm tra App Client ID.
    -	Lấy thông tin người dùng.
    -	Đồng bộ người dùng vào bảng users.
    -	Amazon RDS không lưu mật khẩu người dùng, mật khẩu được Amazon Cognito quản lý.

### 2. Tạo Amazon Cognito User Pool
- Đăng nhập AWS Management Console.
- Chuyển đến dịch vụ Amazon Cognito.
- Chọn Create user pool.
- Chọn phương thức đăng nhập bằng email.
- Thiết lập email là thuộc tính bắt buộc.
- Bật cơ chế tự đăng ký nếu cho phép người dùng tạo tài khoản.
- Bật xác minh email.
- Cấu hình Cognito gửi mã xác minh đến email người dùng.
- Thiết lập chính sách mật khẩu phù hợp:
    -	Độ dài tối thiểu.
    -	Chữ hoa.
    -	Chữ thường.
    -	Chữ số.
    -	Ký tự đặc biệt nếu cần.
- Cấu hình thời gian hết hạn của mã xác minh.
- Hoàn tất tạo User Pool.

![endpoint diagram](/images/5-Workshop/5.3-Backend/30.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/31.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/32.jpg)

### 3. Tạo Cognito App Client
- Mở User Pool vừa tạo.
- Truy cập phần App clients.
- Chọn tạo App Client mới.
- Đặt tên nhận diện ứng dụng frontend.
- Chọn loại ứng dụng phù hợp với Single-page application.
- React chạy trên trình duyệt không thể bảo vệ client secret an toàn.
- Bật Authorization Code Grant.
- Sử dụng Authorization Code Flow kết hợp PKCE.

![endpoint diagram](/images/5-Workshop/5.3-Backend/33.jpg)

### 4. Cấu hình Cognito Domain
- Mở mục quản lý domain của User Pool.
- Tạo Cognito Hosted UI domain.
- Chọn domain prefix duy nhất.
- Kiểm tra domain có thể truy cập.
- Issuer dùng để xác minh JWT có dạng:
- https:cognito-idp.region.amazonaws.com/user-pool-id

### 5. Cấu hình Callback URL và Sign-out URL
- Thu thập URL từ thành viên frontend.
- Cấu hình callback URL cho môi trường cục bộ:
- http://localhost:5173/callback
- Cấu hình sign-out URL cho môi trường cục bộ:
- http://localhost:5173/login
- Cấu hình callback URL của Amplify:
- https://main.d2ykhh6couuz7m.amplifyapp.com/callback
- Cấu hình sign-out URL của Amplify:
- https://main.d2ykhh6couuz7m.amplifyapp.com/login
- Lưu cấu hình App Client sau khi cập nhật.
- Chờ cấu hình được áp dụng trước khi kiểm thử.

### 6.  Cấu hình biến môi trường Backend
- Bổ sung các biến Cognito vào .env của Backend:
- COGNITO_REGION=ap-southeast-1
- COGNITO_USER_POOL_ID=user-pool-id
- COGNITO_APP_CLIENT_ID=app-client-id
- COGNITO_DOMAIN=https://cognito-domain
- Xây dựng issuer từ region và User Pool ID:
- https://cognito-idp.ap-southeast-1.amazonaws.com/user-pool-id
- Xây dựng JWKS URL:
- https://cognito-idp.ap-southeast-1.amazonaws.com/user-pool-id/.well-known/jwks.json

### 7. Cài đặt thư viện xác thực Backend
- Bổ sung thư viện cần thiết để xử lý JWT và gửi HTTP request.
- Có thể sử dụng:
- PyJWT
- cryptography
- httpx
- Cập nhật requirements.txt.
- Cài đặt dependency trong môi trường ảo:
- pip install -r requirements.txt
- Kiểm tra các thư viện import thành công.

### 8. Xây dựng API /auth/me
- Tạo router xác thực với prefix:
- /auth
- Tạo endpoint:
- GET /auth/me
- Bảo vệ endpoint bằng dependency Cognito JWT.
- Frontend gửi request:
- GET /auth/me
- Authorization: Bearer access_token
- Nếu token hợp lệ:
- Xác minh người dùng.
- Tìm hoặc tạo bản ghi trong bảng users.
- Trả thông tin người dùng hiện tại.
- Nếu thiếu bearer token, token hết hạn hoặc không hợp lệ, trả HTTP 401.
- Nếu gặp lỗi nội bộ khi đồng bộ người dùng, không trả stack trace.
- Đăng ký auth router trong main.py.
- Kiểm tra Swagger hiển thị cơ chế bearer authentication phù hợp.

![endpoint diagram](/images/5-Workshop/5.3-Backend/33_2.jpg)

### 9. Đồng bộ người dùng với Amazon RDS
- Khi người dùng đăng nhập thành công lần đầu và gọi /auth/me, Backend tạo bản ghi trong bảng users.
- Kiểm tra người dùng theo cognito_sub.
- Nếu chưa tồn tại, tạo:
- cognito_sub
- username
- email
- created_at
- Nếu đã tồn tại, sử dụng lại user_id cũ.
- Kiểm tra email không trùng với tài khoản khác.
- Kiểm tra dữ liệu trực tiếp trên RDS sau lần đăng nhập đầu tiên.
- Xác nhận chỉ có một bản ghi ứng với một cognito_sub.
- Gọi lại /auth/me.
- Xác nhận số lượng bản ghi không tăng.
- Xác nhận user_id được giữ nguyên.

### 10. Cấu hình CORS cho Authentication
- Cập nhật biến môi trường Backend:
- CORS_ORIGINS=http://localhost:5173,https://main.d2ykhh6couuz7m.amplifyapp.com
- Khởi động lại Backend sau khi thay đổi CORS:
- sudo systemctl restart recipe-backend
- Kiểm tra preflight request từ frontend.
- Kiểm tra request từ Amplify.

### 11. Kiểm thử /auth/me
- Gọi API bằng access token hợp lệ:
- GET /auth/me
- Authorization: Bearer <access_token>
- Xác nhận Backend trả HTTP 200.
- Kiểm tra response có đúng:
- user_id
- cognito_sub
- username
- email
- created_at
- Kiểm tra bảng users trên RDS.
- Xác nhận một người dùng mới được tạo.
- Gọi lại /auth/me bằng cùng tài khoản.
- Xác nhận không tạo user trùng.
- Xác nhận user_id không thay đổi.
- Thử token đã bị sửa payload.
- Xác nhận Backend trả HTTP 401.
- Thử token hết hạn.
- Xác nhận Backend trả HTTP 401.

### 12. Cập nhật Backend có Authentication lên EC2
- Commit source code đã hoàn thiện trên máy phát triển.
- Push code lên GitHub.
- Kết nối SSH đến EC2.
- Chuyển đến repository: cd /home/ubuntu/ai_recipe_finder
- Kiểm tra thay đổi cục bộ: git status
- Lấy source code mới: git pull origin main
- Chuyển đến Backend: cd backend
- Kích hoạt môi trường ảo: source .venv/bin/activate
- Cập nhật biến Cognito trong .env.
- Kiểm tra import:
- python -c "import app.models; print('All ORM models imported successfully')"
- Kiểm tra ứng dụng có thể import:
- python -c "from main import app; print('FastAPI app imported successfully')"
- Khởi động lại service:
- sudo systemctl restart recipe-backend
- Kiểm tra trạng thái:
- sudo systemctl status recipe-backend
- Kiểm tra endpoint công khai.
- Kiểm tra /auth/me không token trả 401.
- Kiểm tra /auth/me với access token hợp lệ trả 200.
- Xác nhận phiên bản mới đã được triển khai thành công.

