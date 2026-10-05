# Week 3 - System Observation

## Introduction

This system observation was conducted on an Ubuntu Linux virtual machine as part of the Week 3 Linux Foundations for Cybersecurity practical exercise.

The purpose of this investigation was to observe the current user, network configuration, running processes, services, listening ports, and recent system log activity using Linux command-line tools.

The investigation was performed using standard Linux commands without making changes to system services.

---

## Current User

The following commands were used to identify the current user:

```bash
whoami
id
```

The `whoami` command showed that the current user was:

```text
[ushani]
```

The `id` command provided information about the user's UID, GID, and group memberships.

For example:

```text
uid=[1000]([ushani]) gid=[1000]([ushani]) groups=[1000(ushani),4(adm),24(cdrom),27(sudo),30(dip),46(lugdev),122(lpadmin),134(lxd),135(sambashare)]
```

### Observation

The Ubuntu VM was being accessed using a normal user account rather than directly using the root account. The `id` command also showed the groups to which the user belongs.

---

## Network Configuration

The following commands were used to investigate the network configuration:

```bash
ip addr
ip route
```

The active network interface was:

```text
[enp0s3]
```

The IP address assigned to the Ubuntu VM was:

```text
[10.0.2.15]
```

The default route was through:

```text
[10.0.2.2]
```

using the:

```text
[enp0s3]
```

network interface.

### Observation

The `ip addr` command was used to identify the active network interface and its assigned IP address.

The `ip route` command showed the default route used by the Ubuntu VM to communicate with networks outside its local network.

---

## 4. Running Processes

The following command was used to view currently running processes:

```bash
ps aux
```

Several processes were running on the Ubuntu VM. Examples observed were:

* **[PROCESS 1]**
* **[PROCESS 2]**
* **[PROCESS 3]**
* **[PROCESS 4]**

The `ps aux` command displays information such as the user running a process, process ID (PID), CPU usage, memory usage, and the command used to start the process.

### Observation

The Ubuntu VM had multiple processes running at the same time. These included operating-system processes and processes associated with applications and system services.

---

## Running Services

The following command was used to identify currently running services:

```bash
systemctl --type=service --state=running
```

Several services were running on the system. Examples observed were:

* **[accounts-daemon.service]**
* **[avahi-daemon.service]**
* **[colord.service]**
* **[cron.service]**

### Observation

The `systemctl` command showed the services that were currently active on the Ubuntu VM.

These services perform different system functions such as networking, system management, scheduling, logging, or other operating-system tasks.

No services were disabled or stopped during this investigation.

---

## Listening TCP/UDP Ports

The following command was used to identify listening TCP and UDP ports:

```bash
ss -tuln
```

The following listening ports were observed:

* **[53]**
* **[631]**
* **[80]**

### Observation

The `ss -tuln` command displays network sockets that are currently listening for connections.

A listening port indicates that a service or application is waiting for network communication on that port.

Only the ports observed in the Ubuntu VM were recorded in this investigation.

---

## Disk Space

The following command was used to check available disk space:

```bash
df -h
```

### Observation

The `df -h` command displayed the disk space usage of the mounted file systems in a human-readable format.

The command showed information such as:

* Total disk space
* Used disk space
* Available disk space
* Percentage of disk space used
* Mounted file system

The Ubuntu VM had sufficient/limited available disk space based on the output observed.

---

## Memory Usage

The following command was used to inspect memory usage:

```bash
free -h
```

### Observation

The `free -h` command displayed the amount of total, used, free, shared, cached, and available memory.

This command provided a quick overview of the current memory usage of the Ubuntu VM.

---

## Recent System Logs

The following command was used to inspect recent system log entries:

```bash
journalctl -n 50
```

The recent log entries contained information such as:

* Timestamp: **[Oct 02 10:25:52]**
* Process/service: **[systemd]**
* Message: **[Started Tracker metadata extracter.]**

### Observation

The `journalctl` command provided information about recent system activity.

The log entries included timestamps, process or service names, and messages describing events that occurred on the Ubuntu system.

---

## Warning-Level Logs

The following command was used to inspect recent warning-level log messages:

```bash
journalctl -p warning -n 30
```

### Observation

This command displayed recent log entries with warning-level priority or higher.

The observed entries were:

* **[systemd: snap.snapd-desktop-integration.snapd-desktop-integration.service: Failed with result 'exit-code']**
* **[gnome-shell: g_object_get: assertion 'G_ISOBJECT (objetc)' failed]**
* **[tracker-miner-f: Endpoint failed to fully write cursor: Interrupted]**

---

## Security Observations

The system investigation provided several observations about the Ubuntu VM:

1. A normal user account was being used to operate the system.
2. The VM had an active network interface with an assigned IP address.
3. A default network route was configured through a gateway.
4. Multiple operating-system processes were running.
5. Several system services were active.
6. Some TCP/UDP ports were listening for network connections.
7. System logs contained records of recent system activity.
8. File permissions can be used to restrict access to sensitive information.
9. The Linux command line provides useful tools for investigating system activity.

These observations demonstrate why processes, services, network connections, permissions, and logs are important areas when investigating the security of a Linux system.

---

## Commands Used During the Investigation

The following commands were used:

```bash
whoami
pwd
ls -la
id
ip addr
ip route
ps aux
ss -tuln
df -h
free -h
systemctl --type=service --state=running
journalctl -n 50
journalctl -p warning -n 30
```

---

## Summary Table

| Area Investigated | Command                                    | Observation                      |
| ----------------- | ------------------------------------------ | -------------------------------- |
| Current user      | `whoami`                                   | [ushani]                  |
| User information  | `id`                                       | UID, GID and groups identified   |
| IP address        | `ip addr`                                  | [10.0.2.15]                |
| Network interface | `ip addr`                                  | [enp0s3]                 |
| Default route     | `ip route`                                 | [10.0.2.2]           |
| Processes         | `ps aux`                                   | Multiple processes were running  |
| Listening ports   | `ss -tuln`                                 | [53], [631], [80]     |
| Disk usage        | `df -h`                                    | Disk usage inspected             |
| Memory usage      | `free -h`                                  | Memory usage inspected           |
| Running services  | `systemctl --type=service --state=running` | Multiple services were running   |
| Recent logs       | `journalctl -n 50`                         | Recent system activity inspected |
| Warning logs      | `journalctl -p warning -n 30`              | Warning-level logs inspected     |

---

## Overall Findings

The investigation showed that the Ubuntu VM was an active Linux system with a configured network interface, running processes, active services, and listening network ports.

The command-line tools provided useful information about the current state of the system. In particular, `ps aux` helped identify running processes, `systemctl` showed active services, `ss` showed listening network ports, and `journalctl` provided information about recent system events.

These commands are useful for basic Linux system administration and cybersecurity investigations because they help identify what is running on a system and how the system is communicating.

---

## Conclusion

This system observation exercise improved my understanding of how to investigate a Linux system from the command line.

I learned how to identify the current user, inspect network configuration, view running processes and services, identify listening ports, check disk and memory usage, and examine system logs.

The investigation also demonstrated that Linux provides built-in command-line tools that can be used to collect useful information during a basic cybersecurity assessment.

No system services were disabled or modified during the investigation.
