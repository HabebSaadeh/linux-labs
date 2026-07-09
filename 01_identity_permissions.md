# Lab 01: Identity, Navigation & Core File Operations

## Part 1: Identity & Information
Commands used to verify session identity parameters and absolute system locations.

1. echo: Prints text arguments or system variables to the terminal screen.
echo "--- Starting Lab 1: Identity & Analysis ---"
Output: --- Starting Lab 1: Identity & Analysis ---

2. whoami: Displays the exact username of the current active session.
whoami
Output: habeb

3. id: Displays the numeric User ID (UID), primary Group ID (GID), and all supplementary groups the current user belongs to.
id
Output: uid=1000(habeb) gid=1000(habeb) groups=1000(habeb),27(sudo)

4. id -un: Uses flags (u for user, n for name) to print only the current username, identical to whoami.
id -un
Output: habeb

5. pwd: Prints the absolute path of your current working directory starting from the root (/).
pwd
Output: /home/habeb

---

## Part 2: Navigation & Location
Practiced filesystem movement, directory creation, and traversal path shortcuts.

1. mkdir: Creates a new, empty directory (folder).
mkdir lab1_workspace

2. cd: Changes the current working directory to a specified path.
cd lab1_workspace

3. pwd: Confirming the absolute path change.
pwd
Output: /home/habeb/lab1_workspace

4. cd (without arguments): Returns you to your Home directory.
cd

---

## Part 3: File Inspection & Comparison
Generated data files and used inspection tools to view precise boundaries and live tracking streams.

1. cd: Move back into the workspace directory.
cd lab1_workspace

2. touch: Creates a new, blank file if it doesn't exist.
touch master_log.txt

3. nano: Added exactly 15 lines of text sentences inside the file.
nano master_log.txt

4. ls: Lists the files and subdirectories inside the current directory.
ls
Output: master_log.txt

5. cat -n: Reads a file and outputs its entire contents with sequential line numbers displayed on the left.
cat -n master_log.txt
Output:
     1  This is line one of the log file.
     2  This is line two of the log file.
     3  This is line three of the log file.
     4  This is line four of the log file.
     5  This is line five of the log file.
     6  This is line six of the log file.
     7  This is line seven of the log file.
     8  This is line eight of the log file.
     9  This is line nine of the log file.
    10  This is line ten of the log file.
    11  This is line eleven of the log file.
    12  This is line twelve of the log file.
    13  This is line thirteen of the log file.
    14  This is line fourteen of the log file.
    15  This is line fifteen of the log file.

6. head -n [number]: Previews the start of a file. Defaults to the first 10 lines.
head -n 10 master_log.txt
Output:
This is line one of the log file.
This is line two of the log file.
This is line three of the log file.
This is line four of the log file.
This is line five of the log file.
This is line six of the log file.
This is line seven of the log file.
This is line eight of the log file.
This is line nine of the log file.
This is line ten of the log file.

7. head -c [bytes]: Displays only the first specified number of bytes.
head -c 25 master_log.txt
Output: This is line one of the l

8. tail -n [number]: Previews the end of a file. Defaults to the last 10 lines.
tail -n 10 master_log.txt
Output:
This is line six of the log file.
This is line seven of the log file.
This is line eight of the log file.
This is line nine of the log file.
This is line ten of the log file.
This is line eleven of the log file.
This is line twelve of the log file.
This is line thirteen of the log file.
This is line fourteen of the log file.
This is line fifteen of the log file.

9. tail -c [bytes]: Displays only the last specified number of bytes.
tail -c 15 master_log.txt
Output: fthe log file.

10. tail -f: Loops continuously, outputting new lines in real-time as they are added to the file (ideal for live logs).
tail -f master_log.txt
Output: (Active stream loop opened successfully)

---

## Part 4: Viewing & Managing Contents
Executed directory cleanups, duplications, and tracked configuration shifts using deltas.

1. cp: Copies files or directories from a source path to a destination path.
cp master_log.txt backup_log.txt

2. diff: Compares two files line-by-line and outputs the exact differences.
diff master_log.txt backup_log.txt
Output: (Zero output, files match perfectly)

3. diff (After manual file modification): Verification of line deltas after changing lines 3 and 12 in the backup file.
diff master_log.txt backup_log.txt
Output:
3c3
This is line three of the log file.
This is a modified line three inside backup.
12c12
This is line twelve of the log file.
This is a modified line twelve inside backup.

4. touch: Updates access and modification timestamps without altering the contents if the file already exists.
touch master_log.txt

5. mv: Moves files or renames them if the destination is in the same directory.
mv backup_log.txt archived_log.txt

6. mkdir: Generated a separate tracking folder.
mkdir temp_configs

7. rmdir: Deletes a directory, but only if it is completely empty.
rmdir temp_configs

8. Folder population test:
mkdir temp_configs
mkdir temp_configs/nested_data
touch temp_configs/nested_data/dummy.txt

9. Empty check failure and recursive wipe:
rmdir temp_configs
Output: rmdir: failed to remove 'temp_configs': Directory not empty

rm: Permanently deletes files. (Requires r to delete folders).
rm -r temp_configs
