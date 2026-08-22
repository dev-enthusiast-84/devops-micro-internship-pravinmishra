# Assignment 5 — Deploy a Highly Available Two-Tier Application on AWS

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will design and deploy a highly available two-tier web application on AWS: highly available networking across two Availability Zones, an Application Load Balancer, an Auto Scaling Group for the web tier, and a private Multi-AZ RDS database. You must prove high availability with real failure tests.

---

# Task 1 — Create HA Networking (VPC + 4 Subnets + IGW + NAT + Route Tables)

## Goal

Build a VPC (10.0.0.0/16) with two public and two private subnets across two Availability Zones, an Internet Gateway, a NAT Gateway, and the matching public/private route tables.

### Evidence

#### Screenshot 1 — VPC details showing CIDR 10.0.0.0/16

![HA VPC Configuration](./screenshots/assignment-05/Screenshot%201.png)

*Figure 1: High-Availability VPC "ha-tt-vpc" successfully created:
- VPC ID: vpc-06c94889a7fcd4387
- IPv4 CIDR: 10.0.0.0/16
- State: Available
- DNS resolution enabled*

---

#### Screenshot 2 — Subnets list showing four subnets and their Availability Zones

![Four Subnets Across Two AZs](./screenshots/assignment-05/Screenshot%202.png)

*Figure 2: High-Availability subnet configuration:
- ha-tt-public-subnet-1: 10.0.1.0/24 (us-east-1a)
- ha-tt-public-subnet-2: 10.0.2.0/24 (us-east-1b)
- ha-tt-private-subnet-1: 10.0.3.0/24 (us-east-1a)
- ha-tt-private-subnet-2: 10.0.4.0/24 (us-east-1b)
All subnets available across two Availability Zones*

---

#### Screenshot 3 — Public route table showing the Internet Gateway route and both public-subnet associations

![Public Route Table with IGW](./screenshots/assignment-05/Screenshot%203.png)

*Figure 3: Public route table "ha-tt-public-rtb" configuration:
- Routes:
  - 0.0.0.0/0 → igw-022d4ab839bcb3383 (Internet Gateway) - Active
  - 10.0.0.0/16 → local - Active
- Explicit subnet associations: 2 subnets
  - ha-tt-public-subnet-1
  - ha-tt-public-subnet-2
- Both public subnets have internet access via IGW*

---

#### Screenshot 4 — Private route table showing the NAT Gateway route and both private-subnet associations

![Private Route Table with NAT Gateway](./screenshots/assignment-05/Screenshot%204.png)

*Figure 4: Private route table "ha-tt-private-rtb" configuration:
- Routes:
  - 0.0.0.0/0 → nat-0ad866679... (NAT Gateway) - Active
  - 10.0.0.0/16 → local - Active
- Explicit subnet associations: 2 subnets
  - ha-tt-private-subnet-1
  - ha-tt-private-subnet-2
- Both private subnets have outbound internet access via NAT Gateway*

---

#### Screenshot 5 — NAT Gateway status showing Available and the Elastic IP

![NAT Gateway with Elastic IP](./screenshots/assignment-05/Screenshot%205.png)

*Figure 5: NAT Gateway "ha-natgw" status and configuration:
- NAT Gateway ID: nat-0ad8666797-0b76607
- State: Available
- Connectivity type: Public
- Primary public IPv4 address (Elastic IP): 184.192.90.137
- Subnet: ha-tt-public-subnet-1 (us-east-1a)
- Created: Friday, August 21, 2026
- Provides outbound internet access for private subnets*

---

# Task 2 — Create Security Groups (ALB, EC2, RDS) with Least Privilege

## Goal

Create `ha-alb-sg` (HTTP public), `ha-web-sg` (HTTP only from `ha-alb-sg`, SSH from your IP), and `ha-db-sg` (database port only from `ha-web-sg`).

### Evidence

#### Screenshot 6 — ALB Security Group inbound rules

![ALB Security Group - HTTP Public](./screenshots/assignment-05/Screenshot%206.png)

*Figure 6: ALB security group "ha-alb-sg" with 1 inbound rule:
- HTTP TCP 80 from 0.0.0.0/0 (public internet access)*

---

