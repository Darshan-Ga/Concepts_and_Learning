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
    
---

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

---

## 💾 3. Storage Management (The DevOps Audits)
* **`du` (Disk Usage)** : Check the size of specific directories/files.
  * `-h` : Human-readable (K, M, G).
  * `-s` : Summarize (only show total, don't list contents).
  * `-c` : Provide a grand total at the bottom.
* **`df` (Disk Free)** : Check the total capacity and available space on the entire hard drive.
  
---

## ✂️ 4. Data Manipulation (The Text Slicers)
* **`wc` (Word Count)** : Count lines, words, or bytes in a file.
  * `-l` : Count lines only (highly used).
* **`diff`** : Compare two files and highlight the exact differences.
* **`cut`** : Slice text vertically (by columns) based on a delimiter.
* **`sort`** : Alphabetize text.
  * `-n` : Sort numerically (prevents `10` from being placed before `2`).
 
 ---

## 📖 5. Text Pagers (Reading Massive Files)
* **`less`** : The modern standard. Loads one screen at a time. Allows scrolling up (`b`), down (`Spacebar`), and searching (`/`).
* **`more`** : The legacy tool. Only allows scrolling down. (Avoid using).

---

## ⚙️ 6. Operators & Pipelines (The Glue)
* **`>` (Standard Output / Overwrite)** : Send output to a file and delete anything previously in that file.
* **`>>` (Append)** : Send output to a file and add it to the bottom safely.
* **`2> /dev/null` (Error Routing)** : Send `stderr` (Permission Denied errors) into the black hole to keep the screen clean.
* **`|` (The Pipe)** : Take the successful output (`stdout`) of the left command and feed it into the input (`stdin`) of the right command.
* **`&` (Background Operator)** : Run a command in the background and immediately return control of the terminal to the user.
* **`&&` (Logical AND)** : Run Command 1, and IF it is successful, immediately run Command 2.
* **`tee` (The T-Junction)** : Print the output to the screen AND save it to a file at the exact same time.

---

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
  
---

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
  * 
---

   ## ⚖️ System Governance & Environment Forensics

📝 **Overview:** These commands provide "situational awareness." They are used to verify your identity, check system health, and manage software packages across different Linux distributions.

### 👤 Identity & Permissions
* 🆔 **`whoami`** : Displays the username of the current active session.
  * 💡 *Use Case:* Verify if a privilege escalation (like `sudo`) successfully changed your user status.
* 🪪 **`id`** : Prints your User ID (UID) and Group IDs (GID).
  * 💡 *Cybersec Tip:* Essential for finding "hidden" group permissions that allow you to read restricted files.
* 🛡️ **`sudo`** (SuperUser Do) : Executes a command with root/administrative privileges.
  * 🛑 *Rule:* Only use this when the system denies access to a standard user.
    
---

### 🖥️ System Diagnostics
* 🛰️ **`uname`** : Displays system and kernel information.
  * ⌨️ `-a` : Shows the kernel version, which is critical for identifying system-level vulnerabilities.
* ⏱️ **`uptime`** : Shows how long the system has been running and the current CPU load.
* 📅 **`date`** : Displays the current system time.
  * 💡 *Use Case:* Crucial for matching log file timestamps to real-world attack times.
* 📍 **`which`** : Shows the full path of an executable command.
  * ⌨️ *Example:* `which python3` tells you exactly which version of Python the system is using.
    
---

### 📦 Package Management (Distro Families)
* 🛒 **`apt`** : The standard manager for **Debian/Ubuntu** (used in Bandit & TryHackMe).
* 📦 **`dnf` / `yum`** : The modern and legacy managers for **RHEL/CentOS/Fedora**.
* 🏹 **`pacman`** : The lightweight manager for **Arch Linux**.
* ❄️ **`portage`** : The advanced, source-based manager for **Gentoo**.
  
---

### 🔌 Power & Session Control
* 🔄 **`reboot`** : Restarts the hardware.
* 🔌 **`shutdown`** : Powers off the machine.
  * ⌨️ `sudo shutdown -h now` : Forces an immediate halt of all processes.
 
---
## 🛡️ User Administration & Remote Transfer

📝 **Overview:** This module covers the core commands required to manage user identities, govern file access (permissions), and securely transfer data across networks. These are essential daily skills for Cloud Security and Server Administration.

### 👥 User & Group Management (Identity)

* 👤 **`useradd`** : Creates a new user account.
  * ⌨️ `-m` : Creates the user's home directory (`/home/username`). *Always use this!*
  * ⌨️ `-s /bin/bash` : Assigns the default shell so the user gets a proper terminal.
* 🔑 **`passwd`** : Sets or updates a user's password.
  * ⌨️ `-l` (Lock) : Locks an account so the user cannot log in (used for compromised accounts).
  * ⌨️ `-u` (Unlock) : Unlocks a previously locked account.
* 🗑️ **`userdel`** : Deletes a user account.
  * ⌨️ `-r` : Completely wipes the user's home directory and files along with the account.
* 🚪 **`su`** (Switch User) : Switches your terminal session to another user.
  * ⌨️ `su - username` : The `-` is critical. It loads that user's full environment variables, not just their permissions.
* 🏢 **`groupadd` / `groupdel`** : Creates or deletes a user group (e.g., `groupadd developers`).
* 📋 **`gpasswd`** : Manages group memberships.
  * ⌨️ `-a [user] [group]` : Adds a user to a group.
  * ⌨️ `-d [user] [group]` : Removes a user from a group.

---

### 🔐 Ownership & Permissions (Access Control)

* 👑 **`chown`** (Change Owner) : Transfers ownership of a file/folder.
  * ⌨️ `chown user:group filename` : Changes both the owner and the group simultaneously.
  * ⌨️ `-R` : Recursive. Changes ownership for a folder and everything inside it.
* 🎭 **`chgrp`** : Changes only the group ownership of a file.
* 🛑 **`chmod`** (Change Mode) : Modifies read, write, and execute permissions.
  * ⌨️ `-R` : Recursive application to folders.
  * 💡 *The Permission Math Cheat Sheet:*
    * **Read (r) = 4** | **Write (w) = 2** | **Execute (x) = 1**
    * `7` (4+2+1) = Full Access
    * `6` (4+2) = Read/Write
    * `5` (4+1) = Read/Execute
    * *Example:* `chmod 755 script.sh` (Owner can do everything, Group and Others can only read/execute).
* 🧮 **`umask`** : Sets the default security restrictions for any *new* file you create. It automatically subtracts permissions so new files aren't born fully exposed.

---

### 🚀 Remote Data Transfer (Network)

* 📦 **`scp`** (Secure Copy Protocol) : Copies files over an encrypted SSH connection. Fast, secure, but starts over if the connection drops.
  * ⬆️ **Push:** `scp local_file.txt user@192.168.1.10:/remote/path/`
  * ⬇️ **Pull:** `scp user@192.168.1.10:/remote/file.txt /local/path/`
  * 🚩 **Important Flags:**
    * `-r` : Recursive (for copying folders).
    * `-P` (Capital P) : Specifies a custom SSH port if the server isn't using the default port 22.
* 🔄 **`rsync`** (Remote Sync) : The advanced transfer tool. It only copies the *differences* (deltas) between files and resumes automatically if the connection fails. Essential for large backups.
  * ⌨️ **Syntax:** `rsync -avz /local/folder/ user@192.168.1.10:/remote/backup/`
  * 🚩 **Important Flags:**
    * `-a` (Archive) : Preserves all permissions, ownership, and timestamps.
    * `-v` (Verbose) : Shows you exactly what is transferring on the screen.
    * `-z` (Compress) : Zips the data during transit to save bandwidth.
    * `--progress` : Displays a live progress bar.
      
---

### 🔌 nc (Netcat) : The "Swiss Army Knife" of networking.

Used for reading and writing raw data across network connections. It is the most basic way to "talk" to a specific port.

* **Connect:** nc 192.168.1.10 30000 (Opens a pipe to send text to that port).

* **Listen:** nc -l 4444 (Turns your machine into a temporary server waiting for a connection).

  * 🚩 Important Flags:

  * **-v :** Verbose; tells you if the connection was successful or failed.

  * **-z :** Zero-I/O; used for scanning (it checks if a port is open without sending data).

  * **-u :** Uses UDP instead of the default TCP.
 
 ---
 
### 🔐 openssl s_client : The "Secret Handshake" envoy.

Used to connect to services protected by SSL/TLS encryption. Essential when a raw tool like nc is rejected by a secure port.

* **Connect:** openssl s_client -connect 192.168.1.10:30001

  * ⌨️ Syntax: After the certificate data stops scrolling, type your message and hit Enter.

  * 🚩 Important Flags:

  -quiet : Hides the technical certificate/session data so you only see the text conversation.

  -brief : Provides a very short summary of the connection security.

  ---

* **🛰️ nmap (Network Mapper) :** The "Security Drone."
The industry standard for discovering devices on a network and finding which "doors" (ports) are open.

Basic Scan: nmap 192.168.1.10 (Scans the 1,000 most common ports).

Specific Scan: nmap -p 80,443,30000-30005 192.168.1.10

   * 🚩 Important Flags:

   -sV : Version Detection; tries to determine exactly what software and version is running on a port.

   -sS : TCP SYN Scan; a faster, "stealthier" way to scan without completing a full connection.

   -O : OS Detection; attempts to guess if the server is running Linux, Windows, or another OS.

   ## 🌐 The Networking Toolkit: Core Diagnostic Commands

These commands form the foundation of network troubleshooting and security auditing in Linux. They allow us to map network paths, verify connectivity, and inspect open ports.

### 1. `ifconfig` / `ip a` (Network Interface Configuration)
*   **What it does:** Think of this as asking the server, "Who are you?" It displays your machine's current network configuration, including your local IP address, subnet mask, and MAC address. *(Note: `ip a` is the modern standard, while `ifconfig` is the legacy tool).*
*   **How to use it:** 
    ```bash
    ip a
    # or
    ifconfig
    ```
*   **Important Flags:**
    *   `ip -br a`: Prints the output in a clean, brief, and highly readable table format.
    *   `ifconfig -a`: Shows all network interfaces, even the ones that are currently turned off.

### 2. `ping` (Packet Internet Groper)
*   **What it does:** The most basic connectivity test. It sends a tiny data packet (ICMP Echo Request) to a target server and waits for it to bounce back. It tells you *if* a server is alive and *how long* the trip took (latency).
*   **How to use it:**
    ```bash
    ping google.com
    ```
*   **Important Flags:**
    *   `-c [number]`: (Count) Stops the ping after a specific number of requests. Essential for scripting so your script doesn't run forever. (e.g., `ping -c 4 google.com`)
    *   `-i [seconds]`: (Interval) Changes the wait time between pings. 

### 3. `traceroute` / `tracepath` 
*   **What it does:** If `ping` tells you the destination is reachable, `traceroute` maps the exact road you took to get there. It lists every single router (called a "hop") your data passes through. This is crucial for finding out exactly *where* a connection is failing.
*   **How to use it:**
    ```bash
    traceroute google.com
    ```
*   **Important Flags:**
    *   `-n`: (Numeric) Displays raw IP addresses instead of trying to resolve their domain names. This makes the command run significantly faster.
    *   `-m [number]`: (Max Hops) Limits how far the trace will go before giving up (default is usually 30).

### 4. `mtr` (My Traceroute)
*   **What it does:** The ultimate diagnostic tool. It combines the functionality of `ping` and `traceroute` into a single, real-time, dynamically updating dashboard. It shows packet loss and latency for every single hop along the route simultaneously.
*   **How to use it:**
    ```bash
    mtr google.com
    ```
*   **Important Flags:**
    *   `-r`: (Report mode) Instead of opening an interactive dashboard, it runs the test in the background and prints a final, clean text report. Perfect for saving to log files.
    *   `-c [number]`: Sets how many ping cycles to run before generating the report.

### 5. `netstat` (Network Statistics)
*   **What it does:** The Cloud Security Engineer's radar. It shows you exactly which ports are open on your machine, who is currently connected to them, and what services are listening for traffic. 
*   **How to use it:**
    ```bash
    netstat
    
```
*   **Important Flags (The Holy Grail Combo):**
    Security engineers almost always use this exact combination of flags:
    ```bash
    sudo netstat -tulpn
    ```
    *   `-t`: Show **TCP** ports.
    *   `-u`: Show **UDP** ports.
    *   `-l`: Show only **Listening** ports (doors waiting for a connection).
    *   `-p`: Show the **Program** (PID/Process Name) using that port.
    *   `-n`: Show **Numeric** IP addresses and port numbers (skips slow DNS lookups).
```
---

### 6. 'ss -tulpn' 

* **`ss`** : Socket Statistics (modern replacement for `netstat`).
  * `-t` : **TCP** (shows connections using the "handshake" protocol).
  * `-u` : **UDP** (shows "fire and forget" protocol connections).
  * `-l` : **Listening** (shows only the ports waiting for incoming connections).
  * `-p` : **Process** (shows the name of the program using the port—requires `sudo`).
  * `-n` : **Numeric** (shows port numbers as digits instead of service names).
  * `-tulpn` : **Combined** (the "gold standard" for a full security audit of open doors and the programs behind them).
 
  * 
---
*Note: This playbook is a living document. As I learn more and encounter new DevOps concepts, I am going to continually update this list.*
