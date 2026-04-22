**Active**, **Forest**, **Sauna**, and **Cicada** are the "Big Four" for understanding the basics of GPP passwords, Kerberoasting, AS-REPRoasting, and basic service misconfigurations (like your recent `SeBackupPrivilege` win).

To be fully ready for the OSCP's AD set, you need to move from **single-hop** exploitation to **lateral movement**

https://docs.google.com/spreadsheets/d/18weuz_Eeynr6sXFQ87Cd5F0slOj9Z6rt/htmlview

After "Big Four"...

## **Return** 
**Difficulty:** Easy/Medium 
**Why next:** This box is a perfect bridge from the fundamentals learnt to more creative service exploitation. It focuses on how local services (like printers or scanners) interact with the domain.

- **Key Lesson:** Capturing credentials by redirecting service authentications.

- **Exam Value:** High. Using common office hardware/software misconfigurations as an entry point is often tested.
    

## 2. **Blackfield** (The Core AD Grind)

**Difficulty:** Hard
**Why second:** After learning how to exploit the "SeBackupPrivilege" on Cicada, Blackfield is the logical next step. It forces you to deal with multiple users, service accounts, and the **Domain Backup** process.

- **Key Lesson:** Moving from a low-privilege user to a "Backup Operator" and finally to Domain Admin.

- **Value:** Extreme. This box mirrors the "layered" approach of the exam, where one credential discovery leads to a slightly higher-privileged share or service.
 
## 3. **Flight**

**Difficulty:** Hard 
**Why last:** Flight is heavy on **enumeration** and **forced authentication**. It is a "noisier" box that requires a very organized approach to your notes.

- **Key Lesson:** NTLM relaying, complex LDAP enumeration, and understanding how different web/service components tie into AD.

- **Value:** This is excellent preparation for the "Post-Exploitation" phase of the exam, where you have to map out a larger network of users to find the weak link.