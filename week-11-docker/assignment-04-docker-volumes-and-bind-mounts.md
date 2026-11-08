# Assignment 4 — Docker Volumes and Bind Mounts

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will use Docker Bind Mounts and Docker Volumes to persist logs and application data outside a container’s lifecycle. You will verify that data remains available after containers are removed and recreated.

---

**FLAG (renamed and restructured):** Your submitted file was `assignment-04-docker-volumes.md`. Upstream renamed it to `assignment-04-docker-volumes-and-bind-mounts.md` and split the tasks differently. Task 1 maps onto the new Task 1. Task 2 has no evidence in your snapshot, and its requirements have changed, so it's left as placeholders. Your old file should be removed once you confirm. I haven't deleted it yet.

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

**FLAG:** Confirm `0.0.0.0:80->80/tcp` is visible in this image. Your earlier screenshot shows `docker ps` for `myweb`.

---

#### Screenshot 4 — Nginx Welcome Page

![Task 1 - Screenshot 7](screenshots/assignment-04/task1/Screenshot%207.png)

**FLAG:** Upstream requires the VM public IP in the address bar and your full name as a caption directly below. Please confirm both are visible.

---

#### Screenshot 5 — Bind-Mounted Log Files

![Task 1 - Screenshot 8](screenshots/assignment-04/task1/Screenshot%208.png)

**FLAG (partial):** This shows `ls ~/nginx-logs`. Upstream also requires an access-log entry created when you opened the Nginx page. Please confirm it's visible or add it.

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

**FLAG:** Your snapshot has no Task 2 evidence (its folder is empty). The Task 2 requirements in upstream are also different from the earlier version. All screenshots below are placeholders.

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

**FLAG:** Not in your snapshot.

---

# Public Application URL

**Application URL:** `Add your VM public IP URL here`

**FLAG:** Not in your snapshot.

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

**FLAG:** Your earlier submission marked LinkedIn as optional and had no URL. The current upstream marks it as required.

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
- [ ] Custom Docker network created **(FLAG: Task 2, no evidence)**
- [ ] Docker Volume created **(FLAG: Task 2, no evidence)**
- [ ] Backend Dockerfile and image created **(FLAG: Task 2, no evidence)**
- [ ] Frontend Dockerfile and image created **(FLAG: Task 2, no evidence)**
- [ ] Both containers mount `shared-data` **(FLAG: Task 2, no evidence)**
- [ ] Backend writes data to the Docker Volume **(FLAG: Task 2, no evidence)**
- [ ] Frontend reads the same data from the Docker Volume **(FLAG: Task 2, no evidence)**
- [ ] Updated data appears after browser refresh **(FLAG: Task 2, no evidence)**
- [ ] Data remains after frontend and backend containers are removed and recreated **(FLAG: Task 2, no evidence)**
- [ ] All required screenshots included **(FLAG: 7 of 22 present)**
- [ ] Storage Persistence Notes completed **(FLAG: missing)**
- [ ] Public application URL included **(FLAG: missing)**
- [ ] LinkedIn post URL and screenshot included **(FLAG: missing)**
- [ ] Full name visible in terminal screenshots **(FLAG: confirm)**
- [ ] Browser screenshots have full-name captions **(FLAG: confirm)**
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

---

# Appendix — Earlier Submission Evidence (kept as written)

**Task 1 (extra, not mapped above):**
- `docker images`: ![](screenshots/assignment-04/task1/Screenshot%201.png)
- `docker search nginx`: ![](screenshots/assignment-04/task1/Screenshot%202.png)
- `docker run` with the Bind Mount: ![](screenshots/assignment-04/task1/Screenshot%205.png)
