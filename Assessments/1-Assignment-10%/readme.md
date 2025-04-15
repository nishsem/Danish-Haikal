# Network Protocol Vulnerability Lab Walkthrough

## A. Objective

The goal of this lab is to explore the vulnerabilities of common network protocols (FTP, TELNET, SSH, HTTP) by performing brute force attacks to recover passwords and then using those credentials to sniff network traffic. You will also analyze the security of these protocols and propose mitigation strategies.

## B. Lab Tasks

### 1. Enumerate the Vulnerable VM to Discover Usernames
**Objective**: Identify potential usernames for brute force attacks.

**Steps**:
1. **Scan the Target VM**: We first performed an `nmap` scan to identify open ports for FTP, SSH, TELNET, and HTTP (ports 21, 22, 23, and 80).
   - Example `nmap` command:
```bash
     nmap -sV -p 21,22,23,80 192.168.X.X
```
![image](https://github.com/user-attachments/assets/ba7a1c88-d02e-4cb1-89cc-0327be8a80ab)

![Screenshot 2025-04-10 185403](https://github.com/user-attachments/assets/509e08b0-6fcf-4b6e-b6ca-29bda5de624e)

### 2. Perform Brute Force Attacks

#### 2.1 FTP, TELNET, and SSH

**Assumed Username**: Based on enumeration or hints from the VM, we assumed the username is `msfadmin`.

**Tool Used**: Hydra & NetExec 
**Wordlist Used**: `/usr/share/wordlists/rockyou.txt

**Commands Used**:

- **FTP**:
  ```bash
  hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt ftp://192.168.X.X
  ```
- **TELNET**:
  ```bash
  hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt telnet://192.168.X.X
  ```
- **SSH**:
  
