# Detecting Initial Access in Linux Systems
**Date:** 2026-04-07

**Objective:**
- Understand the role and risk of SSH in Linux environments
- Learn how Internet-exposed services can lead to breaches
- Utilize process tree analysis to identify the origin of the attack
- Practice detecting Initial Access techniques in realistic labs


**Environment:** TryHackMe – “Linux Threat Detection 1” room  
**Tools Used:** CLI

---

## INITIAL ACCESS VIA SSH
> You can start from: cat /var/log/auth.log | grep "sshd"
### When did the ubuntu user log in via SSH for the first time?

Given the assistance, I would say to grep "ubuntu" and incorporate " | head", however, the result won't display the date in the desired format (YYYY-MM-DD):
<img width="2218" height="488" alt="image" src="https://github.com/user-attachments/assets/2c5c110c-6a41-4763-874b-394779314e37" />


So I know its October 22, however, the year isn't present, so I removed " | head" and searched for the next best thing regarding the ubuntu user:
<img width="2218" height="1064" alt="image" src="https://github.com/user-attachments/assets/6ea1ea16-82f5-45f7-b1d8-0b599887438e" />

The results are quite a lot and drags onto 2025, but as we can see, the month and day date are still the same

- 2024-10-22


##


### Did the ubuntu user use SSH keys instead of a password for the above found date? (Yea/Nay)

<img width="2214" height="1172" alt="image" src="https://github.com/user-attachments/assets/12079bb2-cffe-4543-b442-391105acea4d" />

Yes, it was up until the annotated date in the year 2026 that the ubuntu user used SSH keys instead of a password


---


## DETECTING SSH ATTACKS
>  try to uncover the breach that started via SSH password brute force. Continue with the /var/log/auth.log
### When did the SSH password brute force start?

Because it's asking about brute force, I'll look at the failed login attempts and see if I can spot a streak of failed attempts from a suspicious IP address

<img width="2222" height="1182" alt="image" src="https://github.com/user-attachments/assets/55fdecb4-17d7-47c7-815d-55bcc91cc844" />

Seen here, the suspicious IP attempts to SSH into the root account through different ports on 2025-08-21

##

### Which four users did the botnet attempt to breach?

<img width="2204" height="1148" alt="image" src="https://github.com/user-attachments/assets/9567abaa-f58d-4ce1-9e1a-cd9b276d9e18" />
- root, sol, roy, user

##

### Finally, which IP managed to breach the root user?

For this approach, I'll see what attempts were accepted

<img width="1421" height="771" alt="Screenshot 2026-04-11 at 5 01 59 PM" src="https://github.com/user-attachments/assets/3d878813-e86c-454f-afac-9aaa069d1cb8" />

Some things to note are:
- The IP address is different from many others (i.e, suspicious IP is external)
- The threat actor utilized a password to log in
- This external IP was seen making multiple failed attempts previously @ the root account


--- 


## INITIAL ACCESS VIA SERVICES
> Use /var/log/nginx/access.log on the VM to answer the questions. Analyze TryPingMe web logs to detect the attacker's actions.
### What is the path to the Python file the attacker attempted to open?

<img width="1440" height="413" alt="Screenshot 2026-04-11 at 5 23 01 PM" src="https://github.com/user-attachments/assets/5abb5412-ddc3-4558-892a-ca78c2f1f453" />
- /opt/trypingme/main.py

##

### Looking inside the opened file, what's the flag you see there?

<img width="1440" height="636" alt="Screenshot 2026-04-11 at 5 25 19 PM" src="https://github.com/user-attachments/assets/3390211d-17d5-4b9a-9c85-b0fc77265a8b" />
- THM{i_am_vulnerable!}


---


## DETECTING SERVICE BREACH
> Look at the TryPingMe breach from the previous task through the auditd angle. Use ausearch
### What is the PPID of the suspicious whoami command?

<img width="1424" height="221" alt="Screenshot 2026-04-11 at 6 06 07 PM" src="https://github.com/user-attachments/assets/b57e603d-0413-4fc3-ac77-ac936cd8b0d0" />

- 1018

##

### Moving up the tree, what is the PID of the TryPingMe app?

<img width="1424" height="494" alt="Screenshot 2026-04-11 at 6 09 27 PM" src="https://github.com/user-attachments/assets/60ff646b-93d6-4edd-960c-2c5b73f18cbd" />

- 577

**Process Chain**
1. whoami → find its ppid = the process that launched whoami was PID 1018
2. PID 1018 (ausearch --pid 1018) → /bin/sh -c "ping -c 2 ; whoami"
   - PID 1018 is a shell executing a command string
3. That shell → ppid = 577
   - (ausearch --pid 577)
5. PID 577 → python3 /opt/trypingme/main.py


##


### Which program did the attacker use to open a reverse shell?

<img width="1424" height="268" alt="Screenshot 2026-04-11 at 6 17 21 PM" src="https://github.com/user-attachments/assets/cfddc6de-5457-4258-ae72-c82b5d975229" />

- python


#### PPID and Proctitle Command Understanding
Initially, I had a hard time figuring out in what use case does this command apply (with everyone praising AI, it actually led me in circles and gave me semi-false information even with full process chain info), it wasn't until I used my own critical thinking to solve this question I had. The command in the recent question is searching for additional processes where the the PPID applies. So in this case, I initially searched for "whoami" which had a PPID of 1018. When I used ausearch to search for "ls", its PPID was 1021. 

Now to shift. Because they both have different PPIDs, it means that they are the child of a different process (which is the shell) ALTHOUGH the shell's process will take you back to when the python3 command was ran. The shell process is ONLY different because of the different commands it ran, but the shell will ALWAYS be a product of python3

<img width="1424" height="487" alt="Screenshot 2026-04-11 at 7 46 10 PM" src="https://github.com/user-attachments/assets/855aa1df-a707-49f8-92f7-369d0f4fff69" />

This is the best understanding that I can come up with that wont make me want to throw my brain in the garbage. 


---


## ADVANCED INTIAL ACCESS
