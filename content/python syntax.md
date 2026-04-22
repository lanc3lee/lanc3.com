

SyntaxError: invalid non-printable character U+200B

```
sed -i 's/\xe2\x80\x8b//g' <exploit>.py
```

uses `sed` (stream editor) to search for the hex code of the zero-width space (`\xe2\x80\x8b`) and deletes it globally throughout the file.

----
