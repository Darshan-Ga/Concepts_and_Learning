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

---
*Note: This playbook is a living document. As I learn more and encounter new DevOps concepts, I am going to continually update this list.*
