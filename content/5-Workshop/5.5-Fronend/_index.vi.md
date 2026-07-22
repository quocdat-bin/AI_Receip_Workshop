---
title : "Triển khai Frontend & Xác thực (Frontend & Auth)"
date : 2026-07-07
weight : 5
chapter : false
pre : " <b> 5.4 </b> "
---

### 1. Cấu hình ứng dụng React
Để Frontend có thể giao tiếp mượt mà với các dịch vụ AWS (Cognito, API Gateway) và dễ dàng mở rộng trong tương lai, toàn bộ mã nguồn React được tổ chức chặt chẽ theo mô hình module hóa (Feature-based).

#### Đây là tổng thể toàn bộ cấu trúc thư mục:
![endpoint diagram](/images/5-Workshop/5.5-Fronend/1.jpg)

#### Đây là src/app/ (Điểm khởi đầu ứng dụng)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/2.jpg)

-	App.jsx: Component gốc điều phối toàn bộ ứng dụng. 
-	ErrorBoundary.jsx: Xử lý lỗi hệ thống/giao diện ngoài ý muốn. 
-	StyleGuide.jsx: Trang quy chuẩn thiết kế và các thành phần mẫu. 

#### src/routes/ (Định tuyến)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/3.jpg)
-	AppRoutes.jsx: Cấu hình các đường dẫn (routes) chính của ứng dụng. 
-	RequireAuth.jsx & RequireAdmin.jsx: Các route bảo vệ yêu cầu quyền đăng nhập hoặc quyền quản trị viên (Admin). 
-	ScrollToTop.jsx: Tự động cuộn lên đầu trang khi chuyển hướng route. 
-	NotFoundPage.jsx: Route tự định tuyến nếu không hợp lệ

#### src/layouts/ (Bố cục giao diện)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/4.jpg)
-	MainLayout.jsx: Khung giao diện chính cho người dùng thông thường. 
-	AdminLayout.jsx: Khung giao diện dành riêng cho trang quản trị. 
-	AuthLayout.jsx: Khung giao diện cho các trang xác thực (Đăng nhập, Đăng ký). 
-	components/: Bao gồm Navbar, Sidebar, Footer, Logo, LanguageSwitcher. 

#### src/features/ (Các tính năng chính)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/5.jpg)
**Mỗi thư mục con đại diện cho một module chức năng độc lập:**
-	home/: Trang chủ với Banner, danh mục nổi bật, gợi ý AI, đánh giá và mẹo dinh dưỡng. 
-	recipes/: Quản lý công thức nấu ăn (Chi tiết công thức, bộ lọc, tìm kiếm nguyên liệu bằng AI, đánh giá, bình luận, ghi chú cá nhân). 
-	calculator/: Công cụ tính toán dinh dưỡng, chỉ số cơ thể (như BMI) và phân phối macro khẩu phần ăn. 
-	cooking/: Chế độ nấu ăn tương tác từng bước (CookingMode). 
-	wheel/: Tính năng vòng quay ngẫu nhiên (SpinWheel) giúp giải quyết câu hỏi "Hôm nay ăn gì?". 
-	shopping/: Quản lý danh sách mua sắm nguyên liệu (ShoppingList). 
-	collections/: Lưu trữ và quản lý các bộ sưu tập công thức cá nhân. 
-	nutrition/: Theo dõi chỉ số dinh dưỡng hằng ngày, biểu đồ lịch sử và mục tiêu cá nhân. 
-	chat/: Trợ lý ảo AI tư vấn nấu ăn và tìm kiếm công thức. 
-	profile/: Quản lý trang cá nhân, danh sách yêu thích và cài đặt tài khoản. 
-	auth/: Trang đăng nhập, đăng ký, xác thực email (VerifyEmail, Callback). 
-	admin/: Trang quản trị hệ thống (Thống kê, quản lý người dùng, công thức và danh mục). 

#### src/services/ (Xử lý dữ liệu & API)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/6.jpg)
-	Services: Chứa các tệp giao tiếp API và logic xử lý theo từng domain như RecipeService, AuthService, ChatService, NutritionService, ShoppingListService, AdminService... 

