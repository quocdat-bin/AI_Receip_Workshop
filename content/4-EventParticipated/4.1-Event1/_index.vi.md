---
title: "Event 1"
date: 2026-07-07
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch “AWS FIRST CLOUD AI JOURNEY COMMUNITY DAY”

### Mục Đích Của Sự Kiện

-	Cập nhật các xu hướng thực tiễn về Cloud Computing và cách ứng dụng Generative AI (GenAI) vào môi trường doanh nghiệp.
-	Chia sẻ các best practices trong việc thiết kế kiến trúc phần mềm nền tảng, xây dựng hệ thống đa tác vụ (multi-agent) và bảo mật hạ tầng mạng.
-	Tạo không gian kết nối cộng đồng IT, truyền cảm hứng và định hướng bộ kỹ năng cần thiết cho kỹ sư trong kỷ nguyên AI.


### Danh Sách Diễn Giả

-	**Nguyễn Gia Hưng** - Solutions Architect, AWS Vietnam (Founder FCAJ)
-	**Tinh Truong** - Platform Engineer, GoTymeX
-	**Anh Pham** - Cloud Consultant, G-AsiaPacific Vietnam
-	**Thinh Nguyen** - DevOps Engineer, FCAJ
-	**Uyển Lê, Thảo Nguyễn, Mai Nguyễn** - GenAI Engineers, VIB
-	**Duc Dao** - Solutions Architect, Cloud Kinetics
-	**Vy Lâm** - Senior Business Systems Analyst, VPBank


### Nội Dung Nổi Bật

####	Bức tranh việc làm IT thay đổi & Tư duy mới của Kỹ sư:
Khi AI liên tục kéo chi phí phát triển phần mềm xuống thấp, nhu cầu ra mắt sản phẩm mới cũng theo đó tăng vọt. Trong hoàn cảnh này, chỉ nắm lý thuyết thôi không đủ để khẳng định điều gì; giá trị thật sự nằm ở chỗ kỹ sư hiểu chính xác bài toán nghiệp vụ và mang ra được một sản phẩm chạy thật để chứng minh năng lực của mình.
#### Cung cấp đúng ngữ cảnh mà AI cần:
Phần lớn hiện tượng ảo giác (hallucination) xuất phát từ việc mô hình bị quá tải thông tin. Thay vì đổ hàng loạt tài liệu chung chung lấy từ internet vào mô hình (kiểu mà các diễn giả gọi đùa là bẫy "Internet Builder"), kỹ sư sẽ có được câu trả lời gọn gàng hơn hẳn khi thu hẹp phạm vi và chỉ đưa vào đúng phần ngữ cảnh thật sự cần thiết.
####	Tự động hóa công việc thường ngày với Amazon Q (AI Agent):

Một cái nhìn thực tế về cách tận dụng trợ lý ảo để xử lý file Excel, dựng dashboard BI và tự động tóm tắt cuộc họp, mọi thứ được liên kết liền mạch giữa các công cụ văn phòng thông qua MCP (Model Context Protocol).
####	Thiết kế mạng và Bảo mật với Amazon CloudFront:
Mô hình Flat-rate pricing được trình bày như một lớp bảo vệ trước "bill shock", tức những cú nhảy chi phí bất ngờ xuất hiện sau lượng truy cập bất thường hoặc một đợt tấn công DDoS. Nhờ phân tán tải qua các điểm biên Point of Presence (PoP) của AWS và kết hợp với VPC Origin, CloudFront có thể đấu nối trực tiếp vào private subnet, giúp backend hoàn toàn tách biệt khỏi internet công cộng.
####	Bài học thực chiến từ Hackathon & Phát triển sản phẩm GenAI (UTM Morpo):
Nhóm kể lại chặng chạy nước rút 36 giờ để xây dựng công cụ sinh giao diện (UI) trên nền Serverless, được điều phối bởi 3 AI Agent cùng phối hợp. Bước đột phá lớn nhất của họ: thay vì tiêu tốn thời gian và token để tạo lại toàn bộ giao diện sau mỗi lần, họ cho phép người dùng tinh chỉnh trực tiếp từng component và CSS ngay trên màn hình. Từ đó rút ra hàng loạt bài học quản trị rủi ro giá trị, để mắt tới ngân sách token, tránh sinh ra code dư thừa, và dồn sức tối đa cho tính năng cốt lõi thay vì cố nhồi nhét mọi ý tưởng.
####	Đối mặt với tính định hướng (Deterministic) của LLM:
Một buổi phân tích chuyên sâu về cách LLM thực sự hoạt động, kể cả sự thật khó chịu rằng kết quả đầu ra vẫn có thể sai lệch ngay cả khi đặt Temperature = 0, do hạn chế phần cứng GPU và cơ chế tối ưu suy luận từ phía nhà cung cấp. Những giải pháp phòng vệ được gợi ý: chạy cùng một prompt nhiều lần rồi bầu chọn kết quả, tự host mô hình, ép JSON mode, và quan trọng nhất là thiết kế các downstream service đủ linh hoạt để dung nạp những sai lệch ngẫu nhiên của AI.
####	Xây dựng hệ thống Multi-Agent quy mô doanh nghiệp:
Một case study về cách điều phối nhiều AI Agent chuyên biệt để đánh giá tín dụng startup. Bảo mật và tuân thủ được đặt làm trọng tâm thiết kế, siết chặt bề mặt tấn công MCP, chặn Prompt Injection, duy trì audit trail và luân chuyển khóa API theo lịch định sẵn.


