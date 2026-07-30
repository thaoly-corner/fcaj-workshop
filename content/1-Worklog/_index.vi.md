---
title: "Nhật ký công việc"
date: 2026-06-01
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

**Trang này** tổng hợp toàn bộ hành trình nhật ký công việc (worklog) trong suốt quá trình thực tập kéo dài **8 tuần** (từ tháng 06/2026 đến tháng 08/2026). Nội dung tập trung vào việc nghiên cứu kiến trúc đám mây AWS, xây dựng và phát triển hệ thống RAG (Retrieval-Augmented Generation) từ môi trường local lên hạ tầng Serverless trên đám mây, cũng như quá trình kiểm thử, tối ưu hóa và hoàn thiện báo cáo thực tập.

Dưới đây là tóm tắt nội dung công việc chi tiết qua các tuần thực tập:

**Tuần 1:** [Làm quen với môi trường làm việc, định hướng đề tài RAG và tìm hiểu các kiến thức nền tảng về hệ thống đám mây](1.1-week1/)

**Tuần 2:** [Tìm hiểu kiến trúc mạng cơ bản trên AWS (VPC, Subnet, Route Table, IGW, NAT Gateway) và thiết lập mô hình 2-tier an toàn với Amazon RDS/Aurora](1.2-week2/)

**Tuần 3:** [Xây dựng nguyên mẫu RAG chạy local, tích hợp mô hình nhúng BAAI/bge-m3, kết nối cơ sở dữ liệu vector Qdrant/pgvector và LLM (Gemini)](1.3-week3/)

**Tuần 4:** [Bắt đầu dịch chuyển kiến trúc sang AWS Serverless, xử lý các thủ tục xác thực Amazon Bedrock và vượt qua các rào cản kiểm soát tài khoản từ AWS Support](1.4-week4/)

**Tuần 5:** [Triển khai hệ thống lên AWS: Cấu hình Amazon Aurora Serverless với `pgvector`, migrate luồng Vectorize và xây dựng RAG API trên AWS Lambda kết hợp API Gateway](1.5-week5/)

**Tuần 6:** [Rà soát mã nguồn Backend/Lambda, ghép nối tổng thể pipeline hoàn chỉnh (End-to-End), xử lý triệt để các lỗi phát sinh và viết tài liệu kỹ thuật nội bộ](1.6-week6/)

**Tuần 7:** [Tối ưu hóa hiệu năng truy vấn vector, tinh chỉnh câu lệnh Prompt, đồng thời bắt đầu cấu trúc nội dung và biên soạn các chương cốt lõi cho Báo cáo thực tập](1.7-week7/)

**Tuần 8:** [Tiếp tục kiểm thử toàn diện, sửa lỗi phát sinh (debugging) trên hệ thống RAG bản cloud, rà soát định dạng và hoàn thiện toàn văn Báo cáo thực tập chính thức](1.8-week8/)