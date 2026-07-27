
`impacket-psexec` is useful for turning administrative credentials into an interactive **SYSTEM** shell in Active Directory pentesting. 

---

## 1. What is it?

Based on the original Sysinternals `psexec`, this Python implementation allows you to execute processes on remote Windows systems.

**The Key Difference:** Unlike other Impacket tools (like `wmiexec`), `psexec` uploads a small executable to the remote `ADMIN$` share, registers it as a service, and runs it. This results in an interactive shell with **SYSTEM** privileges—the highest level of access on a Windows host.

---

## 2. Core Syntax

The syntax follows the standard Impacket format. You must target the `ADMIN$` share, so administrative rights are required.

### With a Password

`impacket-psexec [[domain/]username[:password]@]<targetAddress>`

### With a Hash (Pass-the-Hash)

Common scenario where you’ve dumped a local admin's NTLM hash:

`impacket-psexec Administrator@10.10.10.100 -hashes :aad3b435b51404eeaad3b435b51404ee:5fbc3013289561a90f11417088927022`

_(Note: If you only have the NT hash, the LM portion `aad3...` can be left blank or filled with zeros)._

---

## 3. Why use `psexec` over other tools?

You will likely choose between `psexec`, `wmiexec`, and `smbexec`. Here is why `psexec` is unique:

- **SYSTEM Privileges:** It automatically attempts to escalate the service to `NT AUTHORITY\SYSTEM`.
    
- **True Interactive Shell:** It feels more like a real shell than `wmiexec` (which is semi-interactive).
    
- **Reliability:** It is often the "heavy hitter" that works when others fail, provided the user is a local admin.
    

---

## 4. The "Double-Edged Sword" 

Be aware of how `psexec` works for your documentation and lab practice:

1. **It is Noisy:** It uploads an `.exe` file to the target (usually with a random name like `BTOqXz.exe`). Modern Antivirus (AV) and EDR often flag this immediately.

2. **Service Creation:** It creates and then deletes a Windows service. This leaves logs (Event ID 7045).

3. **Requirements:** It **requires** the `ADMIN$` share to be writeable and the "File and Printer Sharing" service to be enabled.


---

## 5. Troubleshooting 

If `psexec` fails during a lab, check these three things:

- **Is the user a Local Admin?** If they aren't, you won't have access to `ADMIN$`.
    
- **Is it blocked by AV?** If you see "Access Denied" despite having correct admin credentials, the target's AV might be deleting the uploaded executable. In this case, try **`impacket-wmiexec`**, which is much stealthier as it doesn't upload a file.
    
- **Firewall:** `psexec` requires SMB port **445** to be open.
    

---

## 6. Comparison Table

|**Tool**|**Stealth Level**|**Privilege Level**|**Method**|
|---|---|---|---|
|**psexec**|Low (Noisy)|**SYSTEM**|Uploads .exe & creates Service|
|**wmiexec**|High (Quiet)|**User/Admin**|Uses WMI (no file upload)|
|**smbexec**|Medium|**SYSTEM**|Creates service (no file upload)|

### Pro-Tip

If you get a shell and it's unstable, use `psexec` to upload a dedicated reverse shell (like a staged `msfvenom` payload or `nc.exe`) to a writable directory and execute it to get a more stable connection back to your listener.