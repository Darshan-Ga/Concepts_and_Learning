# 🚀 Phase 1 & 2 Master Cheat Sheet: Linux & DevOps

## 📂 1. File & Directory Operations (The Basics)
* **`ls`** : List directory contents.
  * `-l` : Long format (shows permissions, owner, size).
  * `-a` : All (shows hidden files starting with a dot).
  * `-la` : Combined (shows everything in long format).
* **`cat`** : Read and print the entire contents of a file.
  * *Trick:* `cat ./-file` (Bypass shell trap for files starting with a dash).
* **`cp`** : Copy a file.
  * `-r` : Recursive (used to copy entire directories/folders).
* **`mv`** : Move a file to a new location OR Rename a file.
* **`touch`** : Create a brand new, empty file instantly.
* **`ln`** : Create a link (Hard link by default).
  * `-s` : Create a Soft Link (Shortcut/Pointer).

## 🔍 2. Reconnaissance & Search (The Security Tools)
* **`find`** : Search the filesystem by metadata.
  * `-type f` : Search only for files.
  * `-type d` : Search only for directories.
  * `-name "text"` : Search by exact name.
  * `-iname "text"` : Case-insensitive name search.
  * `-size` : Search by size (e.g., `1033c` for exact bytes, `+50M` for larger than 50MB).
  * `-user` : Search by file owner.
  * `-group` : Search by group owner.
  * `-mmin` : Search by modified minutes (e.g., `-10` for less than 10 mins ago).
  * `-mtime` : Search by modified days.
  * `-perm` : Search by specific permissions (e.g., `0777`).
  * `-executable` : Find files that can be run as programs.
* **`file`** : Read "Magic Bytes" to identify the true file type (e.g., ASCII text vs. ELF binary).
  * `-i` : Output the MIME type string.
* **`grep`** : Search for specific words or patterns *inside* a file.

