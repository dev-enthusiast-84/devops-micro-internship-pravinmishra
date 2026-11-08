# Assignment 2 — Multi-Stage Docker Build for a React Application

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will build both a single-stage and an optimized multi-stage Docker image for a React application, compare the resulting image sizes, and deploy the optimized version using a production-ready Nginx runtime container.

Complete this assignment locally on your own computer where Docker is installed and running.

---

# Task 1 — Prepare the Project

## Goal

Prepare the React application for Docker image creation.

### Evidence

#### Screenshot 1 — Contents of the `.dockerignore` File

![Screenshot 1](screenshots/assignment-02/Screenshot%201.png)

The file must exclude `node_modules`, `build`, and `.env`.

---

# Task 2 — Create a Single-Stage Docker Image

## Goal

Create a baseline single-stage Docker image and run the application on port 3000.

### Evidence

#### Screenshot 2 — Contents of `Dockerfile.single`

![Screenshot 2](screenshots/assignment-02/Screenshot%202.png)

---

#### Screenshot 3 — Single-Stage Application in Browser

![Screenshot 3](screenshots/assignment-02/Screenshot%203.png)

Expected at `http://localhost:3000`, with your full name visible in the application.

---

# Task 3 — Create a Multi-Stage Docker Build

## Goal

Create an optimized multi-stage Docker image with separate builder and Nginx runtime stages, then run the application on port 80.

### Evidence

#### Screenshot 4 — Contents of the Multi-Stage Dockerfile

![Screenshot 4](screenshots/assignment-02/Screenshot%204.png)

---

#### Screenshot 5 — Multi-Stage Application in Browser

![Screenshot 5](screenshots/assignment-02/Screenshot%205.png)

Expected at `http://localhost`, with your full name visible in the application.

---

# Task 4 — Compare Docker Image Sizes

## Goal

Compare the single-stage and multi-stage image sizes and calculate the percentage reduction.

### Evidence

#### Screenshot 6 — Docker Image Size Comparison

![Screenshot 6](screenshots/assignment-02/Screenshot%206.png)

---

### Percentage Reduction Calculation

Record the image sizes and calculate the reduction using the same unit for both images.

```text
Single-stage image size: 2.17 GB (2,170 MB)

Multi-stage image size: 95.1 MB

Percentage reduction =
((Single-stage image size − Multi-stage image size)
÷ Single-stage image size) × 100

Percentage reduction: ((2,170 − 95.1) ÷ 2,170) × 100 ≈ 95.6%
```

*Note: the 2,170 MB conversion and the percentage were calculated from the sizes in your earlier notes (2.17 GB and 95.1 MB). Please confirm they match your `docker images` output.*

---

# Task 5 — Analyze the Optimization Results

## Goal

Evaluate the advantages of using multi-stage Docker builds.

### Notes

The single-stage image (react-app:single, built on node:20) is 2.17 GB (505 MB compressed). The multi-stage image (react-app:multistage, built with node:18-alpine and served by nginx:alpine) is 95.1 MB (26.6 MB compressed). That is about a 95.6% reduction in size (94.7% compressed). The smaller runtime image is more secure because it contains only Nginx and the static build/ output. Node.js, npm, node_modules, source code and build tools are left out, so there are fewer packages that could have known CVEs (security vulnerabilities). This shrinks the attack surface: an attacker has fewer binaries, shells and libraries to exploit, and no dev dependencies or source files can leak at runtime. The smaller size also speeds up deployment. Registry pushes and pulls move about 27 MB instead of about 505 MB, so new nodes start faster, scaling and rollbacks are quicker, and storage and bandwidth costs go down. For build caching, I copy package*.json and run npm ci before COPY . .. The dependency layer is then reused from cache unless the package files change, so source-only edits rebuild in seconds. A .dockerignore file that excludes node_modules, build and .git also keeps the build context small.

#### Screenshot 7 — Analysis included in your submission document (kept from your earlier submission)

![Screenshot 7](screenshots/assignment-02/Screenshot%207.png)

**FLAG:** Upstream does not list a Screenshot 7. The written analysis above is the required content. Keep this image or remove it, as you prefer.

---

# Task 6 — Explore Additional Production Optimizations (Optional)

## Goal

Explore one or more additional production optimization techniques.

### Optional Work

You may choose to:

- Configure an Nginx health check
- Configure cache headers for static assets
- Experiment with a lighter runtime image
- Compare the resulting image size with your original multi-stage image

Screenshots are optional.

![Screenshot Optional](screenshots/assignment-02/Screenshot%20Optional.png)

---

# LinkedIn Requirement

## Goal

Create a LinkedIn post describing what you built, what a multi-stage Docker build is, the image-size reduction achieved, and key learnings from the assignment.

### Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

https://www.linkedin.com/posts/maneetta-antony-452075387_docker-devops-react-share-7524014003975610369-8KmX/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAF86Sz4BPT7sueDLOfQEmLqLbCo5V7ah-Jo

---

#### LinkedIn Post Screenshot

![LinkedIn post](screenshots/assignment-02/linkedin%20post.png)

---

# Submission Instructions

- Complete all required tasks in sequence.
- Include Screenshots 1–6 exactly as specified.
- Include the percentage-reduction calculation and Task 5 analysis.
- Include the LinkedIn post URL and screenshot.
- Ensure that your full name is visible in all required screenshots.
- Do not expose passwords, keys, tokens, account IDs, or other sensitive information.

---

# Completion Checklist

- [x] Assignment completed locally
- [x] `.dockerignore` created and verified (Screenshot 1)
- [x] `Dockerfile.single` created (Screenshot 2)
- [x] Single-stage container verified in the browser (Screenshot 3)
- [x] Multi-stage `Dockerfile` created (Screenshot 4)
- [x] Multi-stage container verified in the browser (Screenshot 5)
- [x] Both Docker image sizes captured (Screenshot 6)
- [x] Percentage reduction calculated
- [x] Optimization analysis completed
- [x] LinkedIn post URL and screenshot included
- [ ] Full name visible in all required screenshots **(FLAG: confirm for Screenshots 1–6)**
- [x] No sensitive information exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory), focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations through hands-on experience.

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
