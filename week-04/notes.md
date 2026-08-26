# Week 4 Notes — Permissions, Searching, and Virtual Machines

**Student Name:** Christopher Windham

**Date Completed:** 08/25/2026

Summarize this week's key concepts in your own words — not copy-pasted definitions.

## Key Concepts This Week

- File permissions: read/write/execute × owner/group/other, and reading `ls -l`
- Changing permissions with `chmod` (symbolic and numeric) — and THE GATEKEEPER'S RULE
- Windows ACLs, read with `Get-Acl` (the real-world `icacls` tool does the same job, but is not available in the simulator)
- Wildcards (`*`, `?`, `[ ]`) and searching inside files with `grep`/`Select-String`
- Virtual machines: host vs. guest, the hypervisor, Type 1 vs. Type 2, isolation
- The VM lifecycle: create, start, stop (deallocate), snapshot, delete — and what each costs
- Golden snapshots — how your Weeks 6–12 lab machines are made

## In My Own Words

**Decode `-rw-r-----` audience by audience: who can do what to this file?**

```
The first group on the far left is the owner and they have permissions to read and write to the file. The next group is the "group/team" assigned under the owner and they have permission to read the file. Finally, the last group will be the "other" group, and they have no permissions for this file.
```

**What is a hypervisor, and what are its two jobs?**

```
The hypervisor is the program that sits between the hardware and the guest/virtual machine. It the divides the host CPU time, memory, and disk among the virtual machines. The hypervisors two jobs are to allocate resources and manages what each virtual machine uses.
```

**A stopped VM still costs a little money. What is it paying for, and what's the only way to reach a true zero?**

```
It's paying for the disk space to store the VM but not paying for any resources since it's stopped. The only way to reach true zero is to delete the VM.
```

---

## Submission Checklist

- [x] I summarized each concept in my own words, not copied definitions

- [x] I answered all three "In My Own Words" prompts

- [x] This file is committed to my portfolio repo at `week-04/notes.md`
