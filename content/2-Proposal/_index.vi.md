---
title: "Bản đề xuất"
date: 2026-10-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Hệ thống Backup & Disaster Recovery đa vùng trên AWS
## Giải pháp sao lưu, sao chép liên vùng và cảnh báo sự cố

### 1. Tóm tắt điều hành
Đề tài xây dựng hệ thống Backup & Disaster Recovery (BDR) đa vùng trên AWS, giúp một ứng dụng web chạy trên EC2 và cơ sở dữ liệu RDS tiếp tục hoạt động khi vùng chính gặp sự cố. Vùng chính là Singapore (ap-southeast-1), vùng dự phòng là Sydney (ap-southeast-2).

Hệ thống kết hợp bốn nhóm giải pháp: sao lưu tự động bằng AWS Backup, sao chép dữ liệu liên vùng (RDS read replica và S3 cross-region replication), giám sát sức khỏe bằng Route 53 và CloudWatch, và cảnh báo qua Amazon SNS. Quyền truy cập được quản lý bằng IAM, gồm nhóm BackupAdmins và vai trò dịch vụ AWSBackupDefaultServiceRole.

Kết quả cuối cùng gồm một kiến trúc BDR hoạt động thực tế, số liệu đo RTO (thời gian khôi phục) và RPO (lượng dữ liệu có thể mất), cùng bộ tài liệu workshop có thể tái sử dụng cho doanh nghiệp vừa và nhỏ.

### 2. Tuyên bố vấn đề
*Vấn đề hiện tại*  
Ứng dụng chỉ chạy trong một vùng AWS là điểm lỗi đơn: khi vùng đó gián đoạn, cả dịch vụ và dữ liệu đều không truy cập được. Nhiều đơn vị vẫn sao lưu thủ công, không có chính sách lưu giữ rõ ràng, không kiểm thử khôi phục và không biết mất bao lâu để phục hồi hay mất bao nhiêu dữ liệu.

*Giải pháp*  
Hệ thống BDR đa vùng giải quyết ba việc. Thứ nhất, sao lưu tự động theo lịch (DailyBackupPlan, lưu giữ 30 ngày) vào BackupDR-Vault. Thứ hai, luôn có bản sao dữ liệu ở vùng Sydney: RDS read replica và S3 sao chép liên vùng. Thứ ba, phát hiện sự cố bằng Route 53 health check, cảnh báo qua CloudWatch và SNS, rồi đo được RTO và RPO khi chuyển sang vùng dự phòng.

*Lợi ích*  
- Giảm rủi ro mất dữ liệu và gián đoạn dịch vụ khi vùng chính gặp sự cố.
- Tối ưu chi phí lưu trữ nhờ vòng đời S3: Standard, sau 30 ngày chuyển Standard-IA, sau 90 ngày chuyển Glacier.
- Có số liệu RTO/RPO thực đo để làm cơ sở lập kế hoạch dự phòng.
- Có tài liệu workshop từng bước để học tập và nhân rộng.

### 3. Kiến trúc giải pháp
Kiến trúc theo mô hình hai vùng: Singapore chạy ứng dụng thật, Sydney giữ bản sao dữ liệu sẵn sàng tiếp quản. Giám sát sức khỏe đặt ở Route 53 và CloudWatch (vùng us-east-1, nơi AWS ghi metric của Route 53).

![Kiến trúc BDR hai vùng](/images/2-Proposal/bdr_architecture.png)

*Dịch vụ AWS sử dụng*  
- *VPC*: backup-dr-vpc, mạng riêng cho ứng dụng và cơ sở dữ liệu.  
- *Amazon EC2*: app-server chạy ứng dụng web, Elastic IP giữ địa chỉ công khai cố định.  
- *Amazon RDS*: backup-dr-db và bản sao đọc liên vùng backup-dr-db-replica-syd ở Sydney.  
- *Amazon S3*: bucket có versioning, mã hóa, lifecycle và sao chép sang Sydney.  
- *AWS Backup*: BackupDR-Vault và DailyBackupPlan, lưu giữ 30 ngày.  
- *Amazon Route 53*: health check kiểm tra sức khỏe ứng dụng.  
- *Amazon CloudWatch*: alarm Route53-HealthCheck-Alarm phát cảnh báo khi health check thất bại.  
- *Amazon SNS*: topic BackupDR-Alerts-Route53 gửi email cảnh báo.  
- *AWS IAM*: người dùng mai-admin, nhóm BackupAdmins, vai trò AWSBackupDefaultServiceRole.  

*Thiết kế thành phần*  
- *Ứng dụng*: EC2 app-server truy cập qua Elastic IP, dùng cơ sở dữ liệu RDS backup-dr-db.  
- *Sao lưu*: AWS Backup chạy DailyBackupPlan hằng ngày cho các tài nguyên được gán vào plan.  
- *Sao chép liên vùng*: RDS tạo bản sao đọc ở Sydney; S3 sao chép đối tượng sang bucket ở Sydney.  
- *Giám sát*: Route 53 health check kiểm tra ứng dụng, CloudWatch alarm theo dõi kết quả.  
- *Cảnh báo*: SNS gửi email tới quản trị viên khi alarm chuyển sang trạng thái ALARM.  

