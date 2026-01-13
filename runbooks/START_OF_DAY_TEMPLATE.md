# Start of Day Checklist (Copy/Paste into Terminal + ChatGPT)

Use this at the start of each day to restore context fast and start the right task.

---

## A) Pre-Flight (2 minutes)

1) Start Ubuntu VM (VirtualBox) and wait until it finishes booting.

2) Go to repo and sync:
- Command(s):
  - cd ~/cloud-labs/cloud-engineer-60d
  - git pull
  - git status
- Expected:
  - On branch main
  - Up to date with origin/main

3) Confirm your location:
- Command:
  - pwd
- Expected:
  - /Users/prkr/cloud-labs/cloud-engineer-60d

---

## B) Confirm the “Save File” (CURRENT_STATE.md)

1) Open:
- Command:
  - nano CURRENT_STATE.md

2) Verify:
- Day: __
- Phase: __
- Next Exact Step: __

(If you changed it, save + exit, then commit at the end of this checklist.)

---

## C) Create Today’s Notes File

1) Create/open:
- Command:
  - nano notes/day__.md

2) Paste this template:

# Day __ — Title

## Objective
- 

## Commands Practiced
- 

## Exercises / Proof
- 

## What Broke / Confused Me
- 

## Fix + Verification
- 

## Key Takeaways
- 

## Next Step
- 

---

## D) VM Connectivity Check (SSH)

1) Check SSH works:
- Command(s):
  - ssh ubuntu-lab
  - hostname
  - whoami
  - ip a | grep "inet "
  - exit

2) Record VM IP (only if it changed):
- VM IP: __

---

## E) Optional: Start-of-Day Commit (only if you edited CURRENT_STATE)

If you changed CURRENT_STATE.md at the start of the day:
- Command(s):
  - git add CURRENT_STATE.md
  - git commit -m "Day __ start state"
  - git push

---

## F) Start ChatGPT Session (Copy/Paste)

Session context:
Here is my CURRENT_STATE.md:
(paste file contents)

Today is Day __. Let’s begin. The goal is to complete the Next Exact Step.


