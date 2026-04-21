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

### What is the path of the script that initiated the "hostname" command?

<img width="1440" height="417" alt="Screenshot 2026-04-15 at 11 55 16 AM" src="https://github.com/user-attachments/assets/b5956e8f-5922-457d-a080-6790ee6ef484" />

- /home/itsupport/debug.sh

##

### What was the last Discovery command launched by the script?

I'll be explaining this for my sake.

<img width="1440" height="783" alt="Screenshot 2026-04-15 at 12 10 07 PM" src="https://github.com/user-attachments/assets/f2932dcd-59d4-4c22-964f-c7386c73d4c0" />

Keyword is discovery command

<img width="1440" height="783" alt="Screenshot 2026-04-15 at 12 26 53 PM" src="https://github.com/user-attachments/assets/b6f02531-e319-45d1-aee8-9bc8db7cdaca" />
- ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu

##

### Looking at the script content, what's the email of the script author?

<img width="708" height="135" alt="Screenshot 2026-04-15 at 12 58 55 PM" src="https://github.com/user-attachments/assets/f480bb3b-b7ff-4010-8a87-de9c728b5dec" />

<img width="879" height="167" alt="Screenshot 2026-04-15 at 1 01 59 PM" src="https://github.com/user-attachments/assets/2ed52916-e7a0-4c6b-863c-668034f4f8c9" />

<img width="883" height="674" alt="Screenshot 2026-04-15 at 1 02 38 PM" src="https://github.com/user-attachments/assets/f6d6e4c2-a136-4e53-95b9-0ac662a9a522" />

- greg@tryhackme.thm


---


## MOTIVATION FOR ATTACKS

