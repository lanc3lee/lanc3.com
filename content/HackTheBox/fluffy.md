
https://app.hackthebox.com/machines/Fluffy

As is common in real life Windows pentests, you will start the Fluffy box with credentials for the following account: 
j.fleischman / J0elTHEM4n1990!

```
nmap -sCV -v -p- -T4 10.129.232.88 -oA nmap/fluffy
...111
Not shown: 65517 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-07-13 20:46:41Z)
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: fluffy.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2026-07-13T20:48:10+00:00; +6h59m49s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.fluffy.htb, DNS:fluffy.htb, DNS:FLUFFY
| Issuer: commonName=fluffy-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-30T16:09:59
| Not valid after:  2106-04-30T16:09:59
| MD5:   f5e3:ec00:5fd1:2a95:a76b:2fd6:4726:4d67
|_SHA-1: 6867:9230:5123:dcf1:9352:e081:4148:7fef:13c7:6c0a
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: fluffy.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2026-07-13T20:48:10+00:00; +6h59m49s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.fluffy.htb, DNS:fluffy.htb, DNS:FLUFFY
| Issuer: commonName=fluffy-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-30T16:09:59
| Not valid after:  2106-04-30T16:09:59
| MD5:   f5e3:ec00:5fd1:2a95:a76b:2fd6:4726:4d67
|_SHA-1: 6867:9230:5123:dcf1:9352:e081:4148:7fef:13c7:6c0a
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: fluffy.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2026-07-13T20:48:10+00:00; +6h59m49s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.fluffy.htb, DNS:fluffy.htb, DNS:FLUFFY
| Issuer: commonName=fluffy-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-30T16:09:59
| Not valid after:  2106-04-30T16:09:59
| MD5:   f5e3:ec00:5fd1:2a95:a76b:2fd6:4726:4d67
|_SHA-1: 6867:9230:5123:dcf1:9352:e081:4148:7fef:13c7:6c0a
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: fluffy.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2026-07-13T20:48:10+00:00; +6h59m49s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.fluffy.htb, DNS:fluffy.htb, DNS:FLUFFY
| Issuer: commonName=fluffy-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-04-30T16:09:59
| Not valid after:  2106-04-30T16:09:59
| MD5:   f5e3:ec00:5fd1:2a95:a76b:2fd6:4726:4d67
|_SHA-1: 6867:9230:5123:dcf1:9352:e081:4148:7fef:13c7:6c0a
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49691/tcp open  msrpc         Microsoft Windows RPC
49692/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49700/tcp open  msrpc         Microsoft Windows RPC
49713/tcp open  msrpc         Microsoft Windows RPC
49726/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

```

nxc ldap 10.129.232.88 -u 'j.fleischman' -p 'J0elTHEM4n1990!' --users

![[fluffy-nxc-ldap-01.png]]![[fluffy-nxc-ldap-02.png]]

----

what does this tell us?

```
nxc ldap 10.129.232.88 -u 'j.fleischman' -p 'J0elTHEM4n1990!' --trusted-for-delegation 

LDAP        10.129.232.88   389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:fluffy.htb)

LDAP        10.129.232.88   389    DC01             [+] fluffy.htb\j.fleischman:J0elTHEM4n1990! 

LDAP        10.129.232.88   389    DC01             DC01$
```
This output tells us that **`DC01$` (the Domain Controller's computer account) is configured with "Unconstrained Delegation"** in the `fluffy.htb` domain.

When you run `nxc` with the `--trusted-for-delegation` flag, it queries the `userAccountControl` attribute of objects in Active Directory. Specifically, it is looking for the `ADS_UF_TRUSTED_FOR_DELEGATION` flag.


1. Since the Domain Controller itself is trusted for unconstrained delegation, if you can coerce an account with higher privileges (like a Domain Admin) to authenticate to this DC for a service (like SMB, LDAP, or RPC), that user's TGT will be cached in the DC's memory.
    
2. **High-Risk Exposure:** Because this is the Domain Controller, _every_ service on this machine is inherently high-value. If you compromise this host, you effectively have the ability to intercept the credentials of any domain user who happens to connect to it.
    

 finding unconstrained delegation on a Domain Controller is an "instant win" condition if you can trigger the right authentication.

- **The Workflow:** You don't need to "hack" the DC yet. You need to "trick" a privileged user into connecting to it.
    
- **Common Techniques:** * **Printer Bug / PetitPotam:** If the DC has the Print Spooler service enabled, you can force it to connect to an attacker-controlled machine (or another machine you control) using `dcom` or other RPC methods.
    
    - **Spooler Service:** Even if the DC is the one being delegated to, you are looking to capture the TGT of an admin who connects to services on the DC.
        

### Important Distinction

Note that `nxc` returned `DC01$`. This confirms it is the **Computer Account** that is trusted. This is common in some lab environments or misconfigured enterprise environments, but it is extremely dangerous.

next steps:

1. **Map the services:** Check what services are running on `DC01` (use `nxc smb 10.129.232.88 -u ... -p ... --shares` or `--sessions`).
    
2. **Check for Coercion:** Verify if the `MS-RPRN` (Print Spooler) interface is available, as that is the most common way to trigger the delegation capture.
    
3. **Document the Finding:** In report, this is a "High/Critical" finding. Identified a configuration that allows for full domain compromise via TGT harvesting.