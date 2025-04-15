# Lab 1: Cryptographic Attacks – Brute Force & Traffic Analysis

**Time Allocated:** 3 Hours  
**Total Marks:** 15  
**Protocols:** FTP, TELNET, SSH, HTTP  
**Tools Used:** Hydra, Burp Suite, Wireshark

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

- **FTP Brute Force**

**Tool Used:** `Hydra`  
**Password List:** `password.txt`

**Commands:**

```bash
hydra -l msfadmin -P /usr/share/wordlists/password.txt ftp://192.168.43.137
```
 **Valid Credentials Found:**
 
![image](https://github.com/user-attachments/assets/67c69495-67fd-43a3-ab64-b01b7925da10)

- **TELNET Brute Force**

**Tool Used:** `Hydra`  
**Username:** `msfadmin`  
**Password List:** `password.txt`

**Commands:**

```bash
hydra -l msfadmin -P /usr/share/wordlists/password.txt telnet://192.168.43.137
```
 **Valid Credentials Found:**
 ![image](https://github.com/user-attachments/assets/66a3a2bb-a32f-4800-a680-4827bd10f93a)

- **SSH Brute Force**

**Tool Used:** `medusa`  
**Username:** `msfadmin`  
**Password List:** `password.txt`

**Commands:**

```bash
medusa -h 192.168.43.137 -u msfadmin -P /usr/share/wordlists/password.txt -M ssh
```
 **Valid Credentials Found:**
 
 ![image](https://github.com/user-attachments/assets/b1f433be-8c77-45f9-bc40-05d92186c266)

## 2.2 HTTP Brute Force – DVWA Module

**Tool Used:** `Burp Suite → Intruder`  
**Target URL:** `http://192.168.43.137/dvwa/vulnerabilities/brute/`  

---

### Steps:

1. **Access DVWA:**
   - Go to `http://192.168.43.137/`
   - Click on **DVWA**

![image](https://github.com/user-attachments/assets/a99c9620-af95-404b-a6ce-427a2160684c)

   - Login using the default credentials:
     - **Username:** `admin`
     - **Password:** `password`
    
![image](https://github.com/user-attachments/assets/19f24102-768e-4415-8c7d-c189121b4220)

   - Navigate to **DVWA Security** → Set **Security Level** to **Low**

![image](https://github.com/user-attachments/assets/953a1295-dab6-4920-ae5a-f94193eac92d)

2. **Navigate to Brute Force Page:**
   - Go to **Brute Force**

![image](https://github.com/user-attachments/assets/43c59070-1e50-46f4-9b4b-05decae6322e)

   - This is a page with a form to enter **username** and **password** to brute-force a login system.

3. **Intercept with Burp Suite:**
   - Set your browser to use Burp's proxy.

![image](https://github.com/user-attachments/assets/14422fb8-712b-4380-a4bd-3d4276193f59)

   - open burpsuite and turn on intercept option

![image](https://github.com/user-attachments/assets/3aeaf266-46f7-47ba-b2f9-00c536421afb)

   - Fill in sample credentials (e.g., `admin:1234`) and submit the form.
   - Burp will intercept the POST request.

![image](https://github.com/user-attachments/assets/468ec129-d198-48e8-9369-3f9a1ceb6e62)

4. **Send to Intruder:**
   - Right-click the intercepted request → **Send to Intruder**

![image](https://github.com/user-attachments/assets/eb905850-3271-4e17-a564-f5b683ab9c54)

   - Set the attack type to **Cluster Bomb**

![image](https://github.com/user-attachments/assets/9f3d8bba-3344-48d1-b655-77f4f7643970)

5. **Set Payload Positions:**
   - In the request:

![image](https://github.com/user-attachments/assets/84b76575-1742-4088-89b5-4c15dedf074e)

```bash
GET /dvwa/vulnerabilities/brute/?username=admin&password=123&Login=Login HTTP/1.1
```
     
   - Highlight the value of `username` and `password` to mark them as payload positions.


5. **Load Password Wordlist:**
   - Load username and password list in **Payload Options**

![image](https://github.com/user-attachments/assets/01899074-cc35-4957-bf7a-839068202f25)


![image](https://github.com/user-attachments/assets/da4c4e51-2ed5-4047-ba62-0094e19a0eb3)

6. **Launch Attack:**
   - Click **Start Attack**
   - Look through the results for a request with a noticeably **larger content length**.
   - This indicates a different (likely successful) response compared to the others.

![image](https://github.com/user-attachments/assets/4e197ec6-6935-4dce-b8c1-7ce8955382b6)

---

### Verifying the Successful Login:

- **Steps to Verify:**
  1. Right-click the highlighted response.
  2. Select **"Show response in browser"**.
  3. Copy the temporary URL provided by Burp.
  4. Paste the URL into your browser.

![image](https://github.com/user-attachments/assets/4b330a2f-2821-48d0-a296-d59b40e176b7)

![image](https://github.com/user-attachments/assets/f85005d9-de50-4c18-91f2-dc809597d9b2)

