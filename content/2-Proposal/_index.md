---
title: "Proposal"
date: 2026-10-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Multi-Region Backup & Disaster Recovery System on AWS
## A solution for backup, cross-region replication, and incident alerting

### 1. Executive Summary
This project builds a multi-region Backup & Disaster Recovery (BDR) system on AWS so that a web application running on EC2 and an RDS database keep working when the primary region fails. The primary region is Singapore (ap-southeast-1) and the disaster recovery region is Sydney (ap-southeast-2).

The system combines four groups of solutions: automated backups with AWS Backup, cross-region data replication (RDS read replica and S3 cross-region replication), health monitoring with Route 53 and CloudWatch, and alerting through Amazon SNS. Access is managed with IAM, including the BackupAdmins group and the AWSBackupDefaultServiceRole service role.

The final outcome is a working BDR architecture, measured RTO (recovery time) and RPO (potential data loss) figures, and a reusable workshop guide for small and medium-sized businesses.

### 2. Problem Statement
*The Problem*  
An application running in a single AWS region is a single point of failure: when that region is disrupted, both the service and its data become unreachable. Many organizations still back up manually, have no clear retention policy, never test recovery, and do not know how long recovery takes or how much data they would lose.

*The Solution*  
The multi-region BDR system addresses three things. First, automated scheduled backups (DailyBackupPlan, 30-day retention) into BackupDR-Vault. Second, a copy of the data is always available in the Sydney region: an RDS read replica and cross-region S3 replication. Third, incidents are detected with Route 53 health checks and alerted through CloudWatch and SNS, and RTO and RPO can be measured when failing over to the DR region.

*Benefits*  
- Reduces the risk of data loss and service disruption when the primary region fails.
- Optimizes storage cost with an S3 lifecycle: Standard, then Standard-IA after 30 days, then Glacier after 90 days.
- Provides measured RTO/RPO figures as a basis for disaster recovery planning.
- Provides a step-by-step workshop guide for learning and replication.

### 3. Solution Architecture
The architecture follows a two-region model: Singapore runs the live application, while Sydney holds a copy of the data ready to take over. Health monitoring sits in Route 53 and CloudWatch (in the us-east-1 region, where AWS records Route 53 metrics).

![Two-region BDR architecture](/images/2-Proposal/bdr_architecture.png)

*AWS Services Used*  
- *VPC*: backup-dr-vpc, a private network for the application and database.  
- *Amazon EC2*: app-server runs the web application; an Elastic IP keeps the public address fixed.  
- *Amazon RDS*: backup-dr-db and the cross-region read replica backup-dr-db-replica-syd in Sydney.  
- *Amazon S3*: a bucket with versioning, encryption, lifecycle rules, and replication to Sydney.  
- *AWS Backup*: BackupDR-Vault and DailyBackupPlan, 30-day retention.  
- *Amazon Route 53*: a health check that monitors application health.  
- *Amazon CloudWatch*: the Route53-HealthCheck-Alarm alarm raises an alert when the health check fails.  
- *Amazon SNS*: the BackupDR-Alerts-Route53 topic sends alert emails.  
- *AWS IAM*: the mai-admin user, the BackupAdmins group, and the AWSBackupDefaultServiceRole role.  

*Component Design*  
- *Application*: the EC2 app-server is reached through the Elastic IP and uses the RDS database backup-dr-db.  
- *Backup*: AWS Backup runs DailyBackupPlan daily for the resources assigned to the plan.  
- *Cross-region replication*: RDS creates a read replica in Sydney; S3 replicates objects to a bucket in Sydney.  
- *Monitoring*: the Route 53 health check probes the application and a CloudWatch alarm tracks the result.  
- *Alerting*: SNS emails the administrator when the alarm enters the ALARM state.  