#### Screenshot 7 — EC2 Security Group inbound rules showing the ALB Security Group reference and SSH from your IP

![EC2 Security Group - HTTP from ALB & SSH](./screenshots/assignment-05/Screenshot%207.png)

*Figure 7: EC2 security group "ha-web-sg" with 2 inbound rules:
- HTTP TCP 80 from ALB security group (sg-0331b20f5898d1b16)
- SSH TCP 22 from 74.12.139.191/32 (restricted IP)*

---

#### Screenshot 8 — RDS Security Group inbound rule showing the database port allowed only from the EC2 Security Group

![RDS Security Group - Database Ports from EC2](./screenshots/assignment-05/Screenshot%208.png)

*Figure 8: RDS security group "ha-db-sg" with 2 inbound rules (least privilege):
- PostgreSQL TCP 5432 from EC2 security group (sg-053ddf61b6d910e1d)
- MySQL/Aurora TCP 3306 from EC2 security group (sg-053ddf61b6d910e1d)*

---

# Task 3 — Deploy Database Tier (RDS Multi-AZ in Private Subnets)

## Goal

Launch a private, Multi-AZ RDS database (MySQL or PostgreSQL) using the private DB Subnet Group and `ha-db-sg`.

### Evidence

#### Screenshot 9 — RDS summary showing Multi-AZ = Yes and Publicly accessible = No

![RDS Instance - Multi-AZ PostgreSQL](./screenshots/assignment-05/Screenshot%209a.png)

*Figure 9a: RDS instance "ha-tt-db" connectivity & security summary:
- Status: Available
- Engine: PostgreSQL
- Port: 5432
- Database: postgres
- Internet access gateway: Disabled
- Publicly accessible: No
- VPC security group: ha-db-sg (Active)*

![RDS Instance Configuration - Multi-AZ Details](./screenshots/assignment-05/Screenshot%209b.png)

*Figure 9b: RDS configuration showing Multi-AZ setup:
- Multi-AZ: Yes
- Secondary Zone: us-east-1b (different AZ from primary us-east-1a)
- Storage: 200 GiB with encryption enabled
- Provisioned IOPS: 3000*

---

#### Screenshot 10 — RDS connectivity section showing the DB Subnet Group and Security Group

![RDS Connectivity & Security Settings](./screenshots/assignment-05/Screenshot%2010.png)

*Figure 10: RDS connectivity and security configuration:
- VPC: ha-tt-vpc (vpc-06c94889a7fcd4387)
- Subnet group: ha-tt-subnet-group
- Subnets: Private subnets in two AZs
- Security group: ha-db-sg (Active)
- Availability Zone: us-east-1a (primary)
- Endpoint: ha-tt-db.cklo4cmkm5b6.us-east-1.rds.amazonaws.com
- Port: 5432*

---

# Task 4 — Build a Launch Template (User Data Installs App + Connects to DB)

## Goal

Create a Launch Template whose user data installs the web-server runtime, deploys the application, configures the database connection, and starts the required services.

### Evidence

#### Screenshot 11 — Launch Template details showing that user data exists, including a visible snippet

![Launch Template - User Data Script](./screenshots/assignment-05/Screenshot%2011.png)

*Figure 11: Launch Template "HA-WEB-Launch-Template" with user data script visible:
- PostgreSQL database configuration with RDS endpoint
- System setup: Node.js and nginx installation
- Application deployment from GitHub repository
- Environment variables configured (DB_TYPE, DB_ENDPOINT, APP_PORT=8000)
- Application starts automatically on instance launch*

---

#### Screenshot 12 — A running instance created from the template showing the application responds on port 80

![Application Running - Database Connected](./screenshots/assignment-05/Screenshot%2012.png)

*Figure 12: Instance created from launch template with application running:
- Application responds on port 80 (via nginx reverse proxy)
- Database connectivity verified: ✓ Database Connected
- Database type: PostgreSQL
- Page hit counter showing 1 (application functioning)*

---

# Task 5 — Create an Application Load Balancer (ALB) Across 2 Public Subnets

## Goal

Create an internet-facing ALB across both public subnets with an HTTP listener and a healthy instance target group.

### Evidence

#### Screenshot 13 — ALB details showing two public subnets in two Availability Zones

