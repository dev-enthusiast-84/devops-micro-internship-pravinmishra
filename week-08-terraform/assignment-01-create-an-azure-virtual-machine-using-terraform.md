# Assignment 1 — Create an Azure Virtual Machine using Terraform

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will use Terraform to provision a complete Azure Virtual Machine environment, including a resource group, virtual network, subnet, public IP, network interface, and a Linux-based virtual machine. You will set up and verify the required local tools, define the infrastructure in Terraform, initialize the project, review and apply the plan, verify the running VM through Azure CLI, capture the public IP output, and destroy the resources after testing.

---

# Task 0 — Set Up and Verify the Terraform and Azure CLI Environment

## Goal

Prepare your local environment for Terraform deployment by installing Terraform, Azure CLI, and the HashiCorp Terraform extension in VS Code, signing in to your Azure account, and confirming that all required tools are working correctly.

### Evidence

#### Screenshot 1 — Terminal showing successful `terraform version` output

Add your screenshot here.

---

#### Screenshot 2 — Terminal showing successful `az version` output

Add your screenshot here.

---

#### Screenshot 3 — VS Code Extensions panel showing the HashiCorp Terraform extension installed and enabled

Add your screenshot here.

---

# Task 1 — Create a New Terraform Project and Define the Infrastructure

## Goal

Create a new Terraform project and define the complete Azure Virtual Machine environment in `main.tf` by using the official Terraform Registry documentation.

### Evidence

#### Screenshot 4 — VS Code showing the AzureRM provider configuration and resource group configuration in `main.tf`

![Screenshot 1](screenshots/assignment-01/Screenshot%201.png)

---

#### Screenshot 5 — VS Code showing the Linux virtual machine configuration and public IP `output` block in `main.tf`. Ensure that the VM password is hidden or redacted

![Screenshot 2](screenshots/assignment-01/Screenshot%202.png)

> **Note (deviation from spec):** The earlier version of this assignment asked for an Ubuntu 18.04 VM. This deployment uses Ubuntu 22.04 LTS (`Canonical` / `0001-com-ubuntu-server-jammy` / `22_04-lts`) instead. The 18.04 image (`Canonical:UbuntuServer:18_04-lts-gen2`) was confirmed available in East US, but the VM was kept on 22.04 for this submission.

---

# Task 2 — Initialize Terraform

## Goal

Initialize the Terraform working directory and download the required provider components.

### Evidence

#### Screenshot 6 — Terminal showing the successful `terraform init` output

![Screenshot 3](screenshots/assignment-01/Screenshot%203.png)

---

# Task 3 — Plan and Apply the Configuration

## Goal

Review the Terraform execution plan and provision the Azure resources.

### Evidence

#### Screenshot 7 — Terraform plan summary showing the proposed resources

![Screenshot 4](screenshots/assignment-01/Screenshot%204.png)

---

#### Screenshot 8 — Terraform apply output showing successful completion

![Screenshot 5](screenshots/assignment-01/Screenshot%205.png)

---

#### Screenshot 9 — Terraform output showing the public IP address of the VM

![Screenshot 6](screenshots/assignment-01/Screenshot%206.png)

### Question

VM Public IP Address: [Enter the public IP shown by terraform output]

---

# Task 4 — Verify the Deployment

## Goal

Confirm through Azure CLI that the virtual machine was created successfully and is currently running.

### Evidence

#### Screenshot 10 — Azure CLI output showing the deployed VM name and `VM running` status

![Screenshot 7](screenshots/assignment-01/Screenshot%207.png)

---

# Task 5 — Destroy the Resources

## Goal

Remove all Azure resources created by Terraform after completing the deployment and verification.

### Evidence

#### Screenshot 11 — Terminal showing successful `terraform destroy` completion

![Screenshot 8](screenshots/assignment-01/Screenshot%208.png)

---

### Notes

Write a short paragraph explaining what you learned or any issues you encountered.

Three Azure errors blocked the deployment, and each had a different fix. The first two were unregistered resource providers: Microsoft.Network and Microsoft.Compute were not registered on the subscription, so Terraform got MissingSubscriptionRegistration errors. Registering each provider with az provider register --wait fixed them. The third was a capacity restriction: Standard_B1s was not available in East US, so I checked which sizes were unrestricted and priced the options before switching to Standard_D2as_v4. Along the way, I found a hardcoded VM password in main.tf. I moved it to a sensitive variable, added a .gitignore to keep state files and secrets out of git, and wrote a README covering setup and teardown. The main lesson is that Azure fails at apply time, not plan time, so provider registration and regional capacity both need checking before you run terraform apply.

---

# Task 6 — Share Your Terraform Progress on WhatsApp

## Goal

Share your Terraform deployment progress on WhatsApp by using Screenshot 8, the provided Terraform caption, and your generated DMI Leaderboard progress link.

### Evidence

#### Screenshot 12 — Published WhatsApp Status showing your Terraform deployment progress and DMI Leaderboard progress link

Add your screenshot here.

> Ensure that no passwords, account IDs, subscription IDs, private phone numbers, or personal messages are visible.

---

# Submission Instructions

- Complete all tasks in sequence and include all required screenshots specified in Tasks 0–6.
- Do not expose passwords, keys, account IDs, or other sensitive information in screenshots.

---

# Completion Checklist

- [ ] Installed Terraform and verified it using `terraform version` (Task 0)
- [ ] Installed Azure CLI and verified it using `az version` (Task 0)
- [ ] Signed in to Azure using `az login` (Task 0)
- [ ] Confirmed the correct Azure subscription (Task 0)
- [ ] Installed and enabled the HashiCorp Terraform extension in VS Code (Task 0)
- [x] Created the `terraform-azure-vm` project directory and `main.tf`
- [x] Added the Terraform and AzureRM provider configuration
- [x] Defined the resource group, virtual network, subnet, public IP, and network interface
- [x] Defined the Linux virtual machine with username and password-based authentication
- [x] Added the Terraform output for the VM public IP address
- [x] Completed `terraform init` successfully
- [x] Reviewed the Terraform execution plan using `terraform plan`
- [x] Completed `terraform apply` successfully
- [x] Captured and recorded the VM public IP using `terraform output`
- [x] Verified that the VM is running using Azure CLI
- [x] Completed `terraform destroy` successfully
- [ ] Shared Terraform deployment progress on WhatsApp by following Task 6
- [ ] Captured a screenshot of the published WhatsApp Status
- [x] Captured all required screenshots for Tasks 1–5
- [x] Checked that no passwords, keys, account IDs, or other sensitive information are visible in the screenshots

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
