
For pentest & audits, **Nmap** is best with detailed and accurate scans but it's slow compared to **Rustscan** and **Masscan** 

**Rustscan** is popular for OSCP & CPTs prep as it is incredibly fast at scanning all 65,535 ports on a single machine.
Rustscan doesn't actually do version detection itself. It scans all ports at lightning speed and then automatically "pipes" the open ports it finds into Nmap for the heavy lifting.

```
rustscan -a <target-IP> --ulimit 5000 -- -sCV -oN nmap_report.txt
```

- **`-a <target-IP>`**: target **IP Address**
    
- **`--ulimit 5000`**: This is a critical Rustscan flag. It tells your system how many "open files" (sockets) it can handle at once. `5000` is the "sweet spot" for speed without crashing your Kali networking.
    
- **`--` (The Handoff)**: Everything after these two dashes is passed directly to **Nmap**. Rustscan will automatically append the open ports it found to the end of your Nmap command.
    
- **`-sCV`**: The classic Nmap combo:
    
    - `-sC`: Run default safe scripts.
        
    - `-sV`: Determine service/version info.
        
- **`-oN nmap_report.txt`**: Saves the output to a file



**Masscan** is the fastest port scanner in existence. It can scan the entire internet in under 6 minutes. a.k.a "Internet-Scale" Scanner

- **When it's better:** When you are scanning a massive range of IP addresses (e.g., a `/16` or `/8` network) for a single specific port (like finding all open Port 445 servers in a company).
    
- **The Trade-off:** It is "stateless." It doesn't keep track of connections, so it is **not** great at version detection (`-sV`) or OS fingerprinting. It just tells you "Port is Open" or "Port is Closed."
    
- **Tip:** Use Masscan to find _which_ IPs are alive, then feed those specific IPs into Nmap for detail.


Further Reading:
https://www.reddit.com/r/Pentesting/comments/1qbna2n/nmap_vs_rustscan_vs_masscan_which_one_is_better/

https://medium.com/@2s1one/nmap-vs-masscan-vs-rustscan-myths-and-facts-62a9b462241e