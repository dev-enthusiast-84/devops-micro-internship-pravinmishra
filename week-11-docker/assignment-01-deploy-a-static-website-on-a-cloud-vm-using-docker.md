# Assignment 01 — Deploy a Static Website on a Cloud VM Using Docker

---

## Purpose

In this assignment, you will provision a cloud virtual machine on AWS or Azure, automate Docker installation using Cloud-Init, containerize a static website using Docker and Nginx, and deploy it so that it is accessible through the VM public IP address.

> Choose either AWS or Azure. You do not need to complete the assignment on both platforms.

---

# Task 1 — Provision the Cloud Virtual Machine

## Goal

Provision a Linux virtual machine with internet connectivity.

### Evidence

#### Screenshot 1 — Cloud VM Overview

![Screenshot 1](screenshots/assignment-01/Screenshot%201.png)

Expected to show: running VM, public IP address, Security Group or NSG inbound rules (SSH 22 from your IP, HTTP 80 from Anywhere).

---

# Task 2 — Configure Cloud-Init for Docker Installation

## Goal

Automatically install Docker during VM provisioning using Cloud-Init.

### Evidence

#### Screenshot 2 — Cloud-Init Docker Installation Log

![Screenshot 2](screenshots/assignment-01/Screenshot%202.png)

Expected to show the output of `cat /var/log/cloud-init-output.log` with Docker installation activity.

---

# Task 3 — Verify Docker Installation

## Goal

Verify that Docker was installed successfully and that the Docker daemon is running.

### Evidence

#### Screenshot 3 — Docker Version and Running Docker Daemon

![Screenshot 3](screenshots/assignment-01/Screenshot%203.png)

Expected to show `docker --version` and `docker ps`.

---

# Task 4 — Clone the Application Repository

## Goal

Download the static website source code.

### Evidence

#### Screenshot 4 — Application Project Files

![Screenshot 4](screenshots/assignment-01/Screenshot%204.png)

Expected to show the contents of the `Azure-Static-Website` project directory after cloning.

---

# Task 5 — Create a Dockerfile

## Goal

Containerize the static website using Nginx.

### Evidence

#### Screenshot 5 — Dockerfile Contents

![Screenshot 5](screenshots/assignment-01/Screenshot%205.png)

Expected to show `cat Dockerfile`, using `nginx:alpine`, copying the website files to the Nginx web root, and exposing port 80.

---

# Task 6 — Build the Docker Image

## Goal

Build a Docker image for the static website.

### Evidence

#### Screenshot 6 — Docker Image Verification

![Screenshot 6](screenshots/assignment-01/Screenshot%206.png)

Expected to show `docker images` including the `static-site` image with the `latest` tag.

---

# Task 7 — Deploy the Docker Container

## Goal

Run the containerized static website and map it to port 80 on the VM.

### Evidence

#### Screenshot 7 — Running Docker Container

![Screenshot 7](screenshots/assignment-01/Screenshot%207.png)

Expected to show `docker ps` with the running `static-site` container and the port mapping `0.0.0.0:80->80/tcp`.

---

# Task 8 — Verify the Deployment

## Goal

Verify that the static website is publicly accessible through the VM public IP address.

### Evidence

#### Screenshot 8 — VM Public IP Address

![Screenshot 8](screenshots/assignment-01/Screenshot%208.png)

Expected to show the output of `curl ifconfig.me`.

---

#### Screenshot 9 — Deployed Static Website

![Screenshot 9](screenshots/assignment-01/Screenshot%209.png)

Expected to show the deployed static website, with the VM public IP visible in the browser address bar.

---

# Public Application URL

**VM Public IP / Application URL:** `Add your application URL here`

---

# LinkedIn Requirement

## Goal

Create a LinkedIn post describing what you deployed, the deployment process, and key learning outcomes.

### Evidence

**LinkedIn Post URL:** https://www.linkedin.com/posts/maneetta-antony_docker-aws-ec2-share-7523972097316966400-rICe/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAF86Sz4BPT7sueDLOfQEmLqLbCo5V7ah-Jo

#### LinkedIn Post Screenshot

![Linkedin Post](screenshots/assignment-01/Linkedin%20Post.png)

---

# Submission Instructions

- Complete all tasks in sequence.
- Include all required screenshots.
- Ensure that your full name is visible in all required screenshots.
- Do not expose passwords, private keys, access keys, tokens, account IDs, or other sensitive information.
- Follow the Assignment Submission Guidelines.

---

# Completion Checklist

- [x] Cloud VM provisioned successfully
- [ ] Public IP enabled
- [ ] SSH port 22 restricted to my IP address
- [ ] HTTP port 80 enabled from Anywhere
- [x] Docker installed using Cloud-Init
- [x] Cloud-Init Docker installation log captured
- [x] Docker installation verified
- [x] Static website repository cloned
- [x] Dockerfile created and verified
- [x] Docker image built successfully
- [x] Docker container is running with port 80 mapped
- [x] Website is accessible through the VM public IP
- [ ] Public application URL included
- [x] LinkedIn post published
- [ ] All required screenshots included
- [x] No sensitive information exposed

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

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
