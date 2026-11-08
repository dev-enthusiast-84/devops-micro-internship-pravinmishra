# Assignment 4 — Deploy EpicBook on Ubuntu VM + MySQL RDS with Secure Cloud Network

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will deploy the EpicBook web application in AWS using a secure two-tier architecture: an Ubuntu EC2 instance with Nginx in a public subnet, and a private MySQL RDS database with restricted security-group access. The completed deployment must prove that the frontend, backend, and private database communicate successfully end to end.

---

# Task 1 — Create VPC + Public/Private Subnets + Routing

## Goal

Create the AWS network for the EpicBook deployment with a public subnet for the EC2 instance and private subnets for Amazon RDS MySQL.

### Evidence

#### Screenshot 1 — VPC details showing CIDR 10.0.0.0/16

![VPC Configuration - EpicBook VPC](./screenshots/assignment-04/Screenshot%201.png)

*Figure 1: VPC "epicbook-vpc" successfully created with IPv4 CIDR block 10.0.0.0/16. Status: Available. DNS resolution enabled.*

---

#### Screenshot 2 — Subnets list showing the public subnet and both private database subnets with their CIDR ranges.

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

Create security groups that allow users to access the EpicBook application while restricting database access to the EC2 instance.

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

Launch the EC2 instance that will host the EpicBook application and connect to it through SSH.

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

#### Screenshot 7 — Terminal showing a successful SSH login with the `ubuntu@...` prompt

![SSH Connection - Ubuntu Instance Access](./screenshots/assignment-04/Screenshot%207.png)

*Figure 7: Successful SSH login to EC2 instance:
- Connected to ubuntu@ec2-54-208-29-58.compute-1.amazonaws.com
- Ubuntu 26.04 LTS running
- System information displayed with login successful*

---

# Task 4 — Install Required Software on EC2

## Goal

Prepare Ubuntu to download and run EpicBook, receive HTTP traffic, and connect to RDS.

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

Launch a private MySQL database accessible only from the application EC2 security group.

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

Download EpicBook and populate bookstore with the schema, authors, and books required by the application.

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

Run EpicBook on port 8080 and connect its Sequelize models to bookstore in RDS.

### Evidence

#### Screenshot 14 — Terminal showing the repository cloned and the `ls` output

![EpicBook Repository - Cloned and Ready for Deployment](./screenshots/assignment-04/Screenshot%2014.png)

*Figure 14: EpicBook repository successfully cloned to EC2 instance:
- Directory: ~/theepicbook
- Files present: Installation & Configuration Guide.md, README.md, config, db, models, node_modules, package.json, package-lock.json, public, routes, server.js, views
- Repository ready for backend deployment and environment configuration*

---

#### Screenshot 15 — Terminal showing EpicBook running, or ss -tulpn output showing port 8080 listening

![Backend Running - Node.js Server with Database Initialization](./screenshots/assignment-04/Screenshot%2015.png)

*Figure 15: EpicBook backend successfully running with Node.js:
- Command: npm run start
- MVC project initialized (mvc_project@201.0.0)
- Database tables created: Author, Book, Cart, Checkout, Cartbook
- SQL queries executed with InnoDB engine
- App listening on PORT 8080
- Environment variables injected from .env file (debug enabled)*

---

#### Screenshot 16 — `curl` output proving the backend responds; a 200, 301, or 404 response is acceptable if the service responds

![Backend API Response - curl Test Successful](./screenshots/assignment-04/Screenshot%2016.png)

*Figure 16: Backend API successfully responding to HTTP requests:
- Command: curl -I http://localhost:8080
- Response: HTTP/1.1 200 OK
- Server: Express (X-Powered-By header)
- Content-Type: text/html; charset=utf-8
- Content-Length: 24858 bytes
- Backend connectivity verified and responding correctly*

---

# Task 8 — Configure Nginx as a Reverse Proxy and Enable Persistence.

## Goal

Expose EpicBook through Nginx on port 80 and keep its Node.js process running after SSH disconnects and server reboots.

### Evidence

#### Screenshot 17 — `nginx -t` success output

![Nginx Configuration Test - Syntax Valid](./screenshots/assignment-04/Screenshot%2017.png)

*Figure 17: Nginx configuration validation successful:
- Command: sudo nginx -t
- Output: nginx: configuration file /etc/nginx/nginx.conf test is successful
- Configuration syntax verified and ready to serve
- Minor warning about conflicting server name (non-critical)*

---

#### Screenshot 18 — Nginx configuration snippet showing location / forwarding requests to http://127.0.0.1:8080