### 4. Technical Implementation
*Implementation Phases*  
The project is implemented in 7 phases, each producing a result that can be verified with screenshots in the AWS Management Console:  
1. *Foundation*: create the IAM user, the BackupAdmins group, the AWSBackupDefaultServiceRole role, and the backup-dr-vpc VPC.  
2. *Application and database in Singapore*: set up the EC2 app-server with an Elastic IP and the RDS database backup-dr-db.  
3. *S3 storage*: enable versioning, encryption, and lifecycle rules.  
4. *Cross-region replication*: create the RDS read replica in Sydney and enable S3 cross-region replication; verify that data is in sync.  
5. *Centralized backup*: create BackupDR-Vault and DailyBackupPlan with 30-day retention.  
6. *Monitoring and alerting*: Route 53 health check, CloudWatch alarms for the health check, EC2, and RDS, and SNS email notifications.  
7. *Testing and measurement*: simulate an incident, fail over to Sydney, measure RTO and RPO, and record the results.  

*Technical Requirements*  
- An AWS account with administrative permissions; work is done mainly in the Console, with the CLI used when needed.  
- Two regions: ap-southeast-1 (primary) and ap-southeast-2 (DR); Route 53 metrics are only available in us-east-1, so alarms are placed in that region.  
- Basic knowledge of EC2, RDS, S3, IAM, and VPC networking.  

### 5. Timeline & Milestones
- *Phases 1 to 3 and 5*: foundation, application and database, S3, AWS Backup: completed.  
- *Phase 4*: cross-region replication is implemented; synchronization still needs to be verified.  
- *Phase 6*: CloudWatch alarms for EC2 and RDS in Singapore are still to be added.  
- *Phase 7*: formal RTO/RPO measurement and documentation are still to be done.  

Specific dates for each phase will be added according to the internship schedule.

### 6. Budget Estimation
The largest costs come from resources running continuously in two regions (EC2, RDS, and the RDS replica) and from cross-region data transfer. Exact figures are taken from the AWS Pricing Calculator based on the chosen configuration.

*Cost Drivers*  
- EC2 app-server: running hours by instance type, EBS volume, Elastic IP.  
- RDS backup-dr-db: instance running hours, storage capacity.  
- RDS replica (Sydney): running hours of a second instance, storage, cross-region data transfer.  
- S3 and replication: capacity per storage class, number of requests, data transfer to Sydney.  
- AWS Backup: storage of backups retained for 30 days.  
- Route 53: number of health checks per month.  
- CloudWatch: number of alarms per month.  
- SNS: number of notifications sent.  

*Cost Control*  
- Use S3 lifecycle rules to move older data to Standard-IA and then Glacier.  
- Limit backup retention to 30 days.  
- Stop or delete the RDS replica, Elastic IP, and demo resources after finishing the workshop.  
- Set up AWS Budgets to alert when costs exceed a threshold.  

### 7. Risk Assessment
*Risk Matrix*  
- Costs exceed the estimate: medium impact, medium probability.  
- Cross-region data not yet in sync: high impact, low probability.  
- Missing IAM permissions causing backup or replication failures: medium impact, medium probability.  
- Alerts not firing because of the wrong region: high impact, low probability.  
- Live demo fails during the defense: high impact, medium probability.  

*Mitigation Strategies*  
- Cost: AWS Budgets, and stop or delete resources after use.  
- Synchronization: verify the replica and S3 replication before measuring RTO/RPO.  
- IAM permissions: use the standard service role and check policies before running.  
- Alerts: place the Route 53 alarm in us-east-1 and test-trigger the alert.  
- Demo: combine a live demo with a pre-recorded video.  

*Contingency Plans*  
- If the live demo fails, switch to the pre-recorded video and step-by-step screenshots.  
- If the Singapore region is actually disrupted, recover using the backups and the replica in Sydney.  

### 8. Expected Outcomes
*Technical Improvements*: Application and database data always have a copy in Sydney, backups run automatically every day, incidents are reported by email as soon as the health check fails, and RTO and RPO are formally measured.  
*Long-term Value*: A workshop guide with step-by-step verification screenshots, and a reference model for small and medium-sized businesses that want to deploy BDR on AWS with controllable costs.