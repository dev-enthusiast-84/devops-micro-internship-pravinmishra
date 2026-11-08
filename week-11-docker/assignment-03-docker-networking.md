# Assignment 3 — Docker Networking

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will explore Docker networking by using default bridge, custom bridge, multiple bridge, and host network modes. You will verify container communication, service discovery, network isolation, public access, and host networking on a Linux VM or EC2 instance.

---

---

# Task 1 — Deploy a Standalone Application Using the Default Bridge Network

## Goal

Deploy an Nginx web server using Docker’s default bridge network and access it through the VM public IP address.

### Evidence

#### Screenshot 1 — Available Docker Networks

![Task 1 - Screenshot 1](screenshots/assignment-03/task%201/Screenshot%201.png)

The output must include the default `bridge`, `host`, and `none` networks.

---

#### Screenshot 2 — Nginx Image Pull

![Task 1 - Pull Nginx](screenshots/assignment-03/task%201/Screenshot%204.png)

---

#### Screenshot 3 — Running `myweb` Container

![Task 1 - Screenshot 5](screenshots/assignment-03/task%201/Screenshot%205.png)

---

#### Screenshot 4 — Nginx Welcome Page

![Task 1 - Screenshot 6](screenshots/assignment-03/task%201/Screenshot%206.png)

---

# Task 2 — Connect Containers Using a Custom Bridge Network

## Goal

Create a custom bridge network and verify that containers can communicate using container names rather than IP addresses.

### Evidence

#### Screenshot 5 — Custom Bridge Network Created

![Task 2 - Screenshot 2](screenshots/assignment-03/task%202/Screenshot%202.png)

---

#### Screenshot 6 — Running `web` and `client` Containers

Add your screenshot here.

---

#### Screenshot 7 — Service Discovery by Container Name

Add your screenshot here.

---

#### Screenshot 8 — Custom Network Inspection

![Task 2 - Screenshot 7](screenshots/assignment-03/task%202/Screenshot%207.png)

---

# Task 3 — Demonstrate Multi-Network Isolation

## Goal

Deploy frontend, backend, and database containers across two separate Docker networks. Verify allowed communication and confirm that the frontend cannot directly reach the database.

### Evidence

#### Screenshot 9 — Two Docker Networks

![Task 3 - Screenshot 1](screenshots/assignment-03/task3/Screenshot%201.png)

![Task 3 - Screenshot 2](screenshots/assignment-03/task3/Screenshot%202.png)

---

#### Screenshot 10 — Running Multi-Network Containers

![Task 3 - Screenshot 8](screenshots/assignment-03/task3/Screenshot%208.png)

---

#### Screenshot 11 — Frontend Network Inspection

![Task 3 - Screenshot 10](screenshots/assignment-03/task3/Screenshot%2010.png)

---

#### Screenshot 12 — Backend Network Inspection

![Task 3 - Screenshot 9](screenshots/assignment-03/task3/Screenshot%209.png)

---

#### Screenshot 13 — Frontend-to-Backend Communication

Add your screenshot here.

---

#### Screenshot 14 — Backend-to-Database Communication

![Task 3 - Screenshot 13](screenshots/assignment-03/task3/Screenshot%2013.png)

---

#### Screenshot 15 — Frontend-to-Database Isolation

Add your screenshot here.

---

#### Screenshot 16 — Public Frontend Access

![Task 3 - Screenshot 11](screenshots/assignment-03/task3/Screenshot%2011.png)

---

# Task 4 — Deploy an Application Using Docker Host Network Mode

## Goal

Run an Nginx container using Docker host network mode and compare it with bridge networking.

### Evidence

#### Screenshot 17 — Running Host-Networked Container

![Task 4 - Screenshot 2](screenshots/assignment-03/task4/Screenshot%202.png)

---

#### Screenshot 18 — Host Network Mode Verification

![Task 4 - Screenshot 4](screenshots/assignment-03/task4/Screenshot%204.png)

---

#### Screenshot 19 — Host-Networked Nginx Page

![Task 4 - Screenshot 3](screenshots/assignment-03/task4/Screenshot%203.png)

---

#### Screenshot 20 — Host-Networked Container Cleanup

![Task 4 - Screenshot 5](screenshots/assignment-03/task4/Screenshot%205.png)

---

# Networking Notes

Write a short note explaining:

- Default bridge networking
- Container-name communication on a custom bridge network
- Why the frontend could not access the database in Task 3
- The difference between bridge mode and host network mode

Write your note here.

---

# LinkedIn Requirement

## Goal

Create a LinkedIn post about the Docker networking modes explored, one key lesson about container isolation, and the learning outcomes from this assignment.

### Evidence

#### LinkedIn Post URL

https://www.linkedin.com/posts/maneetta-antony-452075387_docker-devops-cloudcomputing-share-7524751129222701056-xOel/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAF86Sz4BPT7sueDLOfQEmLqLbCo5V7ah-Jo

---

#### LinkedIn Post Screenshot

![Published LinkedIn post](screenshots/assignment-03/task4/linkedin%20post.png)

---

# Submission Instructions

- Complete all tasks in sequence.
- Include Screenshots 1–20 exactly as specified.
- Include the Networking Notes section.
- Include the LinkedIn post URL and screenshot.
- Ensure that your full name is visible in all terminal screenshots.
- Add your full name as a caption below each browser screenshot that shows the standard Nginx page.
- Do not expose private keys, passwords, access keys, tokens, account IDs, or other sensitive information.

---

# Completion Checklist

- [ ] Completed on a Linux VM or EC2 instance
- [x] Docker Engine is running
- [ ] HTTP port 80 is allowed in the VM firewall or cloud security rules
- [x] Default bridge networking verified
- [x] Custom bridge network created
- [ ] Container-name communication verified
- [x] `frontend-network` and `backend-network` created
- [ ] Frontend-to-backend communication verified
- [x] Backend-to-database communication verified
- [ ] Frontend-to-database isolation verified
- [ ] Only the frontend published port 80 in Task 3
- [x] Host network mode verified
- [ ] All required screenshots included
- [ ] Networking Notes completed
- [x] LinkedIn post URL and screenshot included
- [ ] Full name visible in terminal screenshots
- [ ] Browser screenshots include full-name captions
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

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
