# ICS344 Project – Phase 1 Report

**Title:** SSH Service Exploitation on Metasploitable3  
**Student:** Sultan Alatawi  
**Date:** April 27, 2025

---

## Objective
The goal of Phase 1 is to exploit a vulnerable SSH service running on a Metasploitable3 virtual machine (victim) using a Kali Linux machine (attacker).  
This phase demonstrates successful brute-force attacks using Hydra, manual exploitation via Metasploit Framework, and automation through a custom Python script.

---

## Environment Setup
- **Attacker Machine:** Kali Linux  
  - IP: `192.168.8.164/24`
- **Victim Machine:** Metasploitable3 Ubuntu 14.04  
  - IP: `192.168.8.165/24`
- **Network:** Both machines configured on the same private network (`192.168.8.x`).

---

## Task 1.1: Manual Exploitation Using Metasploit

### Steps:
1. Scanned the victim machine using **Nmap** to identify open ports.
2. Found **SSH** service running on **port 22**.
3. Used **Hydra** to brute-force the SSH login credentials:
   - Username: `vagrant`
   - Password: `vagrant`
4. Opened **Metasploit Framework** (`msfconsole`).
5. Loaded the auxiliary module: `auxiliary/scanner/ssh/ssh_login`.
6. Set options and exploited SSH to establish a session.

### Proof:
> (Insert screenshots here:  
> - Nmap scan results  
> - Hydra brute force output  
> - Successful SSH session in Metasploit  
> - `whoami` command output)

---

## Task 1.2: Automated Exploitation Using a Custom Python Script

### Steps:
1. Developed a **Python 3** script named `ssh_exploit.py` using **Paramiko**.
2. Script automatically connects to the victim’s SSH server with valid credentials.
3. Executes the `whoami` command remotely after connecting.

### Proof:
> (Insert screenshots here:  
> - Running the `ssh_exploit.py` script  
> - Successful SSH connection output)

---

## Summary
- Successfully exploited the vulnerable SSH service manually using **Hydra** and **Metasploit**.
- Successfully built and tested a **custom Python script** to automate SSH exploitation as Proof of Concept (PoC).
- Confirmed network configuration allowed exploitation between attacker and victim.

---

# End of Phase 1

