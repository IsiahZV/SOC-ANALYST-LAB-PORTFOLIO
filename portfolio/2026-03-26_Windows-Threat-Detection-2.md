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

