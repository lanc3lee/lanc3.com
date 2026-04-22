*Evil-WinRM* PS C:\> certutil -ca
  Name: Active Directory Enrollment Policy
  Id: {79B47A22-3743-4AD3-9E13-13B6432AE1BB}
  Url: ldap:
0 CAs:
CertUtil: -CA command completed successfully.
*Evil-WinRM* PS C:\> 

`ertutil -ca` command returned **`0 CAs`**.

**What this tells us:** This specific DC is **not** a Certificate Authority. This rules out the popular "ADCS" (Active Directory Certificate Services) escalation path (like ESC1 or ESC8). You can stop looking for certificate-based vulnerabilities on this specific machine.
