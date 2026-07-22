---
title : "Dọn dẹp tài nguyên"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.5. </b> "
---

#### Dọn dẹp tài nguyên

#### Dọn dẹp
**1. Xóa Amazon Cognito**
- Vào Amazon Cognito → User pools.
- Chọn User Pool của AI Recipe Finder.
- Chọn Delete user pool.
- Nhập tên User Pool để xác nhận.

![hosted zone](/images/5-Workshop/5.6-Cleanup/1.jpg)
![hosted zone](/images/5-Workshop/5.6-Cleanup/2.jpg)

**2. Terminate Amazon EC2**
- Vào EC2 → Instances.
- Chọn instance chạy Backend.
- Chọn Instance state → Terminate (delete) instance.
- Xác nhận terminate.
- Chờ trạng thái chuyển sang Terminated.
- Không chỉ chọn Stop, vì ổ đĩa EBS vẫn có thể tiếp tục phát sinh phí.
- Sau đó vào EC2 → Elastic Block Store → Volumes:
- Kiểm tra volume của instance đã bị xóa tự động chưa.


![hosted zone](/images/5-Workshop/5.6-Cleanup/3.jpg)
![hosted zone](/images/5-Workshop/5.6-Cleanup/4.jpg)

**3. Release Elastic IP**
- Vào EC2 → Network & Security → Elastic IP addresses.
- Chọn Elastic IP đã gắn với Backend.
- Nếu còn liên kết, chọn Disassociate Elastic IP address.
- Sau đó chọn Release Elastic IP addresses.
- Xác nhận release.

![hosted zone](/images/5-Workshop/5.6-Cleanup/5.jpg)

**4. Xóa Amazon RDS**
- Vào Amazon RDS → Databases.
- Chọn DB instance của dự án, chẳng hạn food-db-4.
- Chọn Actions → Delete.
- Chọn không tạo final snapshot.
- Bỏ chọn giữ automated backups nếu không cần phục hồi dữ liệu.
- Nhập nội dung xác nhận xóa theo yêu cầu của AWS.
- Chọn Delete.

![hosted zone](/images/5-Workshop/5.6-Cleanup/6.jpg)
![hosted zone](/images/5-Workshop/5.6-Cleanup/7.jpg)