# Assignment 3 — Docker Networking

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will explore Docker networking by using default bridge, custom bridge, multiple bridge, and host network modes. You will verify container communication, service discovery, network isolation, public access, and host networking on a Linux VM or EC2 instance.

---

**FLAG (task change):** Your submitted version used different container names and commands in Tasks 2 and 3 (`frontend`/`backend`/`mongo` with Node/Express and `curl`, instead of `web`/`client` and `wget`). Those tasks are marked below as placeholders where the requirements differ. Your earlier evidence is kept in the Appendix at the bottom so nothing is lost.

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

**FLAG:** Your image is a successful `docker pull nginx`. Upstream asks for `nginx:alpine`. Please confirm which tag was used.

---

#### Screenshot 3 — Running `myweb` Container

![Task 1 - Screenshot 5](screenshots/assignment-03/task%201/Screenshot%205.png)

---

#### Screenshot 4 — Nginx Welcome Page

![Task 1 - Screenshot 6](screenshots/assignment-03/task%201/Screenshot%206.png)

**FLAG:** Upstream requires the VM public IP in the address bar and your full name as a caption directly below. Please confirm both are visible.

---

# Task 2 — Connect Containers Using a Custom Bridge Network

## Goal

Create a custom bridge network and verify that containers can communicate using container names rather than IP addresses.

**FLAG:** Your earlier submission used `frontend` and `backend` (Node/Express) containers, not `web` and `client`. Those screenshots are kept in the Appendix. Placeholders below are for the current task.

### Evidence

#### Screenshot 5 — Custom Bridge Network Created

![Task 2 - Screenshot 2](screenshots/assignment-03/task%202/Screenshot%202.png)

**FLAG (partial):** Shows `docker network ls`. Please confirm `mynetwork` appears in it.

---

#### Screenshot 6 — Running `web` and `client` Containers

Add your screenshot here.

**FLAG:** Your containers are named `frontend` and `backend`. Not in the current spec.

---

#### Screenshot 7 — Service Discovery by Container Name

Add your screenshot here.

**FLAG:** Upstream requires `docker exec client wget -qO- http://web`. Your equivalent was `curl backend` from the frontend container (see Appendix).

---

#### Screenshot 8 — Custom Network Inspection

![Task 2 - Screenshot 7](screenshots/assignment-03/task%202/Screenshot%207.png)

**FLAG (partial):** Upstream expects `web` and `client` connected to `mynetwork`. Your image shows `frontend` and `backend`.

---

# Task 3 — Demonstrate Multi-Network Isolation

## Goal

Deploy frontend, backend, and database containers across two separate Docker networks. Verify allowed communication and confirm that the frontend cannot directly reach the database.

### Evidence

#### Screenshot 9 — Two Docker Networks

![Task 3 - Screenshot 1](screenshots/assignment-03/task3/Screenshot%201.png)

![Task 3 - Screenshot 2](screenshots/assignment-03/task3/Screenshot%202.png)

**FLAG (partial):** Your images show the network creation commands. Upstream asks for `docker network ls` showing both networks. Please confirm.

---

#### Screenshot 10 — Running Multi-Network Containers

![Task 3 - Screenshot 8](screenshots/assignment-03/task3/Screenshot%208.png)

**FLAG (partial):** Confirm `frontend` publishes `0.0.0.0:80->80/tcp` and `backend`/`db` publish no host port. Your containers are named `frontend`, `backend`, and `mongo`.

---

#### Screenshot 11 — Frontend Network Inspection

![Task 3 - Screenshot 10](screenshots/assignment-03/task3/Screenshot%2010.png)

---

#### Screenshot 12 — Backend Network Inspection

![Task 3 - Screenshot 9](screenshots/assignment-03/task3/Screenshot%209.png)

---

#### Screenshot 13 — Frontend-to-Backend Communication

Add your screenshot here.

**FLAG:** Upstream requires `docker exec frontend wget -qO- http://backend` returning the Nginx welcome page. Your equivalent was `curl api` from the frontend container (see Appendix).

---

#### Screenshot 14 — Backend-to-Database Communication

![Task 3 - Screenshot 13](screenshots/assignment-03/task3/Screenshot%2013.png)

