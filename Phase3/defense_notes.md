# Phase 3 – Defensive Strategy Proposal: Blocking SSH Brute-Force with Fail2Ban


This section demonstrates how we secured the vulnerable SSH service on Metasploitable3 using Fail2Ban. After applying the defense, we reran the attack to validate its effectiveness.

---

## Target Info

- IP Address: 172.28.128.3
- Port: 22
- Service: SSH
- Goal: Prevent brute-force attacks by monitoring failed login attempts and automatically banning suspicious IPs.

---

## 🛠 Defense Mechanism: Fail2Ban Setup

We chose Fail2Ban, a popular intrusion prevention tool, to automatically detect failed SSH login attempts and ban the attacker's IP after repeated failures.

---

## Steps

1. Install Fail2Ban on Metasploitable3:

```bash
sudo apt-get update
sudo apt-get install fail2ban
```

2. Configure Jail for SSH:

```bash
[sshd]
enabled = true
filter = sshd
port = ssh
logpath = /var/log/auth.log
maxretry = 3
findtime = 600
bantime = 600
```
This configuration means:

- After 3 failed login attempts (maxretry = 3)
- Within a 10-minute window (findtime = 600)
- The IP will be banned for 10 minutes (bantime = 600)



3. Ensure SSH Filter is Present:

```bash
/etc/fail2ban/filter.d/sshd.conf
```
Why this matters:

This filter contains the regex patterns Fail2Ban uses to identify failed login attempts in /var/log/auth.log. Without it, Fail2Ban won’t know how to detect the brute-force activity.

4. Start Fail2Ban:

```bash
sudo service fail2ban restart
```

---

## 🔁 Testing & Validation

We repeated the same attack using Metasploit after the defense was installed. Fail2Ban detected the repeated failures and banned the attacker's IP after 3 attempts.

---

## Result

- Metasploit attempted brute-force using wordlists
- After 3 incorrect combinations, all further connection attempts were blocked
- The error returned:
  
```
Could not connect: The connection was refused by the remote host (172.28.128.3:22)
```
## 📊 Before-and-After Comparison


-table-

This confirms that the defense successfully mitigated the attack.

---

## 📸 Screenshot


---

This completes Phase 3.
