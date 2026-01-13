## Report Structure

- **Who:** Which user logs in, runs the command, or downloads the file
- **What:** What exact action or event sequence was performed
- **When:** When exactly did the suspicious activity start and ended
- **Where:** Which device, IP, or website was involved in the alert
- **Why:** the reasoning for your final verdict


## Time & Prioritization
- When dealing with alerts, prioritize the **most critical severity** alert and work down to the lowest
- Prioritize alert by earliest time


### Suggestions

- Allow people to understand your report in under 30 seconds
  - Alert type (phishing, malware, suspicious PowerShell, etc.)
  - A one-line description of what happened
  - Impact level (single host, single user, potential lateral movement)
  - Your verdict (True Positive / False Positive / Benign Positive)

## Who
**Which user, account, or process was responsible for the activity**

- **Include:**
  - Username (UPN if available)
  - Hostname / Endpoint name
  - User role or department (if relevant)
  - Logged-in account vs SYSTEM vs service account
  - Parent process owner (for scripts/executables)

- **Example entries:**
  - User: jdoe@company.com
  - Host: HR-LT-023
  - Process Owner: NT AUTHORITY\SYSTEM


## What
**What exactly occurred — actions, behaviors, and alert details**

- **Include:**
  - Alert name and severity
  - Event sequence (high-level)
  - Process names and command-line arguments
  - File names and file paths
  - Parent → child process relationships
  - Network activity observed (connections, downloads)
  - MITRE ATT&CK technique (if identified)

- **Example entries:**
  - PowerShell executed with encoded command 
  - Reverse shell connection initiated
  - Suspicious DNS request to newly registered domain
  - Alert: Suspicious PowerShell Execution (High)


## When
**Precise timing of events (consistency is critical)**

- **Include:**
  - Alert trigger time
  - Start and end time of activity
  - Time zone used (UTC or Local)
  - Chronological event ordering

- **Example entries:**
  - 10:14 – Phishing email delivered
  - 10:16 – Attachment opened
  - 10:17 – PowerShell executed
  - 10:18 – DNS request observed


## Where
**Source and destination of the activity**

- **Include:**
  - Endpoint name and IP address
  - Source IP / Destination IP
  - Domains and URLs contacted
  - File locations on disk
  - Email sender domain (for phishing)
  - Geographic location (if notable)

- **Example entries:**
  - Endpoint IP: 10.0.5.23
  - Destination IP: 185.xxx.xxx.14
  - Domain: update-check[.]xyz
  - File Path: C:\Users\jdoe\Downloads\invoice.pdf.exe


## Why
**Your analysis, verdict, and reasoning (L1-appropriate)**
**- Include:**

- Final verdict:
  - True Positive / False Positive / Benign Positive / Needs Escalation
  - Reasoning based on observed evidence
  - Known indicators of compromise
  - Scope assessment (single host vs multiple)
  - Confidence level (Low / Medium / High)
  - Recommendation or escalation justification

- **Example entries:**
  - Verdict: True Positive
  - Reasoning:
    - Encoded PowerShell execution
    - Outbound connection to suspicious external IP
    - Domain recently registered
    - Scope: Limited to one endpoint
    - Confidence Level: Medium
    - Recommendation: Escalate to L2 for containment and credential reset


## Optional
**Indicators and Artifacts**
- File hashes (SHA256)
- IP addresses
- Domains / URLs
- Email subjects and attachment names
