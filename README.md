# ics344-cybersecurity-project-group01

**Group 01 – 242**

---

### 👥 Team Members

- Rawan Asiri – s202159890
- Sara Alshallali – s202167710
- Budoor Basaleh – s202182010

---

## 📌 Project Summary

This course project is designed to give hands-on experience with real-world offensive security techniques. The project is divided into multiple phases, each focusing on a specific aspect of cybersecurity including service exploitation, automation, security logging, and defensive strategies.

---

## ✅ Phase 1: Setup and Service Exploitation

**Objective**:  
Simulate an attack on a vulnerable SSH service running on Metasploitable3, first using Metasploit, then with a custom-built Python script.

**What We Did**:

- Set up attacker (Kali) and victim (Metasploitable3) environments
- Successfully logged in to SSH using Metasploit’s built-in modules
- Wrote a Python script using Paramiko to automate the same attack
- Captured screenshots as proof of successful access

**Tools Used**:

- Metasploit Framework
- Python (Paramiko)
- Kali Linux
- Metasploitable3

**Files Included**:

- `metasploit.md` – Step-by-step of Metasploit attack
- `custom_script.md` – Explanation + code for the Python script
- `metasploit_attack.png` – Screenshot from Task 1.1
- `custom_script_attack.png` – Screenshot from Task 1.2

---

## 🛠 Phase 2: SIEM Analysis
**Objective**:
This phase focuses on setting up a SIEM platform (Splunk) to collect and analyze logs from both the attacker (Kali Linux) and victim (Metasploitable3) machines. The aim is to detect brute-force attacks, visualize behavior patterns, and analyze system responses.


**What We Did**:
Installed Splunk Enterprise on the Kali Linux machine
Installed Forwarder on the Metasploitable3 machine
Configured log forwarding from Victim and Attacker
Enabled log receiving 
Built visual dashboards to monitor:
 - Failed and successful SSH login attempts
 - Brute-force attack sources
 -Timeline comparisons of attacker vs victim actions


**Tool Used**:
 - Splunk Enterprise
 - Splunk Universal Forwarder
 - Kali Linux
 - Metasploitable3

---

## 🛡 Phase 3: Defensive Measures

To be ADDED

---

## ⚙️ Environment Recap

- **Victim**: Metasploitable3 (Host-Only IP: 172.28.128.3)
- **Attacker**: Kali Linux (Host-Only IP assigned via VirtualBox)
- **Target Service**: SSH (Port 22)
- **Login Used**: msfadmin / msfadmin

---

© 2025 – Group 01 | ICS 344 – King Fahd University of Petroleum and Minerals