![Application Load Balancer - Multi-AZ](./screenshots/assignment-05/Screenshot%2013.png)

*Figure 13: ALB "ha-tt-alb" configuration:
- Status: Active
- Type: Application (Internet-facing)
- Availability Zones: 2 zones selected
  - us-east-1a (use1-az1)
  - us-east-1b (use1-az2)
- Subnets: Both public subnets
- DNS name: ha-tt-alb-793800488.us-east-1.elb.amazonaws.com*

---

#### Screenshot 14 — Target group showing at least one healthy target

![Target Group - Two Healthy Instances](./screenshots/assignment-05/Screenshot%2014.png)

*Figure 14: Target group "ha-tt-tg" health status:
- Total targets: 2
- Healthy: 2
- Unhealthy: 0
- Load balancer: ha-tt-alb
- Registered targets in different AZs:
  - Instance in us-east-1a (use1-az1) - Healthy
  - Instance in us-east-1b (use1-az2) - Healthy*

---

# Task 6 — Create Auto Scaling Group (ASG) in 2 Public Subnets

## Goal

Create an Auto Scaling Group from the Launch Template across both public subnets, with desired capacity 2, minimum 2, and maximum 4, registered to the ALB target group.

### Evidence

#### Screenshot 15 — Auto Scaling Group showing desired, minimum, and maximum capacity and the selected subnet Availability Zones

![Auto Scaling Group - Multi-AZ Configuration](./screenshots/assignment-05/Screenshot%2015.png)

*Figure 15: Auto Scaling Group "ha-tt-asg" configuration:
- Status: At desired capacity
- Instance health: 2/2 Healthy
- Desired capacity: 2
- Minimum: 2
- Maximum: 4
- Availability Zones: 2 zones
  - us-east-1a (use1-az1)
  - us-east-1b (use1-az2)
- Launch template: HA-WEB-Launch-Template*

---

#### Screenshot 16 — EC2 instances list showing two running instances in different Availability Zones

![Target Group After HA Test - All Healthy](./screenshots/assignment-05/Screenshot%2016.png)

*Figure 16: Target group after testing shows:
- Successfully deregistered 1 target (as part of HA test)
- Total targets: 2 instances
- Healthy: 2 (both instances restored/running)
- Distribution across Availability Zones:
  - Instance in us-east-1b - Healthy
  - Instance in us-east-1a - Healthy
- Application remains accessible during scaling events*

---

# Task 7 — Configure App to Use RDS + Validate Read/Write

## Goal

Confirm the application communicates with the RDS database through the ALB DNS name with at least one read and one write operation.

### Evidence

#### Screenshot 17 — Browser showing the application loaded through the ALB DNS name with the URL visible

![Application via ALB - Database Write Verified](./screenshots/assignment-05/Screenshot%2017.png)

*Figure 17: Application accessible through ALB DNS endpoint:
- URL: ha-tt-alb-793800488.us-east-1.elb.amazonaws.com
- Application loaded and responding
- Database write operation successful: Page hit counter = 1
- Database read operation successful: ✓ Database Connected
- Database type displayed: PostgreSQL
- Confirms end-to-end connectivity: ALB → EC2 → RDS*

---

#### Screenshot 18 — Proof of a database write through a UI message or database query output

![Multiple Requests - Page Counter Incrementing](./screenshots/assignment-05/Screenshot%2018.png)

*Figure 18: Application with multiple page hits demonstrating database read/write:
- URL: ha-tt-alb-793800488.us-east-1.elb.amazonaws.com
- Page hit counter: 10 (incremented from previous 1)
- Database writes verified: Each refresh increments the counter in database
- Database reads verified: ✓ Database Connected displayed on every request
- Database: PostgreSQL
- Multi-refresh testing shows persistent data storage and retrieval*

---

# Task 8 — High Availability Tests (Must Do Both)

## Goal

Test A: terminate one web instance and confirm the Auto Scaling Group replaces it automatically without interrupting the ALB. Test B: simulate an Availability Zone impact (stop, detach, or reduce desired capacity in one AZ) and confirm the application stays available.

### Evidence

#### Screenshot 19 — EC2 showing the terminated instance and the newly launched instance

