---
title: "Cloud Architect Game Show"
date: 2026-06-20
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài Thu Hoạch: "Cloud Architect Game Show"

### Mục Đích Sự Kiện & Trải Nghiệm Quan Sát

Sự kiện **Cloud Architect** được tổ chức dưới hình thức Game Show đối kháng trực tiếp giữa 8 đội thi. Dù không trực tiếp thi đấu, việc theo dõi các lượt trả lời và phân tích đáp án qua từng bộ đề đã mang lại cho em nhiều góc nhìn thực tế về cách ứng dụng kiến thức đám mây AWS từ cơ bản đến nâng cao.

---

### Củng Cố Kiến Thức Qua Các Bộ Câu Hỏi Cấp Độ AWS

Các câu hỏi trong cuộc thi được thiết kế bám sát theo bộ khung chứng chỉ chuẩn của AWS, giúp em củng cố lại kiến thức theo từng mức độ:

#### 1. Cấp Độ Foundational (Nền Tảng Cloud)
* **Khái niệm cốt lõi**: Củng cố lại Mô hình trách nhiệm chung (*Shared Responsibility Model*), phân định rõ nhiệm vụ bảo mật hạ tầng của AWS và bảo mật dữ liệu/cấu hình của người dùng.
* **Dịch vụ cơ bản**: Phân biệt phạm vi sử dụng của các dịch vụ tính toán (EC2, Lambda) và lưu trữ (S3, EBS, EFS).

#### 2. Cấp Độ Associate (Thiết Kế Kiến Trúc)
* **Tính sẵn sàng cao (High Availability)**: Luyện tập tư duy thiết kế hệ thống chịu lỗi khi triển khai ứng dụng trên nhiều vùng khả dụng (Multi-AZ) kết hợp Auto Scaling và Load Balancer.
* **Tách rời hệ thống (Decoupling)**: Hiểu rõ vai trò của SQS và SNS trong việc giảm độ gắn kết giữa các dịch vụ, giúp hệ thống vận hành ổn định khi lượng truy cập tăng đột biến.

#### 3. Cấp Độ Professional & Specialty (Tình Huống Phức Tạp)
* **Quản trị rủi ro & Khôi phục sự cố**: Thông qua các kịch bản, BTC khéo léo lồng ghép các kiến thức về Quản trị rủi ro & Khôi phục sự cố. 
* **Tối ưu chi phí & Bảo mật**: Cách kết hợp các hình thức trả phí (Savings Plans, Spot Instances) và cơ chế phân quyền tối thiểu (Least Privilege) với IAM.

---

### Những Điểm Rút Ra & Nhận Thức Mới

* **Nhìn nhận lại các khái niệm hay nhầm lẫn**: Qua phần giải đáp của BTC, em hiểu rõ hơn rằng việc đưa hệ thống lên Cloud không đồng nghĩa với việc tự động bảo mật 100%, mà phụ thuộc rất lớn vào cách cấu hình phân quyền (IAM) và lưu trữ của kỹ sư.
* **Tư duy Đánh đổi (Trade-off)**: Thiết kế kiến trúc Cloud luôn đòi hỏi sự cân bằng giữa 3 yếu tố: Chi phí (Cost), Độ tin cậy (Reliability) và Hiệu năng (Performance). Không có một mô hình chuẩn cho mọi bài toán.

---

### Ứng Dụng Vào Định Hướng Bản Thân

* **Hệ thống hóa lộ trình học AWS**: Đặt mục tiêu chinh phục các chứng chỉ theo đúng lộ trình bài bản: *Cloud Practitioner* $\rightarrow$ *Solutions Architect Associate* $\rightarrow$ các chứng chỉ chuyên sâu (*DevOps / Data Analytics*).
* **Rèn luyện tư duy bài toán thực tế**: Thay vì chỉ học lý thuyết dịch vụ đơn lẻ, tập trung luyện tập thiết kế hệ thống end-to-end dựa trên các bài toán tình huống (Use Cases).

#### Một số hình ảnh khi tham gia sự kiện
*(Do thời điểm tham gia sự kiện em quên chụp ảnh tại hội trường, dưới đây là ảnh chụp màn hình minh chứng điểm danh / check-in thành công trên Portal để thay thế)*

![Ảnh minh chứng check-in trên Portal](images/4-Event/portal.png)
Nhìn chung, đây không đơn thuần là một buổi báo cáo học thuật mà là một trải nghiệm "vừa học vừa chơi" cực kỳ cuốn hút! Format Game Show được BTC thiết kế vô cùng khéo léo—từ không khí thi đấu sôi nổi đến những pha "bẻ lái" chiến thuật đầy bất ngờ—không chỉ giúp các khái niệm Cloud phức tạp trở nên vô cùng trực quan và dễ thấm, mà còn tạo ra một sân chơi tri thức đầy năng lượng. Sự kiện không chỉ trang bị cho em những góc nhìn kiến trúc thực tế.