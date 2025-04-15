# Lab 1: Cryptographic Attacks – Brute Force & Traffic Analysis

**Time Allocated:** 3 Hours  
**Total Marks:** 15  
**Protocols:** FTP, TELNET, SSH, HTTP  
**Tools Used:** Hydra, Burp Suite, Wireshark, tcpdump, Medusa, NetExec

---

## A. Objective

To explore vulnerabilities in common network protocols (FTP, TELNET, SSH, HTTP) by:

- Performing brute force attacks to recover passwords.
- Sniffing traffic using recovered credentials.
- Analyzing security of plaintext vs encrypted protocols.
- Proposing effective mitigation strategies.

---

## B. Lab Tasks

---

### 1. Username Enumeration

**Goal:** Identify valid usernames to perform brute force attacks.

**Tools:** `nmap` & `enum4linux`

**Commands:**
```bash
nmap -sV -p 21,23,22,80 <target-ip>
```

![image](https://github.com/user-attachments/assets/6d091b5a-a442-4d59-abbb-2ccec9e510b0)

```bash
enum4linux -a <target-ip>
```
**Username Found:**

![image](https://github.com/user-attachments/assets/43634cc6-0f64-4e6c-9266-0fb8b6bc5441)

**Result:**  
The output from `enum4linux` revealed the following username:

- `msfadmin`

This username is commonly found on Metasploitable2 and was confirmed through SMB enumeration using `enum4linux`.

---

## 2. Brute Force Attacks

---

### 2.1 FTP, TELNET, SSH

**Tool Used:** `Hydra`  
**Password List:** `password.txt`

**Commands:**

- **FTP Brute Force**
```bash
hydra -l msfadmin -P /usr/share/wordlists/password.txt ftp://192.168.43.137
```
 **Valid Credentials Found:**
![image](https://github.com/user-attachments/assets/411949ea-00e7-4afa-afa9-306550ba8580)

