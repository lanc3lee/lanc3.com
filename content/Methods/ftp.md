
authby in proving grounds is a good lab to learn more about ftp



------

core concept in FTP server management called **Root Jailing** (or **Chrooting**).

When an FTP server is configured, the administrator defines a "Home Directory" for every user. Even though you are connecting to the same IP address, the server looks at _who_ you are and changes what part of the hard drive you are allowed to see.

### 1. The "Anonymous" Jail

When you logged in as `anonymous`, the server's configuration likely pointed that user to the **Application Folder** (where the `zFTPServer.exe` and `Settings.ini` live).

- **Purpose:** Usually, this is so users can download manuals, drivers, or updates.
    
- **Path:** Probably something like `C:\Program Files\zFTPServer\`.
    

### 2. The "Admin" Jail

When you logged in as `admin`, the server pointed you to the **Web Root**.

- **Purpose:** So the admin can upload or edit the website files.
    
- **Path:** Probably something like `C:\wamp\www\` or `C:\Apache24\htdocs\`.
    

### 3. How this helps you

The fact that you are in a different directory as `admin` means you have "escaped" the application folder and moved into the web server's territory.

However, notice that you still can't see the _whole_ `C:` drive. You are still "jailed" inside the web folder. This is why you see `index.php` but you don't see `Windows` or `Users` folders.