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
