## 📂 2. The Filesystem Hierarchy Standard (FHS)

> **Overview:** In Windows, physical drives dictate the structure (e.g., `C:\` and `D:\`). Linux takes a fundamentally different approach: **"Everything is a file."** Hardware, text documents, and network sockets are all treated as files, and they all branch out from a single, unified, inverted tree.

---

### 🌳 The Root & The Core Commands
* **`/` (The Root Directory):** * The absolute top of the filesystem tree. Everything on the machine is located somewhere under `/`.
  * **Security Context:** If an attacker gains "root" privileges, they own this directory and, by extension, the entire server.
    
* **`/bin` (User Binaries):** * Contains the essential, everyday executable commands (`ls`, `cat`, `echo`, `cd`). 
  * **Access:** Any standard user on the system can execute the programs located here.
    
* **`/sbin` (System Binaries):** * Contains highly privileged executable commands used to configure the system (`fdisk` for hard drives, `iptables` for firewalls).
  * **Access:** Locked down. You generally need `sudo` (Administrator) rights to run these programs.

### 🎛️ Configurations & Variables

* **`/etc` (Etcetera / Configuration Center):** * The "Control Room." It contains all the system-wide configuration files that dictate how programs behave.
  * ⚠️ **Security Target:** This is the ultimate goldmine for hackers. They will look for `/etc/passwd` (to see a list of all users) and `/etc/shadow` (to steal hashed passwords). Modifying the `/etc/sudoers` file allows an attacker to give themselves permanent admin rights.
    
* **`/var` (Variable Data):** * Files that are constantly changing while the system is running (like databases and print queues).
  * 🛡️ **SOC Focus:** This contains the `/var/log` directory. If you are hunting a threat actor, this is where you look. The `/var/log/auth.log` file records every single successful and failed SSH login attempt.

### 🏜️ Sandboxes & Scratch Space
* **`/home` (User Directories):** * Segregated personal sandboxes for standard users (e.g., `/home/bandit1`). 
  * **Security Context:** Built-in isolation. By default, `bandit1` cannot read the files inside `/home/bandit2` without explicit permission.
  * 
* **`/tmp` (Temporary Space):** * A scratch space for applications and users to store temporary files. It is usually wiped completely clean when the server reboots.
  * ⚠️ **Security Target:** This folder is **"world-writable"**, meaning any user can save files here. It is frequently exploited by attackers to download, store, and execute malicious scripts because the system doesn't restrict write access here.

### 🔌 Hardware Interfaces
* **`/dev` (Device Files):** * Because "everything is a file," Linux represents your physical hardware (webcams, hard drives, USBs) as text files in this directory.
* 
  * 💡 **Pro-Tip:** `/dev/null` is known as the "Black Hole." If you run a noisy command that spits out hundreds of "Permission Denied" errors, you can route that garbage text into `/dev/null` to instantly delete it and keep your terminal screen clean.
