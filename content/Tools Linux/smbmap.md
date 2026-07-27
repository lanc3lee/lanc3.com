
```
smbmap -H flight.htb -u 'svc_apache' -p 'S@Ss!K@*t13'
```


![[smbmap-flight.png]]


```
smbclient -L //10.129.228.120 -U 'svc_apache%S@Ss!K@*t13'

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        Shared          Disk      
        SYSVOL          Disk      Logon server share 
        Users           Disk      
        Web             Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.228.120 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available

```


-----

smbmap not working as expected, NXC shows different privileges


```
smbmap -H flight.htb -u 'S.Moon' -p 'S@Ss!K@*t13'    

SMBMap - Samba Share Enumerator v1.10.7 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authenticated session(s)                                                          

[+] IP: 10.129.228.120:445      Name: flight.htb                Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                READ ONLY       Logon server share 
        Shared                                                  READ ONLY
        SYSVOL                                                  READ ONLY       Logon server share 
        Users                                                   READ ONLY
        Web                                                     READ ONLY
[*] Closed 1 connections                                                                                                     

┌──(lanc3㉿kali)-[~]
└─$ nxc smb flight.htb -u S.Moon -p 'S@Ss!K@*t13' --shares
SMB         10.129.228.120  445    G0               [*] Windows 10 / Server 2019 Build 17763 x64 (name:G0) (domain:flight.htb) (signing:True) (SMBv1:False) 
SMB         10.129.228.120  445    G0               [+] flight.htb\S.Moon:S@Ss!K@*t13 
SMB         10.129.228.120  445    G0               [*] Enumerated shares
SMB         10.129.228.120  445    G0               Share           Permissions     Remark
SMB         10.129.228.120  445    G0               -----           -----------     ------
SMB         10.129.228.120  445    G0               ADMIN$                          Remote Admin
SMB         10.129.228.120  445    G0               C$                              Default share
SMB         10.129.228.120  445    G0               IPC$            READ            Remote IPC
SMB         10.129.228.120  445    G0               NETLOGON        READ            Logon server share 
SMB         10.129.228.120  445    G0               Shared          READ,WRITE      
SMB         10.129.228.120  445    G0               SYSVOL          READ            Logon server share 
SMB         10.129.228.120  445    G0               Users           READ            
SMB         10.129.228.120  445    G0               Web             READ   
```

difference comes down to **how each tool checks write permissions**.
![[smbmap-vs-nxc.png]]
### Why the Results Differ

#### smbmap's approach

smbmap checks permissions by attempting to **list the directory** (`NetShareGetInfo` or directory listing). It determines READ/WRITE based on what the server _reports_ about the share's ACL, but it doesn't always attempt an actual write operation to verify.

#### nxc (NetExec)'s approach

nxc actively **attempts to write a file** to the share to confirm WRITE access. It tries to create a temporary file, and if that succeeds, it marks the share as WRITE. This is a more reliable/accurate method.

### In Your Case

For the `Shared` share:

- **smbmap** says `READ ONLY` — it checked the ACL or did a listing and inferred read-only
- **nxc** says `READ,WRITE` — it actually tried to write and **succeeded**

This means **nxc is correct** — you likely have write access to `Shared`.

### Why smbmap Gets It Wrong Sometimes

1. **Version differences** — older smbmap versions had less aggressive write-checking
2. **ACL vs actual access** — reported ACLs don't always match effective permissions (e.g., due to nested group policies)
3. **Write-check method** — smbmap may use `SMB2_FILE_WRITE_DATA` flag checks rather than a real write attempt![[smbmap-vs-nxc.png]]