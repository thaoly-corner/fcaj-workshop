---
title: "Agentic AI Build Week Hackathon "
date: 2026-07-25
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

# Bài Thu Hoạch: Agent AI Build Week Hackathon – Innovative AI Solutions & Team Experiences

### Tổng Quan Sự Kiện (Overview)

**Agent AI Build Week Hackathon** là một sân chơi công nghệ bùng nổ, tập trung vào việc hiện thực hóa các ứng dụng Trí tuệ Nhân tạo (AI) thực chiến. Sự kiện trải dài trên nhiều lĩnh vực thiết thực: từ tối ưu trải nghiệm thương mại dịch vụ, phân tích chiến lược doanh nghiệp, giám sát an ninh đám đông cho đến tự động hóa tuân thủ tài chính. 

Thông qua những chia sẻ từ 4 dự án, sự kiện đã đúc kết những giá trị công nghệ cốt lõi:
* **Đổi mới tư duy:** Nắm bắt tốc độ phát triển phần mềm thần tốc hiện nay, ứng dụng tự động hóa quy mô lớn và thay đổi tư duy giải quyết bài toán ngành.
* **Mô hình hóa thực tế:** Kết hợp hoàn hảo giữa Generative AI, Sub-agents, thị giác máy tính (YOLO) và kiến trúc Serverless trên AWS.
* **Tư duy Human-in-the-loop & Tối ưu chi phí:** Duy trì vai trò giám sát của con người trong các quy trình AI và làm chủ bài toán tối ưu hóa chi phí hạ tầng Cloud.


### 1. Các Giải Pháp AI Nổi Bật Tại Hackathon

#### AI-Powered Conversational Food Ordering Agent - One Team
Đội thi đã giải quyết triệt để rào cản đặt đồ ăn truyền thống — nơi khách hàng thường mất kiên nhẫn do phải tải app, tạo tài khoản, và duyệt menu phức tạp. Đồng thời, giải pháp này khắc phục hoàn toàn lỗi "ảo giác" (hallucination) khiến AI đặt sai món mà các chuỗi lớn như McDonald’s từng gặp phải.
* **Giải pháp đa kênh không rào cản:** Triển khai Chatbot AI trực tiếp trên các nền tảng quen thuộc như Zalo/WhatsApp. Khách hàng có thể đặt món bằng ngôn ngữ tự nhiên.
* **Kiến trúc Agent Core & Trí tuệ Quyết định (Decision Intelligence - DI):** Sử dụng AI Engine trung tâm để ghi nhớ ngữ cảnh hội thoại (session memory) và lịch sử đơn hàng. Cụm DI sẽ tự động lên kế hoạch bước tiếp theo, áp dụng mã giảm giá và **bắt buộc thực hiện bước xác nhận (Confirmation step)** để ngăn chặn việc đặt sai hoặc mua số lượng lớn ngoài ý muốn.
* **Demo:** Dashboard quản lý để truy lỗi ...
* **Vượt qua giới hạn API (Tiny Fish):** Thu thập dữ liệu menu thực tế từ website KFC theo thời gian thực để xử lý tình trạng không có API chính thức.
* **Tối ưu chi phí:** Bằng cách dùng bộ nhớ của Agent Core thay vì gọi API Lambda truyền thống liên tục, đội đã giảm thiểu 60% chi phí. Chi phí vận hành ấn tượng ở mức **~$0.006/đơn hàng** (khoảng $88/tháng cho hạ tầng) với độ trễ chỉ 3–4 giây.

#### AI-Driven Business Strategy Analyzer - Signal Scout
Dự án tập trung vào "nỗi đau" của các doanh nghiệp khi phải phân tích chiến lược của đối thủ thông qua các mảnh dữ liệu công khai rải rác (báo cáo tài chính, cơ cấu tổ chức, tin tức).
* **Triết lý lựa chọn đề tài:** Hướng đến ứng dụng và nghiệp vụ. Cần hiểu rõ thị trường: "Phần mềm bạn tạo ra giải quyết vấn đề gì?", "Người dùng mục tiêu là ai?". Từ đó lựa chọn đề tài và cách tiếp cận phù hợp.
* **Kiến trúc Serverless & Sub-agents:** Sử dụng AWS Lambda và Agent Core để điều phối công việc. Các *Sub-agents* đảm nhận nhiệm vụ chuyên biệt: cào dữ liệu, lọc thông tin nhiễu, lưu trữ cấu trúc vào S3/DynamoDB.
* **Tối ưu chi phí:** Sử dụng các dịch vụ của AWS để thay thế TinyFish và Ampify nhằm tối ưu chi phí.
![Hình ảnh mô hình kiến trúc của dự án AI-Driven Business Strategy Analyzer](images/4-Event/ss.png)

