# Lab 01 — My Notes

User: habeb
Machine: local Linux VM

## Section 1 — Identity & Location

Commands I used:
```bash
whoami
id -un
id
pwd
cd /var/log
pwd
cd
pwd
```

Output:
```
habeb
habeb
uid=1000(habeb) gid=1000(habeb) groups=1000(habeb),27(sudo)
/home/habeb
/var/log
/home/habeb
```

---

## Section 2 — Files and Folders

Commands I used:
```bash
mkdir -p labwork/reports/2026
touch labwork/reports/2026/jan.log labwork/reports/2026/feb.log labwork/reports/2026/mar.log
touch labwork/reports/2026/jan.log
cp -r labwork labwork_backup
mv labwork/reports/2026/mar.log labwork/reports/2026/march_final.log
rm labwork_backup/reports/2026/feb.log
rmdir labwork_backup
rm -r labwork_backup
ls -R labwork
```

Output:
```
rmdir: failed to remove 'labwork_backup': Directory not empty

labwork:
reports

labwork/reports:
2026

labwork/reports/2026:
jan.log  march_final.log
```

---

## Section 3 — Inspecting File Contents

Commands I used:
```bash
for i in $(seq 1 15); do echo "log line $i" >> labwork/reports/2026/jan.log; done
cat -n labwork/reports/2026/jan.log
head -n 5 labwork/reports/2026/jan.log
tail -n 3 labwork/reports/2026/jan.log
head -c 20 labwork/reports/2026/jan.log
cp labwork/reports/2026/jan.log labwork/reports/2026/jan_copy.log
sed -i 's/line 7/LINE_SEVEN/' labwork/reports/2026/jan_copy.log
diff labwork/reports/2026/jan.log labwork/reports/2026/jan_copy.log
```

Output (diff between jan.log and jan_copy.log):
```
7c7
< log line 7
---
> log LINE_SEVEN
```

---

## Section 4 — Permissions & Ownership

Commands I used:
```bash
ls -l labwork/reports/2026/jan.log
chmod 640 labwork/reports/2026/jan.log
ls -l labwork/reports/2026/jan.log
chmod u+x labwork/reports/2026/jan.log
ls -l labwork/reports/2026/jan.log
chmod g-w labwork/reports/2026/jan.log
ls -l labwork/reports/2026/jan.log
mkdir restricted_zone
touch restricted_zone/file1 restricted_zone/file2
chmod -R 700 restricted_zone
ls -l restricted_zone
```

ls -l output after each step:
```
-rw-r--r-- 1 habeb habeb 0 Jul 20 10:01 jan.log
-rw-r----- 1 habeb habeb 0 Jul 20 10:01 jan.log
-rwxr----- 1 habeb habeb 0 Jul 20 10:01 jan.log
-rwxr----- 1 habeb habeb 0 Jul 20 10:01 jan.log

drwx------ 2 habeb habeb 4096 Jul 20 10:05 .
-rwx------ 1 habeb habeb    0 Jul 20 10:05 file1
-rwx------ 1 habeb habeb    0 Jul 20 10:05 file2
```

Note: g-w on jan.log did not change the listing because the group already had no write bit after chmod 640 (group was r-- already).

---

## Section 5 — Elevated Privileges

Command without sudo (and the error):
```bash
cat /etc/shadow
```
```
cat: /etc/shadow: Permission denied
```

Command with sudo:
```bash
sudo cat /etc/shadow
```
```
[sudo] password for habeb:
```
(password prompt is for habeb's own password, not root's — confirmed root has no separate password to enter here)

---

## Section 6 — User Management Lifecycle

Commands I used:
```bash
sudo useradd -m labuser01
sudo passwd labuser01
sudo usermod -aG sudo labuser01
groups labuser01
sudo passwd -l labuser01
sudo passwd -u labuser01
sudo usermod -s /bin/sh labuser01
cat /etc/passwd | grep labuser01
su - labuser01
whoami
exit
sudo userdel -r labuser01
```

Line from /etc/passwd for labuser01, before delete:
```
labuser01:x:1001:1001::/home/labuser01:/bin/sh
```

Fields:
- field 1 = username → labuser01
- field 2 = password placeholder → x (real hash lives in /etc/shadow)
- field 3 = UID → 1001
- field 4 = primary GID → 1001
- field 5 = comment/GECOS field → empty
- field 6 = home directory → /home/labuser01
- field 7 = default shell → /bin/sh

---

## Section 7 — Synthesis Challenge

Commands I used:
```bash
sudo useradd -m auditor
sudo passwd -l auditor
sudo mkdir -p /shared/audit_logs
sudo chown auditor:auditor /shared/audit_logs
sudo chmod 700 /shared/audit_logs
sudo usermod -aG auditor habeb
ls -l /shared
```

ls -l output of /shared/audit_logs:
```
drwx------ 2 auditor auditor 4096 Jul 20 10:20 audit_logs
```

Answer for step 4:
Adding myself (habeb) to auditor's group does not give me access, because chmod 700 only grants rwx to the owner — the group and others bits are all 0. To let group members in, the folder would need something like chmod 750 instead. So the setup as built (700) intentionally blocks even group members, including me.
