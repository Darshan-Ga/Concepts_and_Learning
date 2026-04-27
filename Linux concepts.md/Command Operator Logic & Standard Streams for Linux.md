## 🛠️ 3. Command Operator Logic & Standard Streams

> **Overview:** When the Kernel executes a command, it doesn't just print text to a screen. It opens three invisible "pipes" known as **File Descriptors (FDs)** to route data. Mastering how to detach and plug these pipes into different files or commands is the foundational skill for shell scripting, automation, and live log analysis.

---

### 🚰 The Standard Streams (File Descriptors)
Every time a process starts, Linux assigns it three default data streams:

* **`stdin` (Standard Input - FD 0):** Where the program receives its data. By default, this is hooked up to your **keyboard**.
* **`stdout` (Standard Output - FD 1):** Where the program sends its successful results. By default, this is printed to your **terminal screen**.
* **`stderr` (Standard Error - FD 2):** Where the program sends failure or error messages. By default, this is also printed to your **terminal screen**.

### 🔀 Redirection Operators (Data Plumbing)
Redirection allows you to take the output of a command and route it away from your screen and into a file. 

* **`>` (The Overwrite Operator):** * **Function:** Takes `stdout` (FD 1) and forces it into a file. 
  * ⚠️ **Danger (Destructive):** If the target file already contains data, it is instantly wiped clean and replaced. 
  * **Example:** `echo "Hello" > file.txt`
* **`>>` (The Append Operator):** * **Function:** Takes `stdout` and safely attaches it to the *bottom* of a file without deleting existing content.
  * 🛡️ **Security Context:** Used heavily in custom logging scripts to build a continuous record of events over time.
  * **Example:** `echo "World" >> file.txt`
* **`2>` (Error Redirection):** * **Function:** specifically grabs `stderr` (FD 2) and routes it away from your screen, leaving only successful outputs visible.
  * **Example:** `find / -name "password" 2> /dev/null` (Hunts the entire server for a file named "password", but silently throws all the "Permission Denied" errors into the `/dev/null` black hole so you only see the successful hits).

### 🔗 The Pipe (`|`) (Chaining Commands)
While `>` moves data into a *file*, the Pipe `|` moves data into *another command*.
* **Function:** It takes the `stdout` of the command on the left, and jams it directly into the `stdin` of the command on the right.
* 🛡️ **SOC Use Case:** `cat /var/log/auth.log | grep "Failed"`
  * *Logic:* `cat` reads the massive system log file. Instead of printing it to the screen, the pipe hands the text over to `grep`, which filters it and only prints lines containing the word "Failed" (revealing a brute-force SSH attack).

### 📡 Live Threat Monitoring
* **`tail -f` (Follow Mode):** * **Function:** Normally, `tail` just reads the last 10 lines of a file and closes. Adding the `-f` flag locks the File Descriptor open. 
  * **Security Context:** As the Kernel writes new data to the target file, `tail -f` instantly pushes it to your screen. This is the primary way SOC analysts watch live web server logs or authentication logs during an active cyber attack.
