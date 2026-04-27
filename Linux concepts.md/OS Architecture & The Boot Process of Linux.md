# 🏗️ 1. OS Architecture & The Boot Process (Deep Dive)

> **Overview:** Linux (created in 1991 by Linus Torvalds) operates on a strict chain of command and privilege isolation.
>  In a Cloud Security context, understanding this architecture is critical because attackers actively attempt
>  to escalate their privileges from the outermost layer (**User Space**) to the innermost core (**Kernel Space**).
---

## 🔌 Phase 1: The Hardware & Firmware Initialization
Before the operating system even exists in memory, the physical components must wake up.

* **The Hardware (Layer 0):** The physical CPU, RAM, and Disk Drives. It only understands electrical voltage and binary signals.\
  
* **BIOS / UEFI:** The firmware burned into the motherboard.
  
  * **Function:** When you hit the power button, it performs the POST (Power-On Self Test)
    to ensure RAM and CPU are functioning. It then checks a predefined boot order to find a
    storage drive containing an operating system.

## ⚙️ Phase 2: The Bootloader Execution
The motherboard firmware cannot read complex Linux file systems. It needs a specialized program to hand over control.

* **GRUB2 (Grand Unified Bootloader):** The "Ignition sequence."
  
  * **Stage 1:** A tiny script located in the Master Boot Record (MBR) or EFI partition. Its only job is to point to Stage 2.
    
  * **Stage 2:** This reads the `/boot` directory, loads the **Kernel** into the RAM, and loads the
    **Initramfs** (a temporary filesystem) so the Kernel has the essential drivers needed to access the main hard drive.

## 🧠 Phase 3: The Kernel Space (Ring 0 / God Mode)
Once loaded into RAM, the Kernel takes absolute control of the hardware. Written predominantly in **C Language**, this is the core engine of Linux.

* **Privilege Level (Ring 0):** The Kernel operates in a highly protected memory space.
  If malware breaches Ring 0, the entire system is permanently compromised (a "Rootkit" infection).
  
* **Core Responsibilities:**
  
  1. **Process Management:** Determines which applications get CPU time and for how long.
     
  3. **Memory Management:** Allocates RAM to different programs and prevents one program from
     reading another program's memory (crucial for security).
     
  5. **Device Drivers:** Translates standard commands into the specific electronic signals
     required by your exact graphics card, network card, or SSD.
     
* **Constraint:** The Kernel only understands machine code and **System Calls** (`syscalls`).

## 🚀 Phase 4: The Init System (PID 1)
The Kernel does not provide a user interface. Once the Kernel initializes the hardware, it launches the very first software process.

* **Systemd (System and Service Manager):** It is assigned **Process ID 1 (PID 1)**.
  
  * **Function:** It is the "mother of all processes." It mounts the file systems, starts the network interfaces,
     turns on the firewall, and launches background services (Daemons).
    
  * **Cloud Security Context:** When you connect to the Bandit wargames, it is `systemd` that ensures the SSH service is actively
    listening on Port 2220 so you can log in.

## 💻 Phase 5: The User Space & The Shell (Ring 3)
Because humans cannot type raw binary or C-based system calls, we interact with the system in a restricted, 
unprivileged area known as User Space.

* **The Shell (e.g., Bash, Zsh):** The UI and Translator.
  * **Function:** When you type a command like `ls`, the Shell parses the text, locates the `/bin/ls` binary,
    and uses a system call (specifically `fork()` and `execve()`) to ask the Kernel to execute the program.
    
  * **Security Isolation:** If you run a malicious script in the Shell without `sudo` (Administrator) rights,
    the Kernel blocks the script from modifying critical hardware or reading other users' files.
