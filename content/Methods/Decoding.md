






how to identify Base64 encoded strings?

You can tell they are Base64 because:

- They use a specific character set (A-Z, a-z, 0-9).
    
- They often end with one or two `=` signs (which are "padding" characters used in Base64 encoding).
  
  to decode
  
  echo "base64-string" | base64 -d
  
  

example (astronaut)

echo "YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjEuMy80NDQ0IDA+JjE=" | base64 -d