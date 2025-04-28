# Phase 3: Defensive Strategy Proposal

---

## 1. Introduction

In Phase 3 of the project, we implemented a defensive strategy to protect the SSH service on the Metasploitable3 victim machine. After successfully exploiting SSH vulnerabilities in Phase 1 and visualizing the attacks in Phase 2, the goal of this phase was to harden the system and prevent unauthorized access or successful brute-force attacks.

---

## 2. Selected Defensive Mechanism

To secure the SSH service, we applied two layers of defense:

- **SSH Configuration Hardening:**
  - Disabled password-based authentication (`PasswordAuthentication no`)
  - Disabled root login (`PermitRootLogin no`)

- **Network Firewall (UFW) Restriction:**
  - Allowed SSH access only from the Kali Linux attacker's IP (`192.168.8.166`)
  - Blocked all other incoming connections by default (`ufw default deny incoming`)

This dual approach ensured that even if a connection reached the server, it would still be blocked unless it used proper authentication mechanisms.

---

## 3. Implementation Steps

### 3.1 Harden SSH Configuration

- Edited the SSH server configuration file `/etc/ssh/sshd_config`.
- Applied the following settings:

  ```bash
  PermitRootLogin no
  PasswordAuthentication no
