
## Meterpreter

**Yes, it is limited.** Meterpreter is a Metasploit payload — it's delivered and managed through `msfconsole` (the `multi/handler` module). Using Meterpreter means using Metasploit, so it falls under the exam restriction. You get **one use** of Metasploit/Meterpreter on the entire exam.

## msfvenom

**No, it is NOT limited.** OffSec explicitly excludes msfvenom from the restriction. You can use it freely throughout the exam to:

- Generate shellcode
- Create staged/stageless payloads
- Encode payloads
- Convert payload formats (exe, elf, raw, etc.)

The key distinction is that msfvenom is purely a **payload generation tool** — it doesn't provide post-exploitation capabilities or an interactive framework session. It just outputs a file/shellcode that you then use independently.