
---
title: "Worklog Tuần 9"
date: 2026-10-05
weight: 9
chapter: false
pre: "<b>1.9.</b>"
---

### Mục tiêu tuần 9:

* Mô phỏng các tình huống sự cố có thể xảy ra.
* Đánh giá hiệu quả của phương án Backup và DR.
* Đo thời gian khôi phục và mức độ mất dữ liệu.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|---|---|---|---|---|
| 2 | Xây dựng kịch bản mất dữ liệu, xóa nhầm dữ liệu và mất máy chủ ứng dụng. | 05/10/2026 | 05/10/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | Thực hành khôi phục dữ liệu hoặc phiên bản đối tượng S3 phù hợp. | 06/10/2026 | 06/10/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Mô phỏng sự cố EC2 và thực hiện khôi phục máy chủ ứng dụng. | 07/10/2026 | 08/10/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Kiểm tra dữ liệu RDS và khả năng hoạt động của website sau khôi phục. | 09/10/2026 | 09/10/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Ghi nhận thời điểm bắt đầu, kết thúc khôi phục và tính RTO, RPO. | 10/10/2026 | 10/10/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 9:

* Xây dựng các kịch bản mô phỏng sự cố cho hệ thống:
  * Mất hoặc xóa nhầm dữ liệu.
  * Mất máy chủ ứng dụng EC2.
  * Sự cố cơ sở dữ liệu RDS.
  * Mất hoặc sai phiên bản hình ảnh trên S3.

* Sử dụng AWS Backup để thực hiện hoặc kiểm tra quy trình khôi phục EC2 và RDS.

* Sử dụng S3 Versioning để kiểm tra khả năng khôi phục phiên bản đối tượng.

* Kiểm tra trạng thái tài nguyên trước và sau sự cố:
  * Amazon EC2.
  * Amazon RDS.
  * Amazon S3.
  * AWS Backup Recovery Point.

* Tìm hiểu việc sử dụng Amazon CloudWatch để giám sát trạng thái tài nguyên và hỗ trợ phân tích sự cố.

* Ghi nhận thời gian khôi phục để tính RTO:
  * Thời điểm bắt đầu sự cố hoặc quá trình khôi phục theo quy ước kiểm thử.
  * Thời điểm hệ thống khôi phục và hoạt động trở lại.
  * Tổng thời gian khôi phục.

* Đánh giá RPO dựa trên thời điểm dữ liệu gần nhất có thể khôi phục.

* So sánh kết quả kiểm thử với mục tiêu RTO/RPO dự kiến.

* Xác định hạn chế và đề xuất cải thiện phương án Backup và Disaster Recovery.