#### src/ui/ (Thư viện UI Components dùng chung)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/7.jpg)
-	Chứa các thành phần giao diện cơ bản có thể tái sử dụng cao như: Button, Card, Modal, Input, Tabs, Badge, Dropdown, Pagination, cùng với thư mục charts/ tích hợp biểu đồ (BarChart, LineChart, MacroDoughnut). 

#### src/hooks/ & src/contexts/ (State Management & Custom Hooks)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/8.jpg)

-	Contexts: Quản lý trạng thái toàn cục (AuthContext, FavoritesContext, ShoppingListContext). 
-	Hooks: Các custom hooks tối ưu hóa logic (useAuth, useDebounce, useLocalStorage, useAsync, useFavorite). 

### 2.	Cấu hình môi trường & Triển khai
![endpoint diagram](/images/5-Workshop/5.5-Fronend/9.jpg)

-	package.json / package-lock.json: Quản lý thư viện và các tập lệnh chạy dự án (scripts). 
-	vite.config.js: Cấu hình bộ biên dịch Vite. 
-	amplify.yml: Tệp cấu hình triển khai (deployment) thường dùng cho AWS Amplify. 
- .env.example: Mẫu biến môi trường cần thiết khi thiết lập dự án. 

### 3.	Các bước thực hiện

**Bước 1: Chuẩn bị kho lưu trữ (Repository)**
-	Đảm bảo mã nguồn của dự án (bao gồm thư mục chứa frontend) đã được đẩy lên kho lưu trữ Git trực tuyến (như GitHub, GitLab, Bitbucket hoặc AWS CodeCommit).
![endpoint diagram](/images/5-Workshop/5.5-Fronend/10.jpg)

**Bước 2: Kết nối dự án với AWS Amplify**

-	Đăng nhập vào AWS Management Console, tìm kiếm và truy cập vào dịch vụ AWS Amplify.
![endpoint diagram](/images/5-Workshop/5.5-Fronend/11.jpg)

-	Tại trang tổng quan, chọn Host your web app (Triển khai ứng dụng web).
-	Chọn nhà cung cấp mã nguồn (ví dụ: GitHub) và bấm Next.
-	Tìm và chọn kho lưu trữ của bạn (ví dụ: nhi-ai/ai-recipe-finder) cùng nhánh triển khai (ví dụ: main).

![endpoint diagram](/images/5-Workshop/5.5-Fronend/12.jpg)

- Cấp quyền cho AWS Amplify truy cập vào tài khoản và chọn kho lưu trữ (ai-recipe-finder) cùng nhánh (branch) bạn muốn triển khai (ví dụ: main hoặc master).

![endpoint diagram](/images/5-Workshop/5.5-Fronend/13.jpg)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/14.jpg)
- *Thiết lập biến môi trường *
![endpoint diagram](/images/5-Workshop/5.5-Fronend/18.jpg)
- Review lại lần cuối

**Bước 3: Cấu hình Build (Build Settings)**
- Thiết lập ứng dụng: Đặt tên ứng dụng tại mục App name, kiểm tra lại lệnh build (npm run build) và thư mục đầu ra (dist) tại mục Build settings.
- Cấu hình biến môi trường: Mở rộng phần cài đặt nâng cao để thêm các biến môi trường cần thiết
![endpoint diagram](/images/5-Workshop/5.5-Fronend/15.jpg)
![endpoint diagram](/images/5-Workshop/5.5-Fronend/16.jpg)
**Bước 4: Triển khai (Deploy)**
- Kiểm tra lại toàn bộ thông tin cấu hình tại trang OverReview.
- Nhấn nút xác nhận để tiến hành build và triển khai lên hệ thống CDN toàn cầu của AWS.

![endpoint diagram](/images/5-Workshop/5.5-Fronend/17.jpg)

-	Nhấn Save và sau đó bấm Save and deploy.
-	AWS Amplify sẽ tự động thực hiện các công việc:
  -	Kéo mã nguồn về máy chủ ảo.
  -	Cài đặt các gói phụ thuộc (dependencies).
  -	Tiến hành build dự án thành các tệp tĩnh.
  - Phát hành lên hệ thống phân phối nội dung toàn cầu (CDN) của AWS.
