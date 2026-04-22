

gobuster dir -u [http://example.com](http://example.com) -w /path/to/wordlist.txt -x aspx,html,php,txt

gobuster dir -u http://example.com    -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x aspx,html,php,txt

gobuster dir -u 192.168.204.60:10000 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x aspx,html,php,txt

gobuster dir -u 192.168.133.27 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x aspx,html,php,txt


gobuster dir -u 192.168.117.187 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x aspx,html,php,txt

gobuster dir -u 192.168.106.55 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x aspx,html,php,txt

gobuster dir -u http://192.168.105.180 -w /usr/share/wordlists/dirb/common.txt -x php,html,txt,xml,zip,config

gobuster dir -u http://192.168.105.169 -w /usr/share/wordlists/dirb/common.txt -x php,html,txt,xml,zip,config

gobuster dir -u http://192.168.208.189:3128/ -w /usr/share/wordlists/dirb/common.txt -x php,html,txt,xml,zip,config


```
gobuster dir -u http://access.offsec -w /usr/share/wordlists/dirb/common.txt -x php,html,txt,xml,zip,config
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://access.offsec
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8
[+] Extensions:              php,html,txt,xml,zip,config
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 302]
/.htaccess.zip        (Status: 403) [Size: 302]
/.hta.html            (Status: 403) [Size: 302]
/.hta.php             (Status: 403) [Size: 302]
/.hta.zip             (Status: 403) [Size: 302]
/.htaccess.config     (Status: 403) [Size: 302]
/.htaccess            (Status: 403) [Size: 302]
/.hta.xml             (Status: 403) [Size: 302]
/.hta.config          (Status: 403) [Size: 302]
/.htaccess.php        (Status: 403) [Size: 302]
/.hta.txt             (Status: 403) [Size: 302]
/.htaccess.xml        (Status: 403) [Size: 302]
/.htaccess.html       (Status: 403) [Size: 302]
/.htpasswd.html       (Status: 403) [Size: 302]
/.htpasswd.config     (Status: 403) [Size: 302]
/.htpasswd.php        (Status: 403) [Size: 302]
/.htaccess.txt        (Status: 403) [Size: 302]
/.htpasswd            (Status: 403) [Size: 302]
/.htpasswd.xml        (Status: 403) [Size: 302]
/.htpasswd.txt        (Status: 403) [Size: 302]
/.htpasswd.zip        (Status: 403) [Size: 302]
/assets               (Status: 301) [Size: 339] [--> http://access.offsec/assets/]
/aux                  (Status: 403) [Size: 302]
/aux.config           (Status: 403) [Size: 302]
/aux.txt              (Status: 403) [Size: 302]
/aux.xml              (Status: 403) [Size: 302]
/aux.html             (Status: 403) [Size: 302]
/aux.zip              (Status: 403) [Size: 302]
/aux.php              (Status: 403) [Size: 302]
/cgi-bin/             (Status: 403) [Size: 302]
/cgi-bin/.html        (Status: 403) [Size: 302]
/com1.php             (Status: 403) [Size: 302]
/com1                 (Status: 403) [Size: 302]
/com1.txt             (Status: 403) [Size: 302]
/com1.html            (Status: 403) [Size: 302]
/com1.xml             (Status: 403) [Size: 302]
/com1.zip             (Status: 403) [Size: 302]
/com2.config          (Status: 403) [Size: 302]
/com2.txt             (Status: 403) [Size: 302]
/com3                 (Status: 403) [Size: 302]
/com2.php             (Status: 403) [Size: 302]
/com2                 (Status: 403) [Size: 302]
/com2.html            (Status: 403) [Size: 302]
/com2.zip             (Status: 403) [Size: 302]
/com1.config          (Status: 403) [Size: 302]
/com2.xml             (Status: 403) [Size: 302]
/com3.php             (Status: 403) [Size: 302]
/com3.xml             (Status: 403) [Size: 302]
/com3.txt             (Status: 403) [Size: 302]
/com3.html            (Status: 403) [Size: 302]
/com3.zip             (Status: 403) [Size: 302]
/com3.config          (Status: 403) [Size: 302]
/con                  (Status: 403) [Size: 302]
/con.zip              (Status: 403) [Size: 302]
/con.txt              (Status: 403) [Size: 302]
/con.html             (Status: 403) [Size: 302]
/con.config           (Status: 403) [Size: 302]
/con.xml              (Status: 403) [Size: 302]
/con.php              (Status: 403) [Size: 302]
/examples             (Status: 503) [Size: 402]
/forms                (Status: 301) [Size: 338] [--> http://access.offsec/forms/]
/index.html           (Status: 200) [Size: 49680]
/Index.html           (Status: 200) [Size: 49680]
/index.html           (Status: 200) [Size: 49680]
/licenses             (Status: 403) [Size: 421]
/lpt1.xml             (Status: 403) [Size: 302]
/lpt1.zip             (Status: 403) [Size: 302]
/lpt1.php             (Status: 403) [Size: 302]
/lpt1                 (Status: 403) [Size: 302]
/lpt1.config          (Status: 403) [Size: 302]
/lpt2.config          (Status: 403) [Size: 302]
/lpt1.txt             (Status: 403) [Size: 302]
/lpt2.xml             (Status: 403) [Size: 302]
/lpt2.php             (Status: 403) [Size: 302]
/lpt2.zip             (Status: 403) [Size: 302]
/lpt1.html            (Status: 403) [Size: 302]
/lpt2                 (Status: 403) [Size: 302]
/lpt2.html            (Status: 403) [Size: 302]
/lpt2.txt             (Status: 403) [Size: 302]
/nul.php              (Status: 403) [Size: 302]
/nul                  (Status: 403) [Size: 302]
/nul.txt              (Status: 403) [Size: 302]
/nul.zip              (Status: 403) [Size: 302]
/nul.html             (Status: 403) [Size: 302]
/nul.xml              (Status: 403) [Size: 302]
/nul.config           (Status: 403) [Size: 302]
/phpmyadmin           (Status: 403) [Size: 421]
/prn.html             (Status: 403) [Size: 302]
/prn.php              (Status: 403) [Size: 302]
/prn                  (Status: 403) [Size: 302]
/prn.zip              (Status: 403) [Size: 302]
/prn.xml              (Status: 403) [Size: 302]
/prn.config           (Status: 403) [Size: 302]
/prn.txt              (Status: 403) [Size: 302]
/server-info          (Status: 403) [Size: 421]
/server-status        (Status: 403) [Size: 421]
/ticket.php           (Status: 200) [Size: 0]
/uploads              (Status: 301) [Size: 340] [--> http://access.offsec/uploads/]
/webalizer            (Status: 403) [Size: 421]
Progress: 32291 / 32291 (100.00%)

```