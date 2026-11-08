# Assignment 4 — Deploy EpicBook Web App on AWS Using Terraform Modules and Amazon RDS for MySQL

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will use reusable Terraform modules to provision the AWS infrastructure required by EpicBook. You will create a custom VPC, one public subnet, two private database subnets across different Availability Zones, an Internet Gateway, a public route table, Security Groups, an EC2 Linux instance, and a private Amazon RDS for MySQL database.

You will use EC2 `user_data` to install the required software, initialize the EpicBook database, connect the application to Amazon RDS, configure Nginx, validate the complete browser-to-database workflow, destroy the resources after testing, and publish the required LinkedIn post.

---

**FLAG (task change — please read first):** Your submitted version was a single flat Terraform configuration (13 screenshots, manual setup over SSH). The current upstream task requires a **modular** project (root + `network`, `ec2`, `rds` modules, `user_data.sh`) with 35 screenshots. None of your earlier screenshots map cleanly onto the modular slots, so every upstream slot below is left as a placeholder. Your earlier evidence and notes are kept in the **Appendix** at the bottom. Let's decide whether to rebuild in modular form or keep the flat version.

---

# Task 0 — Set Up and Verify the Terraform and AWS CLI Environment

## Goal

Prepare your local environment by installing Terraform, AWS CLI, and the HashiCorp Terraform extension in VS Code, configuring AWS CLI, and confirming that all required tools are working correctly.

## Evidence

### Screenshot 1 — Terraform Version

Add a screenshot of the terminal showing successful `terraform version` output.

Add your screenshot here.

---

### Screenshot 2 — AWS CLI Version

Add a screenshot of the terminal showing successful `aws --version` output.

Add your screenshot here.

---

### Screenshot 3 — HashiCorp Terraform Extension

Add a screenshot of VS Code showing the HashiCorp Terraform extension installed and enabled.

Add your screenshot here.

---

# Task 1 — Create the Modular Terraform Project

## Goal

Create the Terraform project and organize the AWS infrastructure into separate Network, EC2, and RDS modules.

The completed project structure must include the root Terraform files, all three module directories, and `user_data.sh`:

