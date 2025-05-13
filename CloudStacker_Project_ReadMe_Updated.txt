
Project: CloudStacker – Multi-Tier Web Application on AWS (Free Tier)

Overview:
---------
CloudStacker is a scalable, cost-efficient, and modular multi-tier web application architecture deployed entirely using AWS Free Tier services. The architecture separates the web frontend, application backend, and data storage tiers across secure, logically isolated layers in a VPC to simulate real-world enterprise-grade deployments.

Architecture Summary:
---------------------
1. VPC (CIDR: 10.0.0.0/16)
2. Public Subnet: 10.0.0.0/24 – Frontend EC2
3. Private Subnet: 10.0.1.0/24 – Backend EC2 and RDS
4. RDS MySQL Database – Hosted in the private subnet
5. CloudWatch Monitoring – For EC2 instance
6. IAM Roles for Instance Connect and CloudWatch Agent
7. Security Groups for Frontend and Backend tier
8. SNS Email Notification for CloudWatch Alarm

Deployment Steps:
-----------------
1. Created a custom VPC with IPv4 CIDR block 10.0.0.0/16
2. Created two subnets:
   - Public Subnet (10.0.0.0/24) for Frontend EC2
   - Private Subnet (10.0.1.0/24) for Backend EC2 and RDS
3. Attached Internet Gateway to the VPC
4. Configured Route Tables:
   - Public Route Table routed 0.0.0.0/0 to the Internet Gateway
   - Associated with Public Subnet
   - Private subnet retained local-only route for security
5. Launched Frontend EC2 Instance:
   - AMI: Amazon Linux 2
   - Type: t2.micro (Free Tier)
   - Installed Apache via User Data
   - Served sample index.html content
   - Enabled auto-assign public IP
6. Created Backend EC2 Instance in Private Subnet:
   - Type: t2.micro
   - Hosted backend logic
   - Security Group limited to traffic only from Frontend SG on port 8080
7. Deployed RDS MySQL:
   - Type: db.t2.micro
   - Deployed in private subnet
   - Secured to accept only traffic from Backend EC2
   - Verified that database endpoint is private and inaccessible from public
8. Configured IAM Role:
   - EC2InstanceConnect Role attached to EC2 instance for secure SSH access
   - Screenshot: "ec2instanceconnect role"
9. Created two Security Groups:
   - CloudTracker_Frontend:
     - HTTP (80) and SSH (22) allowed from My IP
   - CloudTracker_Backend:
     - App port allowed from Frontend SG only
     - Screenshot: "CloudTracker_Backend security group"
10. Enabled CloudWatch Monitoring:
    - Default metrics: CPUUtilization, NetworkIn/Out
    - Created CloudWatch Alarm:
      - Trigger: CPUUtilization > 70% for 2 consecutive periods
      - Added SNS Email Notification for threshold breach
      - Verified alarm notification delivery to email
      - Screenshot: "cloudtrack alarm"
      - Config screenshot: "setting the in alarm to cpuutilization treshold to 70%"

Security & Best Practices:
--------------------------
- Subnet isolation used to restrict database and backend traffic
- IAM roles configured with least privilege access
- No hardcoded credentials; all database connections securely configured
- EC2 instances launched within Free Tier eligibility limits
- Alarm notifications configured with SNS to ensure operational awareness

Testing:
--------
- Accessed Frontend EC2 public IP to verify Apache is running
- Simulated load to test CPU alarms and email notification
- Verified backend only responds to allowed sources
- Confirmed RDS was accessible only from Backend EC2

Conclusion:
-----------
The CloudStacker project successfully demonstrates how to deploy a secure, modular, and scalable multi-tier web application architecture using only AWS Free Tier services. The configuration closely models industry best practices, particularly in isolating workloads, enforcing IAM principles, ensuring cost efficiency, and implementing real-time monitoring with actionable notifications.

Documentation & Screenshots:
----------------------------
- cloudtrack alarm
- setting the in alarm to cpuutilization treshold to 70%
- ec2instanceconnect role
- CloudTracker_Backend security group
- CloudTracker_Frontend security group

Prepared by: Tolulope Philip Olalere
Role: AWS Solutions Architect
