---
title : "Chuẩn bị cơ sở dữ liệu"
date : 2026-07-07 
weight : 1
chapter : false
pre : " <b> 5.2.1 </b> "
---

### Bước 1 Chuẩn bị 
- Tải Dataset về 
![endpoint diagram](/images/5-Workshop/5.3-Backend/1.jpg)
![endpoint diagram](/images/5-Workshop/5.3-Backend/2.jpg)

### Bước 2 DATA PROCESSING

Thống kê số lượng bản ghi và các trường dữ liệu của Food.
- Dataset-processing/data/reports/recipes/profile.json

Chuyển đổi các trường nguyên liệu, bước nấu và thẻ món ăn về đúng cấu trúc.
- Dataset-processing/data/reports/recipes/ parse_report.json

Kiểm tra dữ liệu thiếu, dữ liệu trùng lặp và dữ liệu không hợp lệ
- dataset-processing/data/reports/recipes/ duplicate_report.json

Gộp và loại bỏ các bản ghi công thức trùng lặp.
- dataset-processing/data/reports/recipes/ merge_report.json

Chuẩn hóa tên món ăn và danh sách nguyên liệu.
Loại bỏ các bản ghi không đáp ứng yêu cầu.
- dataset-processing/data/reports/recipes/ clean_report.json

Kiểm tra lại dữ liệu sau khi xử lý.
Xuất recipes_cleaned.pkl.
- dataset-processing/data/reports/recipes/ verify_report.json

Trích xuất dữ liệu calories cần thiết từ dữ liệu USDA.
- Dataset-processing/data/reports/USDA/energy_report.json

Chuẩn hóa tên thực phẩm và kiểu dữ liệu dinh dưỡng.
- dataset-processing/data/reports/USDA/clean_report.json

Kiểm tra lại dữ liệu sau khi xử lý.
Xuất nutrition_cleaned.pkl.
- dataset-processing/data/reports/USDA/verify_report.json

### Bước 3 Thiết kế cơ sở dữ liệu
1. Xác định dữ liệu cần lưu trữ
2. Thiết kế CSDL
-	Bảng user để lưu thông tin người dùng:
    -	user_id: khóa chính tự tăng.
    -	cognito_sub: mã định danh người dùng từ Amazon Cognito.
    -	username: tên hiển thị.
    -	email: địa chỉ email.
    -	created_at: thời điểm tạo tài khoản.
-	Bảng recipes để lưu công thức món ăn:
    -	recipe_id: khóa chính.
    -	name: tên món ăn.
    -	description: mô tả.
    -	ingredients: danh sách nguyên liệu đã chuẩn hóa.
    -	ingredients_raw: danh sách nguyên liệu ban đầu.
    -	steps: các bước chế biến.
    -	servings: số khẩu phần.
    -	serving_size: kích thước khẩu phần.
    -	tags: các thẻ phân loại.
    -	Các trường thống kê số lượng và trạng thái dữ liệu.
-	Bảng nutrition để lưu dữ liệu dinh dưỡng:
    -	fdc_id: khóa chính từ USDA.
    -	food_name: tên thực phẩm.
    -	data_type: loại dữ liệu USDA.
    -	calories: lượng calories.
-	Bảng favorites để lưu các món ăn yêu thích:
    -	user_id: khóa ngoại tham chiếu users.
    -	recipe_id: khóa ngoại tham chiếu recipes.
    -	created_at: thời điểm thêm món ăn.
    -	Sử dụng cặp user_id và recipe_id làm khóa chính kết hợp của bảng favorites.
-	Bảng search_history để lưu lịch sử tìm kiếm:
    -	history_id: khóa chính tự tăng.
    -	user_id: người thực hiện tìm kiếm.
    -	keyword: từ khóa tìm kiếm.
    -	total_result: số kết quả trả về.
    -	searched_at: thời điểm tìm kiếm.
-	Bảng chat_history để lưu lịch sử trò chuyện:
    -	history_id: khóa chính tự tăng.
    -	user_id: người gửi tin nhắn.
    -	session_id: mã phiên trò chuyện.
    -	message: nội dung tin nhắn.
    -	role: vai trò gửi tin nhắn.
    -	recipe_id: công thức liên quan, có thể để trống.
    -	created_at: thời điểm tạo tin nhắn.
3. Thiết lập quan hệ giữa các bảng
-	Thiết lập quan hệ một–nhiều giữa users và favorites.
-	Thiết lập quan hệ một–nhiều giữa recipes và favorites.
-	Thiết lập quan hệ một–nhiều giữa users và search_history.
-	Thiết lập quan hệ một–nhiều giữa users và chat_history.
-	Thiết lập quan hệ tùy chọn giữa recipes và chat_history.
4. Xác định ràng buộc dữ liệu
- Đặt user_id, recipe_id, fdc_id và các mã lịch sử làm khóa chính.
- Đặt cognito_sub là duy nhất để tránh đồng bộ trùng tài khoản Cognito.
- Đặt email là duy nhất để tránh nhiều tài khoản sử dụng cùng một email.
- Đặt các trường bắt buộc ở trạng thái NOT NULL.
- Cho phép username để trống khi Cognito chưa cung cấp tên hiển thị.
- Cho phép recipe_id trong chat_history để trống khi cuộc trò chuyện không liên quan đến công thức cụ thể.
- Sử dụng kiểu JSON cho:
    - ingredients
    - ingredients_raw
    - steps
    - tags
- Sử dụng utf8mb4 để hỗ trợ đầy đủ ký tự Unicode.
- Sử dụng thời gian mặc định của MySQL cho các trường ngày tạo.
5. Thiết kế index
- Tạo idx_recipe_name cho trường recipes.name.
- Tạo idx_favorite_recipe cho trường favorites.recipe_id.
- Tạo index kết hợp idx_search_user_searched cho:
    - search_history.user_id
    - search_history.searched_at
- Tạo index kết hợp idx_chat_user_created cho:
    - chat_history.user_id
    - chat_history.created_at
- Tạo idx_chat_recipe cho trường chat_history.recipe_id.
6. Tạo các file SQL
- Tạo file schema.sql.
    - .\backend-api\database\seed\schema.sql
- Viết câu lệnh tạo sáu bảng dữ liệu.
- Khai báo khóa chính, khóa ngoại và các ràng buộc.
- Thiết lập database sử dụng utf8mb4.
- Tạo file indexes.sql.
    - .\backend-api\database\seed\ indexes.sql.
- Viết riêng các câu lệnh tạo index để thuận tiện kiểm tra và triển khai.
- Kiểm tra cú pháp của hai file SQL.
- Đối chiếu cấu trúc SQL với dữ liệu đã xử lý.
- *Lưu hai file trong thư mục database của backend.*
7. Kiểm tra thiết kế cơ sở dữ liệu
- Kiểm tra tên bảng và tên cột.
- Kiểm tra kiểu dữ liệu của từng trường.
- Kiểm tra độ dài của các trường chuỗi.
- Kiểm tra khóa chính và khóa ngoại.
- Kiểm tra các ràng buộc UNIQUE và NOT NULL.
- Kiểm tra quan hệ giữa các bảng.
- Kiểm tra khả năng lưu các trường JSON.
- Kiểm tra thiết kế với một số bản ghi dữ liệu mẫu.
- Xác nhận cấu trúc tương thích với MySQL 8.
- Chốt schema.sql và indexes.sql để sử dụng khi tạo cơ sở dữ liệu trên Amazon RDS.
