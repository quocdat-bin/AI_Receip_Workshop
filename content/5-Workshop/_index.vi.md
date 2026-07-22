---
title: "Workshop"
date: 2026-07-07
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# DỰ ÁN AI RECIPE FINDER TRÊN NỀN TẢNG AWS

#### Tổng quan

**AI Recipe Finder** là một nền tảng web thông minh được xây dựng hoàn toàn trên hạ tầng đám mây AWS. Dự án giải quyết trực tiếp bài toán lãng phí thực phẩm và khó khăn trong việc lên thực đơn hàng ngày bằng cách phân tích các nguyên liệu sẵn có của người dùng, từ đó sử dụng AI để đưa ra các gợi ý món ăn chuẩn xác và hỗ trợ toàn diện quá trình nấu nướng.

Hiện nay, người dùng thường tốn nhiều thời gian nghĩ thực đơn, dễ lãng phí thực phẩm thừa trong bếp, gặp khó khăn trong việc tính toán dinh dưỡng thủ công và thiếu các công cụ tương tác thông minh khi nấu nướng.

Để giải quyết vấn đề này, nền tảng ứng dụng Generative AI kết hợp Cloud Computing để tạo ra một hệ sinh thái khép kín: từ gợi ý công thức qua Semantic Search, trò chuyện với trợ lý ảo (Chatbot) để hướng dẫn nấu ăn, đến việc tự động phân tích chi tiết dinh dưỡng (Calories, Protein, Fat, Carbs) dựa trên dữ liệu chuẩn của USDA. Hệ thống cung cấp các tính năng nổi bật được hỗ trợ bởi các dịch vụ AWS cụ thể:

-	Tìm kiếm & Gợi ý thông minh - Ứng dụng mô hình AI từ Amazon Bedrock để phân tích nguyên liệu đầu vào và đưa ra công thức phù hợp bằng Semantic Search, tích hợp "vòng quay chọn món" để giải quyết nhanh câu hỏi "hôm nay ăn gì".
-	Trợ lý AI nấu ăn - Vận hành Chatbot thông qua Amazon Bedrock để hỗ trợ giải đáp thắc mắc theo thời gian thực và tự động đề xuất các nguyên liệu thay thế khi bạn thiếu đồ.
-	Quản lý Dinh dưỡng & Mua sắm - Sử dụng Backend API (chạy trên EC2/App Runner) để tự động tính toán lại định lượng khi đổi số lượng người ăn, hiển thị chi tiết dưỡng chất từ USDA và tự động tạo danh sách đi chợ.
-	Không gian Cá nhân hóa - Được bảo mật và lưu trữ bởi Amazon Cognito, AWS Amplify cùng Amazon S3/RDS, hỗ trợ lưu trữ bộ sưu tập công thức yêu thích, viết "Nhật ký nấu ăn" (Cooking Diary) để lưu ảnh thành phẩm và chia sẻ cho bạn bè.


#### Nội dung

1. [Giới thiệu](5.1-Workshop-overview/)
2. [Triển khai Hạ tầng cơ sở & Backend ](5.3-Backend/)
3. [Triển khai AI Chatbox & Trí tuệ nhân tạo (AI Services)](5.4-Ai/)
4. [Triển khai Frontend & Xác thực (Frontend & Auth)](5.5-Fronend//)
5. [Dọn dẹp tài nguyên](5.6-Cleanup/)
6. [Tài liệu dự án](5.7-ProjectDocument/)