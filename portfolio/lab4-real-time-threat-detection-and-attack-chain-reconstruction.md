# Real-Time Threat Detection and Attack Chain Reconstruction

**Date:** 2026-01-13

**Objective:**
- Monitor and analyze real-time alerts as the attack unfolds.
- Identify and document critical events such as PowerShell executions, reverse shell connections, and suspicious DNS requests.
- Create detailed case reports based on your observations to help the team understand the full scope of the breach.

**Environment:** TryHackMe – “Phishing Unfolding” lab  
**Tools Used:** Elastic


### 📄Personal Assisting Documentation

[SOC Report Guide](/template/soc-analyst-report-template.md)


## Documentation

<img width="1440" height="601" alt="Screenshot 2026-01-13 at 3 49 26 PM" src="https://github.com/user-attachments/assets/00093d46-76ed-43c1-9fe5-ff9767f59dc6" />


### Alert 1000
<img width="1168" height="705" alt="Screenshot 2026-01-13 at 3 51 15 PM" src="https://github.com/user-attachments/assets/ab02e8ee-c9ee-4dd9-9ccb-9742c58fd57a" />

The first alert (1000) appears to be an obvious phishing attempt. From the subject alone, I can strongly assume that this is a true positive. 

Since the time between receiving those alerts are long, I'll asign alert 1000 to myself and begin my report.

"On 01/13/2026 20:47:22 UTC, the recipient "support@tryhatme.com" receives an email from "eileen@trendymillineryco.me". The email contains information regarding an unknown person and inheritance in order to urge the user to enter their banking details. This is a likely phishing attempt as this email lacks content of support services. Escalate to L2 to block sender email account."


## Alert 1002

This alert contains a description noting "Suspicious Parent Child Relationship".

<img width="1422" height="690" alt="Screenshot 2026-01-13 at 4 11 17 PM" src="https://github.com/user-attachments/assets/cced02da-b24e-4cdd-be15-0b749c5cc9e7" />

- Who: host.name: win-3459 - Michelle Smith
- What: Uncommon process creation where services.exe (parent -> Windows service control manager) spawns TrustedInstaller.exe (child -> servicing component used for updates, installations, and cleanup).
- When: 01/13/2026 20:49:46
- Where: C:\Windows\system32\
- Why: Typical OS behavior

In short, there are no signs of masquerading, C2 attempts, malware execution, or priviledge escalation.

"On 2026-01-13 at 20:49:46 UTC, host win-3459 associated with user Michelle Smith (michelle.smith@tryhatme.com) displayed normal Windows operating system behavior. The Service Control Manager (services.exe) spawned TrustedInstaller.exe from the legitimate Windows servicing directory (C:\Windows\servicing) with a working directory of C:\Windows\System32. No additional malicious activity was observed. The alert was classified as a false positive."

**Note:**
- Attackers masquerade legitimate names in wrong locations (i.e., C:\Users\Public\TrustedInstaller.exe). The correct name and directory strengthens the legitimacy, vice versa.
- The working directory(\system32\) and binary path (C:\Windows\servicing\) relate because system32 is the default working directory for Windows services.
- Think: Working directory for context and binary for where and what was executed due to the context.
  - Working directories that contain \Downloads\ and \temp\ usually are more telling in a malicious case
