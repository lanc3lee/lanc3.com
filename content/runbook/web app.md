
Upon seeing a login page or dynamic URL, how do we know if it's vulnerable to SQLi or LFI ?

here's good decision matrix to decide which to test first
# SQLi vs LFI 

- **Quick SQLi Check:** 
  Put a `'` in every parameter. If the page breaks (500 Error) or content disappears, spend 10 minutes testing for SQLi.
    
- **Quick LFI Check:** 
  Try `../../../../../../../../etc/passwd`. If you see "root:x:0:0", you’re done. If you get a "File not found" error that _shows the path_, pay close attention to how it's looking for files.

---------

### Focus on SQL Injection (SQLi)
if the application is clearly **querying a database** to retrieve specific records.

- **Login Pages:** Always try a few basic bypasses first (e.g., `' OR 1=1-- -`). If that doesn't work within 5 minutes, move on to LFI or brute force.
    
- **Search Fields:** If you search for a product and get results, try to "break" the query with a single quote (`'`).
    
- **Ordered Lists:** If you can sort items (e.g., `sort=price`), this is a huge indicator of potential SQLi in the `ORDER BY` clause.
    

---

### Focus on Local File Inclusion (LFI)

if the application is **building the page dynamically** from other files on the server.

- **Templating:** If you see `index.php?content=home`, the server is likely taking "home" and looking for `home.php` or `home.html` in a folder.
    
- **Static File Loading:** If you see parameters that look like paths (e.g., `download.php?file=manual.pdf`), prioritize LFI immediately.
    
- **Discovery Hint:** If you run `dirb` or `ffuf` and find directories like `/inc/`, `/includes/`, or `/templates/`, the site is almost certainly using inclusion logic.

----------

if URL structure is
item.php?id=10
focus on SQLi

if URL structure is
view.php?page=contact.php
focus on LFI / Path Traversal

-----

if input type is 
numeric IDs, search bars, login forms
focus on SQLi

if input type is 
Filenames, Paths, Language switchers (lang=en)
focus on LFI / Path Traversal

-------

if error message is 
"SQL Syntax Error", "Database timeout"
focus on SQLi

if error message is 
"file_get_contents()", "failed to open stream"
focus on LFI / Path Traversal

-------

if behavior is 
Data changes or disappears with a '
focus on SQLi

if behavior is 
page layout stays but content disappears
focus on LFI / Path Traversal

