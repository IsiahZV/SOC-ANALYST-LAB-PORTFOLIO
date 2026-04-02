# Windows Threat Detection (Persistence / Command and Control)
**Date:** 2026-03-19

**Objective:**
- Learn why and how threat actors maintain control of their victims
- Use Windows event logs to uncover various persistence methods


**Environment:** TryHackMe – “Windows Threat Detection 3” room  
**Tools Used:** Windows Event Viewer, Command Prompt

---

## COMMAND AND CONTROL
> detect the C2 setup using Sysmon logs:
C:\Users\Administrator\Desktop\Practice\Task 2\Sysmon.evtx

## 

### Which suspicious archive did the user download?

<img width="1083" height="645" alt="Screenshot 2026-03-29 at 6 53 46 PM" src="https://github.com/user-attachments/assets/413c6673-7f4f-4138-87aa-dd3f360ddbb2" />

- Screenshot annotated for better understanding / reconstruction
- URGENT!.zip

##

### Where did the attackers hide the C2 malware file?

I want to do a little reconstruction before talking about the C2 file and because there was too many events that led up to that moment, I'll try and explain it as best as possible.

User: Administrator

- User downloads URGENT!.zip
  - Zip contains malicious .lnk file
    - User opens ZIP (which shows explorer.exe activity in logs)
      - User executes .lnk
        - .lnk launches powershell
          - Powershell connects to: http[:]//maliciousIp/update[.]exe 
> C2 / Staging
- Payload "update.exe" is downloaded
  - Malware writes files to: \Temp\2\ (staging)
    - \AppData\Roaming (exec. / persistence) -> This file contains a realistic looking file name "PSScriptPolicyTest..."

So the malware hid two files, but the one the question is asking for is in \AppData\Roaming

<img width="877" height="626" alt="Screenshot 2026-03-30 at 6 45 51 PM" src="https://github.com/user-attachments/assets/77a4a5e4-6852-4d60-a0c4-5eb72e673b91" />

##

### What is the domain of the Command and Control server?

By now, it should be known that when domains are queried, Event ID 22 (dns query) becomes the focus

<img width="877" height="626" alt="Screenshot 2026-03-30 at 6 48 39 PM" src="https://github.com/user-attachments/assets/7a32a473-5a5d-44ad-a212-2da4000cde3f" />

- route.m365officesync.workers.dev

---

## PERSISTENCE OVERVIEW

> For this task, try to detect a persistence via the backdoored user account:
C:\Users\Administrator\Desktop\Practice\Task 3\Security.evtx

##

### How many times did the threat actor fail to log in to the Administrator?

To establish, failed logon attempts are Event ID 4625

<img width="883" height="635" alt="Screenshot 2026-04-01 at 8 28 29 PM" src="https://github.com/user-attachments/assets/50f7430b-dca8-4609-84a6-2d49059c5ec3" />

Here, from 8:58:05 - 9:00:20, there have been 6 failed attempts to acces the Administrator account

##

### After the successful login, which backdoor user did the attacker create?

<img width="883" height="635" alt="Screenshot 2026-04-01 at 8 34 01 PM" src="https://github.com/user-attachments/assets/99c13c59-b4a2-4c14-8960-39ee76570fed" />

- @ 9:00:25, the Administrator account was successfully logged into (Event ID 4624)

In between, theres activity where accounts like "winlogon" and "UserManager" have obtained trusted logon process
The attacker accesses the Administrator VM, where the source IP is: 10.14.97.15
A user was also added to security-enabled groups 

- Event ID 4720 is about creating user accounts which we see happening @ 9:01:38

<img width="883" height="635" alt="Screenshot 2026-04-01 at 8 50 18 PM" src="https://github.com/user-attachments/assets/61450094-64be-4215-88ca-acba281df658" />

This account was created under the guise of "support"

##

### Which privileged group was the backdoor user added to?

<img width="883" height="635" alt="Screenshot 2026-04-01 at 9 15 33 PM" src="https://github.com/user-attachments/assets/1e7787dd-4b54-47a6-b0e8-62bf0770bb06" />

This new support account that was made by the obviously breached Administrator account assigned the user to:
- Administrators

---

## PERSISTENCE: TASKS AND SERVICES

> The attackers left two backdoors and restarted the system.
Can you uncover them all using Security and Sysmon logs?
C:\Users\Administrator\Desktop\Practice\Task 4\

##

### Which Windows service was created to persist the Nessie malware?

<img width="1152" height="364" alt="Screenshot 2026-04-02 at 10 42 47 AM" src="https://github.com/user-attachments/assets/62245432-6340-4cbd-922a-0d631692b68c" />

Here, there are two files; security and sysmon. Both files can be utilized to uncover the answer, so I believe its to assess the approach between more than one available resource. Personally, because I get to choose, I do like sysmon logs.

One thing to note is that the two logs also differ in "all time" and "after reboot". 
