https://portal.offsec.com/machine/bullybox-52203/overview/details

192.168.133.27


nmap -sCV -v -p- -T4 192.168.133.27 -oA nmap/bullybox
...
Discovered open port 80/tcp on 192.168.133.27
Discovered open port 22/tcp on 192.168.133.27
...
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 b9:bc:8f:01:3f:85:5d:f9:5c:d9:fb:b6:15:a0:1e:74 (ECDSA)
|_  256 53:d9:7f:3d:22:8a:fd:57:98:fe:6b:1a:4c:ac:79:67 (ED25519)

80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


------

sudo nano /etc/hosts 

add

192.168.133.27   bullybox.local




http://bullybox.local/robots.txt

```
# If the BoxBilling site is installed within a folder such as at
# e.g. www.example.com/boxbilling/ the robots.txt file MUST be
# moved to the site root at e.g. www.example.com/robots.txt
# AND the BoxBilling folder name MUST be prefixed to the disallowed
# path, e.g. the Disallow rule for the /bb-data/ folder
# MUST be changed to read Disallow: /boxbilling/bb-data/
#
# For more information about the robots.txt standard, see:
# http://www.robotstxt.org/orig.html
#
# For syntax checking, see:
# http://www.sxw.org.uk/computing/robots/check.html

User-agent: *
Disallow: /bb-data/
Disallow: /bb-library/
Disallow: /bb-locale/
Disallow: /bb-modules/
Disallow: /bb-uploads/
Disallow: /bb-vendor/
Disallow: /install/
```

searchsploit bullybox   
Exploits: No Results
Shellcodes: No Results
