
`GetNPUsers.py` (AS-REP Roasting)

"cousin" of Kerberoasting. It targets users who have the setting **"Do not require Kerberos preauthentication"** enabled.

- **When to use:** Early in the game, often before having any credentials.

- **Why it's great:** get a crackable hash for a user without ever sending a single password attempt.
 
- **Command:** `GetNPUsers.py active.htb/ -usersfile users.txt -format hashcat -dc-ip 10.129.13.x

---
 example
 
 for Sauna in HTB
 
 nmap -sCV -v -p- -T4 10.129.95.180 -oA nmap/sauna 
 ...
 389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: EGOTISTICAL-BANK.LOCAL0., Site: Default-First-Site-Name)
 ...

domain = EGOTISTICAL-BANK.LOCAL0
file = sauna_wordlist.txt
-dc-ip = 10.129.95.180

GetNPUsers.py EGOTISTICAL-BANK.LOCAL0/ -usersfile sauna_wordlist.txt -format hashcat -dc-ip 



-----

While the current exams usually starts us with a set of low-privileged credentials or a shell, "Assumed Breach" doesn't mean we have access to _everything_. 

Here is why AS-REP Roasting (and its cousin, Kerberoasting) remains critical for the exam and real-world AD pentesting

### 1. Lateral Movement & Privilege Escalation

In an "Assumed Breach," you might start as a user without permissions to see the Domain Controller's sensitive files. However:

- You might find a **list of other usernames** in a shared folder or by querying AD (using `net user /domain`).
  
- You can then run `GetNPUsers.py` against that _new_ list of users.

- If a higher-privileged user (like a Backup Admin or a Web Dev) has **Pre-Auth Disabled**, you can "Roast" them to jump from your low-priv account to theirs.


---

### 2. The "Enumeration" Loop

Exams are a test of your **enumeration methodology**.

- **Step A:** Use initial credentials to dump the user list.
    
- **Step B:** Check that list for "Roastable" accounts (AS-REP Roasting).
    
- **Step C:** Check that list for Service Principal Names (Kerberoasting).
    
- **Step D:** Crack the hashes.
    

If you skip Step B just because you already have _one_ set of credentials, you might miss the only path to becoming a Domain Admin.