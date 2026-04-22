

massive security blunder Microsoft made in the early days of Active Directory Group Policy Preferences (GPP).

### 1. The Static "Secret" Key

Unlike modern Windows passwords (which are salted and hashed), the `cpassword` in a GPP XML file is actually **encrypted**, not hashed.

Microsoft chose to use **AES-256** encryption to "protect" these passwords. However, for the Group Policy to work across an entire domain, every computer needs to be able to decrypt that password to use it. To make this happen, Microsoft published the **AES decryption key** on MSDN (their public developer website).

### 2. Encryption vs. Hashing

This is the fundamental difference:

- **Hashing (John/Hashcat):** A one-way function. You have to guess millions of passwords, hash them, and see if they match the target.
    
- **Encryption (gpp-decrypt):** A two-way function. If you have the key, you can simply "unlock" the original text.
    

Since the key is known and hardcoded into the `gpp-decrypt` tool, it doesn't need to "guess" anything. It just applies the key to the `cpassword` string and reveals the plaintext immediately.

### 3. Why John isn't used

You only use John the Ripper when you have a **Hash** (like an NTLM hash from a SAM database or a Kerberos ticket). Because `gpp-decrypt` is performing a direct decryption using a known key, it’s 100% successful and takes less than a second.

-----

context

cat groups.xml | grep -i "cpassword"

```

<Groups clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}"><User clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}" name="active.htb\SVC_TGS" image="2" changed="2018-07-18 20:46:06" uid="{EF57DA28-5F69-4530-A59E-AAB58578219D}"><Properties action="U" newName="" fullName="" description="" cpassword="edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ" changeLogon="0" noChange="1" neverExpires="1" acctDisabled="0" userName="active.htb\SVC_TGS"/></User>
```

userName="active.htb\SVC_TGS"

cpassword="edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ"

gpp-decrypt edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ

## GPPstillStandingStrong2k18