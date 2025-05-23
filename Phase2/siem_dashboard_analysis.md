﻿# Phase 2: SIEM Dashboard Analysis – Splunk Setup & Log Integration

This part shows how we set up **Splunk** to collect logs from both attacker (Kali) and victim (Metasploitable3) environments, visualize brute-force attacks, and analyze system responses.

---

## Environment Overview

| **System**        | **Purpose**   | **IP Address**    |
| ----------------- | ------------- | ----------------- |
| Kali Linux        | Attacker      | 172.28.128.5      |
| Metasploitable3   | Victim        | 172.28.128.3      |
| Splunk Enterprise | SIEM Platform | Installed on Kali |

---

## Step-by-Step Setup

---

### 1. Install Splunk Enterprise (Kali)

1. **Download Splunk Enterprise:**

```bash
wget -O splunk-9.4.1-e3bdab203ac8-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/9.4.1/linux/splunk-9.4.1-e3bdab203ac8-linux-amd64.deb"
```

2. **Install Splunk Enterprise:**

```bash
sudo dpkg -i splunk-9.4.1-e3bdab203ac8-linux-amd64.deb
```

3. **Start Splunk Enterprise:**

```bash
sudo /opt/splunk/bin/splunk start
```

- Set admin username and password on first run.
- Access Splunk Web on **http://localhost:8000**

---

### 2. Install Splunk Universal Forwarder (Metasploitable3)

1. **Download Splunk Forwarder:**

```bash
wget -O splunkforwarder-9.4.1-e3bdab203ac8-linux-amd64.deb "https://download.splunk.com/products/universalforwarder/releases/9.4.1/linux/splunkforwarder-9.4.1-e3bdab203ac8-linux-amd64.deb"
```

2. **Install Splunk Forwarder:**

```bash
sudo dpkg -i splunkforwarder-9.4.1-e3bdab203ac8-linux-amd64.deb
```

3. **Start Splunk Forwarder:**

```bash
sudo /opt/splunkforwarder/bin/splunk start
```

- Set a new password when prompted.

---

### 3. Configure Log Forwarding from Metasploitable3

1. **Add Splunk (Kali) as a forward-server:**

```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server 172.28.128.5:9997
```

2. **Monitor victim log file:**

```bash
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/auth.log
```

3. **Restart Forwarder:**

```bash
sudo /opt/splunkforwarder/bin/splunk restart
```

---

### 4. Enable Receiving in Splunk Enterprise (Kali)

```bash
sudo /opt/splunk/bin/splunk enable listen 9997
```

---

### 5. Verify Forwarding Status

On Metasploitable3:

```bash
sudo /opt/splunkforwarder/bin/splunk list forward-server
```

- You should see: `172.28.128.5:9997 Active`

---

### 6. Forward Attacker Logs from Kali

1. **Install Splunk Forwarder (if not already):**

```bash
wget -O splunkforwarder-9.4.1-e3bdab203ac8-linux-amd64.deb "https://download.splunk.com/products/universalforwarder/releases/9.4.1/linux/splunkforwarder-9.4.1-e3bdab203ac8-linux-amd64.deb"
sudo dpkg -i splunkforwarder-9.4.1-e3bdab203ac8-linux-amd64.deb
sudo /opt/splunkforwarder/bin/splunk start
```

2. **Configure Kali to forward:**

```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server 127.0.0.1:9997
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/attack.log
sudo /opt/splunkforwarder/bin/splunk restart
```

---

## Logs Forwarded

- **Victim (Metasploitable3):** `/var/log/auth.log`
- **Attacker (Kali):** `/var/log/attack.log`

---

## Splunk Dashboard Panels

After setting up Splunk:

1. Go to **Search & Reporting**.
2. Create a new **Classic Dashboard**.
3. Add each panel using the following table:

---

## 🧩 Panel Searches & Visualizations
### 🔍 Splunk Dashboard Panels and Search Queries

| **Panel Title**                         | **Search Query**                                                                                           |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------|
| Failed SSH Logins Over Time            | `sourcetype=_auth_ "Failed password"`<br>`timechart count by host`                                          |
| Successful SSH Logins with Source IP   | `sourcetype=_auth_ "Accepted password"`<br>`table _time, user, src`                                         |
| Total Successful vs Failed Logins      | `source="/var/log/auth.log" ("Failed password" OR "Accepted password")`<br>`stats count by Result`         |
| SSH Sessions Opened Over Time          | `sourcetype=_auth_ "session opened for user"`<br>`timechart count by user`                                  |
| Source IPs for Brute-Force Attempts    | `sourcetype=_auth_ "Failed password"`<br>`rex "from (?<src_ip>\\d+\\.\\d+\\.\\d+\\.\\d+)"`<br>`stats count by src_ip` |
| Attacker Log Activity (Brute-Force)    | `source="/var/log/attack.log"`<br>`table _time, _raw`                                                       |
| Timeline Comparison: Attacker vs Victim| `(source="/var/log/attack.log" OR source="/var/log/auth.log")`<br>`timechart span=1m count by source`       |
| Count of Attacker Actions vs Victim    | `(source="/var/log/attack.log" OR source="/var/log/auth.log")`<br>`stats count by source`                   |
| Detailed Attack & Response Timeline    | `(source="/var/log/attack.log" OR source="/var/log/auth.log")`<br>`table _time, source, host, _raw`         |

## Dashboard Panel Screenshots (in phase2 folder)

| **Panel**                               | **Screenshot File Name**                            |
| --------------------------------------- | --------------------------------------------------- |
| SIEM Dashboard Overview                 | `siem_dashboard.png`                                |
| Failed SSH Logins Over Time             | `failed_ssh_logins.png`                             |
| Successful SSH Logins with Source IP    | `successful_ssh_logins.png`                         |
| Total Successful vs Failed Logins       | `login_attempt_summary_success_vs_failure.png`      |
| SSH Sessions Opened Over Time           | `ssh_sessions_over_time.png`                        |
| Source IPs for Brute-Force Attempts     | `top_attacker_ips.png`                              |
| Attacker Log Activity Table             | `attacker_log_activity.png`                         |
| Count of Attacker Actions vs Victim     | `Count of Attacker Actions vs Victim Responses.png` |
| Detailed Attack & Response Log Timeline | `Detailed Attack Timeline.png`                      |

---

## ✅ Completion

This setup successfully fulfills the Phase 2 objectives:

- ✅ **SIEM Setup** – Integrated logs from victim and attacker systems.
- ✅ **Log Visualization** – Built visual dashboards for SSH attack activity.
- ✅ **Attack Comparison** – Analyzed and correlated attacker vs victim logs.

This completes **Phase 2: SIEM Dashboard Analysis**.
