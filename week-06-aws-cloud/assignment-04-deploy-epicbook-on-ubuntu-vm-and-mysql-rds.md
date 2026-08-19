
# Assignment 4 — Deploy EpicBook on Ubuntu VM + MySQL RDS with Secure Cloud Network

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will deploy the EpicBook web application in AWS using a secure two-tier architecture: an Ubuntu EC2 instance with Nginx in a public subnet, and a private MySQL RDS database with restricted security-group access. The completed deployment must prove that the frontend, backend, and private database communicate successfully end to end.

---

# Task 1 — Create VPC + Public/Private Subnets + Routing

## Goal

Create `epicbook-vpc` (10.0.0.0/16) with a public subnet (10.0.1.0/24) and a private subnet (10.0.2.0/24), attach an Internet Gateway, and route only the public subnet to it.

### Evidence

#### Screenshot 1 — VPC details showing CIDR 10.0.0.0/16

![VPC Configuration - EpicBook VPC](./screenshots/assignment-04/Screenshot%201.png)

*Figure 1: VPC "epicbook-vpc" successfully created with IPv4 CIDR block 10.0.0.0/16. Status: Available. DNS resolution enabled.*

---

#### Screenshot 2 — Subnets list showing both subnets and their CIDRs

![Subnets Configuration - Public and Private](./screenshots/assignment-04/Screenshot%202.png)

*Figure 2: Two subnets created in the epicbook-vpc:
- epicbook-public-subnet: 10.0.1.0/24 (Available)
- epicbook-private-subnet: 10.0.2.0/24 (Available)*

---

#### Screenshot 3 — Route table showing 0.0.0.0/0 → IGW and association with the public subnet

![Route Table Configuration - IGW Route](./screenshots/assignment-04/Screenshot%203.png)

*Figure 3: Route table "epicbook-rtb" configured with two routes:
- 0.0.0.0/0 → Internet Gateway (igw-0b13780d8a247d7a9) - Active
- 10.0.0.0/16 → local - Active
Associated with epicbook-public-subnet*

---

# Task 2 — Create Security Groups (EC2 + RDS) with Least Privilege

## Goal

Create `epicbook-ec2-sg` (SSH from your IP, HTTP/HTTPS public) and `epicbook-rds-sg` (MySQL 3306 only from `epicbook-ec2-sg`).

### Evidence

#### Screenshot 4 — EC2 security-group inbound rules showing ports and sources

![EC2 Security Group - Inbound Rules](./screenshots/assignment-04/Screenshot%204.png)

*Figure 4: epicbook-ec2-sg security group with 3 inbound rules:
- SSH (TCP 22): 0.0.0.0/0
- HTTP (TCP 80): 0.0.0.0/0
- HTTPS (TCP 443): 0.0.0.0/0*

---

#### Screenshot 5 — RDS security-group inbound rule showing MySQL 3306 allowed from the EC2 security group

![RDS Security Group - MySQL Inbound Rule](./screenshots/assignment-04/Screenshot%205.png)

*Figure 5: epicbook-rds-sg security group with 1 inbound rule:
- MySQL/Aurora (TCP 3306): Source sg-0457f651c920d59fd (epicbook-ec2-sg)*

---

# Task 3 — Launch Ubuntu EC2 in Public Subnet

## Goal

Launch an Ubuntu 20.04 instance in the public subnet with `epicbook-ec2-sg` attached, and connect to it over SSH.

### Evidence

#### Screenshot 6 — EC2 instance summary showing the public IPv4 address, subnet, and security group

![EC2 Instance Details - Network Configuration](./screenshots/assignment-04/Screenshot%206.png)

*Figure 6: EC2 instance "epicbook-Web-Server" (i-0e83924f9489f5594) successfully launched:
- Public IPv4 address: 54.208.29.58
- Instance state: Running
- Subnet: epicbook-public-subnet
- Security group: epicbook-ec2-sg
- Instance type: t3.micro*

---

#### Screenshot 7 — Terminal showing a successful SSH login

![SSH Connection - Ubuntu Instance Access](./screenshots/assignment-04/Screenshot%207.png)

*Figure 7: Successful SSH login to EC2 instance:
- Connected to ubuntu@ec2-54-208-29-58.compute-1.amazonaws.com
- Ubuntu 26.04 LTS running
- System information displayed with login successful*

---

# Task 4 — Install Required Software on EC2

## Goal

Install Node.js, npm, Nginx, and the MySQL client on the instance, and confirm Nginx is running.

### Evidence

#### Screenshot 8 — Output of `node -v` and `npm -v`

![Node.js and npm Installation - Versions Verified](./screenshots/assignment-04/Screenshot%208.png)

*Figure 8: Software versions verified on EC2 instance:
- node -v: v22.22.1
- npm -v: 9.2.0*

---

#### Screenshot 9 — Output of `systemctl status nginx`

![Nginx Web Server - Status Running](./screenshots/assignment-04/Screenshot%209.png)

*Figure 9: Nginx service status:
- Status: active (running)
- Main PID: 29409
- Loaded from /usr/lib/systemd/system/nginx.service
- Service started successfully as reverse proxy server*

---

#### Screenshot 10 — Output of `mysql --version`

![MySQL Client - Version Installed](./screenshots/assignment-04/Screenshot%2010.png)

