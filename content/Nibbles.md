
If you have studied PEN200, HTB Academy or TryHackMe's pentest learning paths, Nibbles is a good first machine to attempt in **Lainkusanagi/TJnull** lists

If you are still working on basics, start with LAME (no longer in **Lainkusanagi/TJnull** lists as of 2026) or Legacy/Blue (very easy) and Shocker (Easy)

Nibbles is more challenging than Legacy, Blue and Shocker as it requires you to use more tools. 

Ping test for IP reachability check is important here. 
Good practice to do it before running NMAP. 
Nibbles has an **IP Blacklist** feature. If you run your `gobuster` or `nmap` too fast (too many threads), it might temporarily block your IP. 
You can do ping test to check if that happened. 

## Runbook & Steps

- **Step 1:** Nmap (Recon) - good practice to do ping test 
    
- **Step 2:** Web Discovery (Source code analysis)
    
- **Step 3:** Directory Brute-forcing (Gobuster)
    
- **Step 4:** Exploit (File Upload)
    
- **Step 5:** Stabilization (The Python PTY trick we discussed!)
    
- **Step 6:** PrivEsc (Sudo -l and Zip exploitation)





### Why Nibbles is a "Level 2" Machine

While **LAME** is about service vulnerabilities, **Nibbles** is about **Web Application** logic.

1. **Web Footprinting:** Unlike LAME, where the vulnerability is right in the Nmap scan, Nibbles requires you to look at the **Source Code** of the webpage to find a hidden directory (`/nibbleblog/`).
    
2. **Admin Bruteforce:** It often requires a very small amount of "password guessing" or using default credentials (`admin:nibbles`), which is a very realistic real-world scenario.
    
3. **Arbitrary File Upload:** You gain access by uploading a malicious PHP shell disguised as a plugin image. This is a "Gold Standard" pentesting skill.
    
4. **Privilege Escalation (The "Boss Fight"):** In LAME, you get root for free. In Nibbles, you land as a user and have to find a **zip file** that you are allowed to run as sudo. It teaches you to "deconstruct" how a user has set up their folder permissions.


┌──(lanc3㉿kali)-[~]
└─$ ping 10.10.10.75    
PING 10.10.10.75 (10.10.10.75) 56(84) bytes of data.
64 bytes from 10.10.10.75: icmp_seq=1 ttl=63 time=169 ms
64 bytes from 10.10.10.75: icmp_seq=2 ttl=63 time=168 ms
64 bytes from 10.10.10.75: icmp_seq=3 ttl=63 time=170 ms
^C
--- 10.10.10.75 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 167.995/168.942/169.944/0.796 ms
                                                                                                                                                         
┌──(lanc3㉿kali)-[~]
└─$ nmap -sCV -v -p- -T4 10.10.10.75 -oA nmap/nibbles
Starting Nmap 7.95 ( https://nmap.org ) at 2026-01-13 12:34 +08
...
Scanning 10.10.10.75 [65535 ports]
Discovered open port 80/tcp on 10.10.10.75
Discovered open port 22/tcp on 10.10.10.75
...
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c4:f8:ad:e8:f8:04:77:de:cf:15:0d:63:0a:18:7e:49 (RSA)
|   256 22:8f:b1:97:bf:0f:17:08:fc:7e:2c:8f:e9:77:3a:48 (ECDSA)
|_  256 e6:ac:27:a3:b5:a9:f1:12:3c:34:a5:5d:5b:eb:3d:e9 (ED25519)
**80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))**
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel



## Hack Smarter

for Apache httpd 2.4.18 ((Ubuntu)) identified in nmap scan, some beginners see an old Apache version and try to find a direct exploit 

While `searchsploit` will show several vulnerabilities for Apache 2.4.18 
(like **CVE-2016-5387 "httpoxy"** or various Denial of Service flaws), none of them lead to the **Remote Code Execution (RCE)** you need for this machine

The concept to grasp here: "**Apache is the Door, not the Key**"
- Apache is simply acting as the web server hosting the real target.
    
- **The Actual Target:** A blogging CMS called **Nibbleblog** (where the machine gets its name).
