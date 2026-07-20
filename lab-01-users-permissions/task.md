# Linux Fundamentals Lab 01 — Users, Permissions & the Filesystem

**Level:** Beginner
**Estimated time:** 60–90 minutes
**Prerequisites:** Access to a Linux machine or VM where you have `sudo` rights (a fresh Ubuntu/Debian VM or a Docker container is fine — do NOT run the user-management sections on a production or shared machine).

## How to use this lab
- Work through the sections **in order** — later sections assume users/files created in earlier ones.
- Every task tells you the *goal*, not the *command*. Figure out the command yourself from what you've studied.
- After each section, there's a **"Prove it"** checkpoint — a command (or output) you should be able to produce to demonstrate you did the task correctly. Screenshot or copy-paste this output into your own notes; you'll want it later if you publish this lab.
- Don't peek at man pages until you've tried recalling the command from memory first — then use `man [command]` or `command --help` to fill gaps.

---

## Section 1 — Identity & Location

1. Find out exactly which username you are currently logged in as, using **two different commands** that give the same answer.
2. Display your numeric User ID, primary Group ID, and every group you belong to, all in a single command.
3. Print your current working directory.
4. Navigate to `/var/log`, confirm you're there, then return to your home directory using the shortest possible command.

**Prove it:** Show the output of the command that lists your UID, GID, and all group memberships.

---

## Section 2 — Files and Folders

1. Create a directory structure: `labwork/reports/2026` (create all levels in as few commands as possible).
2. Inside `2026`, create three empty files: `jan.log`, `feb.log`, `mar.log`.
3. Without opening an editor, update the timestamp on `jan.log` so it looks like it was just modified, without changing its contents.
4. Copy the entire `labwork` folder to a new folder called `labwork_backup`.
5. Rename `mar.log` to `march_final.log` inside the *original* `labwork` folder (not the backup).
6. Delete only the `feb.log` file from the backup copy.
7. Try to delete the (now non-empty) `labwork_backup` folder using the command that only works on empty directories. Note what happens, then delete it properly.

**Prove it:** Show a directory listing of `labwork` and confirm `labwork_backup` no longer exists.

---

## Section 3 — Inspecting File Contents

1. Add at least 15 lines of arbitrary text to `labwork/reports/2026/jan.log` (any method you like).
2. Display the entire file with line numbers visible.
3. Show only the first 5 lines.
4. Show only the last 3 lines.
5. Show only the first 20 bytes of the file.
6. Create a second file, `jan_copy.log`, as an exact copy of `jan.log`, then change one word in the copy. Use a command to show precisely what differs between the two files.
7. (Optional, if you have two directories to compare) Compare `labwork` and a copy of it recursively after making one small change, and identify what the diff tool reports.

**Prove it:** Show the exact diff output between `jan.log` and `jan_copy.log`.

---

## Section 4 — Permissions & Ownership

1. Check the current permissions on `jan.log` (in long-listing format).
2. Using **numeric/octal notation**, set `jan.log` so that:
   - Owner can read and write, but not execute
   - Group can only read
   - Others have no access at all
3. Now, using **symbolic notation only**, add execute permission for the owner without touching anything else.
4. Remove write permission from the group using symbolic notation.
5. Create a new directory called `restricted_zone` containing a couple of dummy files, then apply a permission change **recursively** so that only the owner has full access (read/write/execute) and everyone else has none.
6. If you have a second user account available (or create one — see Section 6), attempt to change the owner of `jan.log` to that user, and separately change its group.

**Prove it:** Show the long-listing (`ls -l`-style) output of `jan.log` after each of steps 2–4, and of `restricted_zone` after step 5.

---

## Section 5 — Elevated Privileges

1. Confirm you cannot view the contents of the system's encrypted password file as a normal user (attempt it, and note the error).
2. View the same file using elevated privileges, and note what's different about being asked for a password (whose password is it?).
3. Explain in your own words (write this as a comment in your lab notes) *why* this elevation model is safer than simply logging in as root all the time.

**Prove it:** Paste the error message from step 1, and confirm (without pasting the file contents) that elevation let you view it in step 2.

---

## Section 6 — User Management Lifecycle

⚠️ Do this only on a disposable VM/container.

1. Create a new user named `labuser01` **with** an automatically created home directory.
2. Set a password for `labuser01`.
3. Add `labuser01` to a secondary group of your choice (create the group first if needed, or use an existing one like `sudo`).
4. Confirm which groups `labuser01` now belongs to.
5. Lock the `labuser01` account's password, then confirm what happens if you try to log in as that user. Unlock it again.
6. Change `labuser01`'s default shell from whatever it currently is to the other one you studied (`/bin/sh` ↔ `/bin/bash`), and confirm the change by inspecting the correct system file directly (don't just take usermod's word for it).
7. Switch your session to `labuser01` with a full environment reload, confirm your identity has changed, then return to your original session.
8. Finally, delete `labuser01`, including their home directory and mail spool.

**Prove it:** Show the single line for `labuser01` from the system's user "phonebook" file *before* you delete the account (do this right after step 6), and label each of the 7 fields.

---

## Section 7 — Synthesis Challenge

Without being told which commands to use, achieve the following in one sitting:

1. Create a user `auditor`, give them a home directory, and lock their password immediately (they should exist but not be able to log in yet).
2. Create a folder `/shared/audit_logs` (use `sudo` since it's outside your home).
3. Make `auditor` the owner of that folder, and restrict permissions so only the owner can read/write/enter it, with no access for group or others.
4. Add yourself to whatever group would let you access the folder without being the owner — decide whether this is actually possible with the setup you just built, and explain why or why not in your notes.

**Prove it:** Long-listing of `/shared/audit_logs` showing ownership and permissions, plus your written explanation for step 4.

---

## Self-Check Rubric

Before publishing, confirm you can answer these without notes:
- What's the difference between `chmod 750` and `chmod u=rwx,g=rx,o=`?
- Why does `/etc/passwd` show `x` in the password field instead of an actual hash?
- What's the practical difference between `userdel` and `userdel -r`?
- Why might `su - user` behave differently from `su user` (no dash)?
- What does being in the `sudo` group actually grant, and how is it different from being root permanently?

---

*This lab is designed for self-study and practice. No solutions are included by design — the goal is recall and troubleshooting, not copy-paste.*
