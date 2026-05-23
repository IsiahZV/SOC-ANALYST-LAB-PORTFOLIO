# File and Hash Analysis
**Date:** 2026-05-17

**Objective:**
- Interpret suspicious filepaths and filenames using heuristics.
- Generate and validate file hashes.
- Leverage VirusTotal and MalwareBazaar to enrich newly observed binaries.
- Extract behaviour from sandbox telemetry and map it to MITRE ATT&CK.

---

### Scenario

It is a Monday in April. Try Daily is preparing a significant release. The EDR tool flags multiple binaries on various workstations during a routine alert sweep. You receive a curated triage package containing those samples. Within 60 minutes, you must provide evidence to showcase whether these files are bait, benign, or malicious.

##

Once the VM loads, you will find:
- Case files on the Desktop, ready for analysis.
- TryDetectThis — accessible via the Desktop shortcut or by navigating to http[://]TryDetectThis[.]thm:8080 in the browser. Use this platform to search for hashes, review vendor detections, inspect file properties, and analyse sandbox behaviour.

---

## FILENAMES AND PATHS

### One of the files included in the CTI Files folder on the Desktop shows one of the indicators mentioned. Can you identify the file and the indicator?

To figure this out, I began by viewing the files properties to see if I can catch any easy indicators

<img width="2256" height="1194" alt="image" src="https://github.com/user-attachments/assets/c836e641-d6da-49bc-9699-c5365da277f8" />

<img width="1704" height="992" alt="image" src="https://github.com/user-attachments/assets/082ced92-85a7-41e1-87c9-9a707be5215b" />

The name of the file is:
- payroll.pdf

Indicator:
- Double extensions

---

## FILE HASH LOOKUP

### What is the SHA256 hash of the file bl0gger?

I'll be utilizing the command prompt to retrieve the hash of the given file:

<img width="1570" height="844" alt="image" src="https://github.com/user-attachments/assets/3349e84d-a83a-429c-8430-dce9c59ee90f" />

- 2672b6688d7b32a90f9153d2ff607d6801e6cbde61f509ed36d0450745998d58


##


### What is the threat classification label used to identify the malicious file?

Typically, I would resort to using VirusTotal but because of the ever changing landscape, the lab provided its own static database for consistency purposes

<img width="2232" height="1120" alt="image" src="https://github.com/user-attachments/assets/af9f7eac-5d1f-49d2-909f-81ab09c65cc9" />

<img width="2234" height="1114" alt="image" src="https://github.com/user-attachments/assets/6324019c-1841-4958-ac2a-7570a2e88c3e" />

- trojan.graftor/flystudio


##


### When was the file first submitted for analysis? (Answer format: YYYY-MM-DD HH:MM:SS)

<img width="2238" height="1120" alt="image" src="https://github.com/user-attachments/assets/e3fed7bd-880e-405d-b6b2-b2ffefca51eb" />

- 2025-05-15 12:03:49


##


### Which vendor classified the Morse-Code-Analyzer file as non-malicious?

Morse-Code-Analyzer is another file in the CTI folder. For this, I'll repeat the same process that I used to obtain the SHA25 hash of the bl0gger file

> Copy the hash
<img width="1810" height="596" alt="image" src="https://github.com/user-attachments/assets/50e122f0-e1df-4713-a058-6c54dd3deb0f" />

> Paste into search
<img width="2202" height="228" alt="image" src="https://github.com/user-attachments/assets/72594623-5ea7-4a50-8068-9691dbd89db6" />

<img width="2238" height="1112" alt="image" src="https://github.com/user-attachments/assets/844e79e7-5e02-4c85-9291-1b2f9aa3123f" />

- CyberFortress


##


### What MITRE technique has been flagged for persistence and privilege escalation for the Morse-Code-Analyzer file?

<img width="1112" height="490" alt="image" src="https://github.com/user-attachments/assets/5d1c0c87-d9d9-4f8f-9de6-cc395274aaac" />

- DLL Side-Loading


---


## SANDBOX ANALYSIS

### What tags are used to identify the bl0gger.exe malicious file on Hybrid Analysis?
> (Answer: Tag1, Tag2, Tag3)

I'm going to copy the hash value found earlier and paste it into Hybrid Analysis

<img width="2236" height="1056" alt="image" src="https://github.com/user-attachments/assets/efe03c6f-0cd4-40da-96ae-b7b5f1d8f0ef" />

<img width="2228" height="1316" alt="image" src="https://github.com/user-attachments/assets/cce7a685-3ce4-4bfc-9afc-bb915b19015a" />

Currently, as of May 18, 2026, there are 3 results for this hash. I've picked the result containing the name of the file (as I'm sure it goes by others given the results). 

