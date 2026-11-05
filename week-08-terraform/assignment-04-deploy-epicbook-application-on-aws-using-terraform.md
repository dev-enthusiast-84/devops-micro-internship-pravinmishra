# Assignment 4 — Deploy EpicBook Application on AWS Using Terraform

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will use Terraform to provision AWS network infrastructure (VPC, public/private subnets, Security Groups), launch an Ubuntu 22.04 EC2 instance, and provision a private Amazon RDS for MySQL instance. You will then deploy EpicBook, connect it to MySQL, and validate the complete user flow.

---

# Task 1 — Create Network Infrastructure with Terraform

## Goal

Define a VPC (10.0.0.0/16) with a public subnet (10.0.1.0/24) and private subnet (10.0.2.0/24), an Internet Gateway with public routing, an EC2 Security Group (SSH 22, HTTP 80), and an RDS Security Group (MySQL 3306 only from the EC2 Security Group).

### Evidence

#### Screenshot 1 — Terraform configuration showing the VPC and both subnet CIDR ranges

![Screenshot 1](screenshots/assignment-04/Screenshot%201.png)

---

#### Screenshot 2 — Terraform configuration showing the Internet Gateway, public route table, and both Security Groups

![Screenshot 2A](screenshots/assignment-04/Screenshot%202A.png)

![Screenshot 2B](screenshots/assignment-04/Screenshot%202B.png)

![Screenshot 2C](screenshots/assignment-04/Screenshot%202C.png)

---

# Task 2 — Provision EC2 Virtual Machine (Ubuntu 22.04)

## Goal

Use Terraform to launch a t2.micro Ubuntu 22.04 EC2 instance in the public subnet with a public IP, then install Node.js, npm, Git, Nginx, and MySQL client.

### Evidence

#### Screenshot 3 — Terraform apply output showing successful EC2 provisioning

![Screenshot 3](screenshots/assignment-04/Screenshot%203.png)

---

#### Screenshot 4 — EC2 instance running in the AWS Console with the public IP and subnet visible

![Screenshot 4](screenshots/assignment-04/Screenshot%204.png)

---

#### Screenshot 5 — Terminal showing successful SSH access and installed software

![Screenshot 5](screenshots/assignment-04/Screenshot%205.png)

---

# Task 3 — Deploy the EpicBook Application

## Goal

Deploy the EpicBook frontend and backend on the EC2 instance and configure Nginx to serve it, following the Installation, Configuration & Troubleshooting Guide.

### Evidence

#### Screenshot 6 — Terminal showing the EpicBook application files and dependency installation

![Screenshot 6](screenshots/assignment-04/Screenshot%206.png)

---

#### Screenshot 7 — Terminal showing the application and Nginx services running

![Screenshot 7](screenshots/assignment-04/Screenshot%207.png)

---

# Task 4 — Set Up Amazon RDS for MySQL with Terraform

## Goal

Provision a private Amazon RDS MySQL instance (db.t3.micro, Publicly accessible: false) restricted to the EC2 Security Group, then initialize the database using the provided SQL dump and connect the EpicBook backend to it.

### Evidence

#### Screenshot 8 — Terraform apply output showing successful RDS provisioning

![Screenshot 8](screenshots/assignment-04/Screenshot%208.png)

---

#### Screenshot 9 — RDS instance in the AWS Console showing the private network configuration and Publicly accessible: No

![Screenshot 9](screenshots/assignment-04/Screenshot%209.png)

---

#### Screenshot 10 — Terminal showing successful database initialization or table verification from EC2

![Screenshot 10](screenshots/assignment-04/Screenshot%2010.png)

---

# Task 5 — Test End-to-End Functionality

## Goal

Confirm EpicBook is accessible through the EC2 public IP and that navigation, cart, order summary, and checkout all work against the MySQL backend.

### Evidence

#### Screenshot 11 — Browser showing the EpicBook application through the EC2 public IP

![Screenshot 11](screenshots/assignment-04/Screenshot%2011.png)

---

#### Screenshot 12 — Browser showing a working product, cart, order summary, or checkout flow

![Screenshot 12](screenshots/assignment-04/Screenshot%2012.png)

---

### Notes

Write a short note describing any issue you faced, how you fixed it, and what you learned.

Issues, fixes, and what I learned

1. Terraform had no AWS credentials. The plan failed with "No valid credential sources found" and then tried the EC2 metadata service (169.254.169.254), which isn't reachable from a laptop. The aws provider block in main.tf doesn't set a profile, and AWS_PROFILE wasn't exported in the shell. Fix: export a named profile before running Terraform (export AWS_PROFILE=dmi-c3-g3-training). Learned: the provider reads credentials from the environment or a profile, and the metadata-service error only means no other source was found.

2. The Ubuntu AMI lookup returned no results. The aws_ami data source in modules/compute filtered on ubuntu/images/hvm-ssd-gp3/ubuntu-jammy-22.04-amd64-server-*. Canonical publishes Jammy under hvm-ssd in us-east-1, so the filter matched nothing. Fix: changed the default ami_name_filter to ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-* and confirmed with aws ec2 describe-images that it returns current images. Learned: hard-coding a name pattern is fragile. Check it against the real catalog with the CLI before running a plan, and keep the owner ID (Canonical, 099720109477) fixed.

3. The plan stopped on a missing sensitive variable. db_password has no default and must be at least 8 characters. Rather than store it in terraform.tfvars, the config expects TF_VAR_db_password in the environment. Learned: keep secrets out of committed files and pass them through environment variables or a secret store.

4. RDS took several minutes to create. The EC2 instance finished in about 13 seconds, while aws_db_instance.mysql was still creating after 2+ minutes. This is normal for a new MySQL instance (roughly 5 to 15 minutes, and Terraform allows up to 40). Learned: the database config was correct; the wait is expected, and a failed creation shows up as an error in Terraform or as a status in the RDS console.

5. Connecting to the instance over SSH. The key pair is epicbook-aws-key, and the public key comes from ~/.ssh/epicbook-aws-key.pub. The connection command is ssh -i ~/.ssh/epicbook-aws-key ubuntu@<ec2_public_ip>. The security group only allows SSH from ssh_allowed_cidr, so a changed IP will time out. Learned: match the private key to the registered public key, and remember that inbound access is limited to a single CIDR.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post about what you achieved in this assignment, with public or "Anyone" visibility.

## Evidence

#### LinkedIn Post URL

[LinkedIn post](https://www.linkedin.com/posts/maneetta-antony_terraform-aws-devops-share-7523605107209551873-00AX/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAAMKjaMBgVWqnGQ7U4godYqOh1rcqonVh74)

---

#### Screenshot 13 — Published LinkedIn post showing the text and at least one image or proof

![Screenshot 13](screenshots/assignment-04/LinkedIn%20Post.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Include the EC2 public IP
- Do not expose database passwords, private keys, or other secrets

---

# Completion Checklist

- [x] Task 1: VPC, subnets, IGW, and Security Groups created with Terraform (Screenshots 1–2)
- [x] Task 2: EC2 provisioned and required software installed (Screenshots 3–5)
- [x] Task 3: EpicBook deployed and Nginx serving the app (Screenshots 6–7)
- [x] Task 4: Private RDS MySQL created and database initialized (Screenshots 8–10)
- [x] Task 5: End-to-end functionality validated (Screenshots 11–12)
- [x] Issue/fix/learning note written (Notes)
- [x] LinkedIn post published and URL submitted (Screenshot 13)
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
