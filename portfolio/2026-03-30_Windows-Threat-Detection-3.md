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