### 4. Triển khai kỹ thuật
*Các giai đoạn triển khai*  
Dự án triển khai theo 7 giai đoạn, mỗi giai đoạn cho ra một kết quả có thể kiểm chứng bằng ảnh chụp màn hình trên AWS Management Console:  
1. *Nền tảng*: tạo IAM user, nhóm BackupAdmins, vai trò AWSBackupDefaultServiceRole và VPC backup-dr-vpc.  
2. *Ứng dụng và cơ sở dữ liệu tại Singapore*: dựng EC2 app-server với Elastic IP và RDS backup-dr-db.  
3. *Lưu trữ S3*: bật versioning, mã hóa và lifecycle.  
4. *Sao chép liên vùng*: tạo RDS read replica ở Sydney và bật S3 cross-region replication; kiểm chứng dữ liệu đồng bộ.  
5. *Sao lưu tập trung*: tạo BackupDR-Vault và DailyBackupPlan với thời gian lưu giữ 30 ngày.  
6. *Giám sát và cảnh báo*: Route 53 health check, CloudWatch alarm cho health check, cho EC2 và RDS, SNS gửi email.  
7. *Kiểm thử và đo lường*: mô phỏng sự cố, chuyển sang Sydney, đo RTO và RPO, ghi lại kết quả.  

*Yêu cầu kỹ thuật*  
- Tài khoản AWS có quyền quản trị; làm việc chủ yếu trên Console, dùng CLI khi cần.  
- Hai vùng: ap-southeast-1 (chính) và ap-southeast-2 (dự phòng); metric của Route 53 chỉ có ở us-east-1 nên cảnh báo đặt tại vùng này.  
- Kiến thức nền về EC2, RDS, S3, IAM và mạng VPC.  

### 5. Lộ trình & Mốc triển khai
- *Giai đoạn 1 đến 3 và 5*: nền tảng, ứng dụng và cơ sở dữ liệu, S3, AWS Backup: hoàn thành.  
- *Giai đoạn 4*: sao chép liên vùng đã triển khai, còn kiểm chứng đồng bộ.  
- *Giai đoạn 6*: còn alarm cho EC2 và RDS tại Singapore.  
- *Giai đoạn 7*: còn đo RTO/RPO chính thức và ghi tài liệu.  

Mốc thời gian cụ thể của từng giai đoạn được bổ sung theo kế hoạch thực tập.

### 6. Ước tính ngân sách
Chi phí lớn nhất nằm ở các tài nguyên chạy liên tục ở hai vùng (EC2, RDS và RDS replica) và dữ liệu truyền liên vùng. Con số cụ thể được lấy từ AWS Pricing Calculator theo cấu hình đã chọn.

*Yếu tố tính phí*  
- EC2 app-server: giờ chạy theo loại instance, ổ đĩa EBS, Elastic IP.  
- RDS backup-dr-db: giờ chạy instance, dung lượng lưu trữ.  
- RDS replica (Sydney): giờ chạy instance thứ hai, dung lượng, truyền dữ liệu liên vùng.  
- S3 và replication: dung lượng theo từng lớp lưu trữ, số request, truyền dữ liệu sang Sydney.  
- AWS Backup: dung lượng bản sao lưu lưu giữ 30 ngày.  
- Route 53: số health check mỗi tháng.  
- CloudWatch: số alarm mỗi tháng.  
- SNS: số thông báo gửi đi.  

*Cách kiểm soát chi phí*  
- Dùng lifecycle S3 để chuyển dữ liệu cũ sang Standard-IA rồi Glacier.  
- Giới hạn thời gian lưu giữ bản sao lưu ở 30 ngày.  
- Dừng hoặc xóa RDS replica, Elastic IP và tài nguyên demo sau khi hoàn thành workshop.  
- Đặt AWS Budgets để cảnh báo khi chi phí vượt ngưỡng.  

### 7. Đánh giá rủi ro
*Ma trận rủi ro*  
- Chi phí vượt dự kiến: ảnh hưởng trung bình, xác suất trung bình.  
- Dữ liệu sao chép liên vùng chưa đồng bộ: ảnh hưởng cao, xác suất thấp.  
- Thiếu quyền IAM làm sao lưu hoặc sao chép lỗi: ảnh hưởng trung bình, xác suất trung bình.  
- Cảnh báo không hoạt động do sai vùng: ảnh hưởng cao, xác suất thấp.  
- Demo trực tiếp lỗi khi bảo vệ: ảnh hưởng cao, xác suất trung bình.  

*Chiến lược giảm thiểu*  
- Chi phí: AWS Budgets, dừng hoặc xóa tài nguyên sau khi dùng.  
- Đồng bộ: kiểm chứng replica và S3 replication trước khi đo RTO/RPO.  
- Quyền IAM: dùng vai trò dịch vụ chuẩn, kiểm tra policy trước khi chạy.  
- Cảnh báo: đặt alarm của Route 53 ở us-east-1 và thử kích hoạt cảnh báo.  
- Demo: kết hợp demo trực tiếp và video quay sẵn.  

*Kế hoạch dự phòng*  
- Nếu demo trực tiếp lỗi, chuyển sang video quay sẵn và ảnh chụp màn hình từng bước.  
- Nếu vùng Singapore thực sự gián đoạn, dùng bản sao lưu và replica ở Sydney để khôi phục.  

### 8. Kết quả kỳ vọng
*Cải tiến kỹ thuật*: Dữ liệu ứng dụng và cơ sở dữ liệu luôn có bản sao ở Sydney, sao lưu tự động hằng ngày, sự cố được báo qua email ngay khi health check thất bại, RTO và RPO được đo chính thức.  
*Giá trị dài hạn*: Bộ tài liệu workshop có ảnh chụp kiểm chứng từng bước, và mô hình tham khảo cho doanh nghiệp vừa và nhỏ muốn triển khai BDR trên AWS với chi phí kiểm soát được.