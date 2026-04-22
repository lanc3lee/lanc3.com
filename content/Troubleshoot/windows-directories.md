
### Root Directory (`C:\`) is Protected

On modern Windows systems, the root of the `C:\` drive has very restrictive **Access Control Lists (ACLs)**.

- Standard users (and even some service accounts like `svc-printer`) can often **read** the root directory, but they are strictly forbidden from **writing** files there.
    
- When you tried to upload or run `nc64.exe` from `C:\`, the system likely blocked the file creation or the execution because the user lacked the `FILE_WRITE_DATA` or `FILE_ADD_FILE` permissions.
    

### 2. Why `C:\ProgramData` is the "Attacker's Playground"

`C:\ProgramData` is a hidden folder used by applications to store data that needs to be shared among all users.

- By default, Windows allows **Authenticated Users** to create folders and write files within `C:\ProgramData`.
    
- This is why most HTB/OSCP walkthroughs suggest using `C:\ProgramData`, `C:\Windows\Temp`, or your own user profile (`C:\Users\<Name>\AppData\Local\Temp`). These are the few places on a locked-down Windows box where a non-admin can actually land a file.
    

---

### 3. Understanding the "Success"

When you put the binary in `C:\ProgramData`, the service (running as **SYSTEM**) could finally "see" and "execute" the file. Even though the service has the power to look anywhere, your initial upload failed because **your current shell** (as `svc-printer`) didn't have the right to put it in `C:\` in the first place.