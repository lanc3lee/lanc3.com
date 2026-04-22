

smbclient -L //< target-ip > -N

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



smbclient //< target-ip >/backup -N

backup here is one of found directories




------


smbclient -L //192.168.x.45 -N     
Anonymous login successful

        Sharename       Type      Comment
        ---------       ----      -------
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 192.168.x.45 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available



**Anonymous login is successful**, but no workgroup listing. 

common when a machine is old (Windows 7/2008 R2) and expects legacy protocols or specific dialect negotiations.

Think of SMB enumeration as trying to answer three questions: 
**What shares exist?**, 
**Who is on the box?**, and 
**Is there a "known" way in?**

deep dive:

## 1. Refining Share Enumeration

The error you saw (`NT_STATUS_RESOURCE_NAME_NOT_FOUND`) often happens because `smbclient` is trying to list shares using an older protocol version that the client and server aren't agreeing on.

- **Try Impacket's `smbclient.py`:** 
  often more robust for CTFs. It handles dialect negotiation better than the standard Linux `smbclient`.
    
    Bash
    
    ```
    impacket-smbclient -no-pass //192.168.115.45
    ```
    
- **Try `smbmap`:** This tool is great because it clearly displays your **permissions** (READ/WRITE) for each share it finds.
    
    Bash
    
    ```
    smbmap -H 192.168.115.45 -u "" -p ""
    ```
    

## 2. The "Who" and "What" (RPC Enumeration)

Even if you can't see the files in a share yet, SMB is a gateway to **RPC (Remote Procedure Call)**. You can often "talk" to the system to get user lists, even without a password.

- **`enum4linux-ng`:** modern successor to `enum4linux`. It will try to pull user accounts, group memberships, and OS information.
    
    Bash
    
    ```
    enum4linux-ng -A 192.168.115.45
    ```
    
- **`rpcclient`:** If you want to be more manual, try connecting with a "null session" (no username/password).
    
    Bash
    
    ```
    rpcclient -U "" -N 192.168.115.45
    # Once inside, try commands like:
    # enumdomusers
    # querydominfo
    ```
    

## 3. Vulnerability Scanning



- **Nmap Scripts:** Nmap has built-in scripts to check for high-impact SMB vulnerabilities 
    
    Bash
    
    ```
    nmap -p 445 --script smb-vuln* 192.168.115.45
    ```
--------


example:

check using more specific vuln scripts, like this example in Internal

```
nmap -p 445 --script smb-vuln-ms17-010 192.168.141.40

Starting Nmap 7.95 ( https://nmap.org ) at 2026-02-06 13:15 +08

Nmap scan report for 192.168.141.40

Host is up (0.027s latency).

  

PORT    STATE SERVICE

445/tcp open  microsoft-ds

  

Host script results:

| smb-vuln-ms17-010: 

|   VULNERABLE:

|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)

|     State: VULNERABLE

|     IDs:  CVE:CVE-2017-0143

|     Risk factor: HIGH

|       A critical remote code execution vulnerability exists in Microsoft SMBv1

|        servers (ms17-010).

|           

|     Disclosure date: 2017-03-14

|     References:

|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143

|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/

|_      https://technet.microsoft.com/en-us/library/security/ms17-010.aspx

```



generic scan (`smb-vuln*`) may have struggled due to the sheer number of scripts running simultaneously, but by targeting `smb-vuln-ms17-010` specifically, Nmap was able to confirm the vulnerability accurately

-------

