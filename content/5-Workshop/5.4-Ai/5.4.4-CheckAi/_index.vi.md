---
title : "Kiểm tra và Xác thực Endpoint AI"
date : 2026-07-07
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---

### Bước 1: Kiểm tra cục bộ trên EC2 bằng curl
1.	Kiểm tra dịch vụ Search:
- curl localhost:8001/health   →   {"status":"ok","recipes_indexed":191000}
2.	Kiểm tra dịch vụ Chat:
- curl localhost:8002/health   →   {"status":"ok","mode":"mysql"}

![overview](/images/5-Workshop/5.4-Ai/18.jpg)

### Bước 2: Kiểm tra qua API Gateway (HTTPS công khai)
-	Dùng Postman hoặc PowerShell gọi endpoint HTTPS đi qua API Gateway:
GET https://lis0c4qiz0.execute-api.ap-southeast-1.amazonaws.com/search/health
→ {"status":"ok","recipes_indexed":191000}
![overview](/images/5-Workshop/5.4-Ai/19.jpg)