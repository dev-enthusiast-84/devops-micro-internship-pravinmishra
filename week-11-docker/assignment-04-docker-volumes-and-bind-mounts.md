# Assignment 4 — Docker Volumes and Bind Mounts

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will use Docker Bind Mounts and Docker Volumes to persist logs and application data outside a container’s lifecycle. You will verify that data remains available after containers are removed and recreated.

---

---

# Task 1 — Persist Nginx Logs Using a Bind Mount

## Goal

Deploy an Nginx container with a Bind Mount and verify that its log files remain on the VM host after the container is removed.

### Evidence

#### Screenshot 1 — Nginx Image Pull

![Task 1 - Pull](screenshots/assignment-04/task1/Screenshot%203.png)

---

#### Screenshot 2 — Host Log Directory

![Task 1 - Screenshot 4](screenshots/assignment-04/task1/Screenshot%204.png)

---

#### Screenshot 3 — Running Nginx Container with Port Mapping

![Task 1 - Screenshot 6](screenshots/assignment-04/task1/Screenshot%206.png)

---

#### Screenshot 4 — Nginx Welcome Page

![Task 1 - Screenshot 7](screenshots/assignment-04/task1/Screenshot%207.png)

---

#### Screenshot 5 — Bind-Mounted Log Files

![Task 1 - Screenshot 8](screenshots/assignment-04/task1/Screenshot%208.png)

---

#### Screenshot 6 — Nginx Container Removed

![Task 1 - Screenshot 9](screenshots/assignment-04/task1/Screenshot%209.png)

---

#### Screenshot 7 — Logs Persist After Container Removal

![Task 1 - Screenshot 10](screenshots/assignment-04/task1/Screenshot%2010.png)

---

# Task 2 — Share Persistent Data Using a Docker Volume

## Goal

Deploy backend and frontend containers that share data through a named Docker Volume. Verify that the data remains after both containers are removed and recreated.

### Evidence

#### Screenshot 8 — Project File Structure

Add your screenshot here.

---

#### Screenshot 9 — Custom Docker Network

Add your screenshot here.

---

#### Screenshot 10 — Docker Volume

Add your screenshot here.

---

#### Screenshot 11 — Backend Dockerfile

Add your screenshot here.

---

#### Screenshot 12 — Backend Image Build

Add your screenshot here.

---

#### Screenshot 13 — Running Backend Container

Add your screenshot here.

---

#### Screenshot 14 — Frontend Dockerfile

Add your screenshot here.

---

#### Screenshot 15 — Frontend Image Build

Add your screenshot here.

---

#### Screenshot 16 — Running Backend and Frontend Containers

Add your screenshot here.

---

#### Screenshot 17 — Backend Write Operation

Add your screenshot here.

---

#### Screenshot 18 — Frontend Reads Shared Data

Add your screenshot here.

---

#### Screenshot 19 — First Shared-Data Update

Add your screenshot here.

---

#### Screenshot 20 — Second Shared-Data Update

Add your screenshot here.

---

#### Screenshot 21 — Container Removal and Recreation

Add your screenshot here.

---

#### Screenshot 22 — Data Persists After Recreation

Add your screenshot here.

---

# Storage Persistence Notes

Write a short explanation covering:

- The difference between a Bind Mount and a Docker Volume
- How Task 1 proved Bind Mount persistence
- How Task 2 proved Docker Volume persistence
- Why Docker Volumes are commonly used for application data

Write your explanation here.

---

# Public Application URL

**Application URL:** `Add your VM public IP URL here`

---

# LinkedIn Requirement

## Goal

Create a LinkedIn post about Docker Volumes and Bind Mounts, including one difference between them, how you verified persistent storage, and your key learning outcomes.

### Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`Add your URL here`

---

#### LinkedIn Post Screenshot

Add a screenshot of the published LinkedIn post here. Include a screenshot of the application displaying shared data.

---

# Submission Instructions

- Complete all tasks in sequence.
- Include Screenshots 1–22 exactly as specified.
- Include the Storage Persistence Notes section.
- Include the public application URL.
- Include the LinkedIn post URL and screenshot.
- Ensure that your full name is visible in all terminal screenshots.
- Add your full name as a clear caption below every browser screenshot.
- Do not expose private keys, passwords, access keys, tokens, account IDs, or other sensitive information.

---

# Completion Checklist

- [x] Nginx image pulled successfully
- [x] Host log directory created
- [x] Bind Mount configured successfully
- [x] Nginx logs remain after container removal
- [ ] Custom Docker network created
- [ ] Docker Volume created
- [ ] Backend Dockerfile and image created
- [ ] Frontend Dockerfile and image created
- [ ] Both containers mount `shared-data`
- [ ] Backend writes data to the Docker Volume
- [ ] Frontend reads the same data from the Docker Volume
- [ ] Updated data appears after browser refresh
- [ ] Data remains after frontend and backend containers are removed and recreated
- [ ] All required screenshots included
- [ ] Storage Persistence Notes completed
- [ ] Public application URL included
- [ ] LinkedIn post URL and screenshot included
- [ ] Full name visible in terminal screenshots
- [ ] Browser screenshots have full-name captions
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