#### Solution Architect Professional Native App - Plan V

* **Bối cảnh & Nỗi đau thực tế (Pain Points)** Khách hàng hoặc cấp trên thường xuyên yêu cầu thiết kế kiến trúc hệ thống và bảng giá chi phí cực gấp trong vòng 2-3 ngày, thậm chí gọi điện đột xuất vào ban đêm yêu cầu có ngay lập tức.  Solution Architect (SA) phải bắt đầu từ trang giấy trắng, vẽ thủ công từng icon trên Draw.io, tự viết mã nguồn hạ tầng (IaC) thủ công và tốn rất nhiều thời gian bóc tách yêu cầu nghiệp vụ phức tạp. Các công cụ phổ biến như ChatGPT hay Gemini khi vẽ sơ đồ thường bị lỗi icon lộn xộn, mũi tên rời rạc và không tuân thủ các quy chuẩn kỹ thuật nội bộ của doanh nghiệp.
* **Xử lý ngôn ngữ tự nhiên & Tài liệu:** Người dùng có thể nhập yêu cầu bằng văn bản tự nhiên (free text) hoặc tải lên các tài liệu, chính sách nội bộ của doanh nghiệp.
* **Kiến trúc Agent & High Engineering:** Tập trung tối ưu hóa kỹ thuật cao cấp về quản lý bộ nhớ, luồng xử lý và ngữ cảnh theo thời gian thực, hiển thị báo cáto từng bước (streaming report) để người dùng theo dõi tiến trình của Agen.
* **Tự động sinh sơ đồ trên Draw.io:** Tự động hóa hoàn toàn việc vẽ sơ đồ kiến trúc trực quan với bố cục chuẩn xác trên Draw.io, cho phép người dùng tự do điều chỉnh thủ công theo ý muốn.
* **Tự động xuất bảng giá & Mã IaC:** Tự động tính toán bảng giá chi phí và xuất mã nguồn Terraform tuân thủ tiêu chuẩn sử dụng Terraform module để tối ưu tính tái sử dụng.
* **Triển khai tự động (Auto-deployment):** Có khả năng tự động chạy mã triển khai để dựng toàn bộ hạ tầng đã vẽ lên AWS nếu doanh nghiệp có nhu cầu khẩn cấp.
##### Luồng hoạt động hệ thống (Workflow)
* Người dùng truy cập qua giao diện web và gửi yêu cầu (free text hoặc tài liệu đính kèm).
* Yêu cầu được chuyển đến các Sub-agents backend để phân tích, đối chiếu với policy doanh nghiệp.
* Hệ thống tiến hành vẽ sơ đồ trên Draw.io, tổng hợp báo cáo real-time, xuất bảng giá và tạo mã IaC (Terraform).
* Lưu trữ cơ sở dữ liệu liên quan và trả kết quả hoàn chỉnh về cho người dùng.
![Hình ảnh mô hình kiến trúc của dự án Solution Architect Professional Native App](images/4-Event/PlanV.png)
#### Real-Time Crowd Flow Monitoring System - 3KA
Hệ thống giải quyết bài toán ùn tắc tại các khu vực đông người (sân bay, siêu thị), giúp giảm thiểu rủi ro an ninh và sự bực bội của khách hàng.
* **Xử lý thời gian thực & AI Agent:** Ứng dụng mô hình **YOLO** nhận diện và theo dõi luồng di chuyển qua video livestream (kết hợp WebSocket và AWS Fargate). AI Agent hoạt động tự chủ để giám sát và gửi thông báo trực tiếp cho nhân viên vận hành.
* **Phân tích luồng (Zone-wise tracking):** Định nghĩa linh hoạt các vùng giám sát. Hệ thống tự động đếm số lượng người, và hiển thị cảnh báo phân cấp màu sắc trực quan trên Dashboard.
* **Bài học kỹ thuật:** Dù gặp một số rào cản về độ trễ mạng (networking limitations) khi demo, dự án đã chứng minh thành công tính khả thi của việc tích hợp Deep AI với dịch vụ Cloud để tự động hóa nhưng vẫn giữ quyền quyết định cho con người.
![Hình ảnh mô hình kiến trúc của dự án Real-Time Crowd Flow Monitoring System](images/4-Event/3KA.png)

