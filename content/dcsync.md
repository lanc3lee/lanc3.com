To perform a **DCSync** attack, a user doesn't actually need to be a Domain Administrator. Instead, they need specific **Control Access Rights** delegated to them on the **Domain Object** itself (the very top of the Active Directory tree).

Mimikatz’s DCSync feature works by pretending to be a Domain Controller (DC) and asking a legitimate DC to replicate user passwords using the Directory Replication Service (DRS) Remote Protocol.

### The Required Privileges

You must have the following three permissions on the **Domain** object:

1. **DS-Replication-Get-Changes** (Commonly seen as "Replicating Directory Changes")
    
2. **DS-Replication-Get-Changes-All** (Commonly seen as "Replicating Directory Changes All")
    
3. **DS-Replication-Get-Changes-In-Filtered-Set** (Required only in specific environments with Read-Only Domain Controllers, but usually included in the set).
    

---

### Why these privileges?

- **Replicating Directory Changes:** This allows the user to request replication updates for a naming context.
    
- **Replicating Directory Changes All:** This is the critical "secret sauce." It allows the user to replicate **secret data**, such as the `unicodePwd` (the NTLM hash) and the `supplementalCredentials` (like clear-text passwords if reversible encryption is enabled).
    

### How to check for this in BloodHound

In **BloodHound Community Edition**, you don't have to manually check every ACL.

1. Search for the **Domain Object** (e.g., `ADMINISTRATOR.HTB`).
    
2. Go to the **Node Info** tab.
    
3. Click on **"Inbound Object Control"**.
    
4. Look for the section labeled **"DCSyncers"**.