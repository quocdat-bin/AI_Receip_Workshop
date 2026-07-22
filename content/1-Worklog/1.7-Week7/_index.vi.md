---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---



### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 6 | - Rà soát, chẩn đoán và khắc phục các lỗi phát sinh trong quá trình kết nối và triển khai giữa EC2 và RDS | 29/05/2026 | 29/05/2026 | <https://000004.awsstudygroup.com/> |
| 7| - Tổng kết cấu hình hạ tầng đã triển khai, tối ưu hóa các quy tắc Security Group và lưu trữ tài liệu kỹ thuật | 30/05/2026 | 30/05/2026 ||
| 2 | - Tiến hành khởi tạo và cấu hình máy chủ Amazon EC2 Instance trên AWS để phục vụ làm môi trường triển khai cho dự án | 01/06/2026 | 01/06/2026 | <https://000004.awsstudygroup.com/> |
| 3 | - Khởi tạo cơ sở dữ liệu trên Amazon RDS; chuẩn bị, chuẩn hóa và nhập (import) các tập dữ liệu ban đầu cho dự án | 02/06/2026 | 02/06/2026 ||
| 4 | - Kiểm tra dữ liệu đầu vào, cấu hình mạng và kết nối giữa máy chủ EC2 với cơ sở dữ liệu Amazon RDS | 03/06/2026 | 03/06/2026 | <https://000004.awsstudygroup.com/> |


### Kết quả đạt được tuần 7:

* **Triển khai máy chủ EC2:** Khởi tạo thành công máy chủ Amazon EC2 với hệ điều hành và các thông số cấu hình phù hợp, sẵn sàng làm môi trường hosting/server cho hệ thống AI Recipe Finder.
* **Thiết lập Cơ sở dữ liệu Amazon RDS:** Khởi tạo instance Amazon RDS thành công; thực hiện cấu hình các schema cần thiết và import toàn bộ dữ liệu món ăn/nguyên liệu vào cơ sở dữ liệu thành công.
* **Kết nối & Xử lý lỗi hệ thống:** Thiết lập thành công kết nối an toàn giữa EC2 Instance và Amazon RDS thông qua việc cấu hình Security Groups và VPC. Đã phát hiện và khắc phục triệt để các lỗi liên quan đến quyền truy cập, timeout kết nối trong quá trình tích hợp.