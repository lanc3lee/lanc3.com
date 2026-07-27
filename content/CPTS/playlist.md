
### Community-Curated CPTS Resources

1. **IppSec’s Unofficial CPTS Playlist**
    
    - YouTube playlist of Hack The Box machines that feature vulnerabilities and methodologies highly relevant to the CPTS exam.

    - **Why it works:** forces you to learn the "why" behind an exploit, which is critical for the manual, multi-step environment of the CPTS exam.

    - [Link to IppSec's CPTS Playlist](https://www.youtube.com/playlist?list=PLidcsTyj9JXItWpbRtTg6aDEj10_F17x5)
        
2. **SavitarX’s CPTS Prep List**
    
    - categorizes machines by attack vector (e.g., AD/Domain, Web/App, Pivoting) and provides a mapping of which techniques to practice for each.
    
    - [SavitarX CPTS Preparation Notes](https://savitar.gitbook.io/mynotes/machines-to-pratice-for/cpts-preparation)
    
3. **Official HTB "CPTS Preparation Track"**
    
    - Hack The Box actually maintains an official "CPTS Preparation Track" on the main platform. 
      It is a set of 16 hand-picked machines designed to bridge the gap between Academy theory and exam-level complexity.
      https://app.hackthebox.com/tracks/76
      
      While Hack The Box does not always publish the list as a static, permanent document
      —because they occasionally rotate machines to keep them relevant
      —the track is designed to transition you from the "theory-heavy" Academy modules to the "operational-heavy" reality of the exam
      
      16 machines in this track are curated to force you to stop relying on step-by-step module instructions and start thinking about **methodology and chaining**.

Unlike the Academy modules, which are often "one technique per lesson," these machines require:

- **Methodical Enumeration:** Identifying a vulnerability isn't enough; you must map the target’s service ecosystem.
    
- **Chaining Exploits:** You will rarely get a shell on the first attempt. You often have to gain a foothold, perform local enumeration, identify a misconfiguration, and use that to pivot or escalate.
    
- **Environment Simulation:** They mimic the "messy" real-world environment where documentation is sparse and rabbit holes are intentional.


biggest hurdle in CPTS isn't usually the initial shell; it's the **post-exploitation and pivoting**.

- **Focus on the "Chains":** Unlike the OSCP (where many boxes are somewhat self-contained), the CPTS exam is an enterprise-network simulation. Look for machines in these lists that involve **"Pivoting"** or **"Lateral Movement."**
    
- **The "Academy" is the Syllabus:** 
  While the lists above are great for practice, the 28 modules in the **Penetration Tester path on HTB Academy** are the actual source of truth for the exam. 
  The exam is essentially a test of ability to apply the tools taught in those 28 modules to an unfamiliar network structure.
  



Tips:

**Focus on the "Skill Assessments" and "AEN (Advanced Exploitation Network)"** modules inside the Academy. Community members consistently report that these are the closest things to the actual exam.