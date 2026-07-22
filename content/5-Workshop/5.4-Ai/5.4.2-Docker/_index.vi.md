---
title : "Triển khai Dịch vụ bằng Docker & CSDL Vector (pgvector)"
date : 2026-07-07
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

### Bước 1: Khởi chạy PostgreSQL Container tích hợp pgvector
-	Chạy Docker container lưu trữ vector 384 chiều trên EC2
### Bước 2: Chạy Script Batch Embedding Dữ liệu lớn (sync_bulk.py)
-	Sử dụng tmux để duy trì tiến trình chạy ngầm khi ngắt SSH

![overview](/images/5-Workshop/5.4-Ai/13.jpg)
- Tiến trình tự động đọc MySQL theo từng lô 1.000 món, mã hóa vector với tốc độ ~32 món/giây và đẩy vào pgvector.

### Bước 3: Kiểm tra Số lượng Bản ghi Vector trong Database
-	Kiểm tra số lượng bản ghi đã được nạp vào pgvector:

![overview](/images/5-Workshop/5.4-Ai/13_2.jpg)
