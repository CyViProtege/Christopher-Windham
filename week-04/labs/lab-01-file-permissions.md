# Week 4 Lab — File Permissions: The Badge Audit (CLI Simulator)

**Student Name:** Christopher Windham

**Date Completed:** 08/15/2026

**Module:** 1 — Digital Infrastructure & CLI | **Week:** 4  
**Submission Path:** `week-04/labs/lab-01-file-permissions.md`

---

## Overview

Lesson 1 revealed what Ivy's badge rings mean: every file carries permissions — who may read, write, and execute it — and one wrongly-set ring can be a whole security incident. This lab has you run a small permissions audit of your own in the CLI Simulator: read the rings on a set of seeded files (Part A), fix three problems deliberately with `chmod` (Part B), and read the same story on the Windows side with `Get-Acl` (Part C). One screenshot from this lab becomes part of ★ Deliverable 1.

**Nothing here can break anything real.** The CLI Simulator is a consequence-free practice space — exactly the right place to change permissions for the first time.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | CyberFoundations CLI Simulator (browser-based, inside the Lab Portal) — no install, no VM, no real terminal required |
| Shell | Parts A and B use **bash**; Part C uses **PowerShell** |
| Prerequisite | Week 4, Lesson 1 completed |

**Before you start:** log into the Lab Portal, open **Week 4 → CLI Simulator**, and find the **"Foundry District Badge Office"** scenario. You'll see two entries in the list: **Badge Office — Bash** (used for Parts A and B) and **Badge Office — PowerShell** (used for Part C). Start with the Bash one. It seeds a small folder of files whose permissions have… issues. That's the point.

---

## Part A — Read the Rings

### Step 1 — Get the Security Report

Run the long listing (`ls -l`) in your starting folder. This is the same listing from Week 3's `ls`, with one flag added — and that flag turns it into a per-file security report.

Command you ran:

```
ls -l
```

Output (the full listing):

```
-rw-r--r-- 1 morgan foundry    82 badge-codes.txt
-rw-r--r-- 1 morgan foundry    66 cleanup.sh
-rw-rw-rw- 1 morgan foundry   151 master-inventory.txt
-rw-r--r-- 1 morgan foundry    66 shift-notes.txt
-rw-r--r-- 1 morgan foundry    49 supply-list.txt
```

### Step 2 — Decode One File Completely

Pick any one file from your listing and decode its full permission string, audience by audience — owner, group, other — the way Lesson 1 decoded `-rwxr-xr--`. Write it as plain-English sentences ("the owner can…, the group can…, everyone else can…").

The file and its permission string:

```
-rw-rw-rw- 1 morgan foundry   151 master-inventory.txt
```

Your plain-English decode:

```
Looking at this file the far left group, the owner, can read and write to this file. The second group, the group, can read and write, same as the owner. Lastly, the far right group, the "other" group, can also read and write this file.
```

### Step 3 — Find the Problem File

One file in this folder is dramatically more permissive than it should be — every ring lit for every audience, or close to it. Find it. (Hint: scan the *other* triplet — the last three characters — down the whole listing. Which file gives strangers the most?)

The most permissive file and why you flagged it:

```
The most permissive file was definitely the master-inventory.txt file because there were no clear defined rules or permissions set for this important document. It was fair game for everyone within this organization to open and edit how they saw fit.
```

---

## Part B — Change the Rings

This is your first time changing permissions, so we follow THE GATEKEEPER'S RULE on every single change: **check who can touch it now, change it, then check again.** That means `ls -l` before *and after* every `chmod`. The simulator's checklist will verify this — a change without both checks will not pass.

### Step 1 — Lock Down the Problem File

The problem file from Part A, Step 3 should not be writable by the group or by other. Fix it: revoke write from group (`g-w`), then revoke both read and write from other. Run `ls -l` before your first change and after your last one.

Commands you ran (in order, including both ls -l checks):

