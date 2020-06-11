# Five Stages of Ethical Hacking - Overview

Created By: Michael Musheev
Last Edited: Jun 11, 2020 12:05 PM

## Reconnaissance - Information Gathering

# Active

 Using tools like nmap, nikto, nessus and much more that gather information and enumerate certain stuff. This is active since direct recon is being executed on the target itself. Unlike Passive recon.

# Passive

 Any kind of recon that does not interact directly with the target machine. Example: Looking for employees of company X on Facebook.  

![Five%20Stages%20of%20Ethical%20Hacking%20Overview%2014c5292f8061462f9d71de3e1ab54c95/Untitled.png](Five%20Stages%20of%20Ethical%20Hacking%20Overview%2014c5292f8061462f9d71de3e1ab54c95/Untitled.png)

Taken from: [https://asmed.com/active-vs-passive-reconnaissance/#:~:text=Active reconnaissance is a type,to gather information about vulnerabilities.&text=This type of recon requires that attacker interact with the target](https://asmed.com/active-vs-passive-reconnaissance/#:~:text=Active%20reconnaissance%20is%20a%20type,to%20gather%20information%20about%20vulnerabilities.&text=This%20type%20of%20recon%20requires%20that%20attacker%20interact%20with%20the%20target).

Active recon kind of falls into place with the second phase which are scanning and enumeration.

# Scanning & Enumeration

This is the second phase. Scanning and Enumeration means using different tools that do just that, like nmap, nikto, nessus, unicornscan and autoscan.

[Process: Scanning and Enumeration](https://resources.infosecinstitute.com/process-scanning-and-enumeration/)

[Penetration Testing: Intelligence Gathering](https://resources.infosecinstitute.com/penetration-testing-intelligence-gathering/)

- [ ]  ***These are great read on scanning and enumeration, I should go over this again. Check whenever done.***

# Gaining Access - "Exploiting"

This is the stage when an attacker successfully exploits a vulnerability on the system. In this stage, the attacker spends time in finding the right exploit and hence exploiting it.

[Process: Gaining and Elevating Access](https://resources.infosecinstitute.com/process-gaining-and-elevating-access/)

# Maintaining Access - Foothold

Here the attacker would find a way to maintain the access on the infected machine. Imagine closing a door, but having someone else's foot preventing it from fully closing. A way so he could go back in whenever the time is right. Call it a backdoor.

Example: Initiating a malicious/vulnerable process that runs in the background and listens on some port. This serves as a future exploit for the attacker to abuse.

[Penetration Testing: Maintaining Access](https://resources.infosecinstitute.com/penetration-testing-maintaining-access/)

# Covering Tracks

You cannot break into someone else's house and leave the vase broken on the floor, would you? 

In this stage the attacker cares for deleting any file/log that indicates suspicious/malicious activity.

[Penetration Testing: Covering Tracks](https://resources.infosecinstitute.com/penetration-testing-covering-tracks/)

---