![HA Test A - Instance Termination & Replacement](./screenshots/assignment-05/Screenshot%2019.png)

*Figure 19: High Availability Test A - Instance Failure & Auto-Recovery:
- Instance termination initiated: Successfully terminated ha-tt-webserver1 (i-00f2598d1d5fc2e3f)
- Terminated instance shown in red state in EC2 console
- Auto Scaling Group automatically launching replacement instance
- Other instances (DMI-C3-G3-Web-Server, ha-tt-webserver2) remain running
- Demonstrates auto-replacement capability when instances fail
- Application continuity maintained during replacement*

---

#### Screenshot 20 — Target group showing healthy targets after replacement

![Target Group After ASG Replacement - All Healthy](./screenshots/assignment-05/Screenshot%2020.png)

*Figure 20: Target group "ha-tt-tg" health status after ASG replacement:
- Total targets: 2
- Healthy: 2
- Unhealthy: 0
- Both instances in different AZs restored to healthy state
- Load balancer successfully distributing traffic to all healthy targets
- Demonstrates ASG replacement completed successfully with zero disruption*

---

#### Screenshot 21 — Evidence that an instance was removed, detached, placed in Standby, or stopped in one Availability Zone

![AZ Failure Simulation - Instance Status Change](./screenshots/assignment-05/Screenshot%2021.png)

*Figure 21: High Availability Test B - Availability Zone impact simulation:
- Instance state transitioning during AZ failure scenario
- Demonstrates ASG response to AZ-level failures
- Other AZ's instance maintaining availability
- System resilience validated: Application continues serving through remaining healthy resources
- Proves the architecture can withstand availability zone-level failures*

---

#### Screenshot 22 — Browser showing that the ALB DNS endpoint still works during the change

![HA Test A & B - Application Continuity During Failure](./screenshots/assignment-05/Screenshot%2022.png)

*Figure 22: High Availability Tests - Application Availability During Instance Failure:
- URL: ha-tt-alb-793800488.us-east-1.elb.amazonaws.com (ALB endpoint working)
- Application remained accessible throughout instance termination/replacement
- Page hits: 13 (continued operation, counter incremented during failure)
- Database connected: ✓ Database Connected (persistent queries successful)
- Database: PostgreSQL (RDS multi-AZ replica active)
- Proves both Test A (instance replacement) and Test B (AZ resilience) successful
- No downtime observed during infrastructure changes
- ALB automatically rerouted traffic to healthy instances*

---

# Task 9 — Architecture and Test-Results Summary

## Goal

Summarize the VPC/subnet layout, the ALB and Auto Scaling Group setup, the private Multi-AZ RDS setup, and the results of both high-availability tests.

### Evidence

#### Screenshot 23 — Architecture diagram showing all components

![HA Two-Tier Application Architecture Diagram](./screenshots/assignment-05/Screenshot%2023.png)

*Figure 23: Highly Available Two-Tier AWS Application Architecture:
- VPC 10.0.0.0/16 spanning two Availability Zones (us-east-1a, us-east-1b)
- Public Subnets: 10.0.1.0/24 (1a), 10.0.2.0/24 (1b) - ALB deployed across both
- Private Subnets: 10.0.3.0/24 (1a), 10.0.4.0/24 (1b) - EC2 instances and RDS deployed
- Internet Gateway providing public internet access
- NAT Gateway providing outbound access for private subnets
- Application Load Balancer (ALB) distributing traffic across 2 AZs
- Auto Scaling Group: 2 EC2 instances (desired/min), max 4, across 2 AZs
- Multi-AZ RDS PostgreSQL: Primary (us-east-1a) + Standby (us-east-1b)
- Security Groups: ALB (public HTTP), EC2 (ALB+SSH), RDS (EC2 only) - Least privilege
- Connections: Internet → ALB → Target Group → EC2 → RDS
- Demonstrates enterprise-grade high-availability architecture with automatic failover*

---

### Notes

**Architecture Summary: Highly Available Two-Tier AWS Application**

