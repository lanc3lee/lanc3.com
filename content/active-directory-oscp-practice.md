# Mastering the OSCP AD Set: Why You Should Chain Hack Academy and THM Wreath

If you’re grinding for the OSCP, you already know the stakes: the **Active Directory (AD) set is worth 40 points**, and in the current exam format, it's usually **all or nothing**. You either compromise the Domain Controller and walk away with nearly 60% of your passing grade, or you get stuck at a pivot point and watch 24 hours slip away.

Most students practice on standalone machines. But the exam isn't standalone; it's a **chain**. To build the right muscle memory, I recommend a specific "1-2 Punch" strategy: Start with **Hack Academy’s OSCP AD Chain #1**, then graduate to **TryHackMe’s Wreath (or Holo)**.

### Step 1: The "Sniper Rifle" — Hack Academy AD Chain #1

Before you try to tackle massive networks, you need to master the "Exam Flow." This is where Hack Academy comes in.

- **Why start here?** It is a purpose-built, 3-machine replica of the OSCP exam environment (2 Clients + 1 DC). It doesn't distract you with 20 different servers; it focuses strictly on the lateral movement and escalation steps you’ll likely see on game day.
    
- **The Best Part:** It’s a downloadable lab (VirtualBox/VMware). This means **zero network lag**. When your exploit doesn't work, you'll know it's because of your configuration, not because a VPN tunnel dropped a packet.
    
- **The Goal:** Use this to perfect your "Initial Access $\rightarrow$ Local PrivEsc $\rightarrow$ Lateral Movement $\rightarrow$ Domain Admin" methodology.
    

### Step 2: The "Masterclass" — THM Wreath or Holo

Once you’ve mastered the 3-machine flow, you need to learn how to handle **complex networking**. This is the #1 reason students fail the AD set: they can't manage their tunnels.

- **THM Wreath:** This isn't just an AD lab; it’s a **pivoting masterclass**. It forces you to use tools like `Ligolo-ng`, `Chisel`, and `sshuttle` to hop through a network that isn't directly reachable.
    
- **THM Holo:** If you finish Wreath and want more, Holo steps it up with a larger environment and more modern AD attacks.
    
- **Why this order?** Hack Academy teaches you the _attacks_; Wreath teaches you how to _deliver_ those attacks through multiple subnets. If you can pivot through Wreath, the single-hop pivot in the OSCP exam will feel like child's play.
    

### The Winning Strategy

Don't just jump into a Pro Lab like Dante or Zephyr if your exam is around the corner—they are massive and can lead to burnout. Instead:

1. **Download Hack Academy Chain #1** to get the 3-machine rhythm down.
    
2. **Clear THM Wreath** to become a pivoting god.