### Những Gì Học Được

- **Tư Duy Kỹ Thuật Xuất Phát Từ Nghiệp Vụ:** 
Luôn bắt đầu từ bài toán kinh doanh và trải nghiệm thực tế của người dùng. Việc tách hệ thống thành các AI Agent hay tập trung xử lý dứt điểm một nỗi đau cốt lõi (chẳng hạn tính năng edit UI thay vì generate liên tục) đem lại giá trị lớn hơn nhiều.

- **Kiến Trúc Kỹ Thuật & An Toàn Thông Tin:** 
Học được cách thiết lập lớp chốt chặn an toàn bằng CloudFront. Với tôi, VPC Origin là một bước tiến đáng kể giúp cô lập kiến trúc backend khỏi những rủi ro đến từ internet.

- **Kiểm soát rủi ro từ hệ thống AI (AI Risk Management):** 
Nhận ra bản chất xác suất của LLM để tránh đặt niềm tin mù quáng vào kết quả trả về. Cần luôn dự trù các tình huống AI bị "ảo giác" (hallucinate), chạm giới hạn token, hoặc trả về sai định dạng để chuẩn bị sẵn cơ chế dự phòng.


### Ứng Dụng Vào Công Việc

-	Hệ thống Multi-Agent: Tách thành các Agent riêng biệt (phân tích sở thích, tra công thức, tính dinh dưỡng) để phần gợi ý cá nhân hóa cho ra kết quả chính xác hơn. 
-	Tối ưu luồng sinh nội dung: Khi người dùng muốn thay đổi, chỉ chỉnh sửa cục bộ (edit) đúng nguyên liệu đó thay vì tạo lại toàn bộ công thức, qua đó tiết kiệm token. 
-	Kiểm soát định dạng (Deterministic): Đặt Temperature ở mức thấp và ép JSON mode để dữ liệu trả về (tên món, nguyên liệu, cách làm) luôn đúng chuẩn. 
-	Bảo mật & Context: Dùng CloudFront cùng VPC Origin để bảo vệ database. Chỉ truyền ngữ cảnh tối thiểu (đúng những nguyên liệu người dùng đang có) vào prompt nhằm ngăn Prompt Injection. 


### Trải nghiệm trong event

Việc tham dự sự kiện **“AWS FIRST CLOUD AI JOURNEY COMMUNITY DAY”** là một trải nghiệm hết sức bổ ích, mang lại cho em cái nhìn toàn diện về cách phối hợp giữa các dịch vụ Cloud của AWS và sức mạnh của Generative AI để dựng nên những ứng dụng an toàn, ổn định. Một vài trải nghiệm đáng nhớ:

#### Học hỏi từ các diễn giả giàu chuyên môn

-	Các chuyên gia và kỹ sư đến từ AWS, VIB, GoTymeX, và Cloud Kinetics đã cởi mở chia sẻ những kinh nghiệm thực chiến quý giá trong việc thiết kế và phát triển phần mềm hiện đại.
-	Thông qua các case study thực tế (như bài toán Hackathon 36 giờ hay hệ thống đánh giá tín dụng startup), tôi hiểu sâu sắc hơn vì sao kiến trúc Multi-Agent lại xử lý các luồng nghiệp vụ phức tạp tốt hơn nhiều so với việc chỉ dựa vào một mô hình đơn lẻ.


#### Trải nghiệm kỹ thuật thực tế

-	Được mổ xẻ kỹ về cơ chế vận hành của LLM, nhờ đó tôi lý giải được vì sao AI "ảo giác" (hallucination) và hiểu tham số Temperature cùng JSON mode ảnh hưởng thế nào đến tính nhất quán của dữ liệu đầu ra.
-	Hình dung rõ ràng quy trình bảo mật hạ tầng mạng nhờ Amazon CloudFront và VPC Origin để cô lập những backend trọng yếu, che chắn hệ thống trước các đợt tấn công DDoS lẫn rủi ro cạn kiệt chi phí ("bill shock").
-	Hiểu được khái niệm MCP (Model Context Protocol) cùng các rủi ro bảo mật đi kèm như Prompt Injection hay MCP Attack Vectors khi trao quyền cho AI tương tác với hệ thống.

#### Bài học rút ra
-	Tách luồng xử lý AI thành các Agent chuyên biệt giúp giảm thiểu sai sót, nâng cao khả năng mở rộng (scalability) và gỡ lỗi hệ thống dễ dàng hơn.
-	Tuyệt đối không tin tưởng hoàn toàn vào kết quả của AI. Cần áp dụng các chiến lược kiểm soát (như chạy nhiều lần rồi bầu chọn kết quả) và luôn thiết kế hệ thống có kịch bản dự phòng khi AI trả về dữ liệu lỗi.
-	Bảo mật là yếu tố sống còn khi đưa ứng dụng GenAI vào môi trường production. Giới hạn quyền truy cập hạ tầng bằng CloudFront/VPC Origin và thiết lập quy trình luân chuyển khóa API (API Key Rotation) là những tiêu chuẩn bắt buộc phải tuân thủ.

#### Một số hình ảnh khi tham gia sự kiện
* Thêm các hình ảnh của các bạn tại đây

![Event 1](/images/4-Event/1.jpg)
![Event 1](/images/4-Event/2.jpg)