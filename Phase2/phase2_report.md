# Phase 2: SIEM-Based SSH Attack Monitoring and Visualization

---

## 1. Introduction

In this phase, we deployed a Security Information and Event Management (SIEM) solution using Splunk Enterprise to collect, analyze, and visualize SSH login activity.  
The primary objective was to monitor attacker behavior and recognize cyberattack patterns through log analysis.

---

## 2. Environment Overview

| Machine | Role | Purpose | IP |
|:--------|:-----|:--------|:--------|
| Kali Linux (VMware) | Attacker | Launched brute-force and scripted SSH attacks | 192.168.8.166 |
| Metasploitable3 (UTM) | Victim | Hosted vulnerable SSH service | 192.168.8.165 |
| MacBook Pro (Host) | SIEM Server | Hosted Splunk Enterprise for log monitoring | 192.168.8.120 |

- **Splunk Version:** 9.4.1
- **Networking Mode:** Bridged Adapter (direct IP access between VMs)

**📸 Screenshot:** Successful ping from Host (Mac) to Victim Machine (Metasploitable3) — proving direct network communication.



---

## 3. Splunk Setup and Listener Configuration

- Installed Splunk Enterprise on macOS using `.dmg` installer.
- Configured a **TCP 9997** data input to receive forwarded syslog messages.
- Verified listener readiness inside Splunk.

**📸 Screenshot:** Insert TCP 9997 input creation screenshot

---

## 4. Attack Activity Preparation

SSH login attacks were simulated during Phase 1:

- **Manual and automated** SSH login attempts from Kali to Metasploitable3.
- Credentials used: `vagrant`/`vagrant`.

---

## 5. Extracting and Preparing Victim Logs

1. Accessed victim via SSH:

    ```bash
    ssh vagrant@192.168.8.165
    ```

2. Copied SSH authentication logs:

    ```bash
    sudo cp /var/log/auth.log /home/vagrant/
    sudo chmod 644 /home/vagrant/auth.log
    ```

3. Transferred logs to the attacker machine:

    ```bash
    scp vagrant@192.168.8.165:/home/vagrant/auth.log .
    ```

**📸 Screenshot:** Insert SCP file transfer evidence

---

## 6. Uploading Victim Logs to Splunk

- Uploaded `auth.log` via Splunk Web GUI under **"Add Data"** → **"Upload File"**.
- Selected source type as `linux_secure`.
- Confirmed file parsing was correct.

**📸 Screenshot:** Insert successful file upload screen

---

## 7. Data Search and Visualization

### 7.1 General SSHD Activity

```spl
search sshd
```

**📸 Screenshot:** Insert general sshd events search results

---

### 7.2 Event Pattern Discovery

- Viewed Splunk's automatic pattern grouping.

**📸 Screenshot:** Insert Patterns tab screenshot

---

### 7.3 Failed Login Attempts

```spl
search sshd "Failed password"
```

**📸 Screenshot:** Insert failed password events screenshot

---

### 7.4 Successful Login Attempts

```spl
search sshd "Accepted password"
```

**📸 Screenshot:** Insert accepted password events screenshot

---

### 7.5 Attackers by IP Address

```spl
search sshd "Failed password"
| rex "rhost=(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by attacker_ip
```

**📸 Screenshot:** Insert attacker IP statistics

---

## 8. Analysis Highlights

- Observed both successful and failed SSH login attempts.
- Identified single and repeated attacker IPs.
- Confirmed brute-force characteristics via authentication failures clustering.

---

## 9. Conclusion

This phase demonstrated effective use of Splunk SIEM to collect, ingest, and analyze SSH logs during attack scenarios.  
By correlating patterns, extracting attacker information, and visualizing events, we highlighted Splunk’s value in real-world intrusion detection and response.

---

# ✅ End of Phase 2
