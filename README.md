# Cloud & DevOps Learning Lab

My hands-on journey from Linux fundamentals to Cloud, DevOps, and DevSecOps.

> **Learning approach:** Beginner-first, practical, one concept at a time. Each lesson includes explanations, commands, examples, exercises, and a progress checklist.

## Course roadmap

- [x] Set up Kali Linux VM for the lab
- [x] Expand the VM disk to 80 GB and configure a 2 GB swapfile
- [ ] Linux fundamentals and terminal navigation
- [ ] Files, directories, and text tools
- [ ] Linux filesystem hierarchy
- [ ] Users, groups, permissions, and `sudo`
- [ ] Processes, services, and `systemd`
- [ ] Networking, DNS, ports, and SSH
- [ ] Bash scripting and automation
- [ ] Git and GitHub
- [ ] Docker and Docker Compose
- [ ] HTTP, Nginx, reverse proxies, and TLS
- [ ] AWS fundamentals and deployment
- [ ] CI/CD with GitHub Actions
- [ ] Infrastructure as Code with Terraform
- [ ] Kubernetes fundamentals
- [ ] Monitoring, logging, and alerting
- [ ] DevSecOps fundamentals
- [ ] Capstone: deploy and monitor an application

## Lab environment

- **OS:** Kali Linux (used here as a general Linux learning environment)
- **Virtual disk:** 80 GB virtual size; about 79 GiB usable inside Linux
- **RAM:** Approximately 3 GB
- **CPU:** 2 virtual CPUs
- **Swap:** 2 GiB swapfile at `/swapfile`
- **Purpose:** Linux fundamentals and Cloud/DevOps practice; Kali-specific penetration-testing tools are not the focus of this track.

Useful checks for the environment:

```bash
df -h /
free -h
swapon --show
lsblk
```

## Lesson 1 — What is Linux?

### Key concepts

- **Linux kernel:** The core that manages hardware resources and provides essential operating-system services, including processes, memory, devices, networking, and filesystems.
- **Operating system/distribution:** A usable Linux system combines the kernel with system tools, utilities, package management, and applications. Kali Linux and Ubuntu are examples of Linux distributions.
- **Terminal:** A text-based interface for entering commands and seeing their output.
- **Shell:** The program that interprets commands entered in the terminal and launches the relevant programs.
- **Command-line interface (CLI):** A way to interact with a system by typing commands rather than relying only on graphical controls.

### First command

```bash
pwd
```

`pwd` means **print working directory**. It displays the path of the directory the shell is currently working in.

Example output (your output may differ):

```text
/home/kali
```

### Check your understanding

- [ ] Explain the difference between a terminal and a shell in your own words.
- [ ] Run `pwd` and identify the directory it prints.

## Lesson 2 — Terminal navigation

### Commands

| Command | Meaning | Example |
|---|---|---|
| `pwd` | Print the current working directory | `pwd` |
| `ls` | List visible items in a directory | `ls` |
| `ls -l` | List items in long/detailed format | `ls -l` |
| `ls -la` | Include hidden items in detailed output | `ls -la` |
| `cd PATH` | Change directory | `cd /tmp` |
| `cd ~` | Go to your user's home directory | `cd ~` |
| `cd` | Also go to your user's home directory | `cd` |
| `cd ..` | Move to the parent directory | `cd ..` |

### Important path symbols

- `/` — the root of the filesystem.
- `~` — shorthand for the current user's home directory.
- `.` — the current directory.
- `..` — the parent directory.
- `/root` — the root user's home directory; it is not the same as `/`.

### Practice sequence

Run one command at a time and observe how the output changes:

```bash
pwd
ls
ls -la
cd /tmp
pwd
cd ~
pwd
```

**Remember:** `ls` lists items; `cd` changes your current location; `pwd` tells you where you are.

### Check your understanding

- [ ] Explain what `/`, `~`, `.`, and `..` mean.
- [ ] Navigate to `/tmp`, verify with `pwd`, then return home.

## Lesson 3 — Files and directories

### Create directories and files

| Command | Meaning | Example |
|---|---|---|
| `mkdir NAME` | Create a directory | `mkdir linux-lab` |
| `touch FILE` | Create an empty file if it doesn't exist | `touch notes.txt` |
| `cat FILE` | Print a file's contents | `cat notes.txt` |
| `cp SOURCE DEST` | Copy a file or directory (directory copies need appropriate options) | `cp notes.txt backups/` |
| `mv SOURCE DEST` | Move or rename a file/directory | `mv notes.txt linux-notes.txt` |
| `rm FILE` | Remove a file | `rm linux-notes.txt` |

### Writing text with redirection

Overwrite a file's contents:

```bash
echo "Linux is my first DevOps skill" > notes.txt
```

Append another line without replacing existing contents:

```bash
echo "I am learning Cloud and DevOps" >> notes.txt
```

Read the result:

```bash
cat notes.txt
```

- `>` redirects output and **overwrites** an existing file.
- `>>` redirects output and **appends** to the file.

### Moving and renaming

```bash
mkdir documents
mv notes.txt documents/
cd documents
mv notes.txt linux-notes.txt
```

The first `mv` moves the file into `documents/`; the second renames it.

### Safety note

