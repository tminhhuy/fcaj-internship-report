---
title: "Blog 2"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AWS VPC là gì?

VPC (Virtual Private Cloud) là một mạng ảo được cách ly về mặt logic trong AWS, nơi bạn khởi chạy và quản lý các tài nguyên. Dưới đây là những thành phần bạn cần nắm.

### Mạng con (Subnet)

* Subnet chia dải địa chỉ IP của VPC thành các mạng nhỏ hơn.
* **Subnet public:** có route đến Internet Gateway.
* **Subnet private:** không có route trực tiếp đến Internet Gateway.
* Mỗi subnet thuộc về đúng một Availability Zone.

### 1. Internet Gateway (IGW)

Cho phép tài nguyên trong VPC giao tiếp với internet, khi route table, địa chỉ IP public và các quy tắc bảo mật cho phép.

### 2. NAT Gateway

Cho phép tài nguyên trong subnet private khởi tạo kết nối đi ra internet, nhưng không cho phép internet chủ động kết nối vào.

### 3. Bảng định tuyến (Route Table)

Xác định lưu lượng mạng được chuyển đi đâu dựa trên địa chỉ IP đích.

### 4. VPC Peering

Kết nối hai VPC khác nhau, cho phép tài nguyên trong đó giao tiếp bằng địa chỉ IP private.

**Ví dụ:**

* VPC A chứa ứng dụng của bạn.
* VPC B chứa một dịch vụ dùng chung.
* VPC Peering cho phép hai VPC giao tiếp riêng tư sau khi cấu hình route và các quy tắc truy cập cần thiết.

### Ghi nhớ nhanh

* Một VPC thuộc về một Region và có thể trải rộng trên nhiều Availability Zone.
* Một VPC có thể chứa nhiều subnet, được xem là public hay private tùy cấu hình định tuyến.
* Mỗi subnet thuộc về đúng một Availability Zone.
* Một network interface của EC2 thuộc về một subnet; một instance có thể có nhiều network interface.
* Các subnet trong cùng VPC giao tiếp với nhau qua route local, tuân theo Security Group và Network ACL.
* VPC Peering kết nối hai VPC, cho phép giao tiếp riêng tư giữa tài nguyên của chúng.

Nguồn : https://www.facebook.com/groups/awsstudygroupfcj/permalink/2318553045576321/?rdid=dZQc4ChgXZA2BKPx#