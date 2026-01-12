60-Day Linux → Cloud Engineer
Transformation Plan (Enhanced v2)
Your original plan, with my additions inserted in-place. Structure preserved; upgrades are
explicitly embedded where they belong.
GLOBAL OPERATING RULES (Applies Every Day, All 60
Days)
1) One Git repo from Day 1 (portfolio-first)
Create a single monorepo and keep all artifacts in it:
Repo name: cloud-engineer-60d
Suggested structure:
cloud-engineer-60d/
notes/
_template.md
day01.md
...
linux/
day02/
day03/
automation/
bash/
python/
cron/
aws/
cli/
iam/
ec2/
s3/
iac/
terraform/
containers/
docker/
ecs/
cicd/
github-actions/
github-actions/
observability/
cloudwatch/
runbooks/
diagrams/
Daily requirement: You commit something every day (notes + any scripts/configs).
2) Daily “Portfolio Artifact” requirement
In addition to your daily lab work, you must produce:
• notes/dayXX.md using the template below
• Any code/config created that day committed to Git
• Evidence of verification (commands run + outputs summarized)
Add this template to notes/_template.md:
• Objective (1–2 lines)
• Commands practiced (bullets)
• What I broke on purpose (at least 1 thing)
• Root cause
• Fix + verification commands
• What I would automate next
3) Forced Failure Drill (build troubleshooting intuition)
Every day, you intentionally create one failure and recover (examples: break permissions, stop a
service, misconfigure a port, break DNS resolution, etc.). You document it in notes/
dayXX.md.
4) Cost Discipline (AWS days especially)
• Default rule: build → verify → capture notes → destroy in the same session unless a
resource is explicitly part of a longer-running exercise.
• You use tags on every AWS resource:
Project=cloud-engineer-60d, Day=XX, Owner=<you>,
Expires=<date>
• You use tags on every AWS resource:
Project=cloud-engineer-60d, Day=XX, Owner=<you>,
Expires=<date>
• Cleanup is part of “Done.” If the environment is not needed, it gets deleted.
Cost reality insert: NAT Gateways, ALBs, RDS, and EKS can create surprise monthly spend if
left running. Your plan supports “production-grade demos” best when infrastructure is
ephemeral and destroyed after validation.
5) Security Discipline
• No long-lived keys in repos.
• AWS auth: prefer IAM Identity Center (SSO) for humans; prefer OIDC for CI/CD (no
stored AWS keys).
• In AWS: prefer SSM Session Manager as the default access pattern (no inbound SSH);
SSH becomes “break-glass.”
6) Phase-end reflection: AWS Well-Architected mindset
At the end of Days 10, 20, 30, 60, add a one-page reflection in your notes:
Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization,
Sustainability.
PHASE 1 — CORE LINUX FOUNDATIONS (Days 1–10)
Goal: Command the operating system that powers most cloud infrastructure.
Day 1 — Server + SSH Handshake ✅ DONE
• Install Ubuntu Server (VM)
• Networking working
• SSH from Mac using key-based authentication (no passwords)
• VS Code Remote-SSH
• Create/edit files remotely
INSERTED ADDITIONS (Day 1): Portfolio + Repeatability
INSERTED ADDITIONS (Day 1): Portfolio + Repeatability
• Create repo cloud-engineer-60d
• Create notes/day01.md using your template
• Commit a “lab baseline”:
◦ SSH config snippet (sanitized)
◦ Steps to rebuild VM access quickly
• Forced failure drill: temporarily break SSH permissions on .ssh/ and recover.
Definition of Done:
• You edit files on Linux from VS Code securely.
• AND your day01.md is committed with a documented failure + recovery.
Day 2 — Filesystem Navigation & Mental Mapping
New Skills:
• Absolute vs relative paths (mental model)
• pwd, ls -lah, tree
• cd navigation patterns (..,
~
,
-)
• Build /cloud-labs/day2/{logs,scripts,configs}
INSERTED ADDITIONS (Day 2): operator ergonomics
• Learn how to self-serve help:
◦ man <command>
◦ <command> --help
• Add linux/day02/ to repo; include a small directory tree and notes.
Exercises:
• Navigate between 5 nested directories blindfolded (no ls)
• Explain Linux filesystem hierarchy: /etc, /var, /home, /opt
• Draw the filesystem tree on paper
• (New) Use man bullets.
hier (or equivalent docs) and summarize filesystem purpose in 5
• (New) Use man bullets.
hier (or equivalent docs) and summarize filesystem purpose in 5
Why This Matters: AWS EBS volumes, Lambda layers, and container filesystems follow this
structure.
Definition of Done:
• You can navigate any Linux system without fear.
• AND notes/day02.md includes your filesystem explanation + directory tree.
Day 3 — File Operations & Disaster Prevention
New Skills:
• cp -r, mv, rm -rf (with -i flag for safety)
• Wildcards: *.log, file?.txt, [abc]*
• Hidden files (.bashrc, .ssh/)
• touch, cat, less, head, tail
INSERTED ADDITIONS (Day 3): safer copying + compression basics
• Add:
◦ rsync basics (safer copying, dry runs)
◦ tar basics (create/extract archives)
Exercises:
• Copy 100 files using wildcards
• Simulate accidental deletion, practice recovery mindset
• Create a "staging" and "production" directory structure
• (New) Use rsync --dry-run to preview a bulk copy before doing it.
Why This Matters: S3 lifecycle policies, log rotation, and backup strategies mirror this.
Definition of Done:
• You move/delete files with zero anxiety.
• AND you can describe when to prefer rsync and when to prefer cp.
• AND you can describe when to prefer rsync and when to prefer cp.
Day 4 — Users, Groups, and the Root Divide
New Skills:
• Linux user model (UID, GID)
• sudo vs su
• /etc/passwd, /etc/group, /etc/shadow
• whoami, id, groups
• Creating users (adduser, usermod)
Exercises:
• Create 3 users: dev, ops, auditor
• Add dev to sudo group
• Explain why root login is disabled in AWS AMIs
INSERTED ADDITIONS (Day 4): least privilege muscle
• Create a directory linux/day04/permissions-lab/
• Demonstrate:
◦ auditor can read logs but cannot edit
◦ dev can edit app files but cannot change system-level config without sudo
• Document the “human → role → permission boundary” analogy to AWS IAM.
Definition of Done: You understand who can do what and why.
Day 5 — Permissions (The Security Foundation)
New Skills:
• ls -l breakdown: drwxr-xr-xr-x
• Read (4), Write (2), Execute (1) binary logic
• chmod (numeric + symbolic)
• chmod (numeric + symbolic)
• chown, chgrp
• Sticky bits, SUID, SGID (awareness)
Exercises:
• Make a script executable
• Fix a "permission denied" error on a web directory
• Explain why .ssh/ must be 700 and private keys 600
INSERTED ADDITIONS (Day 5): forced failure drill upgrade
• Intentionally break one:
◦ Make .ssh too permissive, observe SSH refusal behavior, fix it.
◦ Or break Nginx docroot permissions and restore.
Definition of Done:
• You fix permission errors in under 30 seconds.
• AND you can explain the failure mode clearly in your notes.
Day 6 — Package Management & System Hygiene
New Skills:
• apt update vs apt upgrade vs apt full-upgrade
• Installing/removing packages (nginx, htop, tree, curl, jq)
• /etc/apt/sources.list
• dpkg -l, apt search
Exercises:
• Install Nginx, verify it's running
• Remove a package cleanly
• Simulate a broken dependency scenario
INSERTED ADDITIONS (Day 6): verification discipline
INSERTED ADDITIONS (Day 6): verification discipline
• Add a “verification block” habit:
◦ “After install, I verify with systemctl status, ss -tuln, curl -I”
• Capture key outputs in notes (not huge dumps; just meaningful summaries).
Definition of Done: You maintain a server without breaking it.
Day 7 — Text Processing (The Unix Philosophy)
New Skills:
• grep (search text)
• awk (column processing)
• sed (find/replace)
• Pipes (|) and redirection (>, >>, 2>&1)
• wc, sort, uniq
Exercises:
• Find all ERROR lines in a 10,000-line log file
• Extract IP addresses from /var/log/auth.log
• Chain 5 commands with pipes
INSERTED ADDITIONS (Day 7): “production shaping”
• Add:
◦ cut, tr, xargs (common glue tools)
◦ Use grep -R safely with include/exclude patterns
Definition of Done: You can extract meaning from any text file.
Day 8 — Environment Variables & Shell Customization
New Skills:
• .bashrc, .bash_profile
• export PATH, alias
• export PATH, alias
• Environment variables ($HOME, $USER, $PATH)
• Making changes persistent
Exercises:
• Create aliases for common commands
• Add /usr/local/bin to PATH
• Set AWS_REGION=us-east-1 permanently
INSERTED ADDITIONS (Day 8): shell as an operator tool
• Add:
◦ a safe prompt that shows exit code or git branch (optional)
◦ a “ll alias” and a “ports alias” (example: ss -tuln)
• Document “persistent vs session-only” changes explicitly in notes.
Definition of Done: Your shell feels like your shell.
Day 9 — Vim Survival Skills
New Skills:
• Why Vim exists on every server
• i (insert), Esc, :wq, :q!
• Navigation: hjkl, gg, G
• Delete line (dd), undo (u)
Exercises:
• Edit /etc/hosts without panicking
• Fix a config file over SSH when VS Code fails
• Edit a cron job using crontab -e
INSERTED ADDITIONS (Day 9): operational resilience
• Add:
• Add:
◦ “recover from a bad edit” (swap files, undo, exit without saving)
• Force failure: intentionally typo a config line; restore quickly.
Definition of Done: You don't fear Vim.
Day 10 — Phase 1 Review & Rebuild Challenge
Challenge:
• Destroy your VM
• Rebuild from scratch in under 60 minutes:
◦ SSH keys
◦ 3 users with specific permissions
◦ Directory structure
◦ Nginx installed
◦ Custom .bashrc
INSERTED ADDITIONS (Day 10): phase reflection + runbook
• Write a runbook: runbooks/linux-vm-rebuild.md
◦ Steps + verification commands
◦ Common failure modes
• Well-Architected reflection (1 page) in notes/day10.md
Reflection:
• Explain the Linux filesystem to someone non-technical
• Identify what you'd automate (foreshadowing scripting)
Definition of Done: You understand, not just followed steps.
PHASE 2 — SYSTEM OPERATIONS &
TROUBLESHOOTING (Days 11–20)
Goal: Operate and troubleshoot production-grade systems.
Goal: Operate and troubleshoot production-grade systems.
Day 11 — Processes & Resource Management
New Skills:
• ps aux, pgrep, pidof
• top, htop (real-time monitoring)
• Foreground vs background jobs
• kill, killall, pkill
• Process states (running, sleeping, zombie)
INSERTED ADDITIONS (Day 11): operator controls
• Add:
◦ jobs, bg, fg, nohup
◦ nice / renice (basic awareness)
Exercises:
• Find the PID of Nginx
• Run a script in the background
• Kill a runaway process
Definition of Done: You control running processes confidently.
Day 12 — Services & systemd
New Skills:
• systemctl status/start/stop/restart
• systemctl enable/disable (boot persistence)
• /etc/systemd/system/
• Reading service logs: journalctl -u nginx
Exercises:
Exercises:
• Make Nginx start on boot
• Create a custom service for a Python script
• Troubleshoot a failed service
INSERTED ADDITIONS (Day 12): “service as a contract”
• Include explicit ExecStart, working directory, user/group, environment variables
• Add restart policy reasoning (Restart=on-failure)
• Commit the unit file to Git (sanitized if needed)
Definition of Done: You understand "why servers start themselves."
Day 13 — Logs & Debugging
New Skills:
• /var/log/ structure
• journalctl mastery
• tail -f for live logs
• Log rotation (logrotate)
Exercises:
• Diagnose why SSH login failed
• Find when a service last crashed
• Set up custom application logging
INSERTED ADDITIONS (Day 13): structured debugging habit
• Add “debug checklist”:
1 What changed?
2 What error do logs show?
3 What is the last known good state?
4 Can I reproduce?
5 What’s the smallest fix?
5 What’s the smallest fix?
• Introduce logger (write to syslog/journald) for app logs.
Definition of Done: You read errors like a detective.
Day 14 — Disk & Storage Management
New Skills:
• df -h, du -sh
• Inodes (df -i)
• Mount points (mount, /etc/fstab)
• Finding large files (find, ncdu)
Exercises:
• Identify what's consuming disk space
• Simulate disk pressure (fill /var)
• Add a second disk and mount it
INSERTED ADDITIONS (Day 14): production tools
• Add:
◦ lsof (what is holding a file open)
◦ “inode exhaustion” scenario (not just disk space)
Definition of Done: You prevent "disk full" disasters.
Day 15 — Memory & Swap
New Skills:
• free -h
• Virtual memory concepts
• Swap configuration
• OOM (Out of Memory) killer
• OOM (Out of Memory) killer
Exercises:
• Check memory usage
• Create a swap file
• Simulate memory pressure
INSERTED ADDITIONS (Day 15): evidence-based tuning
• Add a simple “before/after” recording:
◦ baseline memory usage
◦ after load
◦ after fix
• Document: “what I would monitor in production”
Definition of Done: You understand why servers crash under load.
Day 16 — Networking Fundamentals
New Skills:
• IP addresses, subnets, CIDR notation
• ip addr, ip route
• ping, traceroute
• /etc/hosts, /etc/resolv.conf
• DNS resolution (dig, nslookup)
Exercises:
• Explain 10.0.1.0/24
• Trace the path to google.com
• Diagnose a DNS failure
INSERTED ADDITIONS (Day 16): packet-level awareness
• Add awareness: tcpdump (basic sniffing for “is traffic arriving/leaving?”)
Definition of Done: You speak networking fluently.
Definition of Done: You speak networking fluently.
Day 17 — Ports & Services
New Skills:
• What ports are (0–65535)
• Well-known ports (22, 80, 443, 3306)
• ss -tuln, netstat -tuln
• curl, wget, nc (netcat)
Exercises:
• Find what's listening on port 80
• Test if port 22 is open remotely
• Bind a Python HTTP server to port 8000
INSERTED ADDITIONS (Day 17): deeper diagnostics
• Add:
◦ lsof -i :80 (who owns the port)
◦ curl -v (debug headers + TLS negotiation clues)
Definition of Done: Ports make intuitive sense.
Day 18 — Firewalls (ufw)
New Skills:
• ufw enable/disable
• ufw allow/deny rules
• Default policies
• Application profiles
Exercises:
• Allow SSH, HTTP, HTTPS
• Allow SSH, HTTP, HTTPS
• Deny all other inbound
• Block a specific IP
INSERTED ADDITIONS (Day 18): “default deny” mindset
• Require:
◦ default deny inbound
◦ explicit allow-list only
• Document the mapping: ufw rules ↔ Security Groups.
Definition of Done: You harden servers by default.
Day 19 — SSH Hardening & Key Management
New Skills:
• Disable password authentication
• Change default SSH port
• ~/.ssh/config for multiple servers
• SSH agent forwarding (cautiously)
• Fail2ban awareness
Exercises:
• Lock down SSH completely
• Create SSH config for 3 servers
• Generate a new key pair for GitHub
INSERTED ADDITIONS (Day 19): cloud-realistic access model
• Add a note section:
◦ “In AWS production, prefer SSM Session Manager (no inbound SSH).”
◦ SSH becomes break-glass.
• Document when agent forwarding is appropriate vs risky.
Definition of Done: Your SSH config is production-grade.
Day 20 — Phase 2 Capstone: Deploy a Web App
Challenge:
• Deploy a Python Flask app
• Nginx as reverse proxy
• systemd service for auto-restart
• Firewall configured
• Custom domain (using /etc/hosts if no real domain)
Documentation:
• Write a README.md explaining the architecture
• Include troubleshooting steps
INSERTED ADDITIONS (Day 20): operations maturity
• Add a mini runbook: runbooks/flask-nginx-troubleshooting.md
◦ Common symptoms (502/404/connection refused)
◦ Where to look (nginx logs, journald, ports)
◦ Verification commands
• Create a lightweight postmortem template in observability/postmortem-
template.md
• Well-Architected reflection in notes/day20.md
Definition of Done: You built something real.
PHASE 3 — AUTOMATION & VERSION CONTROL
(Days 21–35)
Goal: Stop doing things manually. Everything as code.
Day 21 — Bash Scripting Basics
Day 21 — Bash Scripting Basics
New Skills:
• Shebang (#!/bin/bash)
• Variables, quotes
• $1, $2 (arguments)
• Exit codes ($?)
• Conditionals (if, else)
Exercises:
• Write a backup script
• Accept user input
• Check if a file exists before deleting
INSERTED ADDITIONS (Day 21): “safe bash defaults”
• Introduce as a habit:
◦ set -e (later you’ll improve to stricter modes)
◦ explicit exit codes (exit 1, exit 0)
• Add automation/bash/ folder and commit scripts.
Definition of Done: You write functional scripts.
Day 22 — Bash Scripting Advanced
New Skills:
• Loops (for, while)
• Functions
• Arrays
• Error handling (set -e, trap)
Exercises:
• Process 100 files in a loop
• Write a health-check script
• Write a health-check script
• Create a deployment script
INSERTED ADDITIONS (Day 22): quality gates
• Add ShellCheck to your workflow (local + later in CI)
• Add a minimal “smoke test” script for each bash tool:
◦ run it against a temp directory
◦ verify expected output
• Document “idempotency” (safe to run twice).
Definition of Done: Your scripts are robust.
Day 23 — Cron & Scheduling
New Skills:
• crontab -e
• Cron syntax (* * * * *)
• /etc/cron.d/, /etc/cron.daily/
• Logging cron jobs
Exercises:
• Schedule a backup every night at 2 AM
• Run a script every 15 minutes
• Email results (using mail or logging)
INSERTED ADDITIONS (Day 23): production scheduling
• Require:
◦ logging stdout/stderr to a file
◦ timestamps in logs
◦ a “last-run marker” file
• Add awareness: systemd timers exist (optional), but cron is fine.
Definition of Done: Automation runs without you.
Definition of Done: Automation runs without you.
Day 24 — Git Fundamentals
New Skills:
• git init, git clone
• git add, git commit, git push
• .gitignore
• Commit messages (conventional commits)
Exercises:
• Initialize a repo for your scripts
• Make 10 commits
• Push to GitHub
INSERTED ADDITIONS (Day 24): reconcile with your Day 1 repo
• You already started Git on Day 1; today you formalize:
◦ branch naming conventions
◦ .gitignore standards
◦ commit message discipline
• Add a README.md at repo root describing repo layout.
Definition of Done: Every script is version-controlled.
Day 25 — Git Branching & Collaboration
New Skills:
• git branch, git checkout, git merge
• Feature branches
• Pull requests (GitHub workflow)
• Merge conflicts
• Merge conflicts
Exercises:
• Create a feature branch
• Merge it back to main
• Simulate and resolve a conflict
INSERTED ADDITIONS (Day 25): professional workflow
• Add:
◦ PR template (even if solo)
◦ basic branch protection habits (conceptual now; enforced later)
Definition of Done: You work like a professional developer.
Day 26 — Python for DevOps (Intro)
New Skills:
• Why Python > Bash for complex tasks
• Variables, lists, dictionaries
• Functions
• File I/O
Exercises:
• Read a log file, count errors
• Parse JSON with json module
• Interact with file system
INSERTED ADDITIONS (Day 26): environment hygiene
• Require:
◦ python -m venv .venv
◦ requirements.txt (even if tiny)
• Introduce structured logging basics (don’t just print()).
Definition of Done: You're comfortable in Python.
Day 27 — Python Automation
New Skills:
• subprocess (run shell commands)
• os, shutil modules
• Error handling (try/except)
• Requests library (HTTP calls)
Exercises:
• Write a Python script that backs up files
• Call an API and parse the response
• Automate a system task
INSERTED ADDITIONS (Day 27): tests + linting
• Add:
◦ pytest basics (3–5 tests)
◦ a formatter/linter (pick one stack and stick to it)
• Define “done” for a Python tool:
◦ runs cleanly
◦ returns correct exit codes
◦ logs meaningful errors
Definition of Done: You automate with Python confidently.
Day 28–30 — Mini Automation Project
Project Ideas:
• Server Health Dashboard: Python script that checks disk, memory, services, writes to
log
• Log Aggregator: Collect logs from multiple sources, parse, alert on errors
• Backup System: Automated, versioned, tested restore
• Backup System: Automated, versioned, tested restore
Requirements:
• Git repository
• README with usage
• Error handling
• Scheduled with cron
INSERTED ADDITIONS (Days 28–30): “production-ready” checklist
• Add requirements:
◦ tests (pytest or equivalent)
◦ lint/format check
◦ clear exit codes
◦ documented “how to run locally”
◦ sample outputs committed (small, sanitized)
Definition of Done: Portfolio-worthy project.
Day 31 — Introduction to AWS
New Skills:
• Cloud mental model (renting computers)
• AWS Free Tier
• Root account security: MFA immediately, never use for daily work
• IAM Identity Center (NOT access keys)
• Billing alerts (set $5 budget)
Exercises:
• Create AWS account
• Enable MFA on root
• Set up IAM Identity Center
• Create billing alarm
INSERTED ADDITIONS (Day 31): audit + baseline security
INSERTED ADDITIONS (Day 31): audit + baseline security
• Add:
◦ Enable CloudTrail (multi-region trail mindset)
◦ Create a “security baseline” document in repo: aws/security-
baseline.md
◦ Tagging policy for all resources
• Start an “account checklist” that you can reuse.
Why This Matters: Security-first approach prevents disasters.
Definition of Done: AWS account is production-secure.
Day 32 — AWS CLI & Authentication
New Skills:
• Install AWS CLI v2
• SSO authentication (IAM Identity Center)
• Profiles (~/.aws/config)
• Never hardcode credentials
Exercises:
• Configure SSO profile
• Run aws s3 ls
• Query EC2 instances
INSERTED ADDITIONS (Day 32): operator verification
• Add:
◦ aws sts get-caller-identity as your “am I who I think I am?”
check
◦ a profile convention: dev, lab, prod (even if prod is conceptual)
Definition of Done: You command AWS from the terminal.
Day 33 — IAM Deep Dive
Day 33 — IAM Deep Dive
New Skills:
• Users vs Roles vs Groups
• Policies (managed vs inline)
• Least privilege principle
• Trust policies
• Service roles
Exercises:
• Create a read-only role
• Attach S3 full-access policy
• Explain why roles > users
INSERTED ADDITIONS (Day 33): real-world role patterns
• Add role patterns you will use later:
◦ EC2 role for SSM access
◦ CI/CD role for GitHub Actions via OIDC (no long-lived AWS keys)
• Document “permissions policy vs trust policy” with examples.
Definition of Done: You architect secure access.
Day 34 — EC2 Foundations
New Skills:
• Launch an instance
• Instance types (t3.micro)
• Security Groups (firewall)
• Key pairs
• User data (bootstrap scripts)
Exercises:
Exercises:
• Launch Ubuntu EC2
• SSH in
• Install Nginx via user data
• Terminate cleanly
INSERTED ADDITIONS (Day 34): production default access
• Add SSM Session Manager variant:
◦ Launch EC2 with SSM permissions
◦ Connect without inbound SSH
• Keep SSH as “break-glass” only, when explicitly needed.
Definition of Done: You map local VM → cloud VM mentally.
Day 35 — S3 & Object Storage
New Skills:
• Buckets, objects, keys
• Public vs private
• Versioning
• Lifecycle policies
• S3 CLI commands
Exercises:
• Create bucket
• Upload 100 files with aws s3 sync
• Enable versioning
• Set lifecycle rule (delete after 30 days)
INSERTED ADDITIONS (Day 35): security + hygiene
• Require:
◦ Block public access (unless explicitly doing a static website lab)
◦ Encryption mindset (SSE as default)
◦ Encryption mindset (SSE as default)
• Add a short note: “S3 is not a filesystem; it is object storage.”
Definition of Done: You understand cloud storage.
PHASE 4 — CLOUD ENGINEERING MASTERY (Days
36–60)
Goal: Build production-grade, automated infrastructure.
Day 36 — VPC Fundamentals
New Skills:
• CIDR blocks
• Subnets (public vs private)
• Route tables
• Internet Gateway
• NAT Gateway
Exercises:
• Design a 3-tier architecture on paper
• Create custom VPC with public/private subnets
• Deploy EC2 in private subnet, test NAT
INSERTED ADDITIONS (Day 36): cost-aware networking
• Explicit rule: NAT Gateway labs are create → verify → destroy unless you accept
ongoing cost.
• Add:
◦ VPC Endpoints (concept + when they reduce NAT dependency)
• Commit a diagram: diagrams/day36-vpc.png or diagrams/day36-
vpc.md
Definition of Done: You design networks.
Definition of Done: You design networks.
Day 37 — Load Balancers & Auto Scaling
New Skills:
• ALB vs NLB
• Target groups
• Health checks
• Auto Scaling Groups
• Launch templates
Exercises:
• Deploy 2 EC2 instances behind ALB
• Configure health checks
• Test auto-scaling (add/remove instances)
INSERTED ADDITIONS (Day 37): operational maturity
• Add:
◦ CloudWatch alarms for basic health (CPU, unhealthy hosts)
◦ a rollback plan (how to revert launch template)
Definition of Done: You build fault-tolerant systems.
Day 38 — RDS & Databases
New Skills:
• RDS vs EC2 with database
• Multi-AZ deployments
• Read replicas
• Backup/restore
• Parameter groups
Exercises:
Exercises:
• Launch MySQL RDS
• Connect from EC2
• Enable automated backups
• Test failover (Multi-AZ)
INSERTED ADDITIONS (Day 38): security baseline
• Require:
◦ encryption at rest
◦ least-privilege SG rules (DB not public)
• Add secret handling concept:
◦ store DB creds in a safer place than plaintext (document your approach)
Definition of Done: You manage databases in the cloud.
Day 39 — Lambda & Serverless
New Skills:
• Event-driven computing
• Lambda functions (Python)
• Triggers (S3, EventBridge)
• Environment variables
• CloudWatch Logs
Exercises:
• Create Lambda that resizes images on S3 upload
• Schedule Lambda with EventBridge
• Debug using CloudWatch
INSERTED ADDITIONS (Day 39): production behaviors
• Require:
◦ structured logging (consistent fields)
◦ error handling strategy (retries / dead-letter concept)
◦ error handling strategy (retries / dead-letter concept)
◦ least privilege IAM policy for the function
Definition of Done: You think event-driven.
Day 40 — CloudFormation Intro
New Skills:
• Infrastructure as Code (IaC)
• YAML templates
• Stacks, changesets
• Parameters, outputs
• Drift detection
Exercises:
• Deploy VPC with CloudFormation
• Update stack
• Delete stack cleanly
INSERTED ADDITIONS (Day 40): verification and drift
• Require:
◦ change set review before apply
◦ drift detection note: what it means and why it matters
Definition of Done: You declare infrastructure.
Day 41–43 — Terraform Foundations
Day 41: Core Concepts
• HCL syntax
• Providers, resources
• terraform init/plan/apply
• State files (local vs remote)
• State files (local vs remote)
INSERTED ADDITIONS (Day 41): baseline quality
• Add:
◦ terraform fmt and terraform validate as mandatory steps
◦ a consistent naming + tagging convention
Day 42: Modules & Variables
• Input variables
• Outputs
• Reusable modules
• Workspaces
INSERTED ADDITIONS (Day 42): module discipline
• Add:
◦ “module interface contract” (inputs/outputs documented)
◦ avoid hardcoding region/account details where possible
Day 43: Real Deployment
• Deploy 3-tier VPC with Terraform
• EC2 + RDS + ALB
• Remote state (S3 + DynamoDB)
INSERTED ADDITIONS (Day 43): security + scanning
• Add:
◦ IaC scanning step (Trivy or equivalent)
◦ “plan output saved” pattern (don’t apply blind)
• Document remote state risks + why locking matters.
Definition of Done: You code infrastructure professionally.
Day 44–46 — Docker Mastery
Day 44: Containers 101
Day 44: Containers 101
• Images vs containers
• docker run/build/push
• Dockerfile syntax
• Layers, caching
INSERTED ADDITIONS (Day 44): image hygiene
• Add:
◦ minimal base images mindset
◦ multi-stage builds awareness
◦ image scanning (Trivy) concept
Day 45: Multi-container Apps
• Docker Compose
• Networking between containers
• Volumes, persistence
INSERTED ADDITIONS (Day 45): data handling
• Require:
◦ volumes used intentionally
◦ documented persistence strategy
Day 46: Deploy to ECS
• ECR (container registry)
• ECS Fargate
• Task definitions
• Service auto-scaling
INSERTED ADDITIONS (Day 46): operations
• Add:
◦ health check strategies
◦ log routing expectations (CloudWatch)
Definition of Done: You containerize applications.
Definition of Done: You containerize applications.
Day 47–49 — CI/CD Pipelines
Day 47: Git Workflows
• Trunk-based development
• Pull request reviews
• Branch protection
INSERTED ADDITIONS (Day 47): enforceable quality gates
• Define PR requirements:
◦ lint/test must pass
◦ IaC scan must pass
◦ plan must be attached for infra changes
Day 48: GitHub Actions
• Workflow syntax
• Secrets management
• Lint → Test → Build → Deploy pipeline
INSERTED ADDITIONS (Day 48): modern AWS auth
• Use GitHub Actions → AWS with OIDC (no long-lived AWS keys stored as secrets).
• Add cost visibility: Infracost (or equivalent) in CI for Terraform cost deltas.
Day 49: Production Pipeline
• Deploy Terraform via CI/CD
• Automated testing
• Rollback strategies
INSERTED ADDITIONS (Day 49): safe apply
• Require:
◦ plan on PR
◦ apply only on protected branch with environment approvals
◦ manual approval gate for production-like environments
◦ manual approval gate for production-like environments
Definition of Done: Deployments are automated.
Day 50–52 — Monitoring & Observability
Day 50: CloudWatch
• Metrics, alarms
• Dashboards
• Logs Insights queries
INSERTED ADDITIONS (Day 50): dashboards as code
• Store dashboard definitions / key queries in repo under observability/
cloudwatch/.
Day 51: Application Monitoring
• Custom metrics
• X-Ray tracing
• Error tracking
INSERTED ADDITIONS (Day 51): SLIs/SLOs mindset
• Write down 2–3 SLIs for your app (latency, error rate, availability).
Day 52: Incident Response
• SNS alerting
• Runbooks
• Post-mortem templates
INSERTED ADDITIONS (Day 52): drill
• Run a controlled failure:
◦ take down one service
◦ confirm alert
◦ follow runbook
◦ document resolution
◦ document resolution
Definition of Done: You operate production systems.
Day 53–55 — Kubernetes (Optional but Recommended)
Day 53: K8s Concepts
• Pods, deployments, services
• kubectl basics
• Local cluster (minikube)
INSERTED ADDITIONS (Day 53): cost realism
• Default: local Kubernetes (kind/minikube)
• EKS is optional and should be treated as short-lived demo infrastructure.
Day 54: Helm & Configurations
• ConfigMaps, Secrets
• Helm charts
• Persistent volumes
Day 55: EKS Deployment
• Deploy app to AWS EKS
• Ingress controllers
• Auto-scaling
INSERTED ADDITIONS (Day 55): explicit destroy policy
• If you do EKS: create → validate → document → destroy.
Definition of Done: You orchestrate containers.
Day 56–58 — Capstone Project
Build: Production-Grade 3-Tier App
Original Requirements:
Original Requirements:
• Terraform-managed VPC, EC2 Auto Scaling, RDS
• Dockerized application on ECS
• CI/CD pipeline (GitHub Actions)
• CloudWatch monitoring
• Security: IAM roles, Security Groups, encrypted RDS
• Cost: Under $10/month
INSERTED ADDITIONS (Days 56–58): two-mode capstone (keeps it realistic)
You build both:
Mode A — Production-Grade Demo (ephemeral)
• Full architecture as listed
• Designed to be spun up for demos and destroyed afterward:
◦ make up / make down (or scripts)
◦ terraform apply / terraform destroy guaranteed clean
Mode B — Budget Mode (always-on minimalist)
• Adjust architecture to keep ongoing spend minimal:
◦ reduce always-on components
◦ avoid cost-heavy networking patterns unless essential
• You document tradeoffs explicitly in the README.
Documentation:
• Architecture diagram
• README with setup instructions
• Terraform documentation
• Runbook for operations
INSERTED ADDITIONS (Days 56–58): required artifacts
• diagrams/capstone-architecture.(png|md)
• runbooks/capstone-ops.md
• observability/alerts.md (what alarms exist and why)
• observability/alerts.md (what alarms exist and why)
• “Cost notes” section in README:
◦ what costs money continuously
◦ what is ephemeral
◦ how you keep it under control
Definition of Done: Interview-ready project.
Day 59 — Resume & Portfolio
Activities:
• Translate technical work into resume bullets (STAR method)
• LinkedIn optimization
• GitHub profile (pinned repos, READMEs)
• Personal website (optional but impressive)
INSERTED ADDITIONS (Day 59): packaging
• Pin 2–3 repos/projects (capstone + automation project)
• Add “Architecture + Runbook + CI screenshots” to README(s)
• Draft 6–10 STAR bullets while evidence is fresh.
Definition of Done: Application materials are polished.
Day 60 — Final Review & Next Steps
Reflection:
• Review all 60 days
• Identify strengths (double down)
• Identify gaps (fill before interviews)
INSERTED ADDITIONS (Day 60): final Well-Architected reflection
• Write your final 1-page reflection across the six pillars
• Identify:
• Identify:
◦ your strongest pillar
◦ your weakest pillar
◦ a 30-day improvement plan
Choose Your Path:
• Security Track: AWS Security Specialty, IAM deep dive
• DevOps Track: Advanced CI/CD, GitOps
• Solutions Architect Track: Multi-region, disaster recovery
• Data Engineer Track: Redshift, Glue, EMR
Job Search Strategy:
• Apply to 10 jobs/day
• Customize each application
• Practice behavioral stories
• Technical interview prep (Terraform scenarios, debugging scenarios)
Definition of Done: You're a Cloud Engineer.
HOW WE USE THIS PLAN (Updated)
At every session start, you say:
"Today is Day X. We're doing ______."
You’ll provide:
• Detailed commands
• Exercises with solutions
• Troubleshooting scenarios
• Why it matters context
• Definition of done checklist
INSERTED ADDITION:
I will also require you to produce:
INSERTED ADDITION:
I will also require you to produce:
• notes/dayXX.md (using the template)
• a committed artifact (script/config/diagram/runbook) each day
• one forced-failure recovery write-up each day
DAILY LEARNING STRUCTURE (Updated)
Each day follows this rhythm:
1 Active Recall (5 min): Quiz yourself on yesterday's commands
2 New Input (15 min): Learn new concepts
3 Hands-On Build (30 min): Type everything manually, no copy/paste
4 Documentation (10 min): Write what you learned in your own words
INSERTED ADDITION:
5. Forced Failure Drill (5–10 min): Break one thing, recover, document it
WEEKLY CADENCE (Updated)
• Mon–Thu: New skills
• Fri: Integration day (combine the week's skills)
• Sat: Rebuild Saturday (recreate the week's work from memory, timed)
• Sun: Rest or review weak areas
INSERTED ADDITION: Weekly deliverables
• 1 architecture one-pager (problem, constraints, tradeoffs, failure modes)
• 1 incident drill write-up
• 1 resume bullet drafted
SUCCESS METRICS (Updated)
You're on track if:
You're on track if:
• You can explain concepts to a non-technical person
• You fix errors without Googling every step
• You build things from memory, not copy/paste
• You're documenting your work in Git
INSERTED ADDITION:
• Your repo reads like evidence: runbooks, diagrams, CI, notes, and reproducible builds.
COST MANAGEMENT (Updated)
• AWS Free Tier: Covers most activities
• Budget Alert: Set at $5, alarm at $3
• Daily Cleanup: Terminate resources after each session
• Estimated Total Cost: $0–$20 for entire 60 days
INSERTED ADDITION (Important):
Your capstone’s “production-grade” components are best treated as ephemeral demos unless
you explicitly redesign for always-on budget mode. Cleanup is a hard requirement.
EMERGENCY CONTACTS (Updated)
If stuck for >30 minutes:
1 Check logs (journalctl, CloudWatch)
2 Search AWS documentation
3 Ask in communities (Reddit r/aws, Discord servers)
4 Document the problem (future you will thank you)
INSERTED ADDITION:
5. Open your repo notes/runbooks first — your own documentation becomes your fastest help
desk.
Udemy Course Plan (Integrated with
Enhancements)
Udemy Course Plan (Integrated with
Enhancements)
(Your original list, with usage rules inserted so courses compound rather than compete.)
Strategic Course Management Notes (Enhanced)
• The "Type Everything" Rule: You manually type every command/script into your lab
environment.
• INSERTED: Courses are “lecture”; this 60-day plan is “lab.” Your lab artifacts go to Git
daily.
• INSERTED: After each course module you finish, you must create one concrete artifact
in your repo:
◦ a script, a config, a diagram, or a short runbook.
Phase 1: Foundations (Weeks 0–8)
1 Introduction to Linux - 90 Minute Crash Course (Rick Crisci)
2 Complete Linux Training Course to Get Your Dream IT Job 2026 (Imran Afzal)
3 Bash Scripting and Shell Programming (Jason Cannon)
4 Python Basics Course (Mumshad Mannambeth)
5 Automate the Boring Stuff with Python Programming (Al Sweigart)
6 The Complete Networking Fundamentals Course (David Bombal)
INSERTED: Minimum weekly outputs during Phase 1
• 3 scripts total by end of Week 3
• 1 troubleshooting runbook by end of Week 4
• 1 mini automation project by end of Week 8
Phase 2: Cloud Foundations & Certification (Weeks 9–14)
7 Ultimate AWS Certified Solutions Architect Associate 2026 (Stephane Maarek)
8 Practice Exams | AWS Certified Solutions Architect Associate
INSERTED: Certification study rule
INSERTED: Certification study rule
• Every AWS concept learned must be implemented once via CLI/IaC and documented
(small, fast labs).
Phase 3: The DevOps Toolchain (Weeks 15–20)
9 The Git & Github Bootcamp (Colt Steele)
10 Jenkins: Beginner To Pro (Warp 9 Training) (or GitHub Actions course)
11 HashiCorp Certified: Terraform Associate 2026 (Zeal Vora)
12 Terraform for AWS - Beginner to Expert (Warp 9 Training)
13 Docker Mastery (Bret Fisher)
14 AWS MasterClass: Monitoring and DevOps with AWS CloudWatch (TetraNoodle)
INSERTED: Toolchain reality checks
• CI/CD must use OIDC (no long-lived AWS keys)
• IaC must have fmt/validate/scan gates
• Cost deltas must be visible in PRs (FinOps shift-left)
Phase 4: Job Ready (Weeks 21–26)
15 Kubernetes for the Absolute Beginners (Mumshad Mannambeth)
INSERTED: Kubernetes cost rule
• Local Kubernetes default; cloud clusters only for short-lived demos unless you accept
ongoing spend.
If you want, next session we’ll begin exactly per your protocol:
“Today is Day X. We’re doing ______.”
…and I’ll deliver the exact commands, a forced-failure drill, verification steps, and the Git
artifacts you should commit that day.# **60-Day Linux → Cloud Engineer Transformation Plan**

*Expanded from 30 days to align with the 26-week roadmap's rigor*

---

## **PHASE 1 — CORE LINUX FOUNDATIONS (Days 1–10)**

**Goal:** Command the operating system that powers 90% of cloud infrastructure.

---

### **Day 1 — Server + SSH Handshake** ✅ DONE

* Install Ubuntu Server (VM)
* Networking working
* SSH from Mac using **key-based authentication** (no passwords)
* VS Code Remote-SSH
* Create/edit files remotely

**Definition of Done:** You edit files on Linux from VS Code securely.

---

### **Day 2 — Filesystem Navigation & Mental Mapping**

**New Skills:**
* Absolute vs relative paths (mental model)
* `pwd`, `ls -lah`, `tree`
* `cd` navigation patterns (`..`, `~`, `-`)
* Build `/cloud-labs/day2/{logs,scripts,configs}`

**Exercises:**
* Navigate between 5 nested directories blindfolded (no `ls`)
* Explain Linux filesystem hierarchy: `/etc`, `/var`, `/home`, `/opt`
* Draw the filesystem tree on paper

**Why This Matters:** AWS EBS volumes, Lambda layers, and container filesystems follow this structure.

**Definition of Done:** You can navigate any Linux system without fear.

---

### **Day 3 — File Operations & Disaster Prevention**

**New Skills:**
* `cp -r`, `mv`, `rm -rf` (with `-i` flag for safety)
* Wildcards: `*.log`, `file?.txt`, `[abc]*`
* Hidden files (`.bashrc`, `.ssh/`)
* `touch`, `cat`, `less`, `head`, `tail`

**Exercises:**
* Copy 100 files using wildcards
* Simulate accidental deletion, practice recovery mindset
* Create a "staging" and "production" directory structure

**Why This Matters:** S3 lifecycle policies, log rotation, and backup strategies mirror this.

**Definition of Done:** You move/delete files with zero anxiety.

---

### **Day 4 — Users, Groups, and the Root Divide**

**New Skills:**
* Linux user model (UID, GID)
* `sudo` vs `su`
* `/etc/passwd`, `/etc/group`, `/etc/shadow`
* `whoami`, `id`, `groups`
* Creating users (`adduser`, `usermod`)

**Exercises:**
* Create 3 users: `dev`, `ops`, `auditor`
* Add `dev` to `sudo` group
* Explain why root login is disabled in AWS AMIs

**Why This Matters:** IAM users/roles are the cloud equivalent. Least privilege starts here.

**Definition of Done:** You understand **who** can do **what** and **why**.

---

### **Day 5 — Permissions (The Security Foundation)**

**New Skills:**
* `ls -l` breakdown: `drwxr-xr-x`
* Read (4), Write (2), Execute (1) binary logic
* `chmod` (numeric + symbolic)
* `chown`, `chgrp`
* Sticky bits, SUID, SGID (awareness)

**Exercises:**
* Make a script executable
* Fix a "permission denied" error on a web directory
* Explain why `.ssh/` must be `700` and private keys `600`

**Why This Matters:** S3 bucket policies, IAM permissions, and security groups are permission systems.

**Definition of Done:** You fix permission errors in under 30 seconds.

---

### **Day 6 — Package Management & System Hygiene**

**New Skills:**
* `apt update` vs `apt upgrade` vs `apt full-upgrade`
* Installing/removing packages (`nginx`, `htop`, `tree`, `curl`, `jq`)
* `/etc/apt/sources.list`
* `dpkg -l`, `apt search`

**Exercises:**
* Install Nginx, verify it's running
* Remove a package cleanly
* Simulate a broken dependency scenario

**Why This Matters:** EC2 user data scripts, AMI baking, and container layers use package managers.

**Definition of Done:** You maintain a server without breaking it.

---

### **Day 7 — Text Processing (The Unix Philosophy)**

**New Skills:**
* `grep` (search text)
* `awk` (column processing)
* `sed` (find/replace)
* Pipes (`|`) and redirection (`>`, `>>`, `2>&1`)
* `wc`, `sort`, `uniq`

**Exercises:**
* Find all ERROR lines in a 10,000-line log file
* Extract IP addresses from `/var/log/auth.log`
* Chain 5 commands with pipes

**Why This Matters:** CloudWatch Logs Insights, Athena queries, and log analysis require this mindset.

**Definition of Done:** You can extract meaning from any text file.

---

### **Day 8 — Environment Variables & Shell Customization**

**New Skills:**
* `.bashrc`, `.bash_profile`
* `export PATH`, `alias`
* Environment variables (`$HOME`, `$USER`, `$PATH`)
* Making changes persistent

**Exercises:**
* Create aliases for common commands
* Add `/usr/local/bin` to PATH
* Set `AWS_REGION=us-east-1` permanently

**Why This Matters:** Container environment variables, Lambda config, and Terraform variables work the same way.

**Definition of Done:** Your shell feels like *your* shell.

---

### **Day 9 — Vim Survival Skills**

**New Skills:**
* Why Vim exists on every server
* `i` (insert), `Esc`, `:wq`, `:q!`
* Navigation: `hjkl`, `gg`, `G`
* Delete line (`dd`), undo (`u`)

**Exercises:**
* Edit `/etc/hosts` without panicking
* Fix a config file over SSH when VS Code fails
* Edit a cron job using `crontab -e`

**Why This Matters:** Sometimes you *only* have terminal access. No GUI. No VS Code.

**Definition of Done:** You don't fear Vim.

---

### **Day 10 — Phase 1 Review & Rebuild Challenge**

**Challenge:**
* Destroy your VM
* Rebuild from scratch in under 60 minutes:
  * SSH keys
  * 3 users with specific permissions
  * Directory structure
  * Nginx installed
  * Custom `.bashrc`

**Reflection:**
* Explain the Linux filesystem to someone non-technical
* Identify what you'd automate (foreshadowing scripting)

**Definition of Done:** You **understand**, not just followed steps.

---

## **PHASE 2 — SYSTEM OPERATIONS & TROUBLESHOOTING (Days 11–20)**

**Goal:** Operate and troubleshoot production-grade systems.

---

### **Day 11 — Processes & Resource Management**

**New Skills:**
* `ps aux`, `pgrep`, `pidof`
* `top`, `htop` (real-time monitoring)
* Foreground vs background jobs
* `kill`, `killall`, `pkill`
* Process states (running, sleeping, zombie)

**Exercises:**
* Find the PID of Nginx
* Run a script in the background
* Kill a runaway process

**Why This Matters:** EC2 auto-scaling, Lambda concurrency limits, and ECS task management.

**Definition of Done:** You control running processes confidently.

---

### **Day 12 — Services & systemd**

**New Skills:**
* `systemctl status/start/stop/restart`
* `systemctl enable/disable` (boot persistence)
* `/etc/systemd/system/`
* Reading service logs: `journalctl -u nginx`

**Exercises:**
* Make Nginx start on boot
* Create a custom service for a Python script
* Troubleshoot a failed service

**Why This Matters:** ECS services, Kubernetes deployments, and auto-recovery mechanisms.

**Definition of Done:** You understand "why servers start themselves."

---

### **Day 13 — Logs & Debugging**

**New Skills:**
* `/var/log/` structure
* `journalctl` mastery
* `tail -f` for live logs
* Log rotation (`logrotate`)

**Exercises:**
* Diagnose why SSH login failed
* Find when a service last crashed
* Set up custom application logging

**Why This Matters:** CloudWatch Logs, X-Ray traces, and incident response.

**Definition of Done:** You read errors like a detective.

---

### **Day 14 — Disk & Storage Management**

**New Skills:**
* `df -h`, `du -sh`
* Inodes (`df -i`)
* Mount points (`mount`, `/etc/fstab`)
* Finding large files (`find`, `ncdu`)

**Exercises:**
* Identify what's consuming disk space
* Simulate disk pressure (fill `/var`)
* Add a second disk and mount it

**Why This Matters:** EBS volumes, S3 storage classes, disk IOPS limits.

**Definition of Done:** You prevent "disk full" disasters.

---

### **Day 15 — Memory & Swap**

**New Skills:**
* `free -h`
* Virtual memory concepts
* Swap configuration
* OOM (Out of Memory) killer

**Exercises:**
* Check memory usage
* Create a swap file
* Simulate memory pressure

**Why This Matters:** EC2 instance sizing, RDS memory configs, Lambda memory limits.

**Definition of Done:** You understand why servers crash under load.

---

### **Day 16 — Networking Fundamentals**

**New Skills:**
* IP addresses, subnets, CIDR notation
* `ip addr`, `ip route`
* `ping`, `traceroute`
* `/etc/hosts`, `/etc/resolv.conf`
* DNS resolution (`dig`, `nslookup`)

**Exercises:**
* Explain 10.0.1.0/24
* Trace the path to google.com
* Diagnose a DNS failure

**Why This Matters:** VPCs, subnets, route tables, and Security Groups.

**Definition of Done:** You speak networking fluently.

---

### **Day 17 — Ports & Services**

**New Skills:**
* What ports are (0–65535)
* Well-known ports (22, 80, 443, 3306)
* `ss -tuln`, `netstat -tuln`
* `curl`, `wget`, `nc` (netcat)

**Exercises:**
* Find what's listening on port 80
* Test if port 22 is open remotely
* Bind a Python HTTP server to port 8000

**Why This Matters:** Security Groups, NACLs, and load balancer target groups.

**Definition of Done:** Ports make intuitive sense.

---

### **Day 18 — Firewalls (ufw)**

**New Skills:**
* `ufw enable/disable`
* `ufw allow/deny` rules
* Default policies
* Application profiles

**Exercises:**
* Allow SSH, HTTP, HTTPS
* Deny all other inbound
* Block a specific IP

**Why This Matters:** Security Groups are cloud firewalls.

**Definition of Done:** You harden servers by default.

---

### **Day 19 — SSH Hardening & Key Management**

**New Skills:**
* Disable password authentication
* Change default SSH port
* `~/.ssh/config` for multiple servers
* SSH agent forwarding (cautiously)
* Fail2ban awareness

**Exercises:**
* Lock down SSH completely
* Create SSH config for 3 servers
* Generate a new key pair for GitHub

**Why This Matters:** AWS Systems Manager, bastion hosts, and zero-trust architecture.

**Definition of Done:** Your SSH config is production-grade.

---

### **Day 20 — Phase 2 Capstone: Deploy a Web App**

**Challenge:**
* Deploy a Python Flask app
* Nginx as reverse proxy
* systemd service for auto-restart
* Firewall configured
* Custom domain (using `/etc/hosts` if no real domain)

**Documentation:**
* Write a `README.md` explaining the architecture
* Include troubleshooting steps

**Definition of Done:** You built something real.

---

## **PHASE 3 — AUTOMATION & VERSION CONTROL (Days 21–35)**

**Goal:** Stop doing things manually. Everything as code.

---

### **Day 21 — Bash Scripting Basics**

**New Skills:**
* Shebang (`#!/bin/bash`)
* Variables, quotes
* `$1`, `$2` (arguments)
* Exit codes (`$?`)
* Conditionals (`if`, `else`)

**Exercises:**
* Write a backup script
* Accept user input
* Check if a file exists before deleting

**Why This Matters:** EC2 user data, Lambda functions, and automation.

**Definition of Done:** You write functional scripts.

---

### **Day 22 — Bash Scripting Advanced**

**New Skills:**
* Loops (`for`, `while`)
* Functions
* Arrays
* Error handling (`set -e`, `trap`)

**Exercises:**
* Process 100 files in a loop
* Write a health-check script
* Create a deployment script

**Definition of Done:** Your scripts are robust.

---

### **Day 23 — Cron & Scheduling**

**New Skills:**
* `crontab -e`
* Cron syntax (`* * * * *`)
* `/etc/cron.d/`, `/etc/cron.daily/`
* Logging cron jobs

**Exercises:**
* Schedule a backup every night at 2 AM
* Run a script every 15 minutes
* Email results (using `mail` or logging)

**Why This Matters:** EventBridge, Lambda scheduled triggers, and automated tasks.

**Definition of Done:** Automation runs without you.

---

### **Day 24 — Git Fundamentals**

**New Skills:**
* `git init`, `git clone`
* `git add`, `git commit`, `git push`
* `.gitignore`
* Commit messages (conventional commits)

**Exercises:**
* Initialize a repo for your scripts
* Make 10 commits
* Push to GitHub

**Why This Matters:** CodeCommit, infrastructure versioning, and collaboration.

**Definition of Done:** Every script is version-controlled.

---

### **Day 25 — Git Branching & Collaboration**

**New Skills:**
* `git branch`, `git checkout`, `git merge`
* Feature branches
* Pull requests (GitHub workflow)
* Merge conflicts

**Exercises:**
* Create a feature branch
* Merge it back to `main`
* Simulate and resolve a conflict

**Why This Matters:** CI/CD pipelines, Terraform environments, and team workflows.

**Definition of Done:** You work like a professional developer.

---

### **Day 26 — Python for DevOps (Intro)**

**New Skills:**
* Why Python > Bash for complex tasks
* Variables, lists, dictionaries
* Functions
* File I/O

**Exercises:**
* Read a log file, count errors
* Parse JSON with `json` module
* Interact with file system

**Why This Matters:** Boto3 (AWS SDK), automation scripts, and Lambda functions.

**Definition of Done:** You're comfortable in Python.

---

### **Day 27 — Python Automation**

**New Skills:**
* `subprocess` (run shell commands)
* `os`, `shutil` modules
* Error handling (`try/except`)
* Requests library (HTTP calls)

**Exercises:**
* Write a Python script that backs up files
* Call an API and parse the response
* Automate a system task

**Why This Matters:** AWS Lambda, API integrations, and infrastructure automation.

**Definition of Done:** You automate with Python confidently.

---

### **Day 28–30 — Mini Automation Project**

**Project Ideas:**
* **Server Health Dashboard:** Python script that checks disk, memory, services, writes to log
* **Log Aggregator:** Collect logs from multiple sources, parse, alert on errors
* **Backup System:** Automated, versioned, tested restore

**Requirements:**
* Git repository
* README with usage
* Error handling
* Scheduled with cron

**Definition of Done:** Portfolio-worthy project.

---

### **Day 31 — Introduction to AWS**

**New Skills:**
* Cloud mental model (renting computers)
* AWS Free Tier
* **Root account security:** MFA immediately, never use for daily work
* **IAM Identity Center** (NOT access keys)
* Billing alerts (set $5 budget)

**Exercises:**
* Create AWS account
* Enable MFA on root
* Set up IAM Identity Center
* Create billing alarm

**Why This Matters:** Security-first approach prevents disasters.

**Definition of Done:** AWS account is production-secure.

---

### **Day 32 — AWS CLI & Authentication**

**New Skills:**
* Install AWS CLI v2
* SSO authentication (IAM Identity Center)
* Profiles (`~/.aws/config`)
* Never hardcode credentials

**Exercises:**
* Configure SSO profile
* Run `aws s3 ls`
* Query EC2 instances

**Why This Matters:** Every cloud engineer lives in the CLI.

**Definition of Done:** You command AWS from the terminal.

---

### **Day 33 — IAM Deep Dive**

**New Skills:**
* Users vs Roles vs Groups
* Policies (managed vs inline)
* Least privilege principle
* Trust policies
* Service roles

**Exercises:**
* Create a read-only role
* Attach S3 full-access policy
* Explain why roles > users

**Why This Matters:** IAM is the foundation of cloud security.

**Definition of Done:** You architect secure access.

---

### **Day 34 — EC2 Foundations**

**New Skills:**
* Launch an instance
* Instance types (t3.micro)
* Security Groups (firewall)
* Key pairs
* User data (bootstrap scripts)

**Exercises:**
* Launch Ubuntu EC2
* SSH in
* Install Nginx via user data
* Terminate cleanly

**Why This Matters:** EC2 = Linux skills + cloud networking.

**Definition of Done:** You map local VM → cloud VM mentally.

---

### **Day 35 — S3 & Object Storage**

**New Skills:**
* Buckets, objects, keys
* Public vs private
* Versioning
* Lifecycle policies
* S3 CLI commands

**Exercises:**
* Create bucket
* Upload 100 files with `aws s3 sync`
* Enable versioning
* Set lifecycle rule (delete after 30 days)

**Why This Matters:** S3 backs most AWS services.

**Definition of Done:** You understand cloud storage.

---

## **PHASE 4 — CLOUD ENGINEERING MASTERY (Days 36–60)**

**Goal:** Build production-grade, automated infrastructure.

---

### **Day 36 — VPC Fundamentals**

**New Skills:**
* CIDR blocks
* Subnets (public vs private)
* Route tables
* Internet Gateway
* NAT Gateway

**Exercises:**
* Design a 3-tier architecture on paper
* Create custom VPC with public/private subnets
* Deploy EC2 in private subnet, test NAT

**Why This Matters:** Every AWS deployment lives in a VPC.

**Definition of Done:** You design networks.

---

### **Day 37 — Load Balancers & Auto Scaling**

**New Skills:**
* ALB vs NLB
* Target groups
* Health checks
* Auto Scaling Groups
* Launch templates

**Exercises:**
* Deploy 2 EC2 instances behind ALB
* Configure health checks
* Test auto-scaling (add/remove instances)

**Why This Matters:** High availability is non-negotiable.

**Definition of Done:** You build fault-tolerant systems.

---

### **Day 38 — RDS & Databases**

**New Skills:**
* RDS vs EC2 with database
* Multi-AZ deployments
* Read replicas
* Backup/restore
* Parameter groups

**Exercises:**
* Launch MySQL RDS
* Connect from EC2
* Enable automated backups
* Test failover (Multi-AZ)

**Why This Matters:** Data persistence is critical.

**Definition of Done:** You manage databases in the cloud.

---

### **Day 39 — Lambda & Serverless**

**New Skills:**
* Event-driven computing
* Lambda functions (Python)
* Triggers (S3, EventBridge)
* Environment variables
* CloudWatch Logs

**Exercises:**
* Create Lambda that resizes images on S3 upload
* Schedule Lambda with EventBridge
* Debug using CloudWatch

**Why This Matters:** Serverless = infrastructure you don't manage.

**Definition of Done:** You think event-driven.

---

### **Day 40 — CloudFormation Intro**

**New Skills:**
* Infrastructure as Code (IaC)
* YAML templates
* Stacks, changesets
* Parameters, outputs
* Drift detection

**Exercises:**
* Deploy VPC with CloudFormation
* Update stack
* Delete stack cleanly

**Why This Matters:** Manual console work doesn't scale.

**Definition of Done:** You declare infrastructure.

---

### **Day 41–43 — Terraform Foundations**

**Day 41: Core Concepts**
* HCL syntax
* Providers, resources
* `terraform init/plan/apply`
* State files (local vs remote)

**Day 42: Modules & Variables**
* Input variables
* Outputs
* Reusable modules
* Workspaces

**Day 43: Real Deployment**
* Deploy 3-tier VPC with Terraform
* EC2 + RDS + ALB
* Remote state (S3 + DynamoDB)

**Why This Matters:** Terraform is industry standard for multi-cloud.

**Definition of Done:** You code infrastructure professionally.

---

### **Day 44–46 — Docker Mastery**

**Day 44: Containers 101**
* Images vs containers
* `docker run/build/push`
* Dockerfile syntax
* Layers, caching

**Day 45: Multi-container Apps**
* Docker Compose
* Networking between containers
* Volumes, persistence

**Day 46: Deploy to ECS**
* ECR (container registry)
* ECS Fargate
* Task definitions
* Service auto-scaling

**Why This Matters:** Containers are the deployment standard.

**Definition of Done:** You containerize applications.

---

### **Day 47–49 — CI/CD Pipelines**

**Day 47: Git Workflows**
* Trunk-based development
* Pull request reviews
* Branch protection

**Day 48: GitHub Actions**
* Workflow syntax
* Secrets management
* Lint → Test → Build → Deploy pipeline

**Day 49: Production Pipeline**
* Deploy Terraform via CI/CD
* Automated testing
* Rollback strategies

**Why This Matters:** Manual deployments are career-limiting.

**Definition of Done:** Deployments are automated.

---

### **Day 50–52 — Monitoring & Observability**

**Day 50: CloudWatch**
* Metrics, alarms
* Dashboards
* Logs Insights queries

**Day 51: Application Monitoring**
* Custom metrics
* X-Ray tracing
* Error tracking

**Day 52: Incident Response**
* SNS alerting
* Runbooks
* Post-mortem templates

**Why This Matters:** You can't fix what you can't see.

**Definition of Done:** You operate production systems.

---

### **Day 53–55 — Kubernetes (Optional but Recommended)**

**Day 53: K8s Concepts**
* Pods, deployments, services
* `kubectl` basics
* Local cluster (minikube)

**Day 54: Helm & Configurations**
* ConfigMaps, Secrets
* Helm charts
* Persistent volumes

**Day 55: EKS Deployment**
* Deploy app to AWS EKS
* Ingress controllers
* Auto-scaling

**Why This Matters:** Many companies use Kubernetes.

**Definition of Done:** You orchestrate containers.

---

### **Day 56–58 — Capstone Project**

**Build: Production-Grade 3-Tier App**

**Requirements:**
* Terraform-managed VPC, EC2 Auto Scaling, RDS
* Dockerized application on ECS
* CI/CD pipeline (GitHub Actions)
* CloudWatch monitoring
* Security: IAM roles, Security Groups, encrypted RDS
* Cost: Under $10/month

**Documentation:**
* Architecture diagram
* README with setup instructions
* Terraform documentation
* Runbook for operations

**Why This Matters:** This is your portfolio centerpiece.

**Definition of Done:** Interview-ready project.

---

### **Day 59 — Resume & Portfolio**

**Activities:**
* Translate technical work into resume bullets (STAR method)
* LinkedIn optimization
* GitHub profile (pinned repos, READMEs)
* Personal website (optional but impressive)

**Example Bullet:**
*"Automated deployment of fault-tolerant 3-tier architecture using Terraform, reducing provisioning time from 4 hours to 12 minutes while cutting infrastructure costs 40% through right-sized Auto Scaling policies."*

**Definition of Done:** Application materials are polished.

---

### **Day 60 — Final Review & Next Steps**

**Reflection:**
* Review all 60 days
* Identify strengths (double down)
* Identify gaps (fill before interviews)

**Choose Your Path:**
* **Security Track:** AWS Security Specialty, IAM deep dive
* **DevOps Track:** Advanced CI/CD, GitOps
* **Solutions Architect Track:** Multi-region, disaster recovery
* **Data Engineer Track:** Redshift, Glue, EMR

**Job Search Strategy:**
* Apply to 10 jobs/day
* Customize each application
* Practice behavioral stories
* Technical interview prep (LeetCode for cloud: Terraform scenarios)

**Definition of Done:** You're a Cloud Engineer.

---

## **HOW WE USE THIS PLAN**

At every session start, you say:

> **"Today is Day X. We're doing ______."**

I'll provide:
* Detailed commands
* Exercises with solutions
* Troubleshooting scenarios
* Why it matters context
* Definition of done checklist

---

## **DAILY LEARNING STRUCTURE**

Each day follows this rhythm:

1. **Active Recall** (5 min): Quiz yourself on yesterday's commands
2. **New Input** (15 min): Learn new concepts
3. **Hands-On Build** (30 min): Type everything manually, no copy/paste
4. **Documentation** (10 min): Write what you learned in your own words

---

## **WEEKLY CADENCE**

* **Mon-Thu:** New skills
* **Fri:** Integration day (combine the week's skills)
* **Sat:** Rebuild Saturday (recreate the week's work from memory, timed)
* **Sun:** Rest or review weak areas

---

## **SUCCESS METRICS**

You're on track if:
* You can explain concepts to a non-technical person
* You fix errors without Googling every step
* You build things from memory, not copy/paste
* You're documenting your work in Git

---

## **COST MANAGEMENT**

* **AWS Free Tier:** Covers most activities
* **Budget Alert:** Set at $5, alarm at $3
* **Daily Cleanup:** Terminate resources after each session
* **Estimated Total Cost:** $0-$20 for entire 60 days

---

## **EMERGENCY CONTACTS**

If stuck for >30 minutes:
1. Check logs (`journalctl`, CloudWatch)
2. Search AWS documentation
3. Ask in communities (Reddit r/aws, Discord servers)
4. Document the problem (future you will thank you)

---
