
to check for any saved passwords
cmdkey /list

(Get-PSReadlineOption).HistorySavePath

whoami /priv

look for unquoted service paths (famous in xampp)
wmic service get name,displayname,pathname,startmode | findstr /i "Auto" | findstr /i /v "C:\Windows\\" | findstr /i /v """


Search for "password" in config files
findstr /si password *.xml *.ini *.txt *.config

systeminfo

icacls "C:\xampp"


if seeing (M)

wmic service get name,displayname,pathname,startmode | findstr /i "C:\xampp"

Look specifically for services where `StartMode` is `Auto`

-----

 `wmic service where "pathnamelike '%xampp%'" get name,displayname,pathname,startmode,startname`

-----
```
# Search for ANY service that lives inside the C:\xampp folder
Get-WmiObject win32_service | Where-Object {$_.PathName -like "*C:\xampp*"} | Select-Object Name, DisplayName, PathName, StartMode, StartName

# If the above fails, check for the standard names manually
get-service Apache*
get-service mysql*
```


----

use tools such as _PowerUp_ or _JAWS_ to try to find some low-hanging fruit in the system's configuration. 


example, in shenzi
These tools reveal a policy that will install MSI packages as SYSTEM.

```
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer



reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer




```

tasklist /v

------

check 
C:\xampp\properties.ini