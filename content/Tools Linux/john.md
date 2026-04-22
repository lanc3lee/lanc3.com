john the ripper

Unlike Hashcat, **John the Ripper** is designed to be "lazy" (in a good way). You usually don't need to provide a mode.


```
 john --wordlist=/usr/share/wordlists/rockyou.txt authby-hash.txt                                                                                                    
Warning: detected hash type "md5crypt", but the string is also recognized as "md5crypt-long"
Use the "--format=md5crypt-long" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (md5crypt, crypt(3) $1$ (and variants) [MD5 128/128 ASIMD 4x2])
Will run 6 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
elite            (?)     
1g 0:00:00:00 DONE (2026-02-01 15:27) 3.333g/s 84480p/s 84480c/s 84480C/s lovestruck..260989
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 



```

cracked is "elite"

----

john --wordlist=/usr/share/wordlists/rockyou.txt hash