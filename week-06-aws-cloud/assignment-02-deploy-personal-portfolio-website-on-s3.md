# Assignment 2 — Deploy Personal Portfolio Website on S3

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will deploy a static personal portfolio website quickly and reliably using Amazon S3 Static Website Hosting. You will download the portfolio template, create an S3 bucket, upload the static files, enable static website hosting, configure public read access, and validate the deployment through the S3 website endpoint.

---

# Task 1 — Download the Website Template Locally

## Goal

Download or clone the portfolio website template from GitHub and confirm `index.html` is present.

### Evidence

#### Screenshot 1 — File Explorer or terminal showing the template folder contents with `index.html` visible

![Portfolio Template Structure - VS Code Explorer](./screenshots/assignment-02/Screenshot%201.png)

*Figure 1: Portfolio template folder structure in VS Code showing index.html, style.css, privacy.html, README.md, and images/ folder. The index.html file is open showing the HTML structure with hero section and portfolio content.*

---

# Task 2 — Create an S3 Bucket for Website Hosting

## Goal

Create a globally unique S3 bucket in your chosen AWS region.

### Evidence

#### Screenshot 2 — S3 bucket created screen showing the bucket name and region

![S3 Bucket Created - Bucket Details](./screenshots/assignment-02/Screenshot%202.png)

*Figure 2: S3 bucket successfully created with name "pravin-portfolio-maneetta-use-east-1" in the US East 1 region. The bucket overview page shows the Objects tab is active.*

---

# Task 3 — Upload Website Files to the Bucket

## Goal

Upload the contents of the template folder (not the folder itself) so `index.html` sits at the bucket root.

### Evidence

#### Screenshot 3 — S3 bucket Objects view showing `index.html` at the root level

![S3 Objects View - Files Uploaded](./screenshots/assignment-02/Screenshot%203.png)

*Figure 3: S3 Objects view displaying 6 uploaded objects with index.html at the root level, along with privacy.html, README.md, style.css, terms.html, and an images/ folder containing the website assets.*

---

# Task 4 — Enable Static Website Hosting

## Goal

Enable S3 Static Website Hosting with `index.html` as the index document and `error.html` as the error document.

### Evidence

#### Screenshot 4 — Static website hosting enabled screen showing the website endpoint

![Static Website Hosting Enabled](./screenshots/assignment-02/Screenshot%204.png)

*Figure 4: S3 bucket Properties showing "Static website hosting" is Enabled with hosting type "Bucket hosting" and the bucket website endpoint: http://pravin-portfolio-maneetta-use-east-1.s3-website-us-east-1.amazonaws.com*

---

# Task 5 — Make the Website Public (Bucket Policy + Permissions)

## Goal

Adjust Block Public Access settings and save a bucket policy that grants public read access to the website objects.

### Evidence

#### Screenshot 5 — Bucket policy page showing the policy saved successfully, with the bucket name visible

![Bucket Policy - Public Read Access Configured](./screenshots/assignment-02/Screenshot%205.png)

*Figure 5: Bucket Permissions page showing the success message "Successfully edited bucket policy" with the bucket policy JSON visible. The policy grants PublicReadGetObject action on s3:pravin-portfolio-maneetta-use-east-1/* for public read access to all objects.*

---

# Task 6 — Verify Website Works (Public Endpoint Test)

## Goal

Load the site through the S3 website endpoint and confirm the homepage, images, and CSS load correctly.

### Evidence

#### Screenshot 6 — Browser showing the live website with the S3 website endpoint visible in the address bar

![Live Website - Portfolio Deployed on S3](./screenshots/assignment-02/Screenshot%206.png)

*Figure 6: Browser displaying the live portfolio website accessed via the S3 website endpoint (pravin-portfolio-maneetta-use-east-1.s3-website-us-east-1.amazonaws.com). The homepage shows the portfolio with navigation menu (Home, University, Blog, Book, Program, Contact), hero banner with profile image, and the DMI tagline.*

---

# Task 7 — (Optional) Update One Small Detail and Re-Upload

## Goal

Edit a small visible detail, re-upload it to S3, and confirm the change appears live.

### Evidence

#### Screenshot 7 (optional) — Before/after view, or a browser view showing the updated text

![Live Website - Updated with DevOps Highlight](./screenshots/assignment-02/Screenshot%207.png)

*Figure 7: Browser showing the updated portfolio website with a visible change - the word "DevOps" is now highlighted in green in the hero text "Start your DevOps Journey here". This demonstrates the successful re-upload and cache refresh of the updated content.*

---

# Submission Instructions

- Add all required screenshots in your submission
- Include the live S3 Website Endpoint URL
- Do not expose sensitive AWS account information

---

## Live Website URL

🌐 **S3 Website Endpoint:** http://pravin-portfolio-maneetta-use-east-1.s3-website-us-east-1.amazonaws.com

---

# Completion Checklist

- [x] Task 1: Template downloaded/cloned with `index.html` confirmed (Screenshot 1)
- [x] Task 2: Globally unique S3 bucket created (Screenshot 2)
- [x] Task 3: Website files uploaded with `index.html` at bucket root (Screenshot 3)
- [x] Task 4: Static website hosting enabled (Screenshot 4)
- [x] Task 5: Public-read bucket policy saved (Screenshot 5)
- [x] Task 6: Live website verified through the S3 website endpoint (Screenshot 6)
- [x] Task 7: Optional small update re-uploaded and verified (Screenshot 7)
- [x] S3 Website Endpoint URL included
- [x] No sensitive account information exposed

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
