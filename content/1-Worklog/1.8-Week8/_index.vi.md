
---
title: "Worklog Tuần 8"
date: 2026-09-28
weight: 8
chapter: false
pre: "<b>1.8.</b>"
---

### Mục tiêu tuần 8:

* Thực hành khôi phục tài nguyên từ bản sao lưu.
* Kiểm tra tính toàn vẹn của dữ liệu sau khôi phục.
* Đánh giá khả năng phục hồi của website.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|---|---|---|---|---|
| 2 | Kiểm tra các Recovery Point và lựa chọn bản sao lưu thử nghiệm. | 28/09/2026 | 28/09/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | Thực hành khôi phục EC2 từ bản sao lưu hoặc AMI phù hợp. | 29/09/2026 | 29/09/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Thực hành khôi phục RDS từ snapshot hoặc Recovery Point. | 30/09/2026 | 01/10/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Kiểm tra Versioning và khôi phục phiên bản đối tượng S3 khi có bản phù hợp. | 02/10/2026 | 02/10/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Kiểm tra kết nối website, database và hình ảnh sau khôi phục. | 03/10/2026 | 03/10/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 8:

* Hiểu quy trình khôi phục tài nguyên AWS từ bản sao lưu.

* Thực hành hoặc tìm hiểu quy trình khôi phục máy chủ ứng dụng:
  * Amazon EC2.
  * Amazon EBS.
  * AWS Backup Recovery Point.
  * Kiểm tra cấu hình mạng sau khôi phục.

* Thực hành khôi phục cơ sở dữ liệu:
  * Amazon RDS Snapshot.
  * AWS Backup Recovery Point.
  * Kiểm tra trạng thái database sau khôi phục.
  * Kiểm tra kết nối từ ứng dụng PHP đến RDS.

* Tìm hiểu cách khôi phục dữ liệu hình ảnh:
  * Amazon S3 Versioning.
  * Khôi phục phiên bản đối tượng.
  * Kiểm tra đường dẫn hình ảnh sau khôi phục.

* Kiểm tra hoạt động của website sau quá trình phục hồi:
  * Truy cập website.
  * Hiển thị danh sách sản phẩm.
  * Kiểm tra kết nối cơ sở dữ liệu.
  * Kiểm tra khả năng tải hình ảnh từ S3.

* Sử dụng AWS Management Console và log hệ thống để xác định lỗi phát sinh.

* Ghi nhận các bước cấu hình bổ sung cần thực hiện để đưa website trở lại trạng thái hoạt động.