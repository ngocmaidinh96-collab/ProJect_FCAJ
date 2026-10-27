---
title: "Week 9 Worklog"
date: 2026-10-05
weight: 9
chapter: false
pre: "<b>1.9.</b>"
---

### Week 9 Objectives:

* Simulate potential system failure scenarios.
* Evaluate the effectiveness of the Backup and DR strategy.
* Measure recovery time and the extent of data loss.

### Tasks to Be Completed This Week:

| Day | Task | Start Date | Completion Date | Reference Materials |
|---|---|---|---|---|
| 2 | Develop scenarios involving data loss, accidental data deletion, and application server failure. | 05/10/2026 | 05/10/2026 | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |
| 3 | Practice restoring data or a suitable S3 object version. | 06/10/2026 | 06/10/2026 | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |
| 4 | Simulate an EC2 failure and restore the application server. | 07/10/2026 | 08/10/2026 | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |
| 5 | Verify RDS data and website functionality after restoration. | 09/10/2026 | 09/10/2026 | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |
| 6 | Record recovery start and completion times, and calculate RTO and RPO. | 10/10/2026 | 10/10/2026 | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |

### Week 9 Achievements:

* Develop system failure simulation scenarios:
  * Data loss or accidental data deletion.
  * Amazon EC2 application server failure.
  * Amazon RDS database failure.
  * Missing images or incorrect image versions in S3.

* Use AWS Backup to perform or verify EC2 and RDS recovery procedures.

* Use S3 Versioning to test the ability to restore previous object versions.

* Check resource status before and after simulated failures:
  * Amazon EC2.
  * Amazon RDS.
  * Amazon S3.
  * AWS Backup Recovery Point.

* Learn how to use Amazon CloudWatch to monitor resource status and support incident analysis.

* Record recovery times to calculate RTO:
  * The start time of the incident or recovery process, according to the testing convention.
  * The time when the system is restored and operational again.
  * The total recovery duration.

* Evaluate RPO based on the timestamp of the latest recoverable data.

* Compare test results with the expected RTO and RPO targets.

* Identify limitations and propose improvements to the Backup and Disaster Recovery strategy.