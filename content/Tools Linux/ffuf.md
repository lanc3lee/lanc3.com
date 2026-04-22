**ffuf** (Fuzz Faster U Fool) is a fast web fuzzer written in Go. It has become the industry standard for security researchers and penetration testers due to its speed, flexibility, and ease of use compared to older tools like Dirbuster or Wfuzz.

Here is a breakdown of why it's so powerful and how to master it.

### 1. The Core Concept: The "FUZZ" Keyword

Unlike other tools that automatically append words to the end of a URL, `ffuf` places the wordlist entry wherever you type the keyword **`FUZZ`**. This allows you to fuzz almost anything:

- **Directory/File Discovery:** `http://10.10.10.10/FUZZ`
    
- **Subdomain Discovery:** `http://FUZZ.target.com`
    
- **Parameter Fuzzing:** `http://target.com/admin.php?param=FUZZ`
    
- **Header Fuzzing:** `-H "User-Agent: FUZZ"`
    


# important to master parameter fuzzing (for LFI)

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


-------

Understand the Concept

**Directory Fuzzing:** Tells us _where_ the files are (e.g., there is a `/pages` folder).
    
**Parameter Fuzzing:** Tells us _how_ the application handles those files (e.g., `index.php` is including files from that folder).
    

when we see a structure like `?target=pages&page_id=...`, the application is likely using the value of `page_id` to fetch a file from the `target` directory. This is a classic indicator of a potential **Local File Inclusion (LFI)** vulnerability.



-------

Example - payday box in proving grounds 

ffuf -u 'http://192.168.168.39/index.php?FUZZ=test' -w /usr/share/wordlists/seclists/Discovery/Web-Content/burp-parameter-names.txt -ac

**`-ac` (Auto-Calibration):** This is the most important flag for an exam. It tells `ffuf` to automatically detect the "standard" response size and hide everything that matches it, showing you only the outliers


In web fuzzing, a **Status 200** for every single entry is actually a "False Positive" and is a common hurdle in OSCP-style labs. It means the application is configured to return a "Success" page regardless of whether the parameter is valid or not.

In this case, the **Status Code is useless**, but the **Response Size (Length)** and **Word Count** are the keys to finding the needle in the haystack.


-----
learnt from practising Flight on hackthebox

ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.flight.htb" -u http://flight.htb -fl 155

This command is used for **Virtual Host (vHost) Fuzzing**. It is trying to find hidden subdomains or alternative websites hosted on the same IP address by manipulating the `Host` header.

Here is the breakdown of each part:

### The Command Breakdown

- **`ffuf`**: Fuzz Faster U Fool—a high-speed web fuzzer.
    
- **`-w [path]`**: Specifies the **wordlist**. It will take every line in that text file and plug it into the `FUZZ` keyword.
    
- **`-H "Host: FUZZ.flight.htb"`**: This is the "Host Header." Since you are hitting the main IP/domain (`flight.htb`), `ffuf` replaces `FUZZ` with words from your list (e.g., `dev.flight.htb`, `test.flight.htb`).
    
- **`-u http://flight.htb`**: The **target URL**. Even though the Host header changes, you are still sending the traffic to the same web server.
    
- **`-fl 155`**: This is the **Filter Lines** flag.
    

---

### What is the `-fl 155` flag?

The `-fl` flag tells `ffuf` to **Filter (hide) responses that have exactly 155 lines of text.**

**Why do you need this?** When you fuzz for virtual hosts, the server usually returns a response for _every single request_.

- If you guess a subdomain that **doesn't exist**, the server typically returns its default "Not Found" or "Default" page.
    
- Let's say that default page is exactly 155 lines long.
    
- If you don't filter it, your screen will be flooded with thousands of useless results.
    

By using `-fl 155`, you are telling `ffuf`: _"I don't care about the default page. Only show me results that are a different length, because those are likely the hidden sites I'm looking for."_

---

### Common Ffuf Filter Flags

If line counting isn't working well, you can also filter by other attributes:

|Flag|Description|Use Case|
|---|---|---|
|**`-fc`**|Filter **Code**|Hide `404` or `403` errors (e.g., `-fc 404`).|
|**`-fs`**|Filter **Size**|Hide responses of a certain byte size (e.g., `-fs 1240`).|
|**`-fw`**|Filter **Words**|Hide responses with a specific word count.|

**Summary for Flight:** You likely ran the command once without filters, saw that everything returned a "155 line" response, and now you're using `-fl 155` to hide that noise and find the real `school.flight.htb` or other hidden gems.

why 155 ? how do i know when to change the #

The number **155** isn't a magic number or a standard—it is completely specific to the server's response on the **Flight** box at that exact moment.

You determine which number to use by performing a **calibration run**. Here is the step-by-step process:

### 1. The "Noise" Run

Run your `ffuf` command **without any filters** for just a few seconds, then hit `Ctrl+C` to stop it.

Bash

```
ffuf -w [wordlist] -H "Host: FUZZ.flight.htb" -u http://flight.htb
```

### 2. Identify the Pattern

Look at the results. You will notice that almost every "wrong" guess returns the exact same **Response Code**, **Lines**, **Words**, and **Chars**.

**Example Output:**

Plaintext

```
default      [Status: 200, Size: 1240, Words: 350, Lines: 155]
test         [Status: 200, Size: 1240, Words: 350, Lines: 155]
admin        [Status: 200, Size: 1240, Words: 350, Lines: 155]
```

If you see that every invalid subdomain returns **155 lines**, then 155 is your "noise" floor.

### 3. Apply the Filter

Now that you know 155 is the signature of a "File Not Found" or "Default" page, you tell `ffuf` to hide it so that only the **unique** responses stand out.

Bash

```
# Now you add the filter
ffuf ... -fl 155
```

---

### When should you change the number?

You need to change the number whenever the **server's default behavior** changes. This happens if:

- **You switch targets:** A different HTB box will have a different default page (maybe 10 lines, maybe 200).
    
- **You change protocols:** `http` might return 155 lines, while `https` might return a 403 error with only 10 lines.
    
- **The server configuration updates:** If a developer changes the "404 Not Found" page, the line count will change.
    

### Pro-Tip: Use `-fs` (Filter Size) instead

Line counts (`-fl`) can sometimes be unreliable if the server dynamically injects small bits of text (like a timestamp). **Filtering by Size (`-fs`)** is usually more precise because the total byte count of the default page is less likely to fluctuate.

**The Workflow:**

1. Run with no filters.
    
2. Observe the **Size** of the 99% of results (e.g., `1240`).
    
3. Run again with `-fs 1240`.