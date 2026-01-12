# Repo Setup Runbook (Mac + VS Code)

## Goal
Create a repo folder with a clean structure for daily notes.

## Steps
1) Open VS Code
2) Terminal → New Terminal
3) Go home:
   - cd ~
4) Create project folder:
   - mkdir -p cloud-labs/cloud-engineer-60d
5) Enter it:
   - cd ~/cloud-labs/cloud-engineer-60d
6) Turn it into a repo:
   - git init
7) Create structure:
   - mkdir -p notes runbooks configs
8) Create starter files:
   - touch README.md notes/_template.md notes/day01.md
9) Save work:
   - File → Save All
10) Track + commit:
   - git add .
   - git commit -m "Initial repo structure"
11) Verify:
   - git log --oneline --max-count=3
   - git status

## Common Problems
- If you see dquote>:
  - You started a " quote and didn’t finish it.
  - Press Ctrl+C to cancel.
