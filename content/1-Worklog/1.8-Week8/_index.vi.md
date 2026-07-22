---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---




### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 6 | - Thiết lập cấu hình Amazon Bedrock để phù hợp với giao diện và tính năng xử lý của website AI Recipe Finder | 05/06/2026 | 05/06/2026 | Amazon Bedrock User Guide |
| 7 | - Tiến hành thử nghiệm kết nối giữa API Gateway, Amazon Bedrock và giao diện web; tối ưu hóa luồng gọi API | 06/06/2026 | 11/06/2026 ||
| 2 | - Tìm hiểu, nghiên cứu cơ chế hoạt động và cách cấu hình Elastic IP addresses cho máy chủ EC2 | 08/06/2026 | 08/06/2026 ||
| 3 | - Tìm hiểu về Amazon API Gateway, các khái niệm REST API, routes, tích hợp backend và quản lý lưu lượng truy cập | 09/06/2026 | 09/06/2026 | Amazon API Gateway Developer Guide |
| 4 | - Cấu hình và gán Elastic IP cố định cho máy chủ EC2 để đảm bảo địa chỉ IP không bị thay đổi khi khởi động lại | 10/06/2026 | 10/06/2026 | Tài liệu cấu hình AWS EC2 |


### Kết quả đạt được tuần 8:

* **Nghiên cứu & Cấu hình Elastic IP:** Đã nắm rõ cơ chế và gán thành công Elastic IP cố định cho máy chủ EC2, giúp địa chỉ IP của server không bị thay đổi mỗi khi khởi động lại instance.
* **Nghiên cứu API Gateway:** Hiểu rõ cách tạo, quản lý và bảo mật các API endpoints với Amazon API Gateway để chuẩn bị kết nối giữa giao diện người dùng và hệ thống xử lý phía sau.
* **Tích hợp AWS Bedrock cho Web:** Thiết lập thành công cấu hình tích hợp mô hình AI từ Amazon Bedrock vào ứng dụng web, bước đầu cho phép hệ thống tạo phản hồi và gợi ý món ăn dựa trên dữ liệu đầu vào của người dùng.