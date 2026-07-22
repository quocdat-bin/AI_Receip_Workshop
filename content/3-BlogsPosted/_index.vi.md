---
title: "Các bài blogs đã đăng"
date: 2026-07-07
weight: 3
chapter: false
pre: " <b> 3. </b> "
---


###  [Blog 1 - PHÂN TÍCH LOG AMAZON RDS BẰNG NGÔN NGỮ TỰ NHIÊN VỚI KIRO VÀ MCP](3.1-Blog1/)
Blog này chia sẻ góc nhìn và bài học thực tế về cách ứng dụng AI để giải quyết "nỗi ám ảnh" đọc log thủ công trên Amazon RDS. Bằng cách kết hợp Amazon CloudWatch với giao thức MCP và trợ lý AI Kiro, các kỹ sư hệ thống có thể dễ dàng truy vấn, phân tích nguyên nhân gốc rễ từ hàng ngàn dòng log phức tạp chỉ bằng các câu lệnh ngôn ngữ tự nhiên. Đây là một giải pháp hiệu quả giúp giảm tải cho đội ngũ vận hành, tăng tốc độ xử lý sự cố và mở ra hướng đi mới trong việc quản trị an toàn cơ sở dữ liệu trên môi trường Cloud.

###  [Blog 2 - GIẢI MÃ LOG AWS SITE-TO-SITE VPN ĐỂ GIÁM SÁT MẠNG ](3.2-Blog2/)
Blog này phân tích cách AWS kết hợp AI sinh tạo và ChatOps để nâng cao khả năng giám sát AWS Site-to-Site VPN. Bằng việc sử dụng Amazon Bedrock và AWS DevOps Agent, hệ thống có thể tự động đọc hiểu log BGP/IKE, xác định nguyên nhân sự cố và cung cấp hướng xử lý thông qua email hoặc các nền tảng cộng tác như Slack. Giải pháp này giúp đơn giản hóa công tác vận hành mạng và rút ngắn đáng kể thời gian xử lý sự cố.

###  [Blog 3 - ƯU TIÊN CẢNH BÁO AWS HEALTH VỚI AWS USER NOTIFICATIONS](3.3-Blog3/)
Blog này đi sâu vào phân tích cách giải quyết dứt điểm tình trạng "quá tải thông báo" (notification fatigue) trong quá trình theo dõi các sự kiện vận hành và bảo trì từ AWS Health. Bằng cách kết hợp dịch vụ AWS User Notifications với khả năng tự động hóa của AWS CloudFormation, hệ thống không chỉ tự động lọc bỏ các cảnh báo nhiễu từ những dịch vụ không sử dụng, mà còn phân loại luồng thông tin theo mức độ khẩn cấp (gửi tức thì đối với sự cố nghiêm trọng và gom nhóm tóm tắt đối với thông tin thường). Nhờ phương pháp tiếp cận này, ta có thể duy trì sự tập trung cao độ vào các rủi ro cốt lõi, giữ cho hộp thư luôn gọn gàng và dễ dàng chuẩn hóa quy trình quản lý cho cả một tài khoản đơn lẻ lẫn toàn bộ hệ thống AWS Organizations.