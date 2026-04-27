## 🏷️ Advanced `find`: Filtering by Metadata

> **Overview:** In Linux, a file consists of two things: the **Data** (the actual text or code inside the file) and the **Metadata** (the "data about the data," such as who owns it, how big it is, and when it was created). Security engineers use metadata flags with the `find` command to hunt down anomalies without ever having to read the contents of the files.

---

### 👤 1. Ownership Flags (The Access Audit)
Every file in Linux is assigned to a specific User and a specific Group. If an employee leaves a company, or if a hacker creates a backdoor user, you use these flags to audit their files.

* **`-user [username]`**: Finds files owned by a specific user.
  * *Example:* `find /var/www -user www-data` (Finds all web server files).
* **`-group [groupname]`**: Finds files owned by a specific group.
  * *Example:* `find / -group admin` (Finds files only the admin group controls).

### 📏 2. Size Flags (The Storage Audit)
Used to track down massive log files crashing a server, or highly specific payloads.

* **`-size [number][unit]`**:
  * `c` = Bytes (Characters).
  * `k` = Kilobytes.
  * `M` = Megabytes.
  * `G` = Gigabytes.
* **Operators:** You can use `+` (greater than) or `-` (less than).
  * *Example:* `find /var/log -size +500M` (Finds all log files larger than 500 Megabytes).

### ⏱️ 3. Time Flags (The Forensics Audit)
This is the **most critical** category for a SOC Analyst or Incident Responder. When a server is breached, you need to find exactly what files the hacker touched in the last few hours.

* **`-mtime [days]` (Modified Time):** When the file's contents were last changed.
  * *Example:* `find / -mtime -2` (Finds files modified in the last 48 hours).
* **`-mmin [minutes]` (Modified Minutes):** The high-precision version.
  * *Example:* `find /etc -mmin -15` (Finds configuration files changed in the last 15 minutes—a massive red flag during an attack).

### 🔐 4. Permission Flags (The Security Audit)
Used to find dangerous misconfigurations.

* **`-perm [octal_number]`**: Searches for files with exact permission sets.
  * *Example:* `find / -perm 0777` (Hunts down "world-writable" files that anyone on the server can read, edit, or execute. These are massive security risks).

### 🛠️ Chaining Flags (The Real-World Scenario)
The true power of `find` comes from combining these flags. 

* **The Scenario:** You suspect a hacker gained access 30 minutes ago and dropped a large malware executable in the temporary folder.
* **The Command:** `find /tmp -type f -mmin -30 -size +10M -executable 2> /dev/null`
* **The Logic:** "Search the `/tmp` folder for a file (`-type f`), modified in the last 30 minutes (`-mmin -30`), larger than 10MB (`-size +10M`), that can be run as a program (`-executable`), and throw all permission errors into the black hole (`2> /dev/null`)."
