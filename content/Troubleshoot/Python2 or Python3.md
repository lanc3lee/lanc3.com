Identify the version by looking for these specific syntax markers:

- **The Print Statement:**
    
    - `print "Message"` (No parentheses) → **Python 2**
        
    - `print("Message")` (Parentheses required) → **Python 3**
        
- **Input Handling:**
    
    - `raw_input()` → **Python 2**
        
    - `input()` → **Python 3**
        
- **The Shebang (First line):**
    
    - `#!/usr/bin/env python` or `#!/usr/bin/python` → Usually defaults to **Python 2** on older Linux builds.
        
    - `#!/usr/bin/env python3` → Explicitly **Python 3**.
        

**Quick Tip for OSCP:** If you try to run a Python 2 script with `python3`, it will immediately throw a `SyntaxError` on the first `print` statement.