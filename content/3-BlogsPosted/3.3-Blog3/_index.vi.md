---
title: "Blog 3"
date: 2026-07-017
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# ƯU TIÊN CẢNH BÁO AWS HEALTH VỚI AWS USER NOTIFICATIONS

Bài blog này giới thiệu một giải pháp tinh gọn nhằm khắc phục tình trạng "quá tải thông báo" (notification fatigue) cho các kỹ sư hệ thống. Bằng cách sử dụng AWS User Notifications kết hợp với các template AWS CloudFormation có sẵn, bạn có thể tự động hóa việc chắt lọc và phân loại các sự kiện từ AWS Health. Thay vì nhận mọi cảnh báo một cách thụ động, giờ đây hộp thư của bạn sẽ chỉ ưu tiên hiển thị những sự cố chí mạng cần xử lý ngay, trong khi các thông tin bảo trì ít quan trọng hơn sẽ được gom nhóm gọn gàng.

**Những tính năng và ưu điểm nổi bật:**

-	**Bộ lọc nhiễu chủ động (Proactive Filtering):** Hệ thống sử dụng các quy tắc (Event rules) để loại bỏ những cảnh báo từ các dịch vụ bạn không sử dụng. Chỉ những sự kiện thuộc về kiến trúc cốt lõi của bạn (như Amazon RDS, Direct Connect, v.v.) mới được cho phép đi qua.
-	**Phân luồng theo độ khẩn cấp:** Giải pháp tự động chia thông báo thành hai luồng:
    -	CRITICAL (Nguy cấp): Bắt các sự kiện lỗi hoặc thay đổi lịch trình. Thông báo được gửi đi ngay lập tức.
    -	INFORMATIONAL (Thông tin): Hứng các sự kiện thông thường.
-	**Tính năng gom nhóm thông minh (Built-in Batching):** Đây là điểm sáng giá nhất của AWS User Notifications. Thay vì spam từng email một, hệ thống sẽ gom tất cả các cảnh báo INFORMATIONAL trong một khoảng thời gian (ví dụ: 5 phút) thành một email tóm tắt duy nhất, giúp hộp thư luôn sạch sẽ.
-	**Loại bỏ gánh nặng bảo trì Code:** Không cần phải tự viết hay duy trì các hàm AWS Lambda phức tạp để phân tích nội dung JSON như các cách làm cũ. Mọi thứ được xử lý trực tiếp ở cấp độ dịch vụ (Managed Service).
-	**Quản lý tập trung & Đa tài khoản:** Toàn bộ cấu hình được theo dõi trên một Notification Center duy nhất. Giải pháp cũng hỗ trợ mở rộng quy mô dễ dàng cho hàng chục tài khoản con thông qua tích hợp với AWS Organizations.

**Hình ảnh :**

![Blog3](/images/3-Blog/Blog3.png)

**Hướng dẫn triển khai tự động hóa qua AWS CloudFormation**

Nguyên lý của giải pháp này là "Lọc trước, ưu tiên sau". Việc cài đặt diễn ra rất nhanh chóng thông qua các template CloudFormation được cấu hình sẵn.

*Điều kiện tiên quyết:*
-	Có quyền triển khai CloudFormation stack và cấu hình AWS User Notifications trên tài khoản.
-	Nếu triển khai cho toàn tổ chức, cần quyền truy cập vào tài khoản Quản lý (Payer/Management account) và chuẩn bị sẵn ID của Organization root hoặc nhánh OU (Organizational Unit).

**Bước 1: Lựa chọn chế độ triển khai (Deployment Modes)** 

