---
title : "Xây dựng Backend API bằng FastAPI"
date : 2026-07-07 
weight : 3
chapter : false
pre : " <b> 5.2.3 </b> "
---

### 1. Khởi tạo cấu trúc Backend

- Tạo thư mục backend.
- Tạo môi trường ảo Python:
    - python -m venv .venv
- Kích hoạt môi trường ảo trên Windows:
    - .\venv\Scripts\Activate.ps1
- Cài đặt các thư viện chính
    - .\backend-api\requirements.txt
### 2. Xây dựng ORM Model

- Tạo model User ánh xạ với bảng users.
- Tạo model Recipe ánh xạ với bảng recipes.
- Tạo model Nutrition ánh xạ với bảng nutrition.
- Tạo model Favorite ánh xạ với bảng favorites.
- Tạo model SearchHistory ánh xạ với bảng search_history.
- Tạo model ChatHistory ánh xạ với bảng chat_history.
- Khai báo khóa ngoại và quan hệ giữa các model.
- Import tất cả model trong app/models/__init__.py.

![endpoint diagram](/images/5-Workshop/5.3-Backend/13.jpg)

- Kiểm tra toàn bộ ORM model có thể import thành công:
    - python -c "import app.models; print('All ORM models imported successfully')"


### 3. Xây dựng Pydantic Schema

- Tạo schema dữ liệu công thức dạng rút gọn RecipeItem.
- Tạo schema phản hồi danh sách RecipeListResponse.
- Tạo schema chi tiết RecipeDetailResponse.
- Tạo schema phản hồi dinh dưỡng NutritionResponse.
- Tạo schema cho người dùng, yêu thích và lịch sử khi triển khai các chức năng tương ứng.

![endpoint diagram](/images/5-Workshop/5.3-Backend/14.jpg)

### 4. Xây dựng tầng CRUD

- Tạo hàm truy vấn danh sách công thức.
- Giới hạn số bản ghi trả về trong mỗi request.
- Hỗ trợ phân trang cho danh sách công thức.
- Chỉ lựa chọn các trường cần thiết đối với API danh sách.
- Tạo hàm lấy chi tiết công thức theo recipe_id.
- Tạo hàm lấy ngẫu nhiên một công thức.
- Tạo hàm lấy dữ liệu dinh dưỡng theo fdc_id.
- Tạo các hàm kiểm tra sự tồn tại của bản ghi.
- Sử dụng SQLAlchemy ORM hoặc câu lệnh có tham số.
- Không ghép trực tiếp dữ liệu người dùng vào câu SQL.
- Trả về None nếu không tìm thấy dữ liệu.
- Không cho phép CRUD cập nhật hoặc xóa dữ liệu trong recipes và nutrition.
![endpoint diagram](/images/5-Workshop/5.3-Backend/15.jpg)

### 5. Xây dựng tầng Service

- Tạo service xử lý nghiệp vụ công thức.
- Gọi tầng CRUD để truy xuất dữ liệu từ RDS.
- Chuyển kết quả truy vấn sang schema phản hồi.
- Chuẩn hóa cấu trúc phân trang.
- Kiểm tra giới hạn page và limit.
- Xử lý trường hợp không tìm thấy công thức.
- Tạo service xử lý dữ liệu dinh dưỡng.
- Tách nghiệp vụ khỏi router để dễ kiểm thử và bảo trì.
- Không trả trực tiếp lỗi database cho người dùng.
- Ghi log lỗi cần thiết nhưng không ghi mật khẩu, token hoặc chuỗi kết nối.

![endpoint diagram](/images/5-Workshop/5.3-Backend/16.jpg)

### 6. Xây dựng Recipe & Nutrition API
- Xây dựng API lấy danh sách công thức: GET /recipes
- Xây dựng API lấy ngẫu nhiên công thức: GET /recipes/random
- Xây dựng API lấy chi tiết công thức: GET /recipes/{recipe_id}
- Xây dựng API: GET /nutrition/{fdc_id}

### 7. Khởi tạo ứng dụng FastAPI

- Tạo file main.py.
- Khởi tạo ứng dụng:
- Import các router đã xây dựng.
- Đăng ký router công thức.
- Đăng ký router dinh dưỡng.
- Tạo endpoint kiểm tra trạng thái:
    - GET /
- Trả về trạng thái backend đang hoạt động.
- Chỉ bật tài liệu Swagger trong môi trường phù hợp.
- Không trả stack trace hoặc thông tin nội bộ trong response lỗi.

### 8. Chạy Backend cục bộ

- Kích hoạt môi trường ảo.
- Chuyển đến thư mục backend.
- Khởi động FastAPI:
    - uvicorn main:app --reload
- Truy cập endpoint kiểm tra:
    - http://127.0.0.1:8000/

![endpoint diagram](/images/5-Workshop/5.3-Backend/17.jpg)

- Mở Swagger UI:
    - http://127.0.0.1:8000/docs

![endpoint diagram](/images/5-Workshop/5.3-Backend/18.jpg)

- Kiểm tra backend khởi động không có lỗi import.

### 9. Kiểm thử Recipe API
- Gọi GET /recipes.
- Gọi GET /recipes/random.
- Gọi GET /recipes/{recipe_id} với ID tồn tại.
- Gọi GET /nutrition/167512
- Gọi GET /nutrition/{fde_id} với ID tồn tại.

![endpoint diagram](/images/5-Workshop/5.3-Backend/19.jpg)

### 13. Hoàn thiện API
- Kiểm tra người dùng Cognito được đồng bộ chính xác vào bảng users trên Amazon RDS.
- Hoàn thiện API Favorites để người dùng thêm, xem và xóa công thức yêu thích.

![endpoint diagram](/images/5-Workshop/5.3-Backend/34.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/35.jpg)
- Hoàn thiện API Search History để lưu và truy xuất lịch sử tìm kiếm.

![endpoint diagram](/images/5-Workshop/5.3-Backend/36.jpg)

- Hoàn thiện API Chat History để lưu và quản lý lịch sử trò chuyện.

![endpoint diagram](/images/5-Workshop/5.3-Backend/37.jpg)

- Hoàn thiện các API cá nhân và chức năng Backend còn lại của hệ thống.
![endpoint diagram](/images/5-Workshop/5.3-Backend/38.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/39.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/40.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/41.jpg)

- Yêu cầu access token Cognito đối với các API chứa dữ liệu cá nhân.
- Lấy danh tính người dùng từ access token đã được Backend xác minh.
- Kiểm tra người dùng chỉ có thể truy cập và thay đổi dữ liệu thuộc tài khoản của mình.
- Kiểm thử các API bằng Swagger UI và dữ liệu thực tế trên Amazon RDS.

