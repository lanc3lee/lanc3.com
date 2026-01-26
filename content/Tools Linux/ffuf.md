**ffuf** (Fuzz Faster U Fool) is a fast web fuzzer written in Go. It has become the industry standard for security researchers and penetration testers due to its speed, flexibility, and ease of use compared to older tools like Dirbuster or Wfuzz.

Here is a breakdown of why it's so powerful and how to master it.

### 1. The Core Concept: The "FUZZ" Keyword

Unlike other tools that automatically append words to the end of a URL, `ffuf` places the wordlist entry wherever you type the keyword **`FUZZ`**. This allows you to fuzz almost anything:

- **Directory/File Discovery:** `http://10.10.10.10/FUZZ`
    
- **Subdomain Discovery:** `http://FUZZ.target.com`
    
- **Parameter Fuzzing:** `http://target.com/admin.php?param=FUZZ`
    
- **Header Fuzzing:** `-H "User-Agent: FUZZ"`
    

---

### 2. Powerful Filtering (The Key to ffuf)

Web servers often respond with "200 OK" for everything, or provide "403 Forbidden" for many directories. To find the "needle in the haystack," you must filter out the noise.

- **`-fc` (Filter Code):** Hide specific HTTP status codes.
    
    - `ffuf -fc 404,403` (Hides "Not Found" and "Forbidden")
        
- **`-fs` (Filter Size):** Hide responses of a specific size.
    
    - _If you get 1,000 results that are all 152 bytes, add `-fs 152` to hide them._
        
- **`-fl` (Filter Lines):** Hide responses with a specific number of lines in the body.
    
- **`-fr` (Filter Regexp):** Hide responses that match a specific text pattern (e.g., `-fr "Access Denied"`).
    

---

### 3. Common Use Case Scenarios


#### **A. Recursive Scanning**

While `feroxbuster` does this by default, you can make `ffuf` recursive with the `-recursion` flag. It will find a directory and then start fuzzing inside it.

Bash

```
ffuf -u http://10.10.10.10/FUZZ -w wordlist.txt -recursion -recursion-depth 2
```

#### **B. POST Data Fuzzing**

If you are testing a login form for common usernames:

Bash

```
ffuf -u http://target.com/login.php -X POST -d "username=FUZZ&password=password123" -w usernames.txt
```


#### **C. Subdomain Brute Forcing**

When attacking a target, you often want to find hidden subdomains (like `dev.target.com` or `api.target.com`).

Bash

```
ffuf -u http://FUZZ.target.com -w `/usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt`
```


---

### 4. Advanced "Pro" Features

- **Match Mode (`-mc`, `-ms`, `-ml`):** The opposite of filtering. If you only want to see successful hits (200 OK) and redirects (301), use `-mc 200,301`.
    
- **Output Formats:** If you are documenting a Proving Grounds box, you can save the results to a beautiful HTML file:
    
    - `-o results.html -of html`
        
- **Configuration Files:** You can create a `.ffufrc` file in your home directory to set your favorite default wordlists and thread counts so you don't have to type them every time.
    

### 5. Recommended Wordlists for ffuf

On Kali Linux, the best wordlists for `ffuf` are found in the **SecLists** package:

1. **Directories:** `/usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt`
    
2. **Files:** `/usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt`
    
3. **Subdomains:** `/usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt`
    

### Why use ffuf over others?

- **Speed:** Because it's written in Go, it can handle hundreds of threads without crashing your Kali VM.
    
- **Flexibility:** You can fuzz JSON bodies, Cookie values, and even custom HTTP methods.
    
- **Portability:** It's a single binary. If you have enough space, you can even upload the `ffuf` binary to a compromised Linux box to fuzz other internal servers from the inside!


### "Clean" Command:

This uses a bigger wordlist and **filters out** those 403/279-byte responses so they don't clutter your screen:

Bash

```
ffuf -u http://192.168.204.12/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e .md,.txt,.php,.zip -fc 403 -fs 279 -c
```

**What to look for now:**

- Any result with **Status 200** or **301/302** (Redirect).
    
- Any result with a **Size** that isn't 279.
    
- Look for names like `/backup`, `/dev`, `/test`, or `/old`.