
In Kali, the package is called `passwordsafe`, but the executable binary is shortened - pwsafe

If you're ever unsure what a package just "gave" you, you can list the binaries it installed into your path.

dpkg -L passwordsafe | grep bin

/usr/bin
/usr/bin/pwsafe
