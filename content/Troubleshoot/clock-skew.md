
Kerberos SessionError: KRB_AP_ERR_SKEW (Clock skew too great)

To resolve this, we need to synchronize our Linux machine's clock with the Active Directory domain controller's clock using the ntpdate command.

sudo ntpdate < domain controller >


---

classic "VM Time Trap." Even though `ntpdate` successfully stepped your clock, many Virtual Machines (especially on macOS/MacBook setups) have a "Time Sync" feature that instantly snaps your guest clock back to match your host (your Mac), undoing the fix before the script can run.

Kerberos has a strictly enforced **5-minute tolerance window**. If your Kali clock and the DC clock are further apart than that, the DC rejects the ticket request to prevent replay attacks.

----

If you do the manual set and it _still_ says "Clock skew too great," your VM software is fighting you.

- **VMware:** Go to Settings -> General -> **Uncheck "Synchronize time with host."**
    
- **VirtualBox:** You may need to run a command on your host machine to disable the guest time sync service.