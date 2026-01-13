--- 
title: LAME Walkthrough 
--- 
# LAME

LAME is the first OSCP-like box (in **Lainsunagi/TJnull** lists) for beginners
- first machine ever added to **Hack The Box**. 
  By completing it, you are participating in a "rite of passage" 
- Most walkthroughs/tutorials use LAME as a baseline, meaning if you get stuck, there is a wealth of high-quality educational material to learn _why_ a certain step is taken.
- **Metasploit vs. Manual Exploit:** It is a great machine to try the Metasploit version of an attack first, and then try the manual Python/Bash version to understand how it actually works.
  
### Teaches "Low Hanging Fruit" Principle
  - LAME is famous for having multiple entry points
  - teaches beginners to check service versions against known exploits 
    (`searchsploit` or Google) before trying to find manual "zero-day" bugs

 a ping test for simple reachability check before running nmap

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

