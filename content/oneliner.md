```

```
nxc ldap < ip of DC > -u < username > -p '< password >' --users | fgrep -v '[' | fgrep -vi '-Username-' | awk '{print$ 5}' | tee users