searchsploit -m [Exploit-ID]

The `-m` flag stands for **mirror**. It locates the exploit file and automatically copies it into your current working directory so you don't have to hunt through `/usr/share/exploitdb/`

**Examine first:** Before running it, use `-x` to read the exploit code in your terminal to see if you need to change any variables (like IP addresses or ports): `searchsploit -x 47799`