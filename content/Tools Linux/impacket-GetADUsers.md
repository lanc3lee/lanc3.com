```
impacket-GetADUsers -all -dc-ip flight.htb flight.htb/svc_apache:'S@Ss!K@*t13' | cut -d " " -f 1 | grep -Ev 'Name|Impacket|\-\-|\['
```


```
impacket-GetADUsers -all -dc-ip flight.htb flight.htb/svc_apache:'S@Ss!K@*t13' 
```

![[impacket-getADusers.png]]