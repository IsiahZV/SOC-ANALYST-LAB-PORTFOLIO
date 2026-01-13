# Windows Low Severity Alert Triage & Cheat Sheet
The working directory provides immediate execution context and helps identify user-writable or commonly abused locations before validating binary legitimacy.

For low-severity and process creation alerts, ask in this order:

## Order Flow
### Working Directory
This answers "From what environment is this process operating", suspicious working directories expose intent faster
- i.e.,:
  - C:\Users\*\Downloads\
  - C:\Users\*\AppData\Temp\
  - C:\Windows\Temp\

- Is it system-level or user-writable?
- Does it make sense for this type of process?

If it’s Downloads, Temp, or AppData → pause immediately


### Binary Path
- Is the executable located where it should be?
- Does the directory match the process name?

- i.e.,:
  - svchost.exe in C:\Windows\System32\ → normal
  - svchost.exe in C:\Users\Public\ → 🚩


### Parent Process
- Is the parent expected for this binary?
- User-launched vs system-launched?


### Command-Line Arguments
- Encoded?
- Obfuscated?
- LOLBin abuse?


### Network Activity
- External connections?
- Non-standard ports?
- Rare destinations?


##


### Use this when reviewing process creation alerts, Sysmon logs, and low-severity noise.

## ✅ Commonly Normal / Expected Directories
These are standard Windows locations. Processes running from or with working directories in these paths are usually legitimate unless other red flags exist.

### System-Level Directories
- C:\Windows\System32\
- C:\Windows\SysWOW64\
- C:\Windows\servicing\
- C:\Windows\WinSxS\
- C:\Windows\SystemApps\

- Common Legitimate Processes:
  - services.exe
  - svchost.exe
  - TrustedInstaller.exe
  - lsass.exe
  - winlogon.exe

🟢 Usually benign when paired with correct binary names and parents


### Program Installation Directories
- C:\Program Files\
- C:\Program Files (x86)\

- Expected Behavior:
  - Installed applications
  - Security agents
  - Enterprise software

🟢 Verify publisher if uncertain


### Temporary System Directories (Use Caution)
- C:\Windows\Temp\

🟡 Legitimate use exists, but attackers also abuse this


### ⚠️ Suspicious / High-Risk Directories
Processes executing from or using these as working directories deserve immediate scrutiny.

- User-Writable Locations (High Abuse Rate)
  - C:\Users\*\Downloads\
  - C:\Users\*\Desktop\
  - C:\Users\*\Documents\

🚩 Very common malware staging areas


- Hidden User Data Directories
  - C:\Users\*\AppData\Local\
  - C:\Users\*\AppData\Roaming\
  - C:\Users\*\AppData\Local\Temp\

🚩 Extremely common for malware persistence and execution


Public & Shared Locations
C:\Users\Public\
C:\Users\Public\Documents\
🚩 Used to bypass user-specific restrictions
Root & Unusual Locations
C:\Temp\
C:\\
🚩 Rarely legitimate for executables
❌ Major Red Flags (Immediate Escalation Indicators)
These are almost always suspicious:
Legitimate Windows binaries in user directories:
svchost.exe in Downloads
powershell.exe in AppData
TrustedInstaller.exe in Temp
Random or misspelled directories:
C:\Users\*\AppData\Local\upd8\
C:\Users\*\AppData\Local\sys32\
Executables launched directly from email attachment paths
🧠 Analyst Memory Trick
System binaries live in system folders.
User folders are for documents, not executables.
