
to find flags in linux
find / -name proof.txt
find / -name local.txt 2>/dev/null
find / -name proof.txt 2>/dev/null

------

to find flags in Windows
Get-ChildItem "C:\Users\*\Desktop\*.txt" -ErrorAction SilentlyContinue


-------

```
find / -name proof.txt 2>/dev/null
```

### How it works:

- **`find / -name proof.txt`**: The standard search command.
    
- **`2`**: In Linux, `2` represents the **Standard Error (stderr)** data stream.
    
- **`>`**: The redirection operator.
    
- **`/dev/null`**: A special file (often called the "black hole") that discards any data written to it.
    

By using `2>/dev/null`, you are telling the terminal: "Run the search, but if you hit an error, throw that error message in the trash instead of showing it to me.