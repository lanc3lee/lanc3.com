when having issues getting reverse shells

1. **In a Kali terminal, run:** `sudo tcpdump -i any icmp`
    
2. **In the exploit (#) prompt, run:** `ping -c 3 <kali-IP>
    

- **If you see "IP 192.168.127.240 > 192.168.45.204: ICMP echo request":** The network is fine!


![[tcpdump-reverseshells.png]]