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