![Nginx Configuration - Reverse Proxy Setup](./screenshots/assignment-04/Screenshot%2018.png)

*Figure 18: Nginx configuration for reverse proxy:
- Server block listening on port 80
- Root directory: /var/www/html
- Frontend served from location /
- location / block uses try_files with index.html fallback
- Reverse proxy configured at location /api/
  - proxy_pass http://127.0.0.1:3000/
  - Proper headers forwarded: Host, X-Real-IP, X-Forwarded-For
  - HTTP version 1.1 for proxying*

---

# Task 9 — End-to-End Testing (Frontend ↔ Backend ↔ RDS)

## Goal

Verify the frontend loads publicly, the backend responds through Nginx, and EC2 can query the private RDS database.

### Evidence

#### Screenshot 19 — Browser showing the EpicBook application loaded with the public IP visible

![EpicBook Frontend - Live in Browser](./screenshots/assignment-04/Screenshot%2019.png)

*Figure 19: EpicBook frontend application successfully loaded and running in browser:
- URL: 54.208.29.58/api/ (public IP endpoint visible)
- Title: "The EpicBook!"
- Hero section displaying: "Discover Your Next Great Read"
- Book gallery showing multiple book covers (Behind the Corporate Curtain, The Little Astronaut, Journal)
- Responsive design with navigation and shopping cart
- All frontend assets loaded correctly*

---

#### Screenshot 20 — Terminal showing a successful HTTP request to EpicBook through the public Nginx endpoint using curl -i http://<EC2_PUBLIC_IP>/

![API Response - curl Test from Public Endpoint](./screenshots/assignment-04/Screenshot%2020.png)

*Figure 20: Successful API call through public endpoint via curl:
- Command: curl http://54.208.29.58/api/
- Response: Complete HTML page with proper structure
- Headers include Firebase, Materialize CSS, Font Awesome, jQuery libraries
- Navigation component with logo "The EpicBook"
- Body structure with blue-grey theme and responsive layout
- Demonstrates successful end-to-end communication through Nginx reverse proxy*

---

#### Screenshot 21 — Terminal showing a successful database connectivity test using `SELECT 1;` or similar

![Database Connectivity Test - Private RDS Access Verified](./screenshots/assignment-04/Screenshot%2021.png)

*Figure 21: Successful database connectivity test from EC2 to private RDS:
- Command: mysql -h epicbook-db.cklo4cmkm5b6.us-east-1.rds.amazonaws.com -u admin -p -e "SELECT 1;"
- Query result: Returns "1" confirming successful connection
- EC2 can successfully reach the private RDS database in the private subnet
- Complete end-to-end communication verified: Frontend → Nginx → Backend → Private RDS*

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post sharing your experience deploying EpicBook on an Ubuntu EC2 instance with Nginx and a private Amazon RDS MySQL database.

Include:
- A screenshot of the live EpicBook application showing the book listing
- Your application URL: `http://<EC2_PUBLIC_IP>/`
- A caption in your own words explaining what you deployed, how the main components connect, and what you learned about network security, Nginx, and PM2
- One challenge you faced and how you resolved it, if applicable

Include these hashtags:

#DMIByPravinMishra #AWS #DevOps #CloudComputing

Add the credit note for your track, replace `YOUR-GITHUB-USERNAME` with your GitHub username, and include the tags specified in the assignment guidelines.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`Add your URL here`

---

#### Screenshot 22 — Published LinkedIn post showing your caption, application URL, and live EpicBook screenshot

Add your screenshot here.

> **Privacy Note:** Do not expose passwords, private keys, `.env` values, database credentials, or sensitive AWS account information.

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not expose PEM contents, passwords, `.env` values, or other secrets

---

# Completion Checklist

- [x] Task 1: VPC, public/private subnets, IGW, and public routing created (Screenshots 1–3)
- [x] Task 2: Least-privilege EC2 and RDS security groups created (Screenshots 4–5)
- [x] Task 3: Ubuntu EC2 launched in the public subnet with SSH verified (Screenshots 6–7)
- [x] Task 4: Node.js, npm, Nginx, and MySQL client installed (Screenshots 8–10)
- [x] Task 5: Private MySQL RDS created with no public access (Screenshots 11–12)
- [x] Task 6: Database initialized from the SQL dump (Screenshot 13)
- [x] Task 7: Backend deployed and responding (Screenshots 14–16)
- [ ] Task 8: Nginx proxying requests to EpicBook, with PM2 persistence (Screenshots 17–18)
- [x] Task 9: Frontend, backend, and RDS verified end to end (Screenshots 19–21)
- [x] No sensitive data exposed
- [ ] LinkedIn post published

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
