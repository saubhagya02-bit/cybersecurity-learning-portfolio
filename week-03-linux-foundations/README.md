# Week 3 - Linux Foundations for Cybersecurity

## Overview

This week focused on developing basic Linux command-line skills for cybersecurity investigation and system observation.

Ubuntu Linux was used as the practical environment. The exercises focused on identifying system information, investigating processes and services, checking network configuration, understanding file permissions, and examining system logs.

The main approach was to use Linux command-line tools to observe the system without making unnecessary changes.

---

## Objectives

By completing this week, I aimed to:

* Become comfortable using the Linux command line.
* Identify the current Linux user and user information.
* Inspect files and directories.
* Understand basic network configuration.
* Identify running processes.
* Identify running system services.
* Identify listening TCP/UDP ports.
* Check disk and memory usage.
* Understand Linux file permissions.
* Inspect recent system logs.
* Perform a basic system observation for cybersecurity purposes.

---

## Environment

### Operating System

* Ubuntu Linux
* Ubuntu Virtual Machine
* VirtualBox

### User Account

A normal non-root user account was used during the exercises.

Administrative privileges were used only when necessary.

---

## Command-Line Exercises

The following Linux commands were practised:

| Command                                    | Purpose                                                |
| ------------------------------------------ | ------------------------------------------------------ |
| `whoami`                                   | Displays the current username                          |
| `pwd`                                      | Displays the current working directory                 |
| `ls -la`                                   | Lists files, directories, permissions and hidden files |
| `id`                                       | Displays user ID, group ID and group memberships       |
| `ip addr`                                  | Displays network interfaces and IP addresses           |
| `ip route`                                 | Displays the routing table and default route           |
| `ps aux`                                   | Displays currently running processes                   |
| `ss -tuln`                                 | Displays listening TCP/UDP ports                       |
| `df -h`                                    | Displays disk usage in human-readable format           |
| `free -h`                                  | Displays memory usage                                  |
| `systemctl --type=service --state=running` | Displays currently running services                    |
| `journalctl -n 50`                         | Displays recent system log entries                     |
| `journalctl -p warning -n 30`              | Displays recent warning-level log entries              |

Detailed command observations are documented in [`command-journal.md`](./command-journal.md).

---

## File Permissions Exercise

A separate `security-lab` directory was created in the user's home directory.

The following files were created:

```text
security-lab/
├── public.txt
└── secret.txt
```

The following permissions were applied:

```bash
chmod 600 secret.txt
chmod 644 public.txt
```

### Permission Results

| File         | Permission | Meaning                                               |
| ------------ | ---------- | ----------------------------------------------------- |
| `secret.txt` | `600`      | Owner can read/write; group and others have no access |
| `public.txt` | `644`      | Owner can read/write; group and others can read       |

The `600` permission is more appropriate for information that should only be accessible to the file owner.

The complete exercise is documented in [`permissions-exercise.md`](./permissions-exercise.md).

---

## Processes and Services Investigation

The following commands were used to investigate the running system:

```bash
ps aux
```

and:

```bash
systemctl --type=service --state=running
```

The `ps aux` command was used to identify currently running processes.

The `systemctl` command was used to identify currently running system services.

The investigation was performed for observation purposes. No services were disabled simply to create security findings.

---

## Network Investigation

The following commands were used:

```bash
ip addr
ip route
ss -tuln
```

These commands were used to identify:

* Active network interfaces
* IP addresses
* Default gateway
* Routing information
* Listening TCP/UDP ports

The actual network information collected from the Ubuntu VM is documented in [`system-observation.md`](./system-observation.md).

---

## System Logs Investigation

The following commands were used to inspect system logs:

```bash
journalctl -n 50
```

and:

```bash
journalctl -p warning -n 30
```

The first command was used to examine recent system log entries.

The second command was used to examine recent warning-level log entries.

The investigation focused on identifying:

* Timestamp
* Process or service
* Log message
* Warning-level events

No services were disabled or modified to create findings.

---

## System Observation

A one-page system observation was completed using the information collected from the Linux commands.

The investigation covered:

* Current user
* IP address
* Network interface
* Default route
* Running processes
* Running services
* Listening ports
* Recent system logs
* Warning-level log entries
* Basic security observations

The complete observation is available in [`system-observation.md`](./system-observation.md).

---

## Evidence

Selected terminal screenshots were collected as evidence of the practical exercises.

Recommended evidence:

```text
screenshots/
├── 

```

The screenshots show selected command outputs from the Ubuntu virtual machine.

---

## Skills Demonstrated

Through this week's practical work, I demonstrated the ability to:

* Navigate the Linux command line.
* Identify users and groups.
* Inspect files and directories.
* Examine Linux network configuration.
* Identify running processes.
* Identify running services.
* Identify listening network ports.
* Check disk and memory usage.
* Understand Linux file permissions.
* Inspect system logs.
* Perform basic Linux system observation.
* Document technical findings clearly.

---

## Key Learning

This week demonstrated how Linux command-line tools can be used for basic cybersecurity investigation.

Commands such as `ps`, `ss`, `systemctl`, and `journalctl` provide useful information about what is running on a system, which services are active, which network ports are listening, and what events have recently occurred.

The file permissions exercise also demonstrated how Linux can restrict access to files using owner, group, and other permissions.

Understanding these basic Linux concepts is important for later cybersecurity activities such as system monitoring, incident investigation, and network security analysis.

---

### Files

* [`command-journal.md`](./command-journal.md)
* [`permissions-exercise.md`](./permissions-exercise.md)
* [`system-observation.md`](./system-observation.md)
* [`screenshots/`](./screenshots/)

---

## Conclusion

Week 3 provided practical experience with Linux system investigation using the command line.

I learned how to identify users, inspect system and network information, investigate processes and services, understand file permissions, and examine system logs.

These skills provide a foundation for performing more detailed cybersecurity investigations in later practical exercises.
