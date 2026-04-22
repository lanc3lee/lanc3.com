`GetNPUsers.py` (AS-REP Roasting)
secretsdump.py
smbclient.py
`lookupsid.py` (User Enumeration)

-------
above are tools that handle the "Before" (Enumeration) and "After" (Lateral
RCE methods

`psexec` 
`wmiexec`
**`smbexec.py`**

|**Tool**|**How it works**|**Stealth/Detection**|
|---|---|---|
|**psexec**|Uploads an `.exe`, creates a Service.|**High Alert.** Very noisy; creates files.|
|**wmiexec**|Uses WMI (Port 135). No files uploaded.|**Medium.** Stealthier; semi-interactive.|
|**smbexec**|Creates a temporary service, no `.exe` uploaded.|**Low.** Often bypasses simple AV, but slower.|


---

### 1. `GetNPUsers.py` (AS-REP Roasting)

"cousin" of Kerberoasting. It targets users who have the setting **"Do not require Kerberos preauthentication"** enabled.

- **When to use:** Early in the game, often before having any credentials.

- **Why it's great:** get a crackable hash for a user without ever sending a single password attempt.
 
- **Command:** `GetNPUsers.py active.htb/ -usersfile users.txt -format hashcat -dc-ip 10.129.13.x


### 2. `secretsdump.py` 

Once we have Administrator or SYSTEM access on a machine, this tool dumps all the local hashes (SAM), the LSA secrets, and even the **NTDS.dit** (the entire domain database) from a Domain Controller.

- **When to use:** After getting your first Administrative shell.
 
- **Command:** `secretsdump.py active.htb/Administrator:Ticketmaster1968@10.129.13.x`
    

### 3. `smbclient.py` (The "Stable" smbclient)

Impacket’s version is much more "script-friendly" and often handles complex AD authentication (like Kerberos tokens) better than the native OS tool.

- **When to use:** When need to upload a reverse shell or download files from a restricted share.

- **Command:** `smbclient.py active.htb/Administrator:Ticketmaster1968@10.129.13.84`


### 4. `lookupsid.py` (User Enumeration)

If we find a box that allows "Null Sessions" but won't let us list users via LDAP, this tool uses **SID Cycling** to brute-force usernames.

- **When to use:** When we have no credentials but SMB is open.
 
- **Command:** `lookupsid.py active.htb/guest@10.129.13.x`