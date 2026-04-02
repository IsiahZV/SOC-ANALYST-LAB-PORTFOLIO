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

Here, there are two files; security and sysmon. Both files can be utilized to uncover the answer, so I believe its to assess the approach between more than one available resource. 

One thing to note is that the two logs also differ in "all time" and "after reboot". 

After searching through the symon logs, I was unable to find the exact piece that would solve this question, however, I was able to see the typical suspicous behavior / commands being ran so I shifted to the security event log.

<img width="1404" height="1030" alt="image" src="https://github.com/user-attachments/assets/60ab8bda-855e-44f6-8429-e7716e615338" />

To begin, I've filtered for Event ID 4697 (service creation)

<img width="1378" height="922" alt="image" src="https://github.com/user-attachments/assets/0879ea6c-5fec-4025-9091-241418ddd8f6" />

After scrolling through the security log, I've come to learn that most processes that are normal behavior are MS windows / ede updates and services as well as Google services. One that stuck out was "nessie.exe" or the "Data Protection Service".

- Data Protection Service

##

### Which scheduled task was created to persist the Troy malware?

While still using the security event log, I now believe that understanding the baseline of normal activities of the boot process should be emphasized. Some things to note are that you will typically see events such as GoogleUpdater, MSEdge Update / Browser related updates

<img width="1374" height="852" alt="image" src="https://github.com/user-attachments/assets/f5911a8f-ceaa-4361-bad0-900b78fdc4a8" />

<img width="1364" height="692" alt="image" src="https://github.com/user-attachments/assets/96a2df44-38fd-4f11-86d1-6618a18b598b" />

- AmazonSync

##

### What flag do you get after finding and running the Troy malware?

Referencing the previous screenshots, the troy.exe file can be found in: C:\Program Files\ Common Files

<img width="1784" height="962" alt="image" src="https://github.com/user-attachments/assets/fb51f275-1e2c-4622-a376-6c68931b0c6e" />

Then it asks me to solve a question about the parent commandline which can be found in the sysmon log

<img width="1490" height="788" alt="image" src="https://github.com/user-attachments/assets/63f43c42-fbc9-44f6-b8e2-de7b10e3050e" />

<img width="1368" height="1002" alt="image" src="https://github.com/user-attachments/assets/4098ad85-3053-4e69-9fbf-eda8a29937ad" />

- C:\Windows\system32\svchost.exe -k netsvcs -p -s Schedule

<img width="1474" height="778" alt="image" src="https://github.com/user-attachments/assets/602f245c-9c77-4c98-ad87-abf94c18ed74" />

- THM{c2_is_on_schedule!}

---

## PERSISTENCE: RUN KEYS AND STARTUP

> Use all the tools you know to uncover the newly learned backdoors in: C:\Users\Administrator\Desktop\Practice\Task 5\

##

### What is the parent process image of the "Odin" malware?

> Available files
<img width="1710" height="906" alt="image" src="https://github.com/user-attachments/assets/d8e3be86-0b45-4797-8d0f-fa6432f03cd8" />

Finding images and command line and the like are typically found in the sysmon logs (Which I'm guessing they both are) and in this case, Event ID 1 will show events for process creation

<img width="1378" height="1020" alt="image" src="https://github.com/user-attachments/assets/23d078d9-e476-42b5-808d-940eb8335fc0" />

- C:\Windows\explorer.exe

##

### What is the last line that the "Odin" malware outputs?

<img width="2234" height="1134" alt="image" src="https://github.com/user-attachments/assets/fdb07cf4-6106-443a-9a53-c2715319b096" />

Here, I'm just annotating that the current event is the child process to the preceding event that's seen in the previous question (reconstruction purposes)

- Done doing bad stuff!

##

### What flag do you get after finding and running the "Kitten" malware?

<img width="1366" height="700" alt="image" src="https://github.com/user-attachments/assets/3a791755-864d-44fd-b921-8afff813cb1b" />

Kitten.exe can be found in: C:\Users\Public

<img width="1790" height="1020" alt="image" src="https://github.com/user-attachments/assets/0d5fc273-42df-494d-94e5-93a78db27d5f" />

Now I have to search for the name of the run key added where they're typically stored

<img width="1274" height="314" alt="image" src="https://github.com/user-attachments/assets/be083d9f-3c80-43b7-bb06-2271fc88257f" />

<img width="1280" height="286" alt="image" src="https://github.com/user-attachments/assets/aa8414b3-e318-4d75-9be0-3d00df718245" />

- Basket

<img width="1478" height="276" alt="image" src="https://github.com/user-attachments/assets/6cd9ad22-7926-4ce3-8543-97c5b0aa1fc2" />

- THM{persisting_in_basket!}

--- 
