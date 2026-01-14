



┌──(lanc3㉿kali)-[~]
└─$ **rustscan -a 10.10.11.10 --ulimit 5000 -- -sCV -oN nmap/builder**
...
[~] Automatically increasing ulimit value to 5000.
Open 10.10.11.10:22
Open 10.10.11.10:8080
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sCV -oN nmap/builder" on ip 10.10.11.10
...
Discovered open port 8080/tcp on 10.10.11.10
Discovered open port 22/tcp on 10.10.11.10
...
PORT     STATE SERVICE REASON         VERSION
**22/tcp**   open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ+m7rYl1vRtnm789pH3IRhxI4CNCANVj+N5kovboNzcw9vHsBwvPX3KYA3cxGbKiA0VqbKRpOHnpsMuHEXEVJc=
|   256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOtuEdoYxTohG80Bo6YCqSzUY9+qbnAFnhsk4yAZNqhM
**8080/tcp open  http    syn-ack ttl 62 Jetty 10.0.18
|_http-title: Dashboard [Jenkins]
| http-robots.txt: 1 disallowed entry 
|_/
|_http-favicon: Unknown favicon MD5: 23E8C7BD78E8CD826C5A6073B15068B1
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Jetty(10.0.18)
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel**

...
![[jenkins-2443.png]]

![[jenkins-login-page.png]]

![[jenkins-credentials.png]]

![[jenkins-2443-exploit-poc.png]]