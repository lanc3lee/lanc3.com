

smbclient -L //<target-ip> -N

### 1. The `-L` Flag (List)

- **Purpose:** This tells the tool to **List** the available shares on the target host.
    
- **What it shows:** It will display share names (like `ADMIN$`, `C$`, or in your case, `backup`), the type of share (Disk, IPC, etc.), and any comments attached to them.
    
- **Why use it:** Before you can access files, you need to know the name of the "folder" (share) the server is making available.
    

### 2. The `-N` Flag (No Password)

- **Purpose:** This tells the tool to use a **Null Session** (no password).
    
- **How it works:** It instructs `smbclient` not to prompt you for a password and to attempt the connection with an anonymous/guest identity.
    
- **Why use it:** In security testing (like OSCP), your first step is always to see if the server is "misconfigured" to allow anyone to see the share list without credentials.
  
  
  if it worked, we know the server is vulnerable to **Anonymous SID Enrollment/Guest Access**
  
  
  
  ---------
  
  without -L , connect directly



smbclient //<target-ip>/backup -N

backup here is one of found directories
  