## 💾 3. Storage Management (The DevOps Audits)
* **`du` (Disk Usage)** : Check the size of specific directories/files.
  * `-h` : Human-readable (K, M, G).
  * `-s` : Summarize (only show total, don't list contents).
  * `-c` : Provide a grand total at the bottom.
* **`df` (Disk Free)** : Check the total capacity and available space on the entire hard drive.

## ✂️ 4. Data Manipulation (The Text Slicers)
* **`wc` (Word Count)** : Count lines, words, or bytes in a file.
  * `-l` : Count lines only (highly used).
* **`diff`** : Compare two files and highlight the exact differences.
* **`cut`** : Slice text vertically (by columns) based on a delimiter.
* **`sort`** : Alphabetize text.
  * `-n` : Sort numerically (prevents `10` from being placed before `2`).

## 📖 5. Text Pagers (Reading Massive Files)
* **`less`** : The modern standard. Loads one screen at a time. Allows scrolling up (`b`), down (`Spacebar`), and searching (`/`).
* **`more`** : The legacy tool. Only allows scrolling down. (Avoid using).

## ⚙️ 6. Operators & Pipelines (The Glue)
* **`>` (Standard Output / Overwrite)** : Send output to a file and delete anything previously in that file.
* **`>>` (Append)** : Send output to a file and add it to the bottom safely.
* **`2> /dev/null` (Error Routing)** : Send `stderr` (Permission Denied errors) into the black hole to keep the screen clean.
* **`|` (The Pipe)** : Take the successful output (`stdout`) of the left command and feed it into the input (`stdin`) of the right command.
* **`&` (Background Operator)** : Run a command in the background and immediately return control of the terminal to the user.
* **`&&` (Logical AND)** : Run Command 1, and IF it is successful, immediately run Command 2.
* **`tee` (The T-Junction)** : Print the output to the screen AND save it to a file at the exact same time.

## System Diagnostics & Troubleshooting

**Overview:** When a server's CPU spikes to 100% or an application freezes, these are the commands engineers use to diagnose the hardware, locate the rogue process, and terminate it.

* **`top`** : Opens a live, constantly updating dashboard of all running processes, sorted by CPU and RAM usage. (Press `q` to exit).
* **`ps`** : Takes a static snapshot of currently running processes.
  * *Pro-Tip:* Use `ps aux | grep [name]` to instantly search if a specific application (like a web server) is running.
* **`fuser`** : Identifies exactly which process is using a specific file, directory, or network port.
* **`kill`** : Terminates a process using its Process ID (PID).
  * `-9` : The "Force Kill" flag. (e.g., `kill -9 1234`). It forces the system to instantly destroy the process without letting it save data or shut down cleanly.
* **`nohup` (No Hangup)** : Prevents a background process from being terminated when the user logs out or disconnects from the terminal.
  * *Standard Syntax:* `nohup [command] &`
* **`free`** : Displays the total, used, and available RAM (Memory) on the server.
  * `-h` : Human-readable format (displays in MB/GB).
* **`vmstat`** : Reports comprehensive virtual memory statistics, including CPU activity, block IO, and system processes.

## 🕵️‍♂️ Data Forensics & Manipulation

**Overview:** When investigating compromised servers, cleaning messy logs, or extracting hidden malware payloads, engineers use these commands to filter data, decode strings, and manage compressed archives.

* **`grep`** : Searches through files or terminal output for a specific word, phrase, or pattern.
  * *Pro-Tip:* Use `-i` to ignore uppercase/lowercase, and `-v` to invert the match (print everything that *doesn't* match).
* **`sort`** : Organizes the lines of a text file alphabetically or mathematically.
  * `-n` : Sorts numerically (treating "10" as bigger than "2").
* **`uniq`** : Filters out adjacent, duplicate lines in a file. 
  * *Pro-Tip:* Always pipe data through `sort` first! Use `-u` to show *only* strictly unique lines, or `-c` to count how many times a line appeared.
* **`tr`** : Translates or deletes specific characters from standard output.
  * *Standard Syntax:* `cat file.txt | tr '[old_chars]' '[new_chars]'`
* **`strings`** : Extracts only human-readable text from compiled binaries or unreadable files, ignoring the machine code.
* **`base64`** : Encodes or decodes data into the Base64 standard, often used to safely transmit complex data.
  * `-d` : Decodes the Base64 string back into normal text.
* **`xxd`** : Generates a hex dump of a file, showing the raw hexadecimal bytes alongside their ASCII text translation.
  * *Pro-Tip:* Run `xxd [file] | head` to quickly check a file's header (magic bytes) to see if an attacker faked the file extension.
* **`tar`** : Archives (glues) multiple files and directories together into one single file called a tarball. It does *not* compress size.
  * *Standard Syntax:* `tar -cvf` to create an archive, and `tar -xvf` to extract it.
* **`gzip`** : Compresses files to save disk space. Often used on tarballs to create `.tar.gz` files.
  * `-d` : Decompresses the file (e.g., `gzip -d file.gz`).
* **`bzip2`** : An alternative compression tool. It shrinks files smaller than `gzip`, but uses more CPU power to do it.
  * `-d` : Decompresses the file (e.g., `bzip2 -d file.bz2`).

   ## ⚖️ System Governance & Environment Forensics

📝 **Overview:** These commands provide "situational awareness." They are used to verify your identity, check system health, and manage software packages across different Linux distributions.

### 👤 Identity & Permissions
* 🆔 **`whoami`** : Displays the username of the current active session.
  * 💡 *Use Case:* Verify if a privilege escalation (like `sudo`) successfully changed your user status.
* 🪪 **`id`** : Prints your User ID (UID) and Group IDs (GID).
  * 💡 *Cybersec Tip:* Essential for finding "hidden" group permissions that allow you to read restricted files.
* 🛡️ **`sudo`** (SuperUser Do) : Executes a command with root/administrative privileges.
  * 🛑 *Rule:* Only use this when the system denies access to a standard user.

### 🖥️ System Diagnostics
* 🛰️ **`uname`** : Displays system and kernel information.
  * ⌨️ `-a` : Shows the kernel version, which is critical for identifying system-level vulnerabilities.
* ⏱️ **`uptime`** : Shows how long the system has been running and the current CPU load.
* 📅 **`date`** : Displays the current system time.
  * 💡 *Use Case:* Crucial for matching log file timestamps to real-world attack times.
* 📍 **`which`** : Shows the full path of an executable command.
  * ⌨️ *Example:* `which python3` tells you exactly which version of Python the system is using.

### 📦 Package Management (Distro Families)
* 🛒 **`apt`** : The standard manager for **Debian/Ubuntu** (used in Bandit & TryHackMe).
* 📦 **`dnf` / `yum`** : The modern and legacy managers for **RHEL/CentOS/Fedora**.
* 🏹 **`pacman`** : The lightweight manager for **Arch Linux**.
* ❄️ **`portage`** : The advanced, source-based manager for **Gentoo**.

### 🔌 Power & Session Control
* 🔄 **`reboot`** : Restarts the hardware.
* 🔌 **`shutdown`** : Powers off the machine.
  * ⌨️ `sudo shutdown -h now` : Forces an immediate halt of all processes.
---
*Note: This playbook is a living document. As I learn more and encounter new DevOps concepts, I am going to continually update this list.*
