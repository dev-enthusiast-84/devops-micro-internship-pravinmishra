# Assignment 2 — Multi-Stage Docker Build for a React Application

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build both a single-stage and an optimized multi-stage Docker image for a React application, compare the resulting image sizes, and deploy the optimized version using a production-ready Nginx runtime container.

---

# Task 1 — Prepare the Project

## Goal

Clone `https://github.com/pravinmishraaws/my-react-app.git` and create a `.dockerignore` excluding `node_modules`, `build`, and `.env`.

### Evidence

#### Screenshot 1 — Contents of the `.dockerignore` file

![Screenshot 1](screenshots/assignment-02/Screenshot%201.png)

---

# Task 2 — Create a Single-Stage Docker Image

## Goal

Create `Dockerfile.single`, build `react-single`, and run it on port 3000.

### Evidence

#### Screenshot 2 — Contents of `Dockerfile.single`

![Screenshot 2](screenshots/assignment-02/Screenshot%202.png)

---

#### Screenshot 3 — Browser displaying the application running from the single-stage container

![Screenshot 3](screenshots/assignment-02/Screenshot%203.png)

---

# Task 3 — Create a Multi-Stage Docker Build

## Goal

Create a multi-stage Dockerfile with separate build and Nginx runtime stages, build `react-multistage`, and run it on port 80.

### Evidence

#### Screenshot 4 — Contents of the multi-stage Dockerfile

![Screenshot 4](screenshots/assignment-02/Screenshot%204.png)

---

#### Screenshot 5 — Browser displaying the application running from the multi-stage container

![Screenshot 5](screenshots/assignment-02/Screenshot%205.png)

---

# Task 4 — Compare Docker Image Sizes

## Goal

Compare the single-stage and multi-stage image sizes and calculate the percentage reduction.

### Evidence

#### Screenshot 6 — Docker image list showing both image sizes

![Screenshot 6](screenshots/assignment-02/Screenshot%206.png)

---

# Task 5 — Analyze the Optimization Results

## Goal

Write a 5–8 line analysis covering the percentage reduction, security benefits, reduced attack surface, faster distribution, and one build-caching optimization used.

### Evidence

#### Screenshot 7 — Analysis included in your submission document

![Screenshot 7](screenshots/assignment-02/Screenshot%207.png)

---

### Notes

Image Size Analysis: Single-Stage vs Multi-Stage

The single-stage image (react-app:single, built on node:20) is 2.17 GB (505 MB compressed). The multi-stage image (react-app:multistage, built with node:18-alpine and served by nginx:alpine) is 95.1 MB (26.6 MB compressed). That is about a 95.6% reduction in size (94.7% compressed). The smaller runtime image is more secure because it contains only Nginx and the static build/ output. Node.js, npm, node_modules, source code and build tools are left out, so there are fewer packages that could have known CVEs (security vulnerabilities). This shrinks the attack surface: an attacker has fewer binaries, shells and libraries to exploit, and no dev dependencies or source files can leak at runtime. The smaller size also speeds up deployment. Registry pushes and pulls move about 27 MB instead of about 505 MB, so new nodes start faster, scaling and rollbacks are quicker, and storage and bandwidth costs go down. For build caching, I copy package*.json and run npm ci before COPY . .. The dependency layer is then reused from cache unless the package files change, so source-only edits rebuild in seconds. A .dockerignore file that excludes node_modules, build and .git also keeps the build context small.

---

# Task 6 — Explore Additional Production Optimizations (Optional)

## Goal

Optionally configure an Nginx health check, cache headers, parameterized ports via environment variables, or a lighter runtime image, and compare results.

> Screenshot optional.

![Screenshot Optional](screenshots/assignment-02/Screenshot%20Optional.png)

---

# LinkedIn Post (Optional)

## Goal

Create a LinkedIn post describing what you built, what a multi-stage Docker build is, the image size reduction achieved, and key learnings.

## Evidence

#### LinkedIn Post URL

[LinkedIn post](https://www.linkedin.com/posts/maneetta-antony-452075387_docker-devops-react-share-7524014003975610369-8KmX/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAF86Sz4BPT7sueDLOfQEmLqLbCo5V7ah-Jo)

---

#### Screenshot — Published LinkedIn post

![LinkedIn post](screenshots/assignment-02/linkedin%20post.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- Do not expose sensitive information

---

# Completion Checklist

- [x] Task 1: `.dockerignore` created (Screenshot 1)
- [x] Task 2: Single-stage image built and verified (Screenshots 2–3)
- [x] Task 3: Multi-stage image built and verified (Screenshots 4–5)
- [x] Task 4: Image sizes compared (Screenshot 6)
- [x] Task 5: Analysis written (Screenshot 7 & Notes)
- [x] Task 6: Optional production optimizations explored
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
