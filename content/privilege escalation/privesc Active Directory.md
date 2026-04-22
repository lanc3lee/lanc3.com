
check --users

example:
nxc ldap < ip > -u -p --users

SeAssignPrimaryTokenPrivilege

**JuicyPotato** is the direct successor to the original RottenPotato and is designed specifically to exploit `SeAssignPrimaryTokenPrivilege`.



------

The only requirement to request a TGS (Service Ticket) for a Kerberoastable account is **to be a valid, authenticated Domain User.**

### Why no special permission is needed

In a Windows environment, the Kerberos protocol is designed so that any user can request to "talk" to any service. Think of it like a **phonebook**:

- **The SPN** is the phone number listed in the directory.
    
- **The Domain Controller** is the operator.
    
- **The Ticket** is the operator connecting your call.
    

The Active Directory (AD) default behavior is to grant a Service Ticket to any authenticated user who asks for a valid SPN. The actual _authorization_ (checking if Olivia is allowed to log into the SQL database, for example) happens **at the service itself** after she presents the ticket.

---

### Where `GenericWrite` and other rights come in

While you don't need special rights to _roast_ an existing SPN, permissions become relevant in these two scenarios:

#### 1. The "Targeted" Kerberoasting (Requires `GenericWrite` or `GenericAll`)

If you have `GenericWrite` over a user account that **doesn't** have an SPN, you can:

1. **Manually assign an SPN** to that user (e.g., `setspn -s shortcut/targetuser targetuser`).
    
2. **Request a ticket** for that new SPN.
    
3. **Roast it.**
    
4. **Clear the SPN** to hide your tracks. This allows you to take a "non-roastable" high-privileged user and make them vulnerable.
    

#### 2. The "DCSync" or "WriteDacl" Rights

Rights like `GenericAll` or `WriteDacl` are much more powerful than Kerberoasting. If you have those over an object, you don't need to crack a password offline; you can simply:

- Reset their password directly (`GenericAll`).
    
- Give yourself DCSync rights (`WriteDacl` on the Domain object).
    

---

- **Standard Kerberoasting:** Requires **zero** special AD permissions. Only requires a valid domain session and an existing SPN on a target account.
    
- **Targeted Kerberoasting:** Requires **`GenericWrite`**, **`GenericAll`**, or **`WriteProperty`** (specifically for the `servicePrincipalName` attribute) to turn a normal user into a roastable one.