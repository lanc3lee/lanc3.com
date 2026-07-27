
```
nmap -sCV -v -p- -T4 10.129.234.73 -oA nmap/job
...
Scanning 10.129.234.73 [65535 ports]
Discovered open port 445/tcp on 10.129.234.73
Discovered open port 80/tcp on 10.129.234.73
Discovered open port 25/tcp on 10.129.234.73
Discovered open port 3389/tcp on 10.129.234.73
...
PORT     STATE SERVICE       VERSION
25/tcp   open  smtp          hMailServer smtpd
| smtp-commands: JOB, SIZE 20480000, AUTH LOGIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-favicon: Unknown favicon MD5: 556F31ACD686989B1AFCF382C05846AA
|_http-title: Job.local
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=job
| Issuer: commonName=job
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-07-07T13:50:33
| Not valid after:  2027-01-06T13:50:33
| MD5:   7ea5:097d:f620:da7d:82eb:0499:c6cc:9f92
|_SHA-1: be61:831f:43a2:1ce1:d292:db9a:8115:f66f:2e84:8528
|_ssl-date: 2026-07-08T13:58:17+00:00; -7s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: JOB
|   NetBIOS_Domain_Name: JOB
|   NetBIOS_Computer_Name: JOB
|   DNS_Domain_Name: job
|   DNS_Computer_Name: job
|   Product_Version: 10.0.20348
|_  System_Time: 2026-07-08T13:57:37+00:00
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: JOB; OS: Windows; CPE: cpe:/o:microsoft:windows

```

```
gobuster dir -u http://10.129.234.73 -w /usr/share/wordlists/dirb/common.txt -x php,html,txt,xml,zip,config
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.234.73
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8
[+] Extensions:              html,txt,xml,zip,config,php
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/aspnet_client        (Status: 301) [Size: 158] [--> http://10.129.234.73/aspnet_client/]
/assets               (Status: 301) [Size: 151] [--> http://10.129.234.73/assets/]
/css                  (Status: 301) [Size: 148] [--> http://10.129.234.73/css/]
/Index.html           (Status: 200) [Size: 3261]
/index.html           (Status: 200) [Size: 3261]
/index.html           (Status: 200) [Size: 3261]
/js                   (Status: 301) [Size: 147] [--> http://10.129.234.73/js/]
Progress: 32291 / 32291 (100.00%)
===============================================================
Finished
===============================================================
```

```
└─$ smbclient -L //10.129.234.73 -N                                           
session setup failed: NT_STATUS_ACCESS_DENIED
                                                                                                                                                    
┌──(lanc3㉿kali)-[~]
└─$ smbclient //10.129.234.73 -N 


┌──(lanc3㉿kali)-[~]
└─$ impacket-smbclient -no-pass //10.129.234.73 
Impacket v0.13.0.dev0+20251002.85540.fc92f471 - Copyright Fortra, LLC and its affiliated companies 

[-] [Errno Connection error (//10.129.234.73:445)] [Errno -2] Name or service not known

```

------

hMailServer

## CVE-2025-52372

https://nvd.nist.gov/vuln/detail/CVE-2025-52372#:~:text=An%20issue%20in%20hMailServer%20v.5.8.6,the%20hmailserver%2Finstallation%2FhMailServerInnoExtension.iss%20and%20hMailServer.ini%20components.

https://github.com/mojibake-dev/mojibake-CVE/blob/main/hMailServer/CVE-2025-52372.md

-------

https://www.puckiestyle.nl/vulnlab-job/

From Phishing to admin![Enrique A.](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)

Tools used : sendmail , msfconsole , msfvenom , godpotato
nmap scan

-----

relia also involves phishing

![[relia-smtp-port25.png]]