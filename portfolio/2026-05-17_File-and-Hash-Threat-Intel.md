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