Tải **[CloudFormation template](https://github.com/aws-samples/sample-prioritize-aws-health-alerts-using-user-notifications)** từ bài viết gốc và tải lên AWS Console. Bạn cần chọn 1 trong 4 tham số DeploymentMode phù hợp với quy mô:

-	Linked mode: Bản tiêu chuẩn, dành cho 1 tài khoản đơn lẻ, sử dụng định dạng email mặc định của AWS.
-	Payer mode: Dành cho cấp độ Doanh nghiệp. Cài một lần ở tài khoản cha, áp dụng luật lọc cảnh báo cho toàn bộ các tài khoản con trong AWS Organizations.
-	Combined mode: Dành cho 1 tài khoản nhưng kết hợp thêm EventBridge và Amazon SNS để tự động chèn thẻ [CRITICAL] hoặc [INFORMATIONAL] vào tiêu đề email, giúp nhận diện sự cố nhanh hơn.
-	PayerCombined mode: Kết hợp sức mạnh mở rộng của Payer và khả năng tùy biến tiêu đề email của Combined cho toàn tổ chức.

**Bước 2: Xác nhận đăng ký email (Confirm Subscription)**

-	Sau khi CloudFormation chạy xong (trạng thái CREATE_COMPLETE), hãy kiểm tra hộp thư email bạn đã khai báo.
-	Bắt buộc phải click vào link Confirm subscription trong email. Nếu bỏ qua bước này, AWS sẽ không thể đẩy cảnh báo về cho bạn.

**Bước 3: Kiểm tra cấu hình tại Notification Center**

-	Truy cập vào dịch vụ AWS User Notifications trên console.
-	Kiểm tra 2 bộ cấu hình vừa được tạo ra: Health-Critical-Notifications (xử lý lỗi tức thì) và Health-Informational-Notifications (gom nhóm thông báo phụ).
-	Review lại danh sách các dịch vụ AWS đang được giám sát để đảm bảo không bị sót dịch vụ quan trọng nào (như DynamoDB, ECS,...).

**Bước 4: Kiểm thử hệ thống (Test the Solution)**

-	Truy cập Amazon EventBridge > Chọn Send events.
-	Tạo một sự kiện giả lập (Mock event) với nguồn là aws.health.
-	Kiểm tra hộp thư: Luồng Critical sẽ nảy thông báo ngay lập tức (kèm thẻ đỏ nếu dùng Option C/D), trong khi luồng Informational sẽ đợi đủ 5 phút mới tổng hợp gửi về.

*Lưu ý quan trọng:*

Mặc dù giải pháp quản lý thông báo này vô cùng đắc lực, nhưng giải pháp này được thiết kế theo hướng chủ đích là gọn nhẹ (lightweight). Đi kèm với sự đơn giản đó là một vài điểm đánh đổi (trade-offs) mà mọi người cần nắm rõ trước khi triển khai:
-	Định dạng Email: Mặc định bạn không thể tự chèn thêm chữ (như [CRITICAL]) vào tiêu đề. Tín hiệu ưu tiên dựa vào cách gửi (gửi lẻ hay gom nhóm). Nếu bắt buộc cần thẻ ưu tiên này, hãy dùng chế độ Combined (kết hợp thêm EventBridge + SNS).
-	Giám sát luồng cảnh báo: Luồng EventBridge + SNS (trong tùy chọn Combined) tích hợp sẵn hàng đợi lỗi (DLQ) và CloudWatch alarm, giúp bạn biết ngay nếu bản thân hệ thống gửi thông báo bị "tịt".
-	Không tự động khử trùng lặp: Mỗi vòng đời sự kiện (tạo mới, cập nhật, hoàn tất) sẽ sinh ra một thông báo. Một sự cố có thể tạo ra 2–4 email. Cần dùng thêm AHA hoặc hàm Lambda nếu muốn khử trùng lặp tuyệt đối.
-	Không hỗ trợ leo thang (Escalation): Hệ thống chỉ "đưa thư" chứ không bám sát xem ai đã xử lý. Với các quy trình trực chiến (on-call), nên tích hợp thẳng luồng SNS vào PagerDuty hoặc OpsGenie.
-	Không lưu trữ lịch sử: Thông báo đẩy real-time và trôi đi luôn. Để họp mổ xẻ sau sự cố (post-incident) hoặc làm báo cáo, bạn sẽ cần ghép nối thêm với giải pháp HEIDI hoặc CID Health Events Dashboard.


**[Link bài viết:](https://www.facebook.com/share/p/1EVZSYjUsa/)** https://www.facebook.com/share/p/1EVZSYjUsa/