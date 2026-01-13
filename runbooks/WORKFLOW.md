# WORKFLOW.md — My 60-Day Project “Operating System” (Single Source of Truth)

If I stop for days/weeks/months, I open THIS file first. It tells me exactly:
- how to start a session,
- what to paste into ChatGPT,
- how to do the work,
- how to end the session,
- how to save progress so I never lose my place.

Think of this file like a video game instruction manual + save/load rules.

---

## 0) The Big Idea (Simple)

This project has three “memory systems”:

1) `MASTER_PLAN.md` = the roadmap (what each day covers).
2) `CURRENT_STATE.md` = the save file (where I am right now + what’s next).
3) `notes/dayXX.md` = the proof (what I did today, what broke, what I fixed).

ChatGPT is the helper, but these files are the real memory.

---

## 1) Where Everything Lives (So I Don’t Get Lost)

### My repo (on my Mac)
Repo folder path:
- `~/cloud-labs/cloud-engineer-60d`
Full path example:
- `/Users/prkr/cloud-labs/cloud-engineer-60d`

### IMPORTANT: Mac vs Ubuntu VM
- If my terminal prompt looks like `prkr@MacBookPro ... %` → I’m on my Mac.
- If it looks like `cloud@ubuntu-lab:~$` → I’m inside the Ubuntu VM (SSH).

Most repo documents (`CURRENT_STATE.md`, `notes/`, `runbooks/`) should be edited on the Mac, not inside SSH.

---

## 2) What Each File Is For (No Confusion)

### `MASTER_PLAN.md`
- Contains the full 60-day plan.
- Rarely changes.
- When I forget what Day 17 is, I read this.

### `CURRENT_STATE.md` (MOST IMPORTANT)
- Contains ONE snapshot of my current situation.
- Answers: “Where am I? What’s done? What do I do next?”
- I update it at the end of every session.
- It should be short and clear.

### `notes/dayXX.md`
- My daily notes.
- Contains the details: commands I used, problems I hit, fixes.
- This is where “history” lives.

### `runbooks/WORKFLOW.md` (THIS FILE)
- The step-by-step operating instructions.
- I follow it like a checklist.

---

## 3) Start of Day / Start of Session (LOAD THE SAVE)

### Goal of this section:
Get myself back into the project fast with zero guessing.

### Step 1 — Start the Ubuntu VM
1) Open VirtualBox.
2) Start the Ubuntu Server VM.
3) Wait until it finishes booting.

### Step 2 — Open Mac Terminal
Open Mac Terminal (standalone) or VS Code Terminal. Either works.

### Step 3 — Go to the repo root
In Terminal:
```bash
cd ~/cloud-labs/cloud-engineer-60d
pwd
`
### New Chat
cd ~/cloud-labs/cloud-engineer-60d
git pull
cat CURRENT_STATE.md
