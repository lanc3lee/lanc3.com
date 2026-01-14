



If you started a Python server 
`python3 -m http.server 80`
and it's hanging or you've lost the terminal window, reclaim that port 
with this one-liner


```
sudo kill -9 $(sudo lsof -t -i :80)
```

- **`-t`**: Tells `lsof` to output _only_ the PID number, which is then passed directly to the `kill` command.

Breaking into two quick steps for easier understanding. 

### 1. Identify the PID (Process ID)

Use `lsof` (List Open Files) or `netstat` to find exactly which process is "squatting" on port 80.

**Method A: Using `lsof` (Recommended)**

```
sudo lsof -i :80
```

- **`-i :80`**: Specifically looks for internet connections on port 80.
    
- **Look for the `PID` column** in the output. It will be a 4 or 5-digit number.
    

**Method B: Using `netstat`**

```
sudo netstat -tulpn | grep :80
```

- The PID will be at the end of the line, usually formatted as `PID/python`.
    

---

### 2. Kill the Process

Once you have the PID (let's say it is **1234**), use the `kill` command:


```
sudo kill 1234
```

> **Note:** If the process is stubborn and doesn't close, use the "Force Kill" flag: `sudo kill -9 1234`.

---

