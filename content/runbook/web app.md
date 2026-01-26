
Upon seeing a login page or dynamic URL, how do we know if it's vulnerable to SQLi or LFI ?

here's good decision matrix to decide which to test first
# SQLi vs LFI 

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
