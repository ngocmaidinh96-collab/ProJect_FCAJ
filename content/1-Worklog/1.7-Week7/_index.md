---
title: "Week 7 Worklog"
date: 2026-09-21
weight: 7
chapter: false
pre: "<b>1.7.</b>"
---

### Week 7 Objectives:

* Learn about multi-Region Backup and Disaster Recovery architectures.
* Develop a disaster recovery strategy between Singapore and Sydney.
* Study resource replication and recovery in the standby Region.

### Tasks to Be Completed This Week:

| Day | Task | Start Date | Completion Date | Reference Materials |
|---|---|---|---|---|
| 2 | Learn about RTO, RPO, and Disaster Recovery models. | 21/09/2026 | 21/09/2026 | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |
| 3 | Analyze the architecture using Singapore as the primary Region and Sydney as the standby Region. | 22/09/2026 | 22/09/2026 | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |
| 4 | Learn about copying Recovery Points to another Region and the required conditions. | 23/09/2026 | 24/09/2026 | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |
| 5 | Study recovery strategies for EC2, RDS, and S3 data in the standby Region. | 25/09/2026 | 25/09/2026 | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |
| 6 | Develop a Backup and DR architecture diagram and identify the recovery steps. | 26/09/2026 | 26/09/2026 | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |

### Week 7 Achievements:

* Understand Backup and Disaster Recovery architectures deployed across multiple AWS Regions.

* Learn about the roles of the two Regions:
  * Singapore as the primary Region.
  * Sydney as the standby Region.

* Study data replication mechanisms:
  * AWS Backup Cross-Region Copy.
  * Amazon S3 Cross-Region Replication.
  * Copying Recovery Points to another Region.

* Understand the requirements for implementing cross-Region backups:
  * A Backup Vault in the destination Region.
  * IAM Roles and access permissions.
  * Backup retention policies.
  * The ability to restore resources in the destination Region.

* Learn about services that support availability and monitoring:
  * Amazon Route 53 for DNS management and traffic routing.
  * Amazon CloudWatch for resource monitoring.
  * Amazon SNS for sending alert notifications.
  * AWS Elastic Disaster Recovery for server recovery.

* Develop a Backup and DR architecture diagram for the online shopping website.

* Identify the steps required to restore the application server, database, and product images.

* Understand the relationship between implementation costs, Recovery Time Objective (RTO), and Recovery Point Objective (RPO).