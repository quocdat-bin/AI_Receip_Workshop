---
title : "Tích hợp Động cơ AI & Logic xử lý (Semantic Search & Gemini Chatbot)"
date : 2026-07-07
weight : 3
chapter : false
pre : " <b> 5.3.3 </b> "
---

### Bước 1: Động cơ Embedding Đa ngôn ngữ Cục bộ (embeddings.py)
1.	Sử dụng mô hình sentence-transformers (MiniLM, 384 chiều) chạy trực tiếp trên EC2.
2.	Mô hình hỗ trợ tiếng Việt trực tiếp, giúp người dùng nhập nguyên liệu bằng tiếng Việt (ví dụ: "thịt lợn, trứng, cà chua") mà không cần qua dịch thuật.
### Bước 2: Tích hợp Chatbot Dinh dưỡng (chat.py)
-	Tích hợp Google Gemini API kết hợp cơ chế Function Calling (Tool Calling) tự động:
    -	lookup_nutrition: Tra cứu thông tin calo, protein, carb, fat từ MySQL.
    -	search_recipes: Gợi ý công thức món ăn phù hợp theo ngữ cảnh câu hỏi.
![overview](/images/5-Workshop/5.4-Ai/14.jpg)

### Bước 3: Định tuyến API Gateway (ai-recipe-finder-api)
-	Tạo các Routes trên AWS API Gateway trỏ về EC2 AI Server:
    -	POST /search → Trỏ về [http://54.254.77.58:8001/search](http://54.254.77.58:8001/search)
    -	POST /chat → Trỏ về [http://54.254.77.58:8002/chat](http://54.254.77.58:8002/chat)
    -	GET /health → Kiểm tra trạng thái máy chủ.

![overview](/images/5-Workshop/5.4-Ai/15.jpg)

![overview](/images/5-Workshop/5.4-Ai/16.jpg)

- Cấu hình Routes trên AWS API Gateway.

### Bước 4: Triển khai dịch vụ nền bằng systemd (tự khởi động & tự phục hồi)
-	Đăng ký hai dịch vụ FastAPI thành systemd service trên EC2 để tự chạy cùng máy và tự khởi động lại khi gặp lỗi:
    -	ai-search.service — chạy Uvicorn ở cổng 8001 (/search).
    -	ai-chat.service — chạy Uvicorn ở cổng 8002 (/chat).
sudo systemctl enable --now ai-search ai-chat
*Lưu ý:* pgvector chạy trong Docker container; hai dịch vụ FastAPI chạy trực tiếp bằng systemd nên nhẹ và tự phục hồi sau khi máy khởi động lại.


![overview](/images/5-Workshop/5.4-Ai/17.jpg)