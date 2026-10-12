# Linux Server Administration Baseline Lab

## Scenario
I was given SSH access to a Linux server as a new system administrator. Before making any changes, the goal was to build a baseline of the system: its identity, filesystem, resources and running processes.

## Environment
- OS: Ubuntu 24.04.3 LTS (GNU/Linux 6.14.0-1010-aws x86_64)
- Hosting: AWS cloud instance
- Access method: SSH from Windows
- Tools: terminal (whoami, hostname, pwd, ls, cat, free, df, ps)

## What I did
1. Connected to the server over SSH with `ssh ctfroom@<lab-ip>`.
   - `ssh` opens a secure remote terminal session on another machine.
2. Identified the operating system with `cat /etc/os-release`.
   - `cat` prints a file. `/etc/os-release` is where Linux stores its OS name and version.
   - Result: Ubuntu 24.04.3 LTS (Noble Numbat), Debian-based.
3. Moved into the lab workspace with `cd lab`.
   - `cd` means "change directory".
4. Listed the workspace with `ls`, then `ls -a` to check for hidden files.
   - `-a` shows files whose names start with a dot.
   - Result: 6 items, none hidden.
5. Checked file types, sizes and ownership with `ls -lh`.
   - `-l` is the long format and `-h` makes sizes human-readable.
   - Result: 4 regular files and 2 directories (`backup`, `systembackups`). `website.config` is owned by a different user (`alex`).
6. Sorted by size with `ls -lhS` to find the largest file.
   - `-S` sorts largest first. Directories show a fixed 4.0K, so I compared the regular files only.
   - Result: `corrupted.txt` (42 bytes) is the largest file.
7. Checked memory with `free -h`.
   - `free` shows RAM usage.
   - Result: 454 MiB total, 326 MiB used, 127 MiB available, no swap.
8. Checked disk capacity with `df -h`.
   - `df` means "disk free" and shows each filesystem's size and usage.
   - Result: the root filesystem is 6.8 GB, 52% used.
9. Checked which expected services were running with `pgrep -l <name>` for Java, clawd, Apache and MySQL.
   - `pgrep` searches running processes by name. `-l` prints the name beside the process ID.
   - Only `java` was found (PID 695). I re-checked with `pgrep -fl` and `ps aux | grep -i clawd | grep -v grep`, which search full command lines, and confirmed clawd was not running.
   - `pgrep -fl apache` matched the Java process, so I inspected it with `ps -o args= -p 695`. It was Apache Tomcat 9 (the path pointed to `/var/lib/tomcat9`), so "apache" matched inside its command line, not a separate Apache web server.
   - Result: clawd, Apache and MySQL were not running. Java (Tomcat) was.
   - Note: when I repeated this check later in the session, Tomcat was no longer running (see finding 5).

## What I found
(results and what they mean)

## Problems and how I fixed them
(where I got stuck)

## What I learned
(2-3 lines)
