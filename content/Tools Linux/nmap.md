
sudo nmap -sU -p 1-1024 -v 192.168.182.39 -oA results_UDP



ss -tulpn
use this to check for ports after initial access to look for ports not detected by nmap

sudo nmap -sU -p 1-1024 -v 192.168.105.169 -oA nmap/craft_UDP



sudo nmap -sU -p 1-1024 -v 192.168.105.199 -oA nmap/mice_UDP

------

to check for named pipes

nmap --script smb-enum-pipes -p 445 192.168.141.40