# Assignment 5 — AI-Assisted Sprint Health Report via Jira MCP

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will connect Claude Code to your Jira board through an MCP server, the same way you connected it to GitHub in Week 2, and build a read-only `/sprint-health` skill. The skill reads your current sprint through Jira's API and reports sprint velocity, stories at risk of missing the sprint, and items missing an estimate — but it must never create, edit, comment on, or transition a single ticket itself. You will prove that boundary holds by making a real change on the board yourself and confirming the skill only ever reports, never acts.

---

# Task 1 — Create a Jira API Token

## Goal

Generate an API token from your Atlassian account that the MCP server will use to authenticate with your Jira site. Do not screenshot the token value itself.

### Evidence

#### Screenshot 1 — Jira API token creation confirmation page showing the token name, with the token value not visible

![Screenshot 1 - Jira API Token Creation](./screenshots/assignment-05/Screenshot%201.png)

### Notes You Must Write (Very Important):

Why does the MCP server need your site URL and account email in addition to the token?

The MCP server needs the site URL to know which Jira instance to connect to (since Jira Cloud instances have unique URLs like https://deventhusiastailearningsupport.atlassian.net). The account email identifies which user's credentials are being used for authentication and permission scoping. Together with the token, these three pieces form a complete authentication triplet: the token proves identity and grants API access, the email identifies which user account is making the request, and the site URL specifies which Jira instance to connect to. Without the site URL, the token alone wouldn't know where to send requests; without the email, Jira wouldn't know which user's permissions to apply to the API calls.

---

# Task 2 — Create .mcp.json at the Project Root

## Goal

Create or update `.mcp.json` at your project root with a Jira MCP server block, following the same shape as the GitHub MCP server you configured in Week 2.

### Evidence

#### Screenshot 2 — `.mcp.json` open in VS Code showing the Jira server configuration

![Screenshot 2 - MCP JSON Configuration](./screenshots/assignment-05/Screenshot%202.png)

### Notes You Must Write (Very Important):

Compare this jira block to the github block from Week 2 Assignment 5. The GitHub server ran via npx (a Node.js package); this one runs via uvx (a Python package) — what stays exactly the same shape despite that difference, and why doesn't Claude Code care which language a given MCP server is written in?

The structure that stays exactly the same is the MCP server block format itself: both have a server name as the key (e.g., "github" or "jira"), and both contain a "command" and "args" array that specify how to launch the server process. Claude Code doesn't care about the underlying language because MCP (Model Context Protocol) defines a language-agnostic interface — once a server is running, it communicates with Claude via standard MCP messages (JSON-RPC over stdio) regardless of whether it's Node.js, Python, Go, or any other language. The server's implementation details are completely abstracted away; Claude only cares that the server responds correctly to MCP protocol messages. The "command" field just tells the harness "how to start the process," and the "args" provide the necessary arguments — whether that command is `npx` for Node or `uvx` for Python is irrelevant to Claude Code's integration layer.

---

# Task 3 — Add Your Credentials to settings.local.json

## Goal

Add your Jira site URL, account email, and API token to `.claude/settings.local.json`, and confirm that file is listed in `.gitignore` so it is never committed.

### Evidence

#### Screenshot 3 — `settings.local.json` open in VS Code showing the `env` section, with the actual token value blurred or covered

![Screenshot 3 - Settings Local JSON](./screenshots/assignment-05/Screenshot%203.png)

### Notes You Must Write (Very Important):

Why must JIRA_API_TOKEN live in settings.local.json and never in .mcp.json?

JIRA_API_TOKEN must live in `settings.local.json` (not `.mcp.json`) because `.mcp.json` is typically checked into version control and shared with the team, while `settings.local.json` is listed in `.gitignore` and stays on the user's machine only. API tokens are secrets — they grant direct access to your Jira account and should never be committed to git where they could be exposed in history or shared unintentionally. The MCP server configuration (in `.mcp.json`) specifies *how* to run the server and which environment variables to read from, but the actual secret values go into `.local.json` where they stay secure. When Claude Code starts the MCP server, it looks up the token value from `settings.local.json` at runtime and passes it to the server via the environment, keeping the secret completely out of version control.

---

# Task 4 — Verify the Connection with /mcp

## Goal

Restart Claude Code and confirm the Jira MCP server shows as connected.

### Evidence

#### Screenshot 4 — `/mcp` output showing `jira: connected`

![Screenshot 4 - MCP Connected](./screenshots/assignment-05/Screenshot%204.png)

---

# Task 5 — Run a Live Query to Prove Real Board Data

## Goal

Ask Claude to list the issues in your current active sprint through the Jira MCP connection, and confirm the result matches what you see on your live board in the browser.

### Evidence

#### Screenshot 5 — Claude's response showing the live sprint issue list retrieved via Jira MCP

![Screenshot 5 - Live Sprint Data](./screenshots/assignment-05/Screenshot%205.png)

### Notes You Must Write (Very Important):

How did you confirm this was real board data and not something Claude guessed?

I confirmed this was real board data by:
1. Opening my Jira board in the browser immediately after getting the response from Claude
2. Verifying the exact issue keys (like DMIWMA-17, DMIWMA-18, etc.) matched what appeared on my live board
3. Checking the sprint name ("Sprint 1") and duration ("Aug 6 - Aug 10, 2026") matched exactly
4. Validating specific story details: the story summary "Add footer with version and deploy date", the assignee "Maneetta Antony", story points "1.0", and status "Done" all matched precisely with what I saw in the Jira UI
5. Noting that the subtask list (DMIWMA-18 through DMIWMA-22) and their statuses matched exactly
This wasn't guessed data — Claude retrieved it live through the Jira MCP connection and displayed the exact state of Sprint 1 at that moment.

---

# Task 6 — Build the /sprint-health Skill

## Goal

Create a `/sprint-health` skill restricted to read-only Jira tools plus `Read`, with no issue-mutating tools and no `Write`. Run it and confirm it produces a report covering sprint velocity, at-risk stories, and items missing an estimate.

### Evidence

#### Screenshot 6 — `SKILL.md` frontmatter showing `allowed-tools` limited to read-only Jira tools plus `Read`, with `disable-model-invocation: true`

![Screenshot 6 - Sprint Health Skill Configuration](./screenshots/assignment-05/Screenshot%206.png)

#### Screenshot 7 — `/sprint-health` output showing the full triage report against your real sprint

![Screenshot 7a - Sprint Health Report](./screenshots/assignment-05/Screenshot%207a.png)

![Screenshot 7b - Sprint Health Report Updated](./screenshots/assignment-05/Screenshot%207b.png)

### Notes You Must Write (Very Important):

1. Which Jira MCP tools does this skill's allowed-tools list include, and which mutating tools (create issue, update issue, transition issue, add comment) does it deliberately exclude?

The skill's allowed-tools include these read-only Jira MCP tools:
- `mcp_jira_jira_search` — search for issues
- `mcp_jira_jira_get_issue` — retrieve individual issue details
- `mcp_jira_jira_get_sprint` — fetch sprint information
- `mcp_jira_jira_get_board` — get board configuration and metadata
- Plus `Read` for reading local files

The skill deliberately excludes all mutating tools such as:
- `mcp_jira_jira_create_issue` — would allow creating new issues
- `mcp_jira_jira_update_issue` — would allow editing issue fields
- `mcp_jira_jira_transition_issue` — would allow changing issue status
- `mcp_jira_jira_add_comment` — would allow commenting on issues

2. Why does a Scrum Master need this restriction more than almost any other role in this course?

A Scrum Master needs this restriction more than any other role because their primary job is to observe, measure, and report on team health without directly interfering with the work. If the `/sprint-health` skill had write access, a Scrum Master could accidentally (or intentionally) move stories, close tickets, change estimates, or add notes — actions that should only be done by the engineering team. This restriction enforces a crucial governance boundary: the Scrum Master has eyes everywhere (read access to all sprint data) but hands nowhere (no write access to change anything). The skill enforces the principle that a Scrum Master facilitates and observes, never directs or alters the board state. This is especially critical because a well-meaning Scrum Master could inadvertently mask problems (by closing tickets without the team doing the work) or create confusion if they start making board changes outside the team's normal workflow.

---

# Task 7 — Prove the Skill Never Mutates the Board

## Goal

Manually update one ticket on your board in the browser (for example, move a story to "Done" or add a missing estimate), then run `/sprint-health` again and confirm the new report reflects your change — proving the skill only ever reads live state and never wrote to the board itself.

### Evidence

#### Screenshot 8 — Second `/sprint-health` run showing the report now reflects your manual board change

![Screenshot 8 - Sprint Health Report After Manual Change](./screenshots/assignment-05/Screenshot%207b.png)

### Notes You Must Write (Very Important):

Map this assignment to Gather → Analyze → Human Act → Verify from Week 3 Assignment 6. Which step did you perform manually in the browser, and why must that step stay human?

In the Gather → Analyze → Human Act → Verify model:
- **Gather**: The `/sprint-health` skill gathers live sprint data via the Jira MCP (fully automated)
- **Analyze**: Claude analyzes that data and produces a triage report with velocity, at-risk stories, and missing estimates (fully automated)
- **Human Act**: I manually moved story DMIWMA-3 from "To Do" to "Done" status in the Jira browser UI (human decision-making)
- **Verify**: The second `/sprint-health` run verified the change was reflected — velocity changed from 0/2 to 2/2 points, and the table now shows the Done status (fully automated)

The "Human Act" step must stay human because this is where judgment, accountability, and responsibility live. No automated tool should have the authority to change ticket states — only the person doing the work (or their team consensus) should decide when work is complete. This maintains the integrity of the sprint: if a tool could auto-complete tickets, it could hide incomplete work, mask problems, or represent false progress. The board's state must always reflect human decision-making about what's actually done, because that state drives team conversations, burndown accuracy, and retrospectives.

---

# Submission Instructions

Complete all tasks in sequence.

Your submission must include:
- All 8 required screenshots
- All the required notes

---

# Completion Checklist

- [x] Task 1: Jira API token created, value never screenshotted (Screenshot 1)
- [x] Task 2: `.mcp.json` has the Jira server block (Screenshot 2)
- [x] Task 3: Credentials stored in `settings.local.json`, token blurred, file gitignored (Screenshot 3)
- [x] Task 4: `/mcp` shows the Jira server connected (Screenshot 4)
- [x] Task 5: Live query returned real sprint data, verified against the browser (Screenshot 5)
- [x] Task 6: `/sprint-health` skill created with correct read-only `allowed-tools`, and produced a full report (Screenshots 6–7)
- [x] Task 7: A manual board change was reflected in a second `/sprint-health` run (Screenshot 8)
- [x] Skill never created, edited, transitioned, or commented on any issue
- [x] Reflection answered (Notes)
- [x] No API token value exposed

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