```text
terraform-aws-epicbook/
├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
└── modules/
    ├── network/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    ├── ec2/
    │   ├── main.tf
    │   ├── variables.tf
    │   ├── outputs.tf
    │   └── user_data.sh
    └── rds/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

## Evidence

### Screenshot 4 — Modular Project Structure

Add a screenshot of the VS Code Explorer showing the complete root project and the `network`, `ec2`, and `rds` module directory structure.

Add your screenshot here.

---

# Task 2 — Build the Network Module

## Goal

Create a reusable Terraform network module containing the VPC, subnets, Internet Gateway, routing, and Security Groups required by EpicBook.

The network module must include:

- VPC: `10.0.0.0/16`
- Public subnet: `10.0.1.0/24`
- Private database subnet A: `10.0.2.0/24`
- Private database subnet B: `10.0.3.0/24`
- Private database subnets in different Availability Zones
- Internet Gateway
- Public route table and public-subnet association
- EC2 Security Group allowing SSH and HTTP
- RDS Security Group allowing MySQL port `3306` only from the EC2 Security Group
- Network module variables and outputs

## Evidence

### Screenshot 5 — VPC and Subnets

Add a screenshot of VS Code showing the VPC, public subnet, and two private database subnet configurations.

Add your screenshot here.

---

### Screenshot 6 — Internet Gateway and Public Routing

Add a screenshot of VS Code showing the Internet Gateway, public route table, and route table association.

Add your screenshot here.

---

### Screenshot 7 — EC2 and RDS Security Groups

Add a screenshot of VS Code showing the EC2 and RDS Security Groups, including MySQL access from the EC2 Security Group only.

Add your screenshot here.

---

### Screenshot 8 — Network Module Outputs

Add a screenshot of VS Code showing the network module outputs.

Add your screenshot here.

---

# Task 3 — Build the EC2 Module and User Data Installation Script

## Goal

Create an EC2 module that launches the EpicBook application server inside the public subnet and automatically installs the required server software using EC2 user data.

The EC2 module must:

- Deploy the instance inside the public subnet
- Use the EC2 Security Group from the Network module
- Use a supported Ubuntu LTS AMI
- Assign a public IPv4 address
- Use an EC2 key pair for SSH authentication
- Connect `user_data.sh` through the EC2 `user_data` argument
- Expose the EC2 instance ID and public IP

The `user_data.sh` script must install the required software without storing database credentials or other secrets.

## Evidence

### Screenshot 9 — EC2 Resource and `user_data`

Add a screenshot of VS Code showing the EC2 resource and `user_data` configuration.

Add your screenshot here.

---

### Screenshot 10 — `user_data.sh`

Add a screenshot of VS Code showing `user_data.sh`.

Ensure that no credentials, passwords, private keys, access tokens, or application secrets are visible.

Add your screenshot here.

---

### Screenshot 11 — EC2 Module Variables and Outputs

Add a screenshot of VS Code showing the EC2 module variables and outputs.

Add your screenshot here.

---

# Task 4 — Build the Amazon RDS Module

## Goal

Create an RDS module that provisions Amazon RDS for MySQL inside private database subnets.

The RDS module must include:

- A DB subnet group using both private database subnets
- Amazon RDS for MySQL
- RDS Security Group reference
- `publicly_accessible = false`
- Sensitive variables for database credentials
- RDS endpoint output
- No password output

## Evidence

### Screenshot 12 — DB Subnet Group and RDS MySQL

Add a screenshot of VS Code showing the DB subnet group and RDS MySQL configuration.

Add your screenshot here.

---

### Screenshot 13 — Private RDS and Sensitive Variables

Add a screenshot of VS Code showing `publicly_accessible = false`, the RDS Security Group configuration or reference, and the sensitive database variable configuration.

Ensure that the database password and other sensitive values are hidden.

Add your screenshot here.

---

### Screenshot 14 — RDS Endpoint Output

Add a screenshot of VS Code showing the RDS endpoint output.

Add your screenshot here.

---

# Task 5 — Connect the Terraform Modules from the Root Module

## Goal

Use the root Terraform configuration to call the Network, EC2, and RDS modules and pass values between them.

## Evidence

### Screenshot 15 — Root Module Blocks

Add a screenshot of VS Code showing the root `main.tf` with the Network, EC2, and RDS module blocks.

Add your screenshot here.

---

### Screenshot 16 — Values Passed Between Modules

Add a screenshot of VS Code showing values passed from the Network module to the EC2 and RDS modules.

Add your screenshot here.

---

### Screenshot 17 — Root Outputs

Add a screenshot of VS Code showing the root EC2 public IP and RDS endpoint outputs.

Add your screenshot here.

---

# Task 6 — Initialize, Validate, Plan, and Apply the Terraform Configuration

## Goal

Initialize the modular Terraform project, validate the configuration, review the execution plan, and provision the AWS infrastructure.

## Evidence

### Screenshot 18 — Terraform Initialization

Add a screenshot of the terminal showing successful `terraform init` output.

Add your screenshot here.

---

### Screenshot 19 — Terraform Validation

Add a screenshot of the terminal showing successful `terraform validate` output.

Add your screenshot here.

---

### Screenshot 20 — Terraform Plan

Add a screenshot showing the Terraform plan summary and proposed resources.

Add your screenshot here.

---

### Screenshot 21 — Terraform Apply

Add a screenshot showing successful `terraform apply` completion.

Add your screenshot here.

---

### Screenshot 22 — Terraform Outputs

Add a screenshot showing the EC2 public IP and RDS endpoint returned by `terraform output`.

Add your screenshot here.

---

# Task 7 — Verify EC2, User Data, and Amazon RDS

## Goal

Verify that the EC2 and RDS resources were successfully provisioned and confirm that the EC2 user data script installed the required software.

## Evidence

### Screenshot 23 — EC2 Running

Add a screenshot of AWS CLI showing the EC2 instance running.

Add your screenshot here.

---

### Screenshot 24 — Private RDS Available

Add a screenshot of AWS CLI showing that RDS is available and not publicly accessible.

Add your screenshot here.

---

### Screenshot 25 — Installed Software and Nginx

Add a screenshot of the EC2 terminal showing the required software version checks and the active Nginx service.

Add your screenshot here.

---

# Task 8 — Prepare the EpicBook Database

## Goal

Connect from EC2 to Amazon RDS, create the EpicBook database, import the schema and seed data, and verify the database contents.

## Evidence

### Screenshot 26 — EC2-to-RDS Connection

Add a screenshot of the terminal showing a successful connection from EC2 to Amazon RDS.

Ensure that the database password is not visible.

Add your screenshot here.

---

### Screenshot 27 — EpicBook Tables and Imported Data

Add a screenshot of the terminal showing the EpicBook tables and imported data.

Add your screenshot here.

---

# Task 9 — Deploy and Configure the EpicBook Application

## Goal

Install EpicBook dependencies, configure the application to use Amazon RDS, configure Nginx as a reverse proxy, and start the application.

## Evidence

### Screenshot 28 — Dependencies and `node_modules`

Add a screenshot of the terminal showing successful dependency installation and the `node_modules` directory.

Add your screenshot here.

---

### Screenshot 29 — Nginx Configuration and Service

Add a screenshot of the terminal showing a successful Nginx configuration test and active service status.

Add your screenshot here.

---

### Screenshot 30 — EpicBook on Port `8080`

Add a screenshot of the terminal showing EpicBook running or listening on port `8080`.

Add your screenshot here.

---

# Task 10 — Test End-to-End Functionality

## Goal

Verify that EpicBook, EC2, Nginx, and Amazon RDS work together successfully.

## EC2 Public IP URL

**EC2 Public IP URL:** Add the working EpicBook EC2 public IP URL here

## Evidence

### Screenshot 31 — EpicBook Through the EC2 Public IP

Add a screenshot of the browser showing EpicBook using the EC2 public IP.

Add your screenshot here.

---

### Screenshot 32 — Cart or Checkout Action

Add a screenshot of the browser showing a successful cart or checkout action.

Add your screenshot here.

---

### Screenshot 33 — Corresponding RDS Record

Add a screenshot of the terminal showing the corresponding RDS database record created by the application action.

Ensure that database credentials and other sensitive values are not visible.

Add your screenshot here.

---

# Task 11 — Destroy the Terraform Infrastructure

## Goal

Remove all AWS resources created by the modular Terraform configuration.

## Evidence

### Screenshot 34 — Terraform Destroy

Add a screenshot of the terminal showing successful `terraform destroy` completion.

Add your screenshot here.

---

# Task 12 — LinkedIn Post (Mandatory)

## Goal

Share what you built and learned from the modular AWS Terraform deployment.

Write the post in your own words and include at least one deployment screenshot or other proof. Ensure that the post can be viewed by the submission reviewer.

## Evidence

### Screenshot 35 — Published LinkedIn Post

Add a screenshot of the published LinkedIn post showing the post and at least one deployment image or other proof.

![Screenshot 13 - LinkedIn Post](screenshots/assignment-04/LinkedIn%20Post.png)

**FLAG:** Your earlier LinkedIn post screenshot is kept here. Upstream asks for a post about the *modular* deployment, so please confirm whether this post satisfies Task 12.

## LinkedIn Post URL

**LinkedIn Post URL:** https://www.linkedin.com/posts/maneetta-antony_terraform-aws-devops-share-7523605107209551873-00AX/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAAMKjaMBgVWqnGQ7U4godYqOh1rcqonVh74

---

# Submission Instructions

- Complete Tasks 0–12 in sequence.
- Include all Screenshots 1–35 exactly as specified.
- Ensure that your full name is visible in the required screenshots.
- Include the working EpicBook EC2 public IP URL.
- Include the published LinkedIn post URL.
- Include proof of frontend, backend, and database integration.
- Ensure that the required Terraform root files, module files, and `user_data.sh` are included in the GitHub submission.
- Do not upload Terraform state files, `.pem` files, or a `terraform.tfvars` file containing passwords or other sensitive values.
- Do not expose AWS credentials, account IDs, private SSH keys, RDS passwords, access tokens, Terraform sensitive values, or other confidential information.
- Review all screenshots and files carefully before submitting through GitHub.

---

# Completion Checklist

- [ ] Installed and verified Terraform **(FLAG: not in snapshot)**
- [ ] Installed and verified AWS CLI **(FLAG: not in snapshot)**
- [ ] Configured AWS CLI **(FLAG: not in snapshot)**
- [ ] Confirmed the AWS Region **(FLAG: not in snapshot)**
- [ ] Installed the HashiCorp Terraform extension **(FLAG: not in snapshot)**
- [ ] Created the modular Terraform project **(FLAG: current spec; your version is flat)**
- [ ] Created the root `main.tf`, `variables.tf`, and `outputs.tf` **(FLAG: current spec)**
- [ ] Created the Network module **(FLAG: current spec)**
- [ ] Created the EC2 module **(FLAG: current spec)**
- [ ] Created the RDS module **(FLAG: current spec)**
- [ ] Created the EC2 `user_data.sh` **(FLAG: current spec)**
- [ ] Created VPC `10.0.0.0/16`
- [ ] Created public subnet `10.0.1.0/24`
- [ ] Created private DB subnet A `10.0.2.0/24` **(FLAG: your flat version has a single private subnet `10.0.2.0/24`)**
- [ ] Created private DB subnet B `10.0.3.0/24` **(FLAG: not in your version)**
- [ ] Used different Availability Zones for the database subnets **(FLAG: not in your version)**
- [ ] Created and attached the Internet Gateway
- [ ] Created the public route table
- [ ] Associated the public subnet with the public route table
- [ ] Created the EC2 Security Group
- [ ] Allowed HTTP port `80`
- [ ] Restricted SSH port `22`
- [ ] Created the RDS Security Group
- [ ] Allowed MySQL port `3306` from the EC2 Security Group only
- [ ] Exposed the required Network module outputs
- [ ] Defined the EC2 instance
- [ ] Connected `user_data.sh` using the EC2 `user_data` argument **(FLAG: current spec)**
- [ ] Configured EC2 with a public IP
- [ ] Installed the required software using user data **(FLAG: current spec; your version installed it over SSH)**
- [ ] Created the RDS DB subnet group
- [ ] Created Amazon RDS for MySQL
- [ ] Confirmed RDS is not publicly accessible
- [ ] Configured sensitive database variables
- [ ] Exposed the RDS endpoint
- [ ] Connected all modules through the root module **(FLAG: current spec)**
- [ ] Passed Network module outputs to EC2 and RDS **(FLAG: current spec)**
- [ ] Added root EC2 public IP and RDS endpoint outputs
- [ ] Completed `terraform init`
- [ ] Completed `terraform validate` **(FLAG: not in snapshot)**
- [ ] Reviewed `terraform plan`
- [ ] Completed `terraform apply`
- [ ] Verified EC2 is running
- [ ] Verified RDS is available
- [ ] Verified user data installation **(FLAG: current spec)**
- [ ] Connected to EC2 using SSH
- [ ] Cloned EpicBook
- [ ] Created the `bookstore` database
- [ ] Imported the database schema
- [ ] Imported author seed data
- [ ] Imported book seed data
- [ ] Verified database records
- [ ] Installed EpicBook dependencies
- [ ] Configured EpicBook to use RDS
- [ ] Configured Nginx
- [ ] Started EpicBook
- [ ] Verified port `8080`
- [ ] Loaded EpicBook through the EC2 public IP
- [ ] Verified product viewing
- [ ] Verified Add to Cart
- [ ] Verified the checkout or order workflow
- [ ] Confirmed application actions in Amazon RDS
- [ ] Completed `terraform destroy`
- [x] Published the required LinkedIn post **(FLAG: confirm it meets Task 12)**
- [x] Added the LinkedIn post URL
- [ ] Captured all 35 required screenshots **(FLAG: 13 earlier screenshots kept in Appendix; slots 1–35 are mostly not yet filled)**
- [ ] Confirmed that my full name is visible in the required screenshots
- [x] Checked that no sensitive information is exposed **(FLAG: confirm for the earlier submission)**

---

## About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory), focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations through hands-on experience.

---

## Resources

- EpicBook Repository: [https://github.com/pravinmishraaws/theepicbook](https://github.com/pravinmishraaws/theepicbook)
- EpicBook Installation, Configuration & Troubleshooting Guide: [Installation & Configuration Guide](https://github.com/pravinmishraaws/theepicbook/blob/main/Installation%20%26%20Configuration%20Guide.md)
- DMI Official Website: [https://dmi.pravinmishra.com](https://dmi.pravinmishra.com)
- University: [https://university.pravinmishra.com](https://university.pravinmishra.com)
- Discord Community: [https://discord.pravinmishra.com](https://discord.pravinmishra.com)
- Blog: [https://dmi.pravinmishra.com/blog](https://dmi.pravinmishra.com/blog)
- YouTube Playlist: [https://www.youtube.com/playlist?list=PLFeSNDtI4Cho](https://www.youtube.com/playlist?list=PLFeSNDtI4Cho)
- Pravin Mishra on LinkedIn: [https://www.linkedin.com/in/pravin-mishra-aws-trainer/](https://www.linkedin.com/in/pravin-mishra-aws-trainer/)
- CloudAdvisory on LinkedIn: [https://www.linkedin.com/company/thecloudadvisory/](https://www.linkedin.com/company/thecloudadvisory/)

---

*This submission is part of the DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*

---

# Appendix — Earlier Submission (Flat Configuration, kept as written)

These are your earlier evidence and notes from the flat (non-modular) version of this assignment. They are kept for reference and do not map to the current Tasks 1–11.

### Earlier Screenshot 1 — Terraform configuration showing the VPC and both subnet CIDR ranges

![Screenshot 1](screenshots/assignment-04/Screenshot%201.png)

### Earlier Screenshot 2 — Internet Gateway, public route table, and both Security Groups

![Screenshot 2A](screenshots/assignment-04/Screenshot%202A.png)

![Screenshot 2B](screenshots/assignment-04/Screenshot%202B.png)

![Screenshot 2C](screenshots/assignment-04/Screenshot%202C.png)

### Earlier Screenshot 3 — Terraform apply output showing successful EC2 provisioning

![Screenshot 3](screenshots/assignment-04/Screenshot%203.png)

### Earlier Screenshot 4 — EC2 instance running in the AWS Console

![Screenshot 4](screenshots/assignment-04/Screenshot%204.png)

### Earlier Screenshot 5 — SSH access and installed software

![Screenshot 5](screenshots/assignment-04/Screenshot%205.png)

### Earlier Screenshot 6 — EpicBook application files and dependency installation

![Screenshot 6](screenshots/assignment-04/Screenshot%206.png)

### Earlier Screenshot 7 — Application and Nginx services running

![Screenshot 7](screenshots/assignment-04/Screenshot%207.png)

### Earlier Screenshot 8 — Terraform apply output showing successful RDS provisioning

![Screenshot 8](screenshots/assignment-04/Screenshot%208.png)

### Earlier Screenshot 9 — RDS instance showing Publicly accessible: No

![Screenshot 9](screenshots/assignment-04/Screenshot%209.png)

### Earlier Screenshot 10 — Database initialization or table verification from EC2

![Screenshot 10](screenshots/assignment-04/Screenshot%2010.png)

### Earlier Screenshot 11 — EpicBook through the EC2 public IP

![Screenshot 11](screenshots/assignment-04/Screenshot%2011.png)

### Earlier Screenshot 12 — Product, cart, order summary, or checkout flow

![Screenshot 12](screenshots/assignment-04/Screenshot%2012.png)

### Earlier Notes

Write a short note describing any issue you faced, how you fixed it, and what you learned.

Issues, fixes, and what I learned

1. Terraform had no AWS credentials. The plan failed with "No valid credential sources found" and then tried the EC2 metadata service (169.254.169.254), which isn't reachable from a laptop. The aws provider block in main.tf doesn't set a profile, and AWS_PROFILE wasn't exported in the shell. Fix: export a named profile before running Terraform (export AWS_PROFILE=dmi-c3-g3-training). Learned: the provider reads credentials from the environment or a profile, and the metadata-service error only means no other source was found.

2. The Ubuntu AMI lookup returned no results. The aws_ami data source in modules/compute filtered on ubuntu/images/hvm-ssd-gp3/ubuntu-jammy-22.04-amd64-server-*. Canonical publishes Jammy under hvm-ssd in us-east-1, so the filter matched nothing. Fix: changed the default ami_name_filter to ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-* and confirmed with aws ec2 describe-images that it returns current images. Learned: hard-coding a name pattern is fragile. Check it against the real catalog with the CLI before running a plan, and keep the owner ID (Canonical, 099720109477) fixed.

3. The plan stopped on a missing sensitive variable. db_password has no default and must be at least 8 characters. Rather than store it in terraform.tfvars, the config expects TF_VAR_db_password in the environment. Learned: keep secrets out of committed files and pass them through environment variables or a secret store.

4. RDS took several minutes to create. The EC2 instance finished in about 13 seconds, while aws_db_instance.mysql was still creating after 2+ minutes. This is normal for a new MySQL instance (roughly 5 to 15 minutes, and Terraform allows up to 40). Learned: the database config was correct; the wait is expected, and a failed creation shows up as an error in Terraform or as a status in the RDS console.

5. Connecting to the instance over SSH. The key pair is epicbook-aws-key, and the public key comes from ~/.ssh/epicbook-aws-key.pub. The connection command is ssh -i ~/.ssh/epicbook-aws-key ubuntu@<ec2_public_ip>. The security group only allows SSH from ssh_allowed_cidr, so a changed IP will time out. Learned: match the private key to the registered public key, and remember that inbound access is limited to a single CIDR.
