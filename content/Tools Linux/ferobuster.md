
Feroxbuster is written in Rust and is arguably the best directory brute-forcer available today. It is recursive by default (it finds a folder and automatically starts scanning inside it).

Bash

```
feroxbuster -u http://[TARGET_IP] -x md pdf zip aspx html php txt
```


Feroxbuster has a **built-in default wordlist** path configured in its settings (usually pointing to `/usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt` on Kali). If that file exists, it will use it automatically. However, if it can't find that specific file, it will throw an error and ask you for one.

feroxbuster -u http://192.168.204.12 -x md pdf zip aspx html php txt

### Why Feroxbuster is different from Gobuster

1. **Auto-Recursion:** Feroxbuster will find a directory (like `/images`), and then **automatically** start a new scan inside that directory. Gobuster doesn't do this unless you tell it to, and even then, it's not as efficient.
    
2. **The `-x` Flag:** In Feroxbuster, you don't need commas for extensions; just spaces work fine (though commas are also supported).
    
3. **Speed:** It uses "threads" (parallel connections) extremely well. If the scan is too slow or too fast, you can adjust it with `-t` (threads).
    

### Common Syntax Tips

- **To specify a specific wordlist:** Use `-w`
    
    Bash
    
    ```
    feroxbuster -u http://192.168.204.12 -w /usr/share/wordlists/dirb/common.txt
    ```
    
- **To ignore specific status codes (like 403 or 401):** Use `-C`
    
    Bash
    
    ```
    feroxbuster -u http://192.168.204.12 -C 403,404
    ```
    
- **To save the output to a file:** Use `-o`
    
    Bash
    
    ```
    feroxbuster -u http://192.168.204.12 -o discovery.txt
    ```


sudo apt update && apt install feroxbuster            
Hit:1 https://packages.microsoft.com/repos/code stable InRelease
Hit:2 https://packages.cloud.google.com/apt cloud-sdk InRelease       
Hit:3 http://http.kali.org/kali kali-rolling InRelease                
2073 packages can be upgraded. Run 'apt list --upgradable' to see them.
Installing:                     
  feroxbuster
   
Installing dependencies:
  seclists
  
Summary:
  Upgrading: 0, Installing: 2, Removing: 0, Not Upgrading: 2073
  Download size: 548 MB
  Space needed: 1,947 MB / 1,124 MB available

Warning: More space needed than available: 1,947 MB > 1,124 MB, installation may fail
Continue anyway? [y/N] N
Abort.
   
┌──(root㉿kali)-[/home/lanc3]
└─# 