**FLAG (partial):** Shows a `mongosh` connection to MongoDB on port 27017 (the `mongo` container, not `db`). Please confirm this satisfies the backend-to-database check.

---

#### Screenshot 15 — Frontend-to-Database Isolation

Add your screenshot here.

**FLAG:** Not in your snapshot.

---

#### Screenshot 16 — Public Frontend Access

![Task 3 - Screenshot 11](screenshots/assignment-03/task3/Screenshot%2011.png)

**FLAG:** Your frontend is a custom application, not the standard Nginx welcome page upstream asks for. Please confirm, and check the full-name caption.

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

**FLAG:** Upstream requires the VM public IP in the address bar and your full name as a caption below. Please confirm.

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

**FLAG:** Not in your snapshot.

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

**FLAG:** Your earlier submission marked LinkedIn as optional. The current upstream marks it as required. Your post is in place.

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

- [ ] Completed on a Linux VM or EC2 instance **(FLAG: confirm)**
- [x] Docker Engine is running
- [ ] HTTP port 80 is allowed in the VM firewall or cloud security rules **(FLAG: confirm)**
- [x] Default bridge networking verified
- [x] Custom bridge network created
- [ ] Container-name communication verified **(FLAG: different names and commands; see Task 2)**
- [x] `frontend-network` and `backend-network` created
- [ ] Frontend-to-backend communication verified **(FLAG: see Task 3, Screenshot 13)**
- [x] Backend-to-database communication verified **(FLAG: confirm with `db` naming)**
- [ ] Frontend-to-database isolation verified **(FLAG: Screenshot 15 missing)**
- [ ] Only the frontend published port 80 in Task 3 **(FLAG: confirm)**
- [x] Host network mode verified
- [ ] All required screenshots included **(FLAG: 14 of 20 mapped; see Appendix)**
- [ ] Networking Notes completed **(FLAG: missing)**
- [x] LinkedIn post URL and screenshot included
- [ ] Full name visible in terminal screenshots **(FLAG: confirm)**
- [ ] Browser screenshots include full-name captions **(FLAG: confirm)**
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

---

# Appendix — Earlier Submission Evidence (kept as written)

These screenshots are from your earlier version. They are kept so nothing is lost. Ones already used above are not repeated.

**Task 1 (extra):**
- `docker images` — `task 1/Screenshot 2.png`: ![](screenshots/assignment-03/task%201/Screenshot%202.png)
- `docker search nginx` — `task 1/Screenshot 3.png`: ![](screenshots/assignment-03/task%201/Screenshot%203.png)

**Task 2 (earlier Node/Express version):**
- Frontend Dockerfile: ![](screenshots/assignment-03/task%202/Screenshot%203.png)
- Frontend build: ![](screenshots/assignment-03/task%202/Screenshot%204.png)
- `docker ps` frontend: ![](screenshots/assignment-03/task%202/Screenshot%205.png)
- `docker ps` both containers: ![](screenshots/assignment-03/task%202/Screenshot%206.png)
- Public IP "Hello from Frontend": ![](screenshots/assignment-03/task%202/Screenshot%208.png)
- Backend curl from frontend: ![](screenshots/assignment-03/task%202/Screenshot%209.png)

**Task 3 (earlier MongoDB three-tier version):**
- Project folder structure: ![](screenshots/assignment-03/task3/Screenshot%203.png)
- Database Dockerfile: ![](screenshots/assignment-03/task3/Screenshot%204.png)
- Backend Dockerfile: ![](screenshots/assignment-03/task3/Screenshot%205.png)
- Frontend Dockerfile: ![](screenshots/assignment-03/task3/Screenshot%206.png)
- Image builds: ![](screenshots/assignment-03/task3/Screenshot%207A.png) ![](screenshots/assignment-03/task3/Screenshot%207B.png) ![](screenshots/assignment-03/task3/Screenshot%207C.png)
- Curl API from frontend: ![](screenshots/assignment-03/task3/Screenshot%2012.png)
- Document insertion: ![](screenshots/assignment-03/task3/Screenshot%2014.png)
- Document retrieval: ![](screenshots/assignment-03/task3/Screenshot%2015.png)

**Task 4 (extra):**
- `docker run --network host`: ![](screenshots/assignment-03/task4/Screenshot%201.png)
