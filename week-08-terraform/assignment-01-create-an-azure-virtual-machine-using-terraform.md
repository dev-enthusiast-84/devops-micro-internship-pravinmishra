# Assignment 1 — Create an Azure Virtual Machine using Terraform

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will use Terraform to provision a complete Azure Virtual Machine environment: a resource group, virtual network, subnet, public IP, network interface, and an Ubuntu 18.04 Linux VM. You will initialize, plan, and apply the configuration, verify the running VM via Azure CLI, and destroy the resources after testing.

---

# Task 1 — Create a New Terraform Project and Define the Infrastructure

## Goal

Create a `terraform-azure-vm` project and define the resource group, virtual network, subnet, public IP, network interface, and Ubuntu 18.04 VM (with username/password authentication and a public IP output) in `main.tf`.

### Evidence

#### Screenshot 1 — VS Code showing `main.tf` and the required Azure resources

![Screenshot 1](screenshots/assignment-01/Screenshot%201.png)

---

#### Screenshot 2 — `main.tf` showing the public IP output and VM authentication configuration, with the password hidden or redacted

![Screenshot 2](screenshots/assignment-01/Screenshot%202.png)

> **Note (deviation from spec):** The assignment asks for an Ubuntu 18.04 VM. This deployment uses Ubuntu 22.04 LTS (`Canonical` / `0001-com-ubuntu-server-jammy` / `22_04-lts`) instead. The 18.04 image (`Canonical:UbuntuServer:18_04-lts-gen2`) was confirmed available in East US, but the VM was kept on 22.04 for this submission.

---

# Task 2 — Initialize Terraform

## Goal

Run `terraform init` and confirm the working directory initializes successfully.

### Evidence

#### Screenshot 3 — Terminal showing successful `terraform init` output

![Screenshot 3](screenshots/assignment-01/Screenshot%203.png)

---

# Task 3 — Plan and Apply the Configuration

## Goal

Review `terraform plan`, run `terraform apply`, and record the VM's public IP from the Terraform output.

### Evidence

#### Screenshot 4 — Terraform plan summary showing the proposed resources

![Screenshot 4](screenshots/assignment-01/Screenshot%204.png)

---

#### Screenshot 5 — Terraform apply output showing successful completion

![Screenshot 5](screenshots/assignment-01/Screenshot%205.png)

---

#### Screenshot 6 — Terraform output showing the public IP of the VM

![Screenshot 6](screenshots/assignment-01/Screenshot%206.png)

---

# Task 4 — Verify the Deployment

## Goal

Use Azure CLI to confirm the VM was created and is running.

### Evidence

#### Screenshot 7 — Azure CLI output showing the VM name and running status

![Screenshot 7](screenshots/assignment-01/Screenshot%207.png)

---

# Task 5 — Destroy the Resources

## Goal

Run `terraform destroy` to clean up the Azure resources after testing.

### Evidence

#### Screenshot 8 — Terminal showing successful `terraform destroy` completion

![Screenshot 8](screenshots/assignment-01/Screenshot%208.png)

---

### Notes

Write a short paragraph explaining what you learned or any issues you encountered.

Three Azure errors blocked the deployment, and each had a different fix. The first two were unregistered resource providers: Microsoft.Network and Microsoft.Compute were not registered on the subscription, so Terraform got MissingSubscriptionRegistration errors. Registering each provider with az provider register --wait fixed them. The third was a capacity restriction: Standard_B1s was not available in East US, so I checked which sizes were unrestricted and priced the options before switching to Standard_D2as_v4. Along the way, I found a hardcoded VM password in main.tf. I moved it to a sensitive variable, added a .gitignore to keep state files and secrets out of git, and wrote a README covering setup and teardown. The main lesson is that Azure fails at apply time, not plan time, so provider registration and regional capacity both need checking before you run terraform apply.

---

# Submission Instructions

- Add all required screenshots in your submission
- Include the VM public IP from the Terraform output
- Do not expose Azure credentials, subscription details, or passwords

---

# Completion Checklist

- [x] Task 1: `terraform-azure-vm` project created with all required resources defined (Screenshots 1–2)
- [x] Task 2: `terraform init` completed successfully (Screenshot 3)
- [x] Task 3: Plan reviewed and `terraform apply` completed, public IP recorded (Screenshots 4–6)
- [x] Task 4: VM verified as running via Azure CLI (Screenshot 7)
- [x] Task 5: `terraform destroy` completed successfully (Screenshot 8)
- [x] Learning/issues paragraph written (Notes)
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
