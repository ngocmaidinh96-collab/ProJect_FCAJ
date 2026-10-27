
---
title: "Worklog Tuần 7"
date: 2026-09-21
weight: 7
chapter: false
pre: "<b>1.7.</b>"
---

### Mục tiêu tuần 7:

* Tìm hiểu kiến trúc Backup và Disaster Recovery đa Region.
* Xây dựng phương án dự phòng giữa Singapore và Sydney.
* Nghiên cứu sao chép và khôi phục tài nguyên tại Region dự phòng.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|---|---|---|---|---|
| 2 | Tìm hiểu RTO, RPO và các mô hình Disaster Recovery. | 21/09/2026 | 21/09/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | Phân tích kiến trúc Region chính Singapore và Region dự phòng Sydney. | 22/09/2026 | 22/09/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Tìm hiểu sao chép Recovery Point sang Region khác và điều kiện cần thiết. | 23/09/2026 | 24/09/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Nghiên cứu phương án khôi phục EC2, RDS và dữ liệu S3 tại Region dự phòng. | 25/09/2026 | 25/09/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Xây dựng sơ đồ kiến trúc Backup và DR, xác định các bước phục hồi. | 26/09/2026 | 26/09/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 7:

* Hiểu kiến trúc Backup và Disaster Recovery triển khai trên nhiều AWS Region.

* Tìm hiểu phương án phân chia vai trò giữa hai Region:
  * Singapore làm Region chính.
  * Sydney làm Region dự phòng.

* Tìm hiểu các cơ chế sao chép dữ liệu:
  * AWS Backup Cross-Region Copy.
  * Amazon S3 Cross-Region Replication.
  * Sao chép Recovery Point sang Region khác.

* Nắm được các yêu cầu khi triển khai sao lưu liên vùng:
  * Backup Vault tại Region đích.
  * IAM Role và quyền truy cập.
  * Chính sách lưu giữ bản sao lưu.
  * Khả năng khôi phục tài nguyên tại Region đích.

* Tìm hiểu các dịch vụ hỗ trợ tính sẵn sàng và giám sát:
  * Amazon Route 53 để quản lý DNS và định tuyến lưu lượng.
  * Amazon CloudWatch để giám sát tài nguyên.
  * Amazon SNS để gửi thông báo cảnh báo.
  * AWS Elastic Disaster Recovery để hỗ trợ phục hồi máy chủ.

* Xây dựng sơ đồ kiến trúc Backup và DR cho website bán hàng.

* Xác định các bước cần thiết để khôi phục máy chủ ứng dụng, cơ sở dữ liệu và dữ liệu hình ảnh.

* Hiểu mối quan hệ giữa chi phí triển khai, Recovery Time Objective (RTO) và Recovery Point Objective (RPO).
