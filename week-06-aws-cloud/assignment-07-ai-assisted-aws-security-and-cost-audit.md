# Assignment 7 — AI-Assisted AWS Security and Cost Audit

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will build a read-only Bash script that audits the AWS resources you deployed earlier this week — your S3 static site, EC2 instance(s), security groups, RDS database, and EBS volumes — for common security and cost misconfigurations.

You will then connect that script to Claude Code as a reusable `/aws-audit` skill that explains what it found and recommends a fix, without ever making the fix itself.

Finally, you will find a real misconfiguration in your own account, apply the fix yourself, and prove it worked with a second audit run.

---

# Task 1 — Confirm Your AWS Resources and Set Up Your Workspace

## Goal

Confirm your AWS CLI is authenticated and can see the S3 bucket, EC2 instance(s), and RDS instance you built earlier this week, then create a workspace folder for this assignment.

### Evidence

#### Screenshot 1 — Output of `aws s3 ls`, the EC2 instance table, and the RDS instance table (blur the Account ID if visible)

![Screenshot 1 - AWS Resources](./screenshots/assignment-07/Screenshot%201.png)

---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort`

Add your screenshot here.

---

### Notes You Must Write (Very Important)

**1. Which resources from this week's earlier assignments did you see in the listings?**

Write your answer here.

**2. Why must you confirm your resources exist before writing an audit script against them?**

Write your answer here.

---

# Task 2 — Define Safety Rules in CLAUDE.md

## Goal

Create a `CLAUDE.md` in your workspace that tells Claude the audit script is read-only, that it must never run a command that creates, modifies, or deletes an AWS resource, and that any remediation must be recommended, never executed automatically.

### Evidence

#### Screenshot 3 — `CLAUDE.md` open in VS Code showing all four sections

![Screenshot 2 - CLAUDE.md](./screenshots/assignment-07/Screenshot%202.png)

---

### Notes You Must Write (Very Important)

**1. Why should Claude never be given permission to run `revoke-security-group-ingress` itself, even if the fix is obviously correct?**

Write your answer here.

**2. Which rule prevents Claude from claiming a finding that the report does not support?**

Write your answer here.

---

# Task 3 — Plan the Audit with Claude Code

## Goal

Ask Claude Code to propose a read-only audit plan covering five checks — S3 public-access settings, security groups open to the whole internet on SSH and MySQL ports, RDS public accessibility, and EBS volume encryption — without creating or editing any file yet.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan

![Screenshot 3 - Audit Plan](./screenshots/assignment-07/Screenshot%203.png)

---

### Notes You Must Write (Very Important)

**1. Which part of this task represents the Gather phase?**

Write your answer here.

**2. Did every proposed command start with `describe-`, `get-`, or `list-`? Why does that matter?**

Write your answer here.

---

# Task 4 — Build the AWS Audit Script

## Goal

Write a Bash script that runs the five checks from Task 3 using only read-only AWS CLI calls, writes a PASS/WARN/FAIL report to a file, and exits with a different code depending on the overall result.

Make it executable and confirm it has no syntax errors.

### Evidence

#### Screenshot 5 — Top section of `aws-audit.sh` showing the variables and the checks array

![Screenshot 4a - Audit Script Part 1](./screenshots/assignment-07/Screenshot%204a.png)

![Screenshot 4b - Audit Script Part 2](./screenshots/assignment-07/Screenshot%204b.png)

![Screenshot 4c - Audit Script Part 3](./screenshots/assignment-07/Screenshot%204c.png)

![Screenshot 4d - Audit Script Part 4](./screenshots/assignment-07/Screenshot%204d.png)

![Screenshot 4e - Audit Script Part 5](./screenshots/assignment-07/Screenshot%204e.png)

![Screenshot 4f - Audit Script Part 6](./screenshots/assignment-07/Screenshot%204f.png)

**FLAG:** Upstream expects three separate screenshots here (5: top section, 6: one check function, 7: `bash -n` and `ls -l`). Your six images are kept together under Screenshot 5 for now. Let me know if you want them split to match.

---

#### Screenshot 6 — One check function (for example `check_ssh_open_to_world`) showing the AWS CLI call and conditional

Add your screenshot here.

---

#### Screenshot 7 — Output of `bash -n scripts/aws-audit.sh` and `ls -l scripts/aws-audit.sh`

Add your screenshot here.

---

### Notes You Must Write (Very Important)

**1. What is stored in the checks array, and how does the loop use it?**

Write your answer here.

**2. Why does every AWS CLI call in this script use `--query` and `--output text` instead of parsing raw JSON?**

Write your answer here.

**3. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

Write your answer here.

---

# Task 5 — Run the Baseline Audit

## Goal

Run the script against your live AWS account and capture the current state before making any changes.

### Evidence

#### Screenshot 8 — Output of `./scripts/aws-audit.sh` showing your Full Name and all five checks

![Screenshot 5 - Baseline Audit Report](./screenshots/assignment-07/Screenshot%205.png)

---

#### Screenshot 9 — Output showing the captured exit code and final summary

Add your screenshot here.

---

### Notes You Must Write (Very Important)

**1. What is the overall status of your baseline audit?**

Write your answer here.

**2. Did any check return FAIL or WARN? If so, which one, and what evidence did it show?**

Write your answer here.

**3. If every check passed, what does that tell you about the security posture of your account so far?**

Write your answer here.

---

# Task 6 — Build and Run the /aws-audit Skill

## Goal

Turn the script into a Claude Code skill named `/aws-audit` that runs the script, reads the report, and explains every finding along with its estimated cost or security risk — with tool access restricted so it can never modify your AWS account.

### Evidence

#### Screenshot 10 — `SKILL.md` showing the frontmatter, tool restrictions, and safety rules

![Screenshot 6 - Skill File](./screenshots/assignment-07/Screenshot%206.png)

---

#### Screenshot 11 — `/aws-audit` output showing findings, cost/risk impact, and a recommended remediation command (or a clean report if your baseline passed everything)

![Screenshot 7 - Skill Output](./screenshots/assignment-07/Screenshot%207.png)

---

### Notes You Must Write (Very Important)

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

Write your answer here.

**2. What part is performed by Bash, and what part is performed by Claude?**

Write your answer here.

**3. Why is estimating cost/risk impact something the AI adds on top of a plain PASS/FAIL script?**

Write your answer here.

---

# Task 7 — Fix a Real Finding and Re-Verify

## Goal

Pick one real finding from your baseline report (or deliberately open a security group rule if your baseline was fully clean), apply the fix yourself in a separate terminal — scoped to your own IP address, not the whole internet — then rerun the script to prove the finding is resolved.

### Evidence

#### Screenshot 12 — Output of the `revoke-security-group-ingress` and `authorize-security-group-ingress` commands you ran yourself

![Screenshot 8 - Remediation Command](./screenshots/assignment-07/Screenshot%208.png)

---

#### Screenshot 13 — Rerun of `./scripts/aws-audit.sh` showing the finding is now PASS

![Screenshot 9 - Second Audit Run](./screenshots/assignment-07/Screenshot%209.png)

**FLAG:** Upstream expects a separate note answer for each question below. Your earlier answer is a combined Gather → Analyze → Human Act → Verify summary, placed under question 4.

---

### Notes You Must Write (Very Important)

**1. Which exact finding did you fix, and what command did you run?**

Write your answer here.

**2. Why did you scope the new rule to your own IP address instead of leaving it open to `0.0.0.0/0`?**

Write your answer here.

**3. Did Claude execute the remediation command, or did you? Why does that matter?**

Write your answer here.

**4. Which phase of the Agentic Loop does the Bash script represent? Which phase does Claude's explanation represent? Which phase is you running the fix?**

**Gather:** The audit script performed the Gather step by running read-only AWS CLI commands to collect configuration details from S3, EC2, Security Groups, RDS, and EBS resources without modifying anything.

**Analyze:** Claude performed the Analyze step by reading the audit report and interpreting the findings, explaining the security and cost implications of each PASS, WARN, or FAIL result, and recommending specific fixes.

**Human Act:** I performed the Human Act step by manually executing the remediation command (e.g., updating security group rules) in my own terminal with proper scoping (restricted to my IP, not the entire internet), ensuring human judgment and approval controls every change.

**Verify:** The script performed the Verify step again by re-running the five checks after remediation to confirm the finding was resolved and the configuration was corrected.

**Why Claude must never execute remediation:** Claude must only analyze and recommend because: (1) it prevents accidental misconfigurations or overshooting fixes, (2) it ensures human accountability for every change to production systems, (3) it maintains the principle of least privilege by keeping destructive actions separated from analytical operations, and (4) it forces explicit human approval and understanding of what is being changed and why, which is critical for compliance and security governance.

---

# LinkedIn Post (Required)

## Goal

Create a LinkedIn post including:

- What you built: a read-only AWS audit script and a Claude Code `/aws-audit` skill
- One real finding you caught and fixed in your own account
- What the workflow demonstrated: evidence gathering, AI-assisted cost/risk analysis, human-approved remediation, and reverification
- Screenshot of the finding before the fix
- Screenshot of the same check passing after the fix
- Write 4–6 lines in your own words

Suggested tags:

`#DMIByPravinMishra #AWS #AgenticAI #ClaudeCode #DevOps`

### Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`Add your URL here`

---

#### Screenshot of Published LinkedIn Post

Add your screenshot here.

---

# Submission Instructions

Complete all tasks in sequence.

Your submission must include:

- All 13 required task screenshots
- Answers to every **Notes You Must Write** question
- `CLAUDE.md`
- `scripts/aws-audit.sh`
- `.claude/skills/aws-audit/SKILL.md`
- `reports/aws-audit-report.txt` baseline report and the reverified report from Task 7
- GitHub folder or repository URL containing the assignment files
- Your Full Name visible in the required outputs
- LinkedIn post URL
- Screenshot of the published LinkedIn post
- GitHub repository URL (containing all assignment files)

---

# Completion Checklist

- [ ] Task 1: AWS resources confirmed and workspace created (Screenshots 1–2) **(FLAG: Screenshot 2 missing)**
- [ ] Task 2: `CLAUDE.md` created with project context and safety rules (Screenshot 3)
- [ ] Task 3: Claude produced a read-only five-check audit plan before any script existed (Screenshot 4)
- [ ] Task 4: `aws-audit.sh` built, executable, and passes `bash -n` (Screenshots 5–7) **(FLAG: Screenshots 6 and 7 missing)**
- [ ] Task 5: Baseline audit captured and saved with Full Name visible (Screenshots 8–9) **(FLAG: Screenshot 9 missing)**
- [ ] Task 6: `/aws-audit` skill loads and runs successfully with no Write permission (Screenshots 10–11)
- [ ] Task 7: A real finding was fixed by you and reverified as PASS (Screenshots 12–13)
- [x] Skill never executed a remediation command
- [ ] New security group rule is scoped to your own IP, not `0.0.0.0/0` **(FLAG: not confirmed in snapshot)**
- [ ] All 13 required task screenshots are included **(FLAG: 4 missing — see above)**
- [ ] All "Notes You Must Write" questions are answered in your own words **(FLAG: most upstream note questions unanswered; see Notes)**
- [x] No AWS credentials or unblurred account IDs exposed
- [ ] LinkedIn post published and URL submitted **(FLAG: new required section — not yet done)**
- [ ] GitHub repository URL included in submission
- [ ] All assignment files committed and visible in GitHub repository

---

# Final Submission

Submit your GitHub repository URL containing all assignment files, screenshots, reports, and output.

### GitHub Repository URL

Paste your GitHub repository URL here:

`Add your GitHub repository URL here`

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