*Figure 10: MySQL client version verified on EC2 instance:
- mysql --version: Ver 8.4.10-0ubuntu0.26.04.1 for Linux on x86_64 (Ubuntu)*

---

# Task 5 — Create RDS MySQL in Private Subnet (No Public Access)

## Goal

Create a private MySQL RDS instance in `epicbook-vpc` using a DB Subnet Group over the private subnet, with `epicbook-rds-sg` attached and public access disabled.

### Evidence

#### Screenshot 11 — RDS instance summary showing Publicly accessible: No

![RDS Instance - Private Database Configuration](./screenshots/assignment-04/Screenshot%2011.png)

*Figure 11: RDS MySQL instance "epicbook-db" summary:
- Status: Available
- Engine: MySQL Community
- Class: db.t4g.micro
- Publicly accessible: No (Private database in private subnet)
- Port: 3306*

---

#### Screenshot 12 — Connectivity & security section showing the VPC and attached security group

![RDS Connectivity & Security - Private Subnet Configuration](./screenshots/assignment-04/Screenshot%2012.png)

*Figure 12: RDS connectivity & security details:
- VPC: epicbook-vpc (vpc-02dfd21b0fe73ec48)
- Subnet group: epicbook-db-subnet-group (in private subnet)
- Security group: epicbook-rds-sg (sg-052cb7d99585a0755) - Active
- Internet access gateway: Disabled
- Endpoint: epicbook-db.cklo4cmkm5b6.us-east-1.rds.amazonaws.com*

---

# Task 6 — Initialize Database (SQL Dump Import)

## Goal

Connect to RDS from EC2, create the `epicbook` database, and import the provided SQL dump.

### Evidence

#### Screenshot 13 — Terminal showing successful `SHOW TABLES;` output with tables listed

![Database Initialization - Tables and Data Verified](./screenshots/assignment-04/Screenshot%2013.png)

*Figure 13: Successful database connection from EC2 to RDS and SQL dump import verification:
- Connected to epicbook-db.cklo4cmkm5b6.us-east-1.rds.amazonaws.com
- Database "bookstore" created and initialized
- SHOW TABLES output displays: Author, Book, Cart
- SELECT COUNT(*) queries confirm data import: 53 authors, 54 books*

---

# Task 7 — Deploy EpicBook Backend and Configure Environment Variables

## Goal

Clone the EpicBook repository, install backend dependencies, configure `.env` with the RDS endpoint and credentials, and start the backend on port 3000.

### Evidence

#### Screenshot 14 — Terminal showing the repository cloned and the `ls` output

![EpicBook Repository - Cloned and Ready for Deployment](./screenshots/assignment-04/Screenshot%2014.png)

*Figure 14: EpicBook repository successfully cloned to EC2 instance:
- Directory: ~/theepicbook
- Files present: Installation & Configuration Guide.md, README.md, config, db, models, node_modules, package.json, package-lock.json, public, routes, server.js, views
- Repository ready for backend deployment and environment configuration*

---

#### Screenshot 15 — Terminal showing the backend running, or `ss -tulpn` showing the port open

Add your screenshot here.

---

#### Screenshot 16 — `curl` output proving the backend responds

Add your screenshot here.

---

# Task 8 — Serve Frontend Using Nginx + Reverse Proxy to Backend

## Goal

Copy the frontend files to the Nginx web root and configure Nginx to reverse-proxy `/api/` to the Node backend.

### Evidence

#### Screenshot 17 — `nginx -t` success output

Add your screenshot here.

---

#### Screenshot 18 — Nginx configuration snippet showing the `/api/` reverse proxy

Add your screenshot here.

---

# Task 9 — End-to-End Testing (Frontend ↔ Backend ↔ RDS)

## Goal

Verify the frontend loads publicly, the backend responds through Nginx, and EC2 can query the private RDS database.

### Evidence

#### Screenshot 19 — Browser showing the EpicBook application loaded with the public IP visible

Add your screenshot here.

---

#### Screenshot 20 — Terminal showing a successful API call through the public endpoint

Add your screenshot here.

---

#### Screenshot 21 — Terminal showing a successful database connectivity test (`SELECT 1;` or similar)

Add your screenshot here.

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not expose PEM contents, passwords, `.env` values, or other secrets

---

# Completion Checklist

- [ ] Task 1: VPC, public/private subnets, IGW, and public routing created (Screenshots 1–3)
- [ ] Task 2: Least-privilege EC2 and RDS security groups created (Screenshots 4–5)
- [ ] Task 3: Ubuntu EC2 launched in the public subnet with SSH verified (Screenshots 6–7)
- [ ] Task 4: Node.js, npm, Nginx, and MySQL client installed (Screenshots 8–10)
- [ ] Task 5: Private MySQL RDS created with no public access (Screenshots 11–12)
- [ ] Task 6: Database initialized from the SQL dump (Screenshot 13)
- [ ] Task 7: Backend deployed and responding on port 3000 (Screenshots 14–16)
- [ ] Task 8: Nginx serving the frontend and reverse-proxying to the backend (Screenshots 17–18)
- [ ] Task 9: Frontend, backend, and RDS verified end to end (Screenshots 19–21)
- [ ] No sensitive data exposed

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
