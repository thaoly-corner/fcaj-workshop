---
title: "Báo cáo Công việc - Tuần 2"
date: 2026-06-08
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu Tuần 2:
* Tìm hiểu kiến trúc mạng cơ bản trên AWS (VPC, Subnet, Route Table, Internet Gateway, NAT Gateway).
* Nắm vững các khái niệm bảo mật mạng qua Security Group và Network ACLs.
* Thực hành chuỗi Lab 03: Tự tay khởi tạo một mạng nội bộ ảo (VPC) hoàn chỉnh và triển khai máy chủ ảo (EC2).
* **Nâng cao:** Khởi tạo dịch vụ Cơ sở dữ liệu quan hệ (Amazon RDS/Aurora) trong mạng nội bộ và thiết lập kết nối an toàn từ EC2 đến Database (Kiến trúc 2-tier).

### Các công việc thực hiện trong tuần:
| Thứ | Công việc | Ngày bắt đầu | Ngày kết thúc | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Tìm hiểu lý thuyết về AWS VPC và VPC Security. <br> - Thực hành Lab 03: Khái niệm Subnets, Route table và IGW. | 08/06/2026 | 08/06/2026 | [First Cloud Journey Bootcamp - 2025](https://youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) |
| 3 | - Thiết lập NAT Gateway, Security Group, Network ACLs. <br> - Thao tác tạo VPC, Subnet, IGW trên AWS Console. | 09/06/2026 | 09/06/2026 | [First Cloud Journey Bootcamp - 2025](https://youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) |
| 4 | -Cấu hình Route table, Security groups và tiến hành tạo EC2 Instances trong các Subnet đã tạo. <br> - Kiểm tra kết nối EC2 Instance Connect. | 10/06/2026 | 10/06/2026 | [First Cloud Journey Bootcamp - 2025](https://youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) |
| 5 | - Tìm hiểu về Amazon RDS/Aurora PostgreSQL. <br> - **Thực hành:** Tạo DB Subnet Group và khởi tạo một RDS Instance nằm trong Private Subnet. | 11/06/2026 | 11/06/2026 | [Amazon RDS User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) |
| 6 | - **Thực hành:** Tinh chỉnh Inbound Rules của Database Security Group để chỉ cho phép EC2 truy cập vào cổng 5432. <br> - SSH vào EC2 và test kết nối (psql) đến RDS. | 12/06/2026 | 12/06/2026 | [AWS VPC Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) |

### Thành tựu Tuần 2:

* Tự tay xây dựng thành công một môi trường hạ tầng mạng (VPC) tùy chỉnh trên đám mây, bao gồm đầy đủ Public Subnet, Private Subnet, IGW và NAT Gateway.

* Triển khai thành công máy chủ ảo EC2 và hiểu rõ cách điều hướng traffic (routing) ra ngoài Internet một cách an toàn.

* Nắm bắt và thực hành thành công kiến trúc 2-tier cơ bản: Đặt Web/App Server (EC2) ở tầng ngoài và Database (RDS) ở tầng trong bảo mật (Private Subnet).

* Ứng dụng nhuần nhuyễn Security Group để thiết lập quy tắc tường lửa (Firewall), cô lập hoàn toàn cơ sở dữ liệu khỏi Internet và chỉ cho phép máy chủ EC2 nội bộ được quyền truy cập.