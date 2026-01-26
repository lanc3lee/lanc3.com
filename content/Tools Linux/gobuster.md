

gobuster dir -u [http://example.com](http://example.com) -w /path/to/wordlist.txt -x aspx,html,php,txt

gobuster dir -u http://example.com    -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x aspx,html,php,txt

gobuster dir -u 192.168.204.60:10000 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x aspx,html,php,txt

gobuster dir -u 192.168.133.27 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x aspx,html,php,txt