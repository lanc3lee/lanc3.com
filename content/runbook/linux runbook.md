

after gaining initial access using exploits

### Full TTY Upgrade

upgrade this to a fully functional terminal in 3 steps.

#### Step 1: Spawn the PTY

```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

 It tells Python to "cheat" and create a pseudo-terminal (PTY) that tricks the system into thinking you are logged in via a real console.

#### Step 2: Fix the Local Terminal 

This is where we make `Ctrl+C` and the **Tab** key work.

1. Press **`Ctrl+Z`** on your keyboard (this backgrounds your shell).
    
2. in your **Kali** terminal:

    ```
    stty raw -echo; fg
    ```
    
3. Press **Enter** twice.
    

- **What this does:** `stty raw -echo` tells your Kali terminal to stop interpreting special keys (like `Ctrl+C`) and just send them straight through the pipe to the Nibbles server. `fg` brings your shell back to the front.
    

#### Step 3: Set Environment Variables

Back in the shell, run these two commands to get colors and full-screen support:

```
export TERM=xterm-256color
export SHELL=bash
```

--------

if pkexec found suid, pwnkit will usually work

---------
being running linpeas, try

sudo -l


-------

GTFOBins

sudo sudo /bin/sh

configuration `(ALL) ALL` means user has been granted **full administrative privileges** over the entire system.

- **The First `(ALL)`:** You can act as **any user** (root, www-data, etc.).
    
- **The Second `ALL`:** You can run **any command** on the system.
    

When running `sudo /bin/sh`, the system checked the `/etc/sudoers` file, saw that user is allowed to run "ALL" commands, and executed the shell (`/bin/sh`) with root permissions.
