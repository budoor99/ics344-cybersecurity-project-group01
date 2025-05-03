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
- The IP will be banned for 10 minutes (bantime = 600).


**configure_jail.local.png** is provided to prove applying the defense 


3. Ensure SSH Filter is Present:

```bash
/etc/fail2ban/filter.d/sshd.conf
```
Why this matters:

This filter contains the regex patterns Fail2Ban uses to identify failed login attempts in /var/log/auth.log.
Without it, Fail2Ban won’t know how to detect the brute-force activity.

4. Check Fail2Ban Status Before Starting:

Before applying the defense, you can verify that no jail is active:

```bash
sudo fail2ban-client status
```


5. Start Fail2Ban:

```bash
sudo service fail2ban restart
```
The screenshot **Banned_List_Before.png** confirms that no IP addresses are currently banned and that Fail2Ban is actively monitoring SSH.

---

## 🔁 Testing & Validation

- We reused the same Metasploit attack from Phase 1:

```bash
set RHOSTS 172.28.128.3
set USER_FILE /home/kali/users.txt
set PASS_FILE /home/kali/pass.txt
set BRUTEFORCE_SPEED 5
set THREADS 4
set VERBOSE true
set STOP_ON_SUCCESS true
run
```

We repeated the same attack using Metasploit after the defense was installed. Fail2Ban detected the repeated failures as shown in **Attack_failure.png** and banned the attacker's IP after 3 attempts.

---

## Result

- Metasploit attempted brute-force using wordlists
- After 3 incorrect combinations, all further connection attempts were blocked
- The error returned:
  
```
Could not connect: The connection was refused by the remote host (172.28.128.3:22)
```

As in **Banned_List_After.png**, initially, no IPs were banned (highlighted in yellow), but after rerunning the brute-force attack, Fail2Ban detected multiple failed login attempts and successfully banned the attacking IP 172.28.128.10 (highlighted in red).

### 🔄 Before-and-After Comparison

| **Criteria**                    | **Before Fail2Ban (Phase 1)**                          | **After Fail2Ban (Phase 3)**                          |
|---------------------------------|--------------------------------------------------------|--------------------------------------------------------|
| SSH brute-force attempts        | Allowed unlimited attempts                            | Limited to 3 attempts                                  |
| Successful brute-force login    | ✅ Yes (`vagrant:vagrant`) using Metasploit            | ❌ Not possible – connection refused after 3 tries     |
| System response to attack       | No blocking, attacker could retry endlessly           | IP blocked for 10 minutes after failed attempts        |
| Visibility of attack in logs    | Manual inspection required                            | Detected automatically by Fail2Ban                    |

This confirms that the defense successfully mitigated the attack.

---

## 📸 Screenshot
- **configure_jail.local.png**: This screenshot shows the jail.local file on Metasploitable3 where Fail2Ban is configured to protect the SSH service.
  
- **Attack_Failure.png**: Captures the brute-force attack attempt using Metasploit. After three failed login combinations, further access was denied and connection attempts were refused.
  
- **Banned_List_Before.png**: Shows the initial state of Fail2Ban before applying the defense.
  
- **Banned_List_After.png**: Displays the updated Fail2Ban status after the attack. The attacker's IP (172.28.128.10) is now listed as banned, proving that the tool actively blocked the intrusion after detecting repeated failures.





---

This completes Phase 3.