```
chmod g-w master-inventory.txt
chmod o-rw master-inventory.txt

```

The file's permission string BEFORE and AFTER:

```
-rw-rw-rw- 1 morgan foundry   151 master-inventory.txt
-rw-r--rw- 1 morgan foundry   151 master-inventory.txt
-rw-r----- 1 morgan foundry   151 master-inventory.txt
```

### Step 2 — Make a Script Runnable

The scenario seeds a script (its name ends in `.sh`) that its owner cannot currently execute. Grant the owner execute — and only the owner. Verify with `ls -l` after.

Commands you ran:

```
chmod 744 cleanup.sh
```

Output (the script's corrected permission string):

```
-rwxr--r-- 1 morgan foundry    66 cleanup.sh
```

### Step 3 — Protect the Secrets File

There's a file whose name makes clear it shouldn't be readable by everyone. Revoke other's read. Verify.

Commands you ran:

```
chmod o-r badge-codes.txt
```

Output (the corrected permission string):

```
-rw-r----- 1 morgan foundry    82 badge-codes.txt
```

### Step 4 — Capture Your Audit Evidence (REQUIRED screenshot)

**Read this first — it will save you some confusion.** Every challenge in the CLI Simulator hands you a **fresh copy** of the Badge Office folder. That's deliberate: each challenge is its own clean exercise. You'll see `(fresh filesystem for this challenge)` on the divider line each time. It also means your fixes from earlier challenges will *not* still be showing when you reach the last one. Nothing you did was undone, and nothing you did was wrong — the folder was simply reset.

So build your evidence listing **inside the last challenge**. Apply all three fixes there, one after another, then run one final `ls -l`:

```
chmod g-w master-inventory.txt
chmod o-rw master-inventory.txt
chmod u+x cleanup.sh
chmod o-r badge-codes.txt
ls -l
```

That one listing shows all three fixes together — that's your evidence. Take a screenshot of your simulator session showing it. **This screenshot is required — it is part of ★ Deliverable 1.** Name it `cli-permissions-audit.png`. You'll upload and embed it in the GitHub Commit section below.

---

## Part C — The Same Story, Windows Edition

### Step 1 — Read One File's Guest List

Switch to the PowerShell side of the scenario and run `Get-Acl` on the seeded file the scenario panel points you to. Windows shows a *list* of named accounts and rights instead of three ring-triplets.

Command you ran:

```
Get-Acl shift-notes.txt
```

Output (the owner line and at least one access entry):

```
Path : shift-notes.txt
Owner : morgan
Access : -rw-r--r--
```

### Step 2 — Translate One Entry

Take one access entry from your output and translate it into plain English, the same way you decoded the Linux string in Part A.

Your plain-English translation:

```
The owner is Morgan and she has the most control over the shift-notes.txt. She can read and write the file. The other two groups, the group and other group, both have permissions to read the file. 
```

---

## Analysis Questions

**Analysis Question 1.** In Part A you found the problem file by scanning the *other* triplet. Why is the "other" audience usually the most important one to audit first on a shared system? *(Minimum 2 sentences.)*

```
Your role as a Cybersecurity professional is secure data and granting permissions. On a shared system those roles must be clearly defined in order to prevent access of sensitive information from individuals outside of the owner of said files and the direct team assigned under the owner known as the "group". The other audience however, can be absolutely anyone who has access to the system, and that right there is a massive security threat for any organization if left uncheck.
```

**Analysis Question 2.** THE GATEKEEPER'S RULE requires a check before *and* after every change, even though `chmod` rarely fails. What does the *before* check protect you from, and what does the *after* check protect you from? *(Minimum 2 sentences.)*

```
The Gatekeeper Rule prevents is an essential step for asset protection because you need to know who currently has access to a file before you can make a determination of who need to have their privileges revoked or edited. So, with The Gatekeeper's Rule, you do a quick investigation of a file prior to making changes to verify whether or not changes need to be made in the first place. Once an issue is confirmed and changes are made to the add/revoke misconfigured permissions, then a final check using the same rule of thumb to confirm. Overall the G.K.R protects from inadvertently granting or revoking permissions to the wrong group.
```

**Analysis Question 3.** Lesson 1 called least privilege "granting what's needed and revoking the rest." Pick one of your three fixes from Part B and explain it in least-privilege terms: what was granted that wasn't needed, and who could have taken advantage? *(Minimum 3 sentences.)*

```
Looking at the fixes from Part B we can examine the file master-inventory.txt for this question. Assuming the master-inventory.txt file is a sensitive document for the organization we can see that giving everyone in the organization the ability to read and write to that document would be a serious security issue. So that means the owner, the group/team, and everyone else/other group could make changes as they see fit. In this particular example only the owner needed the permissions so we had to remove the groups ability to write to the document and removes others ability to read and write to it since they did not need to have access.
```

**Analysis Question 4.** Windows ACLs can name specific people; Linux permissions use three fixed audiences. Describe one situation where the Windows approach would be genuinely more useful — and one cost of that extra flexibility. *(Minimum 2 sentences.)*

```
I personally prefer the Bash language over Windows because of it's simplicity, but if I had to give a reason on what would make it more useful it would be the fact that it actually gives a name to the owner of a file. One thing I've noticed is that you have to specifically hone on a file in order to view permissions so it isn't as flexible as bash's ls -l command.
```

---

## Submission Checklist

- [x] Full `ls -l` listing recorded (Part A, Step 1)

- [x] One file fully decoded in plain English, all three audiences (Part A, Step 2)

- [x] Problem file identified with evidence from its permission string (Part A, Step 3)

- [x] Problem file locked down with before/after `ls -l` checks recorded (Part B, Step 1)

- [x] Script made owner-executable and verified (Part B, Step 2)

- [x] Secrets file protected from other's read and verified (Part B, Step 3)

- [x] **REQUIRED:** `cli-permissions-audit.png` uploaded to `assets/screenshots/week-04/` and embedded below (Part B, Step 4)

- [x] `Get-Acl` output recorded and one entry translated (Part C)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-04/labs/lab-01-file-permissions.md`

---

## GitHub Commit Subsection

This lab's written answers are submitted through the **CyberFoundations Lab Portal**, the same way as Week 3.

1. Go to the CyberFoundations Lab Portal and sign in.
2. Open **Week 4 → Lab 01: File Permissions — The Badge Audit**.
3. Fill in the worksheet fields — they match the commands, outputs, and questions in this file.
4. Connect your GitHub account if you haven't already (one-time setup), and select your portfolio repo.
5. Click **Submit to GitHub**. The Portal commits the completed file to `week-04/labs/lab-01-file-permissions.md` for you.

**📸 REQUIRED — your Deliverable 1 screenshot.** Unlike Week 3's optional screenshot, this one is a graded part of ★ Deliverable 1:

1. Go to your portfolio repository on GitHub.com and navigate to `assets/screenshots/week-04/` (create the folder if this is your first Week 4 screenshot).
2. Click **Add file → Upload files**, drag in your screenshot, named `cli-permissions-audit.png` (lowercase, hyphens, no spaces).
3. Scroll down and click **Commit changes**.
4. Click the uploaded image's filename to open it — the image itself will display on the page.
5. Right-click directly on the image and choose **Copy image address** (Chrome/Edge) or **Copy Image Link** (Firefox).
6. Edit this lab file and paste your copied link into the embed below, at the end of Part B:

![Permissions audit — final ls -l](https://github.com/CyViProtege/Week-04-Labs/blob/main/assets/screenshots/week-04/cli-permissions-audit.png?raw=true)

**If right-click doesn't show that option** (e.g., on some trackpads or tablets): click the small download-arrow icon in the top-right of the image preview instead, then copy the URL from your browser's address bar.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
