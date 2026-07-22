---
title: "Bản đề xuất"
date: 2026-07-07
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# AI Recipe Finder   
## Hệ thống gợi ý món ăn thông minh bằng AI trên nền tảng AWS

### 1. Tóm tắt điều hành  
AI Recipe Finder là website hỗ trợ người dùng tìm kiếm món ăn dựa trên nguyên liệu sẵn có. Hệ thống sử dụng AI để gợi ý món ăn, hỗ trợ nấu ăn qua chatbot và phân tích giá trị dinh dưỡng dựa trên dữ liệu chuẩn. Toàn bộ hệ thống được triển khai trên nền tảng Amazon Web Services (AWS) nhằm đảm bảo khả năng mở rộng, ổn định và dễ triển khai thực tế.  

### 2. Tuyên bố vấn đề  
#### Vấn đề hiện tại 
Người dùng thường xuyên gặp khó khăn trong việc quyết định thực đơn hàng ngày dựa trên những nguyên liệu sẵn có hoặc còn sót lại trong bếp, dẫn đến lãng phí thực phẩm. Việc tìm kiếm công thức nấu ăn phù hợp trên các công cụ truyền thống mất nhiều thời gian và thường không khớp chính xác với nguyên liệu thực tế. Bên cạnh đó, việc tự theo dõi và tính toán các chỉ số dinh dưỡng (Calories, Protein, Fat, Carbohydrates) cho từng bữa ăn rất thủ công và phức tạp. Các ứng dụng hiện tại trên thị trường thường thiếu đi sự tương tác thông minh, không có hệ thống tư vấn dinh dưỡng hay hướng dẫn nấu ăn linh hoạt theo thời gian thực. 

#### Giải pháp
Nền tảng AI Recipe Finder được triển khai hoàn toàn trên mô hình Cloud của AWS để giải quyết triệt để các vấn đề trên. Nền tảng sử dụng công nghệ tìm kiếm thông minh bằng AI (Semantic Search) để phân tích nguyên liệu người dùng nhập và đưa ra gợi ý món ăn chuẩn xác. Hệ thống tích hợp Amazon Bedrock để vận hành Chatbot AI, đóng vai trò như một trợ lý ảo hỗ trợ hướng dẫn nấu ăn từng bước và tư vấn thay thế nguyên liệu. Backend API (chạy trên EC2 hoặc App Runner) sẽ kết nối với cơ sở dữ liệu USDA để tự động phân tích chi tiết Calories, Protein, Fat và Carbohydrates cho từng món ăn. Toàn bộ giao diện được cung cấp qua AWS Amplify, kết hợp cơ sở dữ liệu Amazon RDS, tạo ra một kiến trúc tập trung, ổn định và dễ dàng mở rộng.  

#### Lợi ích
Giải pháp mang lại một công cụ đắc lực giúp người dùng cá nhân tiết kiệm thời gian, tối ưu hóa việc sử dụng thực phẩm để tránh lãng phí, và dễ dàng duy trì chế độ ăn uống lành mạnh nhờ dữ liệu dinh dưỡng được tự động hóa. Về mặt kỹ thuật, nền tảng tạo ra một sản phẩm cơ sở hoàn chỉnh minh chứng cho khả năng ứng dụng thực tiễn và hiệu quả của Generative AI kết hợp với Cloud Computing. Đồng thời, kiến trúc linh hoạt của AWS giúp xây dựng một nền tảng vững chắc để nhóm có thể dễ dàng mở rộng thêm các tính năng nâng cao như lên thực đơn cá nhân hóa, nhận diện nguyên liệu qua ảnh, hoặc hướng tới mục tiêu thương mại hóa hệ thống trong tương lai.  

### 3. Kiến trúc giải pháp  
Nền tảng áp dụng kiến trúc đám mây (Cloud-based) trên AWS để quản lý và gợi ý hàng ngàn công thức nấu ăn dựa trên nguyên liệu của người dùng. Các yêu cầu từ giao diện web được tiếp nhận và xử lý bởi Backend API (chạy trên EC2 hoặc App Runner), tra cứu dữ liệu từ Amazon RDS và hình ảnh từ Amazon S3. Các tác vụ nâng cao như tìm kiếm theo ngữ nghĩa (Semantic Search) và Chatbot tư vấn dinh dưỡng được xử lý trực tiếp bởi Amazon Bedrock, trong khi AWS Amplify cung cấp giao diện người dùng mượt mà và ổn định.   

![AI Recipe Finder](/images/2-Proposal/Proposal1.jpg)