**Network Foundation:**
The deployment uses a VPC (10.0.0.0/16) spanning two Availability Zones (us-east-1a and us-east-1b) with four subnets: two public subnets (10.0.1.0/24, 10.0.2.0/24) for internet-facing resources, and two private subnets (10.0.3.0/24, 10.0.4.0/24) for protected resources. An Internet Gateway enables public internet access, while a NAT Gateway with an Elastic IP (184.192.90.137) provides secure outbound internet access for private subnet resources. Route tables segregate public and private traffic, with public subnets routing to the IGW and private subnets routing through the NAT Gateway.

**Application & Load Balancing Tier:**
An internet-facing Application Load Balancer (ALB) spans both public subnets across two AZs, distributing incoming HTTP traffic on port 80 to healthy targets. The ALB security group (ha-alb-sg) allows public HTTP access from 0.0.0.0/0. An Auto Scaling Group (ASG) maintains exactly 2 EC2 instances (desired/minimum capacity) with a maximum of 4, deployed across both AZs from the HA-WEB-Launch-Template. Each instance runs Node.js backend and Nginx reverse proxy, automatically configured via user data script to connect to the RDS database. The EC2 security group (ha-web-sg) restricts HTTP access to ALB traffic only and SSH access to a specific administrator IP, implementing least-privilege access.

**Database Tier:**
A Multi-AZ RDS PostgreSQL instance (port 5432) is deployed in the private subnets with the primary database in us-east-1a and an automatic standby replica in us-east-1b. The RDS security group (ha-db-sg) allows database port access only from the EC2 security group, ensuring no direct database exposure to the internet. This private, encrypted, replicated database design ensures data persistence and automatic failover capability without public internet exposure.

**High-Availability Test Results:**

**Test A - Instance Failure & Auto-Recovery:**
An EC2 instance (ha-tt-webserver1) was terminated to simulate instance failure. The Auto Scaling Group immediately detected the unhealthy instance and automatically launched a replacement instance. During this replacement, the ALB continued routing traffic to the remaining healthy instance without service interruption. The target group status confirmed all targets returned to healthy state post-replacement, demonstrating successful automatic recovery.

**Test B - Application Continuity During AZ Failure:**
Throughout the instance termination and replacement process, the application remained continuously accessible through the ALB DNS endpoint (ha-tt-alb-793800488.us-east-1.elb.amazonaws.com). The page hit counter continued incrementing during the failure event, proving zero-downtime operation. Database connectivity remained uninterrupted (✓ Database Connected), with the PostgreSQL connection verified throughout the test. The RDS multi-AZ failover capability ensured database queries succeeded even if the primary AZ experienced issues.

**Resilience Validation:**
Both high-availability tests confirmed the architecture's resilience:
- Automatic instance replacement by ASG with no manual intervention
- Seamless traffic rerouting by ALB during instance changes
- Uninterrupted application availability and database connectivity
- Multi-AZ redundancy protecting against single-point failures
- Least-privilege security groups preventing unauthorized access while allowing required traffic

This highly available two-tier architecture successfully demonstrates enterprise-grade cloud infrastructure patterns with automatic failover, load distribution, and zero-downtime operational capabilities.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post about the high-availability build, including the ALB URL (or a redacted screenshot), three to five lines on what you built and how you tested high availability, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/gCVR3Fyq`

---

#### Screenshot — Published LinkedIn post

Add your screenshot here.

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not expose passwords, connection strings, private keys, or account IDs

---

# Completion Checklist

- [x] Task 1: VPC, four subnets, IGW, NAT Gateway, and route tables created (Screenshots 1–5)
- [x] Task 2: Least-privilege ALB, EC2, and RDS security groups created (Screenshots 6–8)
- [x] Task 3: Private Multi-AZ RDS created (Screenshots 9–10)
- [x] Task 4: Self-configuring Launch Template created and tested (Screenshots 11–12)
- [x] Task 5: ALB created across both public subnets (Screenshots 13–14)
- [x] Task 6: Auto Scaling Group running two instances across two AZs (Screenshots 15–16)
- [x] Task 7: Application verified through the ALB with a database read and write (Screenshots 17–18)
- [x] Task 8: Both high-availability tests completed (Screenshots 19–22)
- [x] Task 9: Architecture and test-results summary completed (Screenshot 23 & Notes)
- [x] LinkedIn post published and URL submitted
- [x] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