#### Adaptive AML Workflow Engine - Six Pillars Team
Dự án giải quyết bài toán chống rửa tiền (AML). Thực tế, 90–95% các cảnh báo giao dịch hiện nay là cảnh báo giả (false-positive), khiến các nhà phân tích mất quá nhiều thời gian và dễ bị burnout.
* **Tự động hóa quy trình phân loại (Triage):** Các AI Agents đảm nhận việc tra cứu KYC, phân tích giao dịch và tự động tổng hợp bộ hồ sơ bằng chứng.
* **Hiệu suất ấn tượng:** Rút ngắn quá trình xử lý đa bước từ **~3 giờ/ca** xuống chỉ còn một báo cáo tóm tắt gọn gàng để chuyên viên duyệt bước cuối.
* **Minh bạch hóa (Auditability):** Điểm ăn tiền của dự án nằm ở hệ thống ghi log chi tiết, cho phép truy xuất toàn bộ nguồn gốc và lập luận của AI, đáp ứng yêu cầu khắt khe về tính minh bạch của các định chế tài chính.
![Hình ảnh mô hình kiến trúc của dự án Adaptive AML Workflow Engine](images/4-Event/aml.png)
---

### 3. Bài Học Kinh Nghiệm & Tư Duy Cốt Lõi Qua Hackathon

Sự kiện kết thúc với những đúc kết quý báu từ các đội thi:
* **Quản trị phạm vi (Scope Management):** Trong một thời gian ngắn, việc định hình bài toán đủ gọn và hoàn thiện một bản Demo chạy mượt mà quan trọng hơn rất nhiều so với việc vẽ ra một kiến trúc khổng lồ nhưng đầy lỗi. Tính khả thi và cách truyền đạt giá trị mới là yếu tố thuyết phục ban giám khảo.
* **Tư duy Human-in-the-Loop:** AI xuất sắc trong việc tự động hóa và tăng tốc (Copilot), nhưng sự phán đoán và chuyên môn của con người vẫn là chốt chặn an toàn cuối cùng, không thể bị thay thế hoàn toàn.
* **Giá trị của sự lặp lại (Iterative Feedback):** Những lời nhận xét đa chiều từ ban giám khảo—bao gồm cả góc nhìn kỹ thuật, nghiệp vụ kinh doanh và trải nghiệm người dùng (UX)—chính là chất xúc tác để mài giũa sản phẩm sắc bén hơn.

---
#### Một số hình ảnh khi tham gia sự kiện
![Hình ảnh nhóm tham dự sự kiện](images/4-Event/team.png)
> **Tổng kết lại**,  những chia sẻ trong sự kiện không chỉ mang đến một không gian thi đấu công nghệ bùng nổ, đầy kịch tính và dạt dào cảm hứng sáng tạo, mà còn tiếp thêm những góc nhìn thực chiến vô cùng sắc bén về nghệ thuật đóng gói một sản phẩm AI hoàn chỉnh. Qua đó, em không chỉ củng cố vững chắc nền tảng về kiến trúc Serverless, Sub-agents và nghệ thuật tối ưu hóa chi phí đám mây, mà còn xác định được rõ ràng hơn lộ trình nâng cao năng lực bản thân. Đây chắc chắn là một bệ phóng tuyệt vời, giúp em sẵn sàng đối mặt và làm chủ những làn sóng công nghệ mới đang thay đổi từng ngày.
---