#### Dịch vụ AWS sử dụng
-	AWS Amplify: Lưu trữ và triển khai giao diện web (React + Tailwind CSS).
-	Amazon Cognito: Đăng nhập, xác thực danh tính và quản lý quyền truy cập của người dùng một cách bảo mật.
-	Amazon EC2: Vận hành Backend API (FastAPI/Node.js) xử lý logic hệ thống.
-	Amazon RDS: Cơ sở dữ liệu quan hệ lưu trữ thông tin người dùng, lịch sử tìm kiếm và công thức nấu ăn.
-	Amazon S3: Lưu trữ tài nguyên tĩnh, chủ yếu là hình ảnh các món ăn.
-	Amazon Bedrock: Cung cấp mô hình AI (LLMs) để vận hành tính năng Semantic Search và AI Chatbot tư vấn nấu ăn.
-	AWS CloudWatch & Budgets: Theo dõi log hệ thống, giám sát hiệu suất và cảnh báo chi phí.

#### Thiết kế thành phần
-	Giao diện web: AWS Amplify lưu trữ ứng dụng Frontend, cung cấp bảng điều khiển để người dùng nhập nguyên liệu, xem công thức và trò chuyện với AI.
-	Xác thực và quản lý người dùng: Amazon Cognito đảm nhiệm việc đăng ký, đăng nhập và bảo mật danh tính, đảm bảo quyền truy cập an toàn vào các tính năng cá nhân hóa (như lưu món ăn yêu thích, lịch sử tìm kiếm).
-	Xử lý trung tâm: Backend API tiếp nhận yêu cầu từ giao diện, điều phối logic tra cứu công thức từ Food.com và gọi các dịch vụ phân tích.
-	Lưu trữ dữ liệu: Dữ liệu có cấu trúc (thông tin món ăn, tài khoản) được lưu trong Amazon RDS; dữ liệu phi cấu trúc (hình ảnh món ăn) lưu tại Amazon S3.
-	Động cơ AI: Amazon Bedrock xử lý ngôn ngữ tự nhiên, phân tích nguyên liệu đầu vào để tìm kiếm món ăn phù hợp và phản hồi các câu hỏi về nấu ăn qua Chatbot.
-	Phân tích dinh dưỡng: Backend kết nối với cơ sở dữ liệu USDA FoodData Central để trích xuất và tính toán Calories, Protein, Fat, Carbohydrates cho từng gợi ý.
  

### 4. Chức năng chính
#### *Các chức năng của hệ thống triển khai*  
1. Công cụ tìm kiếm và gợi ý thông minh (AI-Powered)
-	Tìm kiếm theo nguyên liệu: Cho phép người dùng nhập các nguyên liệu đang có sẵn để hệ thống truy xuất các công thức nấu ăn tương ứng từ cơ sở dữ liệu.
-	AI gợi ý món ăn: Ứng dụng Semantic Search để hiểu ngữ cảnh, đưa ra gợi ý món ăn chính xác ngay cả khi người dùng nhập từ khóa chung chung.
-	Vòng quay chọn món: Tính năng ngẫu nhiên chọn món ăn giúp giải quyết vấn đề "hôm nay ăn gì" dựa trên sở thích hoặc nguyên liệu sẵn có.
-	Phân tích dinh dưỡng: Tự động tính toán và hiển thị các chỉ số chi tiết (Calories, Protein, Fat, Carbohydrates) cho từng món ăn dựa trên dữ liệu chuẩn của USDA.
2. Trợ lý ảo AI và công cụ hỗ trợ nấu nướng
-	Chatbot hỗ trợ nấu ăn: Trợ lý AI (vận hành bởi Amazon Bedrock) hướng dẫn từng bước nấu nướng, giải đáp thắc mắc về công thức theo thời gian thực.
-	Gợi ý thay thế nguyên liệu: Trợ lý AI tự động đề xuất các nguyên liệu thay thế phù hợp khi người dùng thiếu đồ nhưng vẫn giữ nguyên vẹn hương vị món ăn.
-	Điều chỉnh khẩu phần: Hệ thống tự động tính toán lại định lượng chuẩn xác của các nguyên liệu khi người dùng thay đổi số lượng khẩu phần ăn.
-	Danh sách mua sắm: Tính năng tự động tạo danh sách đi chợ từ các công thức người dùng chọn, giúp quản lý việc mua sắm nguyên liệu dễ dàng.
3. Cá nhân hóa trải nghiệm người dùng
-	Đăng ký / Đăng nhập: Hệ thống xác thực danh tính bảo mật qua Amazon Cognito, giúp đồng bộ dữ liệu người dùng.
-	Lưu món ăn yêu thích và Recipe Collection: Cho phép người dùng lưu lại các món ăn tâm đắc và tự tạo các "Bộ sưu tập công thức" theo chủ đề cá nhân (ví dụ: món chay, món ăn kiêng, tiệc cuối tuần).
-	Lưu lịch sử tìm kiếm: Theo dõi và lưu trữ các tìm kiếm gần đây để tối ưu hóa trải nghiệm truy cập.
-	Ghi chú cá nhân: Tính năng cho phép người dùng đính kèm các ghi chú riêng (ví dụ: gia giảm độ mặn/ngọt) vào từng công thức đã lưu.
4. Nhật ký và chia sẻ
-	Nhật ký nấu ăn (Cooking Diary): Nơi người dùng lưu giữ hành trình nấu nướng của mình, tải lên hình ảnh thành phẩm thực tế và đánh giá độ thành công của món ăn.
-	Chia sẻ: Tích hợp tính năng chia sẻ công thức, thành phẩm hoặc danh sách mua sắm cho bạn bè và người thân qua các nền tảng mạng xã hội hoặc tin nhắn.


### 5. Lộ trình & Mốc triển khai  
**Các giai đoạn triển khai**
- *Giai đoạn chuẩn bị:*
    - Tháng 1: Học tập và tìm hiểu AWS và lên kế hoạch triển khai dự án.
- *Giai đoạn phát triển và triển khai:*
    - Tháng 2: Tiến hành xây dựng thiết kế hệ thống hạ tầng AWS (Amplify, EC2, RDS, S3), xây dựng cơ sở dữ liệu từ nguồn dữ liệu (Food.com, USDA FoodData Central) và phát triển giao diện Frontend cơ bản.
    - Tháng 3: Tích hợp Amazon Bedrock, xây dựng tính năng tìm kiếm AI (Semantic Search), Chatbot hỗ trợ nấu ăn và hoàn thiện các chức năng cá nhân hóa (Recipe Collection, Cooking Diary). Và tiến hành chạy thử hệ thống.
- *Giai đoạn sau triển khai:*
    - Theo dõi hiệu suất, tối ưu hóa chi phí AWS và nghiên cứu mở rộng các tính năng nâng cao trong vòng tháng tiếp theo.

### 6. Ước tính ngân sách  

#### *Chi phí hạ tầng*
-	AWS Amplify: 1,00 USD/tháng (Hosting frontend React, phục vụ các yêu cầu giao diện cơ bản).
-	Amazon EC2: 15,00 USD/tháng (Vận hành 1 instance/service quy mô nhỏ cho Backend API).
-	Amazon RDS : 15,00 USD/tháng (Cấu hình db.t3.micro/db.t4g.micro lưu trữ dữ liệu món ăn và người dùng).
-	Amazon Bedrock: 25,00 USD/tháng (Ước tính xử lý các request AI cho Semantic Search và Chatbot).
-	Amazon Cognito: 0,00 USD/tháng (Sử dụng AWS Free Tier hỗ trợ tối đa 50.000 MAU).
-	Amazon S3: 0,50 USD/tháng (Lưu trữ hình ảnh món ăn và dữ liệu tĩnh).
-	AWS CloudWatch & Budgets: 1,00 USD/tháng (Ghi log hệ thống cơ bản và thiết lập cảnh báo hạn mức chi phí).
-	Truyền dữ liệu (Data Transfer): 2,00 USD/tháng (Băng thông truyền tải dữ liệu giữa các dịch vụ và người dùng).

*Tổng chi phí:* ~59,50 USD/tháng (Khoảng 178,50 USD cho 3 tháng thử nghiệm/demo)
- Chi phí khác: 10,00 – 12,00 USD một lần (Tên miền tùy chọn cho hệ thống, hoặc 0 USD nếu sử dụng subdomain mặc định do AWS Amplify cấp).


### 7. Đánh giá rủi ro  
#### *Ma trận rủi ro*
-	Phát sinh chi phí AI (Amazon Bedrock): Ảnh hưởng cao, xác suất trung bình.
-	Quá tải băng thông (Data Transfer): Ảnh hưởng trung bình, xác suất trung bình.
-	Lỗi kết nối dữ liệu ngoại vi (USDA Database): Ảnh hưởng cao, xác suất thấp.

#### *Chiến lược giảm thiểu*
-	Chi phí AI: Áp dụng cơ chế Cache (lưu trữ tạm) các kết quả tìm kiếm và câu hỏi phổ biến, giới hạn số lượng request.
-	Băng thông: Tối ưu hóa kích thước hình ảnh trước khi lưu lên S3, theo dõi sát chi phí qua AWS Budgets.
-	Dữ liệu: Đồng bộ và lưu trữ sẵn một phần dữ liệu dinh dưỡng cơ bản vào Amazon RDS để giảm phụ thuộc vào API bên ngoài.

#### *Kế hoạch dự phòng*
-	Chuyển sang tìm kiếm từ khóa truyền thống (Full-text search trên cơ sở dữ liệu MySQL) nếu hệ thống AI Bedrock gặp sự cố hoặc chạm ngưỡng ngân sách.
-	Sử dụng Infrastructure as Code (như AWS CloudFormation) để dễ dàng hủy toàn bộ tài nguyên khi không cần thiết và khôi phục lại nhanh chóng cấu hình hệ thống khi cần demo.


