
inspired by this reddit thread
https://www.reddit.com/r/oscp/comments/1q9nldw/oscp_felt_nothing_like_htbpg_how_are_we_supposed/

An active learner who passed CPTS and solved ~100 boxes on HackTheBox & ProvingGrounds failed his first attempt in OSCP

<u>Highlights extracted from reddit</u>
-  don't overcomplicate your methodology... you're always 4-5 commands away from rooting any box you come across
  
- What I felt I did wrong in the start was to thinking too complex. When my mindset changed to this is supposed to be exploited and not super advanced it started to get much easier.
  
- watch the s1ren machine playlist, and follow the ippsec tj null list
  [https://www.youtube.com/watch?v=NQ6jbKqkJ0s&list=PLJrSyRNlZ2EeqkJa12Tu-Ezun9kXvHufN](https://www.youtube.com/watch?v=NQ6jbKqkJ0s&list=PLJrSyRNlZ2EeqkJa12Tu-Ezun9kXvHufN
  
- I did the exam 3 times, and I had 2 times the same AD set. It was extremely difficult and got nothing from it. I failed it with 50 & 60 points.
  
  The third time I got another AD set and it was fucking easy, domain admin in less than 3 hours and passed the exam.
  
- Credentials reuse is probably more common in offsec boxes... 
  with OffSec... almost everything revolves around RCE. 
  HTB (CPTS) will misconfigure a service, let you read a config file, get access to sensitive info so you can move laterally. 
  Their exploitation is all about chaining. 
  OffSec has some of that but the chains are more like a string than a long chain. We have to respect the style of both exams.
  
  
  -------


Question: for Lainkusanagi list, should I focus on HTB or Proving Grounds?

general consensus from the community (and LainKusanagi themselves) is that **Proving Grounds is a better use of your time for the final sprint**, but **HackTheBox is better for building your technical foundation.**

### 1. Use Proving Grounds (Practice) if:

- **You want "Exam-Style" logic:** PG machines are maintained by OffSec. They focus heavily on **deep enumeration** and finding specific CVEs or misconfigurations. The path to root is often simpler than HTB but harder to find because of "rabbit holes."
    
- **You want to practice "The Search":** PG teaches you the vital skill of taking a service version, finding a Public Exploit (PoC), and modifying it to work—which is 80% of the OSCP exam.
    
- **You are close to your exam date:** If you are within 4–6 weeks of your exam, switch almost exclusively to the PG machines on that list.


### 2. Use HackTheBox (Retired) if:

- **You want to learn new techniques:** HTB machines are often "gamified" and more creative. They teach you "out-of-the-box" thinking that PG lacks. If you feel your methodology is stale, an HTB box will force you to learn a new trick (like a specific type of SQLi or a unique file upload bypass).
    
- **You want better learning resources:** HTB has the best walkthroughs in the world (e.g., **0xdf** and **Ippsec**). If you are still in the "I need a hint every 30 minutes" phase, HTB is much better for learning because the explanations are high-quality.
    
- **You need to practice Active Directory:** While the list has PG AD machines, many students find HTB’s AD boxes (like _Forest_ or _Active_) provide a more robust environment for practicing the core AD toolset (Impacket, Bloodhound, etc.).
  
  
  