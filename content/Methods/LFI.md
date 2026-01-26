
Local File Inclusion (LFI) is a web vulnerability that allows an attacker to trick a web application into exposing or running files on the server that were never intended to be accessible.

It usually occurs when an application takes user-supplied input (like a filename or a path) and passes it to a file-processing function (like `include` in PHP or `fs.readFile` in Node.js) without properly sanitizing it

# Example

`https://example.com/index.php?page=contact.php`

If the back-end code looks like this:

PHP

```
<?php
   $file = $_GET['page'];
   include($file);
?>
```

An attacker can change the `page` value to a sensitive file path. Since the code blindly trusts the input, it will "include" and display the contents of that file on the webpage

### Common LFI Attack Techniques

#### 1. Path Traversal (`../`)

Attackers use "dot-dot-slash" sequences to move up out of the web root directory and into the server's internal folders.

- **Payload:** `../../../../etc/passwd`
    
- **Result:** The server displays the list of all users on the Linux system

#### 2. Log Poisoning (LFI to RCE) 

include a log file (like /var/log/apache2/access.log) that contains a piece of malicious PHP code "injected" earlier by visiting a non-existent page, the server will execute that code. turns a simple file read (LFI) into Remote Code Execution (RCE)

#### 3. PHP Wrappers

Attackers can use built-in PHP filters to encode file contents in Base64. This is useful for reading `.php` source files that would otherwise be executed rather than displayed.

- **Payload:** `php://filter/convert.base64-encode/resource=config.php`

------------
**Map the System:** Once you have LFI, always try to read:

- `/etc/passwd` (Users)
    
- `/var/log/apache2/access.log` (For poisoning)
    
- `/home/user/.ssh/id_rsa` (For keys)
-------

## tips
- **Fuzz for parameters first:** Don't assume the parameter is `?page=`. Use `ffuf` with `burp-parameter-names.txt`.
    
- **Test for "Filter Bypass":** If `../../etc/passwd` is blocked, try:
    
    - **Double encoding:** `%252e%252e%252f`
        
    - **PHP Wrappers:** `php://filter/convert.base64-encode/resource=index.php` (This lets you read the source code without executing it).


--------

further practice

## **Poison (HackTheBox - Retired)**
widely considered the best LFI teaching machine

Valentine (HackTheBox - Retired)

**BountyHealer** or **Photobomb**,

--------

oisoning (manipulation of traffic packets) is not allowed in the exam. However, based on this scenario - justifying this to be LFI would suffice. Read more here: [https://shahjerry33.medium.com/rce-via-lfi-log-poisoning-the-death-potion-c0831cebc16d](https://shahjerry33.medium.com/rce-via-lfi-log-poisoning-the-death-potion-c0831cebc16d "https://shahjerry33.medium.com/rce-via-lfi-log-poisoning-the-death-potion-c0831cebc16d")
