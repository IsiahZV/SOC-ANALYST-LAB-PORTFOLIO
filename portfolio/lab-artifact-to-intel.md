# Artifact to Intel
**Date:** 2026-05-24

**Objective:**
- Extract insight from a set of flagged artefacts, and distil the information into usable threat intelligence.

---

### Scenario:

You are an SOC analyst on the SOC team at Managed Server Provider TrySecureMe. Today, you are supporting an L3 analyst in investigating flagged IPs, hashes, URLs, or domains as part of IR activities. One of the L1 analysts flagged two suspicious findings early in the morning and escalated them. Your task is to analyse these findings further and distil the information into usable threat intelligence.

- Flagged IP: 101[.]99[.]76[.]120
- Flagged SHA256 hash: 5d0509f68a9b7c415a726be75a078180e3f02e59866f193b0a99eee8e39c874f

---

### What is the name of the file identified with the flagged SHA256 hash?

I'll begin by searching the hash in VirusTotal

<img width="1440" height="613" alt="Screenshot 2026-09-13 at 6 18 15 PM" src="https://github.com/user-attachments/assets/6f121869-3df8-4ca2-8a3e-5c31a284c956" />

- syshelpers.exe

##

### What is the file type associated with the flagged SHA256 hash?

<img width="1123" height="455" alt="image" src="https://github.com/user-attachments/assets/8d95b716-5d28-4175-a6f5-1fef72a9d7f5" />

- Win32 EXE

##

### What are the execution parents of the flagged hash? List the names chronologically, using a comma as a separator. Note down the hashes for later use.

This information can be found in the "Relations" tab

<img width="1119" height="547" alt="image" src="https://github.com/user-attachments/assets/68c2dca4-0219-4f91-85ec-3e54477db1c9" />

- 361GJX7J: 047c5eec0445746862710d20e50a5dd04510b7e625fa5c1f5d48ce078001c0de
- installer.exe: fa102d4e3cfbe85f5189da70a52c1d266925f3efd122091cdc8fe0fc39033942

##

### What is the name of the file being dropped? Note down the hash value for later use.

This information can also be found under the "Relations" tab

<img width="1076" height="265" alt="image" src="https://github.com/user-attachments/assets/c18c78b3-e2e8-421b-95a5-9e67ac1d159b" />

##

### Research the second hash in question 3 and list the four malicious dropped files in the order they appear (from up to down), separated by commas
> The hash of interest is: 047c5eec0445746862710d20e50a5dd04510b7e625fa5c1f5d48ce078001c0de

I believe reporting on specific files in a certain order is counter intuitive to this question and the analysis whether conduction online or with the provided threat intelligence search application as the answer will resolve itself anyway.

<img width="1070" height="388" alt="image" src="https://github.com/user-attachments/assets/9a5abf87-5384-41b9-a057-ac8b8576ec33" />

- searchhost.exe,syshelpers.exe,nat.vbs,runsys.vbs

##

### Analyze the files related to the flagged IP. What is the malware family that links these files?

<img width="1099" height="551" alt="image" src="https://github.com/user-attachments/assets/57b57ecf-3e2e-409a-a20e-a0765eaf99a1" />

- AsyncRAT

##

### What is the title of the original report where these flagged indicators are mentioned? Use Google to find the report.

<img width="1019" height="356" alt="image" src="https://github.com/user-attachments/assets/b7ceda53-1d54-4b85-b17a-8ba39f18086f" />

<img width="727" height="627" alt="image" src="https://github.com/user-attachments/assets/ab345af7-15ea-4b72-b1fc-a6120ef8c5e2" />

- From Trust to Threat: Hijacked Discord Invites Used for Multi-Stage Malware Delivery

##

### Which tool did the attackers use to steal cookies from the Google Chrome browser?

<img width="656" height="411" alt="image" src="https://github.com/user-attachments/assets/aa476add-f36d-4c25-8bcd-e682bb67fb03" />

- ChromeKatz

##

### Which phishing technique did the attackers use? Use the report to answer the question.

<img width="653" height="554" alt="image" src="https://github.com/user-attachments/assets/3cef0d46-a89c-4376-9e50-ec839fc1aaec" />

- ClickFix

##

### What is the name of the platform that was used to redirect a user to malicious servers?

- Discord

---
