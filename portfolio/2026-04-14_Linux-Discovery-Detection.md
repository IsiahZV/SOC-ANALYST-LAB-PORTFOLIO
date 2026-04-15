# Detecting Discovery Techniques in Linux Systems
**Date:** 2026-04-014

**Objective:**
- Explore how to identify Discovery commands in logs
- Learn common threats endangering Linux servers
- Know how attackers upload malware onto their victims
- Uncover a real cryptominer attack

**Environment:** TryHackMe – “Linux Threat Detection 2” room  
**Tools Used:** CLI

---

## DISCOVERY OVERVIEW

### What is the command's output you discovered?
> Run systemd-detect-virt to detect the system's cloud. 

<img width="526" height="142" alt="Screenshot 2026-04-14 at 8 16 53 PM" src="https://github.com/user-attachments/assets/11ff6bcd-b0a5-48ff-b6fa-daf1f2258e41" />

- amazon

#### Whats the importance of this command?
"systemd-detect-virt" checks the environment and tells you if you're running on physical hardware or a virtual machine. It also displays what virtualization technology is being used (in this case, amazon or AWS virtual machine. In this case, if an attacker learns that this is a cloud-based VM, they may for valuable IAM roles and abuse services (EC2 is common to see in the world of lateral movements and S3 for data exfiltration).

This command may also be ran to avoid sandbox detection and adjust payloads for cloud environments. 

##

### What is the full path to the detected antimalware binary?
> Now run ps aux and look for EDR or antivirus processes.

<img width="1424" height="782" alt="Screenshot 2026-04-14 at 8 32 21 PM" src="https://github.com/user-attachments/assets/35211c03-80e5-4302-a1ef-6e6d4956720c" />


---


## DETECTING DISCOVERY
> For this task, imagine you received a SIEM alert about a spike in Discovery commands.
The first thing you see is that the itsupport user launched the hostname command.
Can you continue the investigation on the VM and find out what really happened?
### 
