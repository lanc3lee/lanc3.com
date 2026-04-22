
**HAITI** (often stylized as **haiti**) is a modern **hash type identifier** tool.

When you find a hashed password (like `$2y$10$abcdef...`) during an engagement, your first problem is knowing what algorithm it uses so you can feed it into a cracker like Hashcat or John the Ripper. That is where `haiti` comes in.

---

### Why use `haiti` specifically?

While there are older tools like `hash-identifier` (which is often pre-installed on Kali), they are frequently outdated and miss modern hash formats. `haiti` is favored by many OSCP students because:

- **Massive Library:** It recognizes over **500+** different hash types.
    
- **Cracker Integration:** It doesn't just name the hash; it tells you the specific **Hashcat mode** (e.g., `-m 1000`) and the **John the Ripper format** (e.g., `raw-sha1`) you need to use. This saves you a lot of time looking up documentation.
    
- **Speed & Accuracy:** It uses modern regex and logic to distinguish between similar-looking hashes (like MD5 vs. MD4).