Linux `rm` generally deletes directly rather than sending files to a recycle bin. Check your current directory and target before removing anything. Avoid destructive commands such as recursive deletion of system paths.

### Practice lab

The following creates a small practice structure under your home directory. If `devops-lab` already exists, inspect it first rather than blindly repeating the setup.

```bash
cd ~
mkdir devops-lab
cd devops-lab
mkdir projects backups
touch app.txt
echo "My first Linux project" > app.txt
cat app.txt
cp app.txt backups/
mv app.txt projects/
mv projects/app.txt projects/devops.txt
ls -R
```

Expected structure:

```text
devops-lab/
├── backups/
│   └── app.txt
└── projects/
    └── devops.txt
```

### Check your understanding

- [ ] Explain the difference between `>` and `>>`.
- [ ] Explain the two common uses of `mv`.
- [ ] Build the practice directory structure and verify it with `ls -R`.

## Lesson 4 — Linux filesystem hierarchy

### The big idea

Linux organizes files and directories under one top-level directory: `/`, called the **root directory**. This is different from `/root`, which is the home directory of the root administrator.

### Important directories

| Path | Purpose | DevOps relevance |
|---|---|---|
| `/` | Root of the filesystem hierarchy | Starting point for absolute paths |
| `/home` | Home directories for regular users | User files and SSH keys |
| `/root` | Root administrator's home directory | Administrative account files |
| `/etc` | System and application configuration | Configuring services and applications |
| `/var` | Variable data that changes during operation | Logs, caches, spool data |
| `/var/log` | Common location for logs | Troubleshooting service and system failures |
| `/tmp` | Temporary files | Short-lived working files |
| `/usr/bin` | Many user commands and executable programs | Finding installed tools |
| `/usr/lib` | Many libraries used by programs | Application dependencies |
| `/opt` | Optional or third-party application software | Some manually installed tools |
| `/dev` | Device interfaces represented as files | Inspecting disk and device nodes |
| `/proc` | Virtual filesystem exposing process and kernel information | Inspecting running system state |

### Explore safely

These commands only display information; they do not modify system files:

```bash
ls /
ls /home
ls /etc
ls /var
ls /var/log
ls /tmp
ls /usr/bin | head
ls /opt
ls /dev | head
ls /proc | head
```

Some directories may look empty or show different contents depending on your system and permissions. That is normal.

### Remember

- `/` is the filesystem root; `/root` is a user's home directory for the administrator.
- `/etc` is primarily for configuration, not personal documents.
- `/var/log` is a key place to look when diagnosing Linux service problems.
- `/proc` is a virtual filesystem, not a normal collection of files stored permanently on disk.
- `/dev` exposes device interfaces; do not modify or delete entries casually.

### Practice and self-check

- [ ] Run `ls /` and identify at least five directories.
- [ ] Explain the difference between `/home` and `/root`.
- [ ] Explain why `/etc` and `/var/log` matter to DevOps.
- [ ] Explore `/proc` and explain why its contents differ from ordinary stored files.

**Progress note:** Lesson 4 has been explained. Mark the exercises complete after running the commands and checking your understanding.

## How lesson notes are maintained

At the end of each lesson, update this README with the new concepts, commands, examples, exercises, common errors, and progress. Keep commands safe and scoped to the practice lab; do not run unfamiliar destructive commands on the system.


### Beginner explanation: what is a configuration file?

A **configuration file** is a file that stores settings telling a program or the operating system how to behave. Think of game settings such as language, sound, or screen resolution: software needs to store choices somewhere. Linux services and applications also use settings files.

Examples:

| File | What it configures |
|---|---|
| `/etc/hostname` | The computer's hostname |
| `/etc/fstab` | Filesystems and swap resources to mount or enable at startup |
| `/etc/hosts` | Local hostname-to-address mappings |
| `/etc/ssh/sshd_config` | SSH server settings, if the server is installed |

You can safely read some settings with commands such as:

```bash
cat /etc/hostname
cat /etc/hosts
```

Reading a configuration file is not the same as editing it. For now, inspect files only; we'll learn safe configuration changes later.

### Why are there so many directories?

Linux separates different kinds of information so the system is easier to organize, secure, maintain, and troubleshoot. Imagine an office building with personal workspaces, a settings room, activity records, and a temporary desk. Different areas have different jobs.

- `/` is the top-level root of the filesystem; all the directories are organized beneath it.
- `/home` is where regular users usually keep personal files.
- `/root` is the home directory for the root administrator; it is not the same as `/`.
- `/etc` commonly stores system-wide configuration.
- `/var/log` is a common place for system and application logs.
- `/tmp` is used for temporary files.
- `/usr/bin` contains many installed commands and programs.
- `/opt` is used for some optional or third-party applications.
- `/dev` exposes device interfaces.
- `/proc` is a virtual filesystem showing live process and kernel information rather than ordinary permanent files.

### Beginner practice

These commands only display information; they do not change system files:

```bash
cat /etc/hostname
ls /
ls /home
ls /etc
ls /var/log
ls /tmp
ls /usr/bin | head
ls /proc | head
```

You do not need to memorize every path today. Start by remembering these four: `/home` = personal files, `/etc` = configuration, `/var/log` = logs, and `/tmp` = temporary files.

## Next lesson

**Lesson 5 — Linux users, groups, permissions, and `sudo`.**