The question is ambiguous as always so it leads the user to guess which tags (#) are relevant.
They are:
- BlackMoon, Discovery, windows-server-utility


##


### What was the stealth command line executed from the file?
Now, I'll click on that result that I've chosen in the previous question and see what information I can gather from it

<img width="2252" height="1176" alt="image" src="https://github.com/user-attachments/assets/912447a5-f797-4ca0-bc77-a65b08ede50e" />

- regsvr32 %WINDIR%\Media\ActiveX.ocx /s


##


### Which other process was spawned according to the process tree?

At this point, I had to pivot to the VM specific application due to changes occuring in Hybrid Analysis

<img width="2218" height="1114" alt="image" src="https://github.com/user-attachments/assets/b41ead58-9b3f-4407-b3cd-939e79b18b90" />

- werfault.exe


##


### Analyze the payroll.pdf file located in the CTI Files folder and answer the questions below. The payroll.pdf application seems to be masquerading as which known Windows file?

<img width="1996" height="888" alt="image" src="https://github.com/user-attachments/assets/e4312648-cdea-429a-9502-038310d088cb" />

- svchost.dll


##


### What associated URL is linked to the file?

For all things associated with network connections via IP / URL, I can likely find its information in the network tabs

<img width="1052" height="474" alt="image" src="https://github.com/user-attachments/assets/de8b542a-989a-489b-b4e7-226a3dc65faa" />

- hxxp://121.182.174.27:3000/server.exe


##


### How many extracted strings were identified from the sandbox analysis of the file?

<img width="1042" height="510" alt="image" src="https://github.com/user-attachments/assets/558fa050-4a18-4c67-928d-ae63b17b0037" />

- 454

---

## THREAT INTELLIGENCE CHALLENGE

> After completing the prior tasks pertaining to threat intelligence via file attributes, I am now tasked with investigating one final file "Challenge.bin.sample"

### What is the SHA256 hash of the file?

<img width="1696" height="898" alt="image" src="https://github.com/user-attachments/assets/3a48a1b2-6011-4ff4-b77e-cf136bf2ea43" />

- 43b0ac119ff957bb209d86ec206ea1ec3c51dd87bebf7b4a649c7e6c7f3756e7


##


### What family labels are assigned to the file on VirusTotal?

You wont find both answers in the same place on VirusTotal so the attached lab source has to be utilized (the question should be updated).

<img width="2234" height="1126" alt="image" src="https://github.com/user-attachments/assets/9945f81c-ea10-4de1-bb73-eb2ef491cd1f" />

- akira, filecryptor

**Why this matters:**
- Doing this (identifying malware family's from a malicious file) helps you understand the threat behavior due to the malware family sharing code, techniques, and objectives which helps make incident response quicker.
- Identifying family's help prioritize severity by understanding the type of impact that could occur, reporting requirements, urgency, and more
  - Commodity = standard remediation
  - Ransomware = legal, exec, LE escalation
- Improves detection engineering by allowing teams to create YARA rules, configure SIEM alerts and EDR detections

- Limitation:
  - Different vendors use different labels for the same malware so analysts have to validate by correlating behavior, infrastructure, ATT&CK mappings, and sandbox results


##


### When was the first time the file was recorded in the wild? (Answer Format: YYYY-MM-DD HH:MM:SS UTC)

Now you have to shift back to VirusTotal which they don't tell you as well since the ITW date is different than whats posted in VT

<img width="2252" height="1236" alt="image" src="https://github.com/user-attachments/assets/ded0d1aa-4b1c-40ed-b6e2-4062826f97c7" />

- 2024-10-30 17:17:24 UTC


##


### Name the text file dropped during the execution of the malicious file.

<img width="2240" height="1124" alt="image" src="https://github.com/user-attachments/assets/740aaa62-02a3-4af4-b328-533b1afd173c" />

- akira_readme.txt

**Why this matters:**
- This is important because dropped files can reveal malwares true functionality, its persistence mechanisms, payloads, and scope of compromise while the original file is just the "loader".
- The dropped files are any file written to the disk by malware after execution that can appear to be executables, DLLs, scripts, scheduled task files, config files, notes, and more
  - Typically found in locations like %AppData%, %Temp%, System32, startup folders, and hidden directories


##


### What PowerShell command is observed to be executed?

<img width="2236" height="1116" alt="image" src="https://github.com/user-attachments/assets/d8574a06-81fa-49e4-a92b-bdcb68ab16e5" />

- Get-WmiObject Win32_Shadowcopy | Remove-WmiObject

**Why this matters:**
- Because powershell is one of th most commonly abused tools, the commands reveal the attackers intent, capabilities, persistence methods, lateral movements, and objectives
- Also used for fileless malware, where instead of writing malware to the disk, powershell downloads payloads into memory, injects codes into processes, executes encoded scripts, and more.
  - Because of this, anti-virus doesnt see a file, much artifacts arent left behind after reboot. Sometimes powershell commands may be the only primary evidence


##


### What MITRE ATT&CK ID is associated with the actions of the command?

For this, reference the previous question's screenshot and read the discription below which states that the use of the command is: "Deletes all Volume Shadow Copies to prevent system restore and file recovery"

<img width="2220" height="1104" alt="image" src="https://github.com/user-attachments/assets/8b2e1027-c75e-4784-8b38-1c39bb28f95a" />

The key is that the purpose is to prevent a function of the endpoint

- T1490


---
