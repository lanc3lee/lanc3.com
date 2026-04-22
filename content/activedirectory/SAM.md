
SAM database lives on every Windows machine and contains the **NTLM hashes** for **local accounts** (like `Administrator`, `Guest`, or local tech support accounts).

- **Credential Reuse:** Many organizations use the same local admin password across all workstations. If you dump the SAM on the Wreath Web Server and get the local admin hash, there is a high chance that same hash works on the Git Server.
    
- **Pass-the-Hash (PtH):** You don't even need to crack the password. You can take the NTLM hash you dumped and use it to log into other machines directly.
