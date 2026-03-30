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

### To which domain did the malware send the discovered data?

<img width="854" height="740" alt="Screenshot 2026-03-29 at 4 25 46 PM" src="https://github.com/user-attachments/assets/34ff5551-a10f-4860-b8b5-db15cc5e2563" />

After assessing if any of the specific EDR tools were present in the system, it then communicates with:
- exfil.beecz.cafe

---

## COLLECTION OVERVIEW

### What is the Facebook password that the user saved in Chrome?

<img width="1440" height="572" alt="Screenshot 2026-03-29 at 5 18 00 PM" src="https://github.com/user-attachments/assets/baf2de52-1d1e-4375-828c-ffdcbc209aed" />

- nsAghv51BBav90!

##

### Which interesting SSH key does the user store on disk?

<img width="977" height="512" alt="Screenshot 2026-03-29 at 5 22 32 PM" src="https://github.com/user-attachments/assets/59423771-6132-4a79-822a-5bd8230f358f" />

- thm-access-database.key

##

### What is the secret PDF file explaining TryHackMe's internal network?

<img width="1126" height="608" alt="Screenshot 2026-03-29 at 5 31 12 PM" src="https://github.com/user-attachments/assets/603239d9-b21b-404f-8bf2-5285df4ae00d" />

- thm-network-diagram-2025.pdf

##

### 📄 Summary
What this section emphasizes that sensitive information is typically stored on a users computer and therefore displaying the search methods (i.e. Accessing a browser-stored password from the GUI, finding a SSH key through the terminal / command prompt, and looking through the files in the computer to find network information. 

---

## DETECTING COLLECTION
> For this task, run a simple data stealer sample and analyze its actions in logs:
C:\Users\Administrator\Desktop\Practice\Task 5\stealer.exe

##

### Looking at Sysmon logs, what directory does the stealer create?

> Running the stealer sample
<img width="1242" height="681" alt="Screenshot 2026-03-29 at 6 24 32 PM" src="https://github.com/user-attachments/assets/fb735e60-69ec-4eb7-893e-cc043509eddd" />

<img width="854" height="681" alt="Screenshot 2026-03-29 at 6 27 51 PM" src="https://github.com/user-attachments/assets/90876beb-a6cf-47c8-9884-0fd92b5bd535" />

Here is where I first ran the executable
- ProcessID: 5680


In the event **right** after, commands are prompted:
- ParentProcessID: 5680
<img width="1058" height="681" alt="Screenshot 2026-03-29 at 6 30 22 PM" src="https://github.com/user-attachments/assets/cd1dca5f-c156-4c38-9c1c-7701a736fa39" />

In the command line row, I see "mkdir" which tells me that a directory is being created in the temp file
- staging_58f1

##

### Which three file extensions does the malware search for?

For this, a more thorough search has to be conducted since the component that prompts the host to search is in the command

> docx
<img width="847" height="681" alt="Screenshot 2026-03-29 at 6 46 11 PM" src="https://github.com/user-attachments/assets/087bd9bb-fd7a-448e-9a5a-10de5b44065f" />


>pdf
<img width="847" height="681" alt="Screenshot 2026-03-29 at 6 46 46 PM" src="https://github.com/user-attachments/assets/2b500f02-dbe0-48c5-a86e-d28380d0573b" />

>xlsx
<img width="847" height="681" alt="Screenshot 2026-03-29 at 6 47 31 PM" src="https://github.com/user-attachments/assets/4382cfe8-21a9-41cc-9bec-ab8b39201c00" />

- The three file extensions are docx, pdf, and xlsx

##

### Which PowerShell cmdlet does the malware use to get clipboard content?

This question will prompt be to search in the earlier phases when the malware was establishing itself and utilizing powershell:

<img width="1083" height="645" alt="Screenshot 2026-03-29 at 6 53 46 PM" src="https://github.com/user-attachments/assets/2e62f611-3e4c-45d0-b13c-553caf336bfb" />

- Get-Clipboard

##

### Which domain does the malware exfiltrate the data to?

Now its time to look for DNS queries / Event ID 22

<img width="846" height="645" alt="Screenshot 2026-03-29 at 6 57 08 PM" src="https://github.com/user-attachments/assets/e20e9cc5-3b2d-4045-85ab-18166f8ca427" />

- collecteddata-storage-2025.s3.amazonaws.com

---

## INGRESS TOOL TRANSFER
> For this task, continue with the VM and test the Ingress Tool Transfer

##

### Open the Chrome browser on the VM and navigate to the URL. What is the flag in the response?

<img width="877" height="318" alt="Screenshot 2026-03-29 at 8 28 36 PM" src="https://github.com/user-attachments/assets/25da4ca6-d167-442a-b9e6-1e520b8e22f2" />

<img width="483" height="243" alt="Screenshot 2026-03-29 at 8 33 45 PM" src="https://github.com/user-attachments/assets/843bda82-637e-433e-b967-052ef5e93b52" />

- THM{just_use_web_browser}

##

### Next, open CMD and download the file from the same URL using curl.exe.

<img width="1192" height="643" alt="Screenshot 2026-03-29 at 8 35 54 PM" src="https://github.com/user-attachments/assets/f4b8c895-be35-4d65-a265-7530f3144f07" />

<img width="1192" height="643" alt="Screenshot 2026-03-29 at 8 36 12 PM" src="https://github.com/user-attachments/assets/45789d48-4db2-4e5f-873f-122713685451" />

- THM{curl_is_cool}

##

### Continue with the same CMD and URL, but now using certutil.exe.

<img width="1192" height="643" alt="Screenshot 2026-03-29 at 8 51 40 PM" src="https://github.com/user-attachments/assets/1ff3ee01-4f3d-4196-8417-310acc3b76ef" />

- THM{abusing_certutil}

##

### Finally, download the same file using PowerShell IWR.

<img width="1192" height="643" alt="Screenshot 2026-03-29 at 8 59 37 PM" src="https://github.com/user-attachments/assets/51d67a22-f78b-457f-b792-43764967acea" />

- THM{power_of_powershell}

---
