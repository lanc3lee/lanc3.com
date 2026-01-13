--- 
title: LAME
--- 


LAME was the first OSCP-like box (in **Lainkusanagi/TJnull** lists) for beginners
- while it is not included in 2026's lists to make room for newer challenges, it's still worth practising
- first machine ever added to **Hack The Box**. 
  By completing it, you are participating in a "rite of passage" 
- Most walkthroughs/tutorials use LAME as a baseline, meaning if you get stuck, there is a wealth of high-quality educational material to learn _why_ a certain step is taken.
- **Metasploit vs. Manual Exploit:** It is a great machine to try the Metasploit version of an attack first, and then try the manual Python/Bash version to understand how it actually works.
  
### Teaches "Low Hanging Fruit" Principle
  - LAME is famous for having multiple entry points, learn to prioritize easier ones and when to skip to next better target
  - teaches beginners to check service versions against known exploits 
    (`searchsploit` or Google) before trying to find manual "zero-day" bugs


If you have practised with learning paths in PEN200, TryHackMe, HTB Academy etc, you could attempt this first box in **Lainkusanagi/TJnull** lists as a black box. 

Pick Adventure Mode instead of Guided Mode
![LAME Adventure Mode|300](assets/adventure-mode-hack-the-box.png)

(i.e. don't read the tasks and hints as they are meant to guide beginners without experience)


## Runbook
1) nmap or rustscan to enumerate ports and services
2) prioritize vulnerable services to check, identify low hanging fruits


**Warning: Spoilers ahead. **

Attempt LAME with a black-box approach first and read further only if you get stuck or pwned it. 



## Overview

nmap scan reveals open ports 21 (ftp), 22 (ssh), 139 & 445 (Samba) and 3632 (distcc)

service OpenSSH 4.7p1 (Debian), it has no known RCE

ftp vsftpd 2.3.4 seems to be the lowest hanging fruit but it's really a rabbit hole to teach us not to blindly follow version-based exploits. 

 Samba smbd 3.0.20-Debian runs at port 139 & 445, which is vulnerable to exploit CVE-2007-2447
 
 As this Samba service runs as root to handle file permissions and the **`username map script`** is triggered during the initial authentication phase, the resulting shell is **immediately root**.

Most learners will stop this challenge after getting root access, eager to attempt more challenging boxes in **Lainsunagi/TJnull** lists.
You can deep dive further (see below)

## Hack Smarter
- ftp vsftpd 2.3.4 presents itself as the easiest attack vector with exploit CVE-2011-2523 ('Smiley Face' backdoor), but it's a rabbit hole as the backdoor is simply not present. 
  LAME predates the backdoor's release, and local firewall rules typically drop traffic on non-standard ports like 6200.  
  This highlights the importance of **Contextual Analysis** over blindly following version-based exploits
  
  Nonetheless, in most CTFs, if you see vsftpd 2.3.4, it’s an instant win. 
  Try triggering it anyway, and quickly move on to the more promising Samba vulnerability


## Dive Deeper
- study why exploit CVE-2011-2523 did not work for ftp vsftpd 2.3.4 
- opportunity to learn how to gain a full interactive shell
- **port 3632** is running **distcc** - LAME's "hidden gem" because beginners focus on the more famous Samba and FTP vulnerabilities.
  Certainly a "unnecessarily" difficult route as you would need to privilege escalate as daemon user (after using distcc exploit) to gain access as root user

## Details
A ping test for simple reachability check before running nmap
ping  <IP-of-target>

nmap -sCV -v -p- -T4 <IP-of-target>

instead of above one-liner, you could use this two-liner
or rustscan (understand the flags used)

┌──(lanc3㉿kali)-[~]
└─$ ping 10.10.10.3                              
PING 10.10.10.3 (10.10.10.3) 56(84) bytes of data.
64 bytes from 10.10.10.3: icmp_seq=1 ttl=63 time=168 ms
64 bytes from 10.10.10.3: icmp_seq=2 ttl=63 time=180 ms
64 bytes from 10.10.10.3: icmp_seq=3 ttl=63 time=169 ms
^C
--- 10.10.10.3 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 168.445/172.466/180.336/5.565 ms



┌──(lanc3㉿kali)-[~]
└─$ **<font size="5">nmap -sCV -v -p- -T4 10.10.10.3 -oA nmap/LAME</font>**
<span style="color: #888888;">Starting Nmap 7.95 ( https://nmap.org ) at 2026-01-13 08:46 +08
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 08:46
Completed NSE at 08:46, 0.00s elapsed
Initiating NSE at 08:46
Completed NSE at 08:46, 0.00s elapsed
Initiating NSE at 08:46
Completed NSE at 08:46, 0.00s elapsed
Initiating Ping Scan at 08:46
Scanning 10.10.10.3 [4 ports]
Completed Ping Scan at 08:46, 0.23s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 08:46
Completed Parallel DNS resolution of 1 host. at 08:46, 0.00s elapsed
Initiating SYN Stealth Scan at 08:46</span>
Scanning 10.10.10.3 [65535 ports]
Discovered open port 21/tcp on 10.10.10.3
Discovered open port 139/tcp on 10.10.10.3
Discovered open port 445/tcp on 10.10.10.3
Discovered open port 22/tcp on 10.10.10.3
Discovered open port 3632/tcp on 10.10.10.3
<span style="color: #888888;">SYN Stealth Scan Timing: About 29.59% done; ETC: 08:50 (0:02:25 remaining)
SYN Stealth Scan Timing: About 48.79% done; ETC: 08:49 (0:01:36 remaining)
SYN Stealth Scan Timing: About 70.73% done; ETC: 08:49 (0:00:50 remaining)
Completed SYN Stealth Scan at 08:49, 165.64s elapsed (65535 total ports)
Initiating Service scan at 08:49
Scanning 5 services on 10.10.10.3
Completed Service scan at 08:49, 11.54s elapsed (5 services on 1 host)
NSE: Script scanning 10.10.10.3.
Initiating NSE at 08:49
NSE: [ftp-bounce] PORT response: 500 Illegal PORT command.
Completed NSE at 08:50, 40.08s elapsed
Initiating NSE at 08:50
Completed NSE at 08:50, 1.19s elapsed
Initiating NSE at 08:50
Completed NSE at 08:50, 0.00s elapsed
Nmap scan report for 10.10.10.3
Host is up (0.17s latency).
Not shown: 65530 filtered tcp ports (no-response)
PORT     STATE SERVICE     VERSION</span>
**21/tcp   open  ftp         vsftpd 2.3.4**
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.14.43
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
**22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)**
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
**139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
3632/tcp open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel**

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name: 
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2026-01-12T19:51:07-05:00
|_smb2-time: Protocol negotiation failed (SMB2)
|_clock-skew: mean: 2h31m16s, deviation: 3h32m10s, median: 1m14s

