# Windows Threat Detection (Discovery)
**Date:** 2026-03-19

**Objective:**
- Detect common Discovery techniques using Windows Event Log
- Learn how to trace the attack origin by reconstructing a process tree
- Find out what data threat actors look for and how they exfiltrate it
- See how the malicious commands are logged by running them yourself


**Environment:** TryHackMe – “Windows Threat Detection 2” room  
**Tools Used:** Windows Event Viewer, Command Prompt

---

## DISCOVERY OVERVIEW

### Open CMD and type "net user Administrator".
> Which privileged group does the user belong to?

<img width="1431" height="607" alt="Screenshot 2026-03-26 at 4 50 50 PM" src="https://github.com/user-attachments/assets/64f91a29-886c-4b69-a413-f5a20c80b77c" />

- Administrator

##

### Open Event Viewer and try to find your command in Sysmon logs.
> What is the "Image" field of the net command you just run?

**Location of sysmon log (step-by-step):**
<img width="1431" height="739" alt="Screenshot 2026-03-26 at 4 54 15 PM" src="https://github.com/user-attachments/assets/777b0f48-291a-4c00-aab2-feb2dbda81bc" />

<img width="1440" height="703" alt="Screenshot 2026-03-26 at 5 00 11 PM" src="https://github.com/user-attachments/assets/9ca6b119-bb68-4e74-99a3-b6e8533f531d" />

> Double-click the sysmon file

<img width="839" height="271" alt="Screenshot 2026-03-26 at 5 05 30 PM" src="https://github.com/user-attachments/assets/02e2c92e-079f-497f-92da-db21908a18d6" />

> Double-click "Operational"

<img width="839" height="634" alt="Screenshot 2026-03-26 at 5 10 41 PM" src="https://github.com/user-attachments/assets/5d42e3c2-35fe-4649-bba8-b12c1af6bf2b" />

> The goal is to look for Event ID 1 (Process creation) and look out for something that would signal relation to the command ran. Unfortunately, around that time, so many processes ran when the VM was connecting / going in and out that there were a ton of Event ID's with the same value. For the sake of learning (lol), I ran it again when it settled.

<img width="1111" height="731" alt="Screenshot 2026-03-26 at 5 21 29 PM" src="https://github.com/user-attachments/assets/7fef50bb-0362-47dd-bb72-727c10a43e37" />

- C:\Windows\System32\net.exe

---

## DETECTING DISCOVERY
> For this task, open the VM and run a phishing attachment sample located at:

C:\Users\Administrator\Desktop\Practice\Task 3\invoice.pdf.exe

##

### Looking at Sysmon logs, what is the first command the invoice.pdf.exe executes?

<img width="994" height="787" alt="Screenshot 2026-03-26 at 6 15 02 PM" src="https://github.com/user-attachments/assets/97641c4a-3d55-4767-beaa-81aae1a3ffac" />

> Running suspicious attachment

Now that I've ran the malware, I have to access Event Viewer / Sysmon logs to uncover the process

Remember that commands that were ran are under Event ID 1

<img width="1440" height="787" alt="Screenshot 2026-03-26 at 6 21 07 PM" src="https://github.com/user-attachments/assets/df0bbf94-4086-4d65-97b4-ac6f7159aeb0" />

> Annotated are only traces of the file to display that multiple commands have been ran (I'm going to display the first command event next)

<img width="1120" height="787" alt="Screenshot 2026-03-26 at 6 28 15 PM" src="https://github.com/user-attachments/assets/a6278736-a51c-4629-b542-bdfa4bfaa62c" />

- @ 10:13:39, the file was first double-clicked

<img width="1117" height="787" alt="Screenshot 2026-03-26 at 6 34 15 PM" src="https://github.com/user-attachments/assets/4cb42358-26c4-4150-bcba-2ffd9e8bfde5" />

- Whoami

##

### Which command did the malware use to check the presence of MS Defender EDR?

As a sidenote, I see way more discovery commands ran such as: findstr, services, systeminfo
These are few of the many as I sift through for the command that checks for MS Defender

<img width="1117" height="787" alt="Screenshot 2026-03-26 at 6 42 02 PM" src="https://github.com/user-attachments/assets/c8f42fd9-f0a2-4505-8512-a786c10102dd" />

- @ 10:14:43, a command was ran to check for the presence for Crowdstrike EDR, then Carbon Black, then MS Defender

<img width="984" height="511" alt="Screenshot 2026-03-26 at 6 49 01 PM" src="https://github.com/user-attachments/assets/d2969277-df33-45b7-9824-f534a9f364c3" />

> Reference picture to show that MS Defender command may appear last in the search

<img width="1122" height="740" alt="Screenshot 2026-03-26 at 6 52 16 PM" src="https://github.com/user-attachments/assets/e1ec1178-0295-43d6-9f14-bcd6ca7ec13a" />

>MS Defender did come last of conducted EDR search

- cmd /c "tasklist /v | findstr MsSense.exe || echo No MS Defender EDR"

##

