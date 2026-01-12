# Day 01 — Repo Setup + Structure

## Goal
Create my project folder and set it up as a Git repo so I can track progress and keep daily notes organized.

## What I learned
- A "repo" is just a normal folder with Git tracking turned on (a time machine for files).
- VS Code is my workspace, and the Terminal is where I run commands.
- "Folders" keep things organized. I will use notes/ for daily write-ups, runbooks/ for how-to guides, and configs/ for settings files.

## Commands I used
- pwd (shows where I am)
  Example: pwd
- cd (move to a folder)
  Example: cd ~
  Example: cd ~/cloud-labs/cloud-engineer-60d
- mkdir -p (make folders, including any missing parents)
  Example: mkdir -p cloud-labs/cloud-engineer-60d
  Example: mkdir -p notes runbooks configs
- git init (turn this folder into a repo)
  Example: git init
- touch (create empty files)
  Example: touch README.md notes/_template.md notes/day01.md
- ls (list what’s in a folder)
  Example: ls
  Example: ls notes

## What I broke / got wrong
- I typed the touch command wrong the first time and got:
  "No such file or directory"
  because I accidentally merged two file paths together.

## How I fixed it
1) I created the notes folder first using:
   mkdir -p notes runbooks configs
2) I re-ran the touch command correctly:
   touch README.md notes/_template.md notes/day01.md
3) I verified the files exist:
   ls
   ls notes

## Proof
Paste your command output here after running:
- pwd
- ls
- ls notes

## Screenshot
![Day 01](../assets/screenshots/day01-ubuntu-setup.png)




