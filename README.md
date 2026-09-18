# 🔐 Linux Privilege Escalation Lab

> Hands-on Linux privilege escalation assessment using Kali Linux and Metasploitable 2.

## 🎯 Objective

The goal of this lab was to follow a basic penetration testing workflow:

**Reconnaissance → Enumeration → Finding → Exploitation → Root Proof → Remediation**

I performed the assessment in a controlled VirtualBox lab environment.

---

## 🖥️ Lab Environment

- 🔴 Attacker: Kali Linux
- 🎯 Target: Metasploitable 2
- 🌐 Target IP: `192.168.56.102`
- 💻 Virtualization: VirtualBox
- 🛡️ Environment: Controlled lab

---

# 🔎 01. Network Connectivity

I first verified that Kali Linux could communicate with the target machine.

### Command

```bash
ping -c 4 192.168.56.102

```
Purpose
To confirm that the target was reachable before starting the assessment.

## 🔍 02. Service Enumeration

I used Nmap to identify open ports and running service versions.

### Command

```bash
nmap -sV 192.168.56.102

```
The -sV option attempts to identify the versions of services running on discovered ports.
This helps identify the available attack surface.

## 🔓 3. Full Port Scan
I performed a full TCP port scan to identify services running on uncommon ports.

### Command

```bash
sudo nmap -p- 192.168.56.102
```

Purpose
The -p- option scans TCP ports 1–65535.
This helps reduce the chance of missing services running on non-standard ports.

## 👤 4. Local Enumeration
After accessing the target, I performed basic local enumeration.
### Commands

```bash
ip a
whoami
```
Purpose
ip a was used to inspect the network configuration.
whoami was used to identify the current user and determine the current privilege level.

## ⚠️ 5. Sudo Privilege Enumeration
I checked which commands the current user was allowed to execute with elevated privileges.
### Command

```bash

sudo -l

```

Finding
The output showed highly permissive sudo privileges, including ALL.
This created a potential privilege escalation path because the user was allowed to execute commands with elevated privileges.

## 🚀 6. Privilege Escalation
The allowed FTP binary was executed with elevated privileges.
### Command
```bash

sudo ftp

```
Inside the FTP interface, I used the following shell escape:
```bash

!/bin/sh
```
This resulted in a shell inheriting the elevated privileges of the FTP process.

## 👑 7. Root Access Verification
After the privilege escalation, I verified the current privilege level.
### Command
```bash
whoami
```
Result
root
This confirmed successful privilege escalation to root.

---

# 🛡️ Security Finding

## Sudo Misconfiguration

The user had excessive sudo permissions.

A shell-capable application could be executed with elevated privileges, creating a path from a lower-privileged user to root-level access.

### Potential Impact

Successful exploitation could allow an attacker to:

- Execute commands as root
- Access sensitive files
- Modify system configurations
- Perform privileged operations
- Fully compromise the affected system

---

# 🔧 Remediation

The system should follow the **Principle of Least Privilege**.

### Recommended Actions

- Remove unnecessary sudo permissions.
- Avoid unrestricted `ALL` privileges.
- Allow only the commands required by the user.
- Regularly review `sudoers` configurations.
- Avoid granting elevated access to shell-capable applications unless required.

---

# 🧠 Skills Demonstrated

- Linux Enumeration
- Network Reconnaissance
- Nmap
- Service Enumeration
- Sudo Enumeration
- Linux Privilege Escalation
- Root Access Verification
- Security Documentation
- Remediation Analysis

---

# 📋 Methodology

```text
Network Connectivity
        ↓
Service Enumeration
        ↓
Full Port Scan
        ↓
Local Enumeration
        ↓
Sudo Enumeration
        ↓
Sudo Misconfiguration
        ↓
Privilege Escalation
        ↓
Root Access


