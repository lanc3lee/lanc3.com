
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