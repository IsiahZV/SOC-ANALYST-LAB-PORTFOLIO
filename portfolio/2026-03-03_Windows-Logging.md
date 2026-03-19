# Windows Logging for SOC

**Date:** 2026-03-03

**Objective:**


**Environment:** TryHackMe – “Windows Logging for SOC” Room  
**Tools Used:** Event Viewer

##

# Security Log: Authentication

### Open the "Practice-Security.evtx" file on the VM's Desktop. Which IP performed a brute force of the THM-PC?

<img width="2240" height="1182" alt="image" src="https://github.com/user-attachments/assets/3ade91cf-351f-4cc0-b97b-5cfbef8618a7" />

The first step is to filter for the 4625 Event ID which is the failed login attempt

<img width="810" height="826" alt="image" src="https://github.com/user-attachments/assets/5ba63790-9a20-4de4-864f-19e4fb8d532b" />

Before continuing, I want to establish that all of these logs pertain to THM-PC.

<img width="1532" height="982" alt="image" src="https://github.com/user-attachments/assets/71a398a6-c40b-4678-9384-19f635c37dcd" />

Boxed are groupings of attempted accounts. The first two within the same second are "Administrator" and the second was "support", followed by "hepdesk" and so on. 
Multiple attempts to different accounts are made in the same second / minute from one IP:
- 10.10.53.248

<img width="1518" height="682" alt="image" src="https://github.com/user-attachments/assets/08d3bedd-2d9d-464b-9e15-2ca40b582470" />

##

### Which user has been breached as a result of the attack?

Now I'll move onto the successful logons (Event ID 4624)

<img width="810" height="818" alt="image" src="https://github.com/user-attachments/assets/b1aa92af-083a-4a95-8a7d-6362cff96a8a" />

Whats important to uncover first is the Logon Type which should be "10" and remember that THM-PC is the host of concern

<img width="1518" height="976" alt="image" src="https://github.com/user-attachments/assets/57cb89ec-c17e-4eef-907c-bb7c2f0f2498" />

Note the host "THM-PC" and the Login Type

<img width="1516" height="848" alt="image" src="https://github.com/user-attachments/assets/c7972a66-70a9-4da9-ad7a-944bba3c25a9" />

Already, I see an account name of interest from the suspicious IP. 
Since a Logon Type 10 (RDP) is preceded by a Logon Type 3 (network) event, I can confirm by searching for the Type 3 event and correlate the activity. 

<img width="1518" height="974" alt="image" src="https://github.com/user-attachments/assets/9d7d5753-0e3d-4c77-8ed5-2870b57a309b" />

Although unable to see, the Source Network Address below contains the same suspicious IP address.

- Administrator

##

### What was the Logon ID of the malicious RDP login?

This can be found in the screenshots above that show the RDP logon (Type 10)

- 0x183C36D

##

# Security Log: User Management

### Which user was created by the attacker soon after the RDP login

This is in continuation of the last section pertaining to authentication. I know now that the Administrator account was breached and that its Logon ID is 0x183C36D. 

To find logs that relate to created accounts, I will filter for the 4720 Event ID.

<img width="1538" height="1022" alt="image" src="https://github.com/user-attachments/assets/68db70cc-ffec-4da5-b0c8-e7132c92120d" />

In the screenshot, I see that the compromised Administrator account has created an account called:

- svc_sysrestore

##

### Which two privileged groups was the backdoor user added to?
**(Answer in alphabetical order, e.g. "Administrators, Power Users")**

Now I'm going to filter for the 4732 Event ID which displays what security groups that svc_sysrestore has been added to. 
- It's also important to note the time of account creation being 10:54:58.
- The Security ID is: S-1-5-21-1966530601-3185510712-10604624-1013

<img width="1520" height="996" alt="image" src="https://github.com/user-attachments/assets/f179beb9-8a48-4e3f-861c-d97a6de481f4" />

Once the account was created, a second later, it was added to the Users group, this is a default action after creating an account.

<img width="1512" height="684" alt="image" src="https://github.com/user-attachments/assets/793f89df-4353-46ed-aedd-dd1277ee0718" />

Next, the account was added to the Backup Operators group

<img width="1522" height="684" alt="image" src="https://github.com/user-attachments/assets/d81c5d82-0349-4bcc-830f-7da0a84a6a39" />

Finally, the account was added to the Remote Desktop Users group

Note that the Logon ID field matches all throughout this entire process. This means that as soon as the attacker officially gained access to the Administrator account, it was assigned the unique Logon ID value and has stayed consistent for the entire analysis as we can track the actions of that session. 

##

# SYSMON: PROCESS MONITORING

### Which web browser does Sarah use to browse the web?

<img width="2246" height="1190" alt="image" src="https://github.com/user-attachments/assets/da745dfd-d898-4821-a0fe-30b263c6a2d0" />

I'll begin by filtering for Event Code: 1

<img width="1520" height="974" alt="image" src="https://github.com/user-attachments/assets/f617834d-e49e-4679-93d9-9ce7bfd5f6bd" />

For reference, the Parent Process always comes first, what were concerned with is what comes of this Parent Process which is the Process Info or "Child Process". 

- User Context:
  - THM-PC\sara.miller
  - LogonID:  0x1F98906
- Parent Info:
  - PPID -> 4228
  - Parent Image: C:\Windows\explorer.exe
  - Parent Command Line: C:\Windows\Explorer.EXE
- Process Info:
  -   PID -> 412
  -   Image: C:\Program Files (x86)\Google\Chrome\Application\chrome.exe
  -   CommandLine: "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe"
  -   CurrentDirectory: C:\Program Files (x86)\Google\Chrome\Application\

The web browser used would be:
- Google Chrome

##

### Which file did Sarah download from the browser?

Now time to move up the log to the next event. The event after this contains the user account opening the notepad app, but this will be irrelevant. 

<img width="1522" height="1010" alt="image" src="https://github.com/user-attachments/assets/6dce27d2-77b6-423e-8e06-b794c843ab8a" />

- C:\Users\sarah.miller\Downloads\ckjg.exe

The change of Original File Name may be of concern. 

Although the question doesn't prompt it, I did research on the file hash on VirusTotal

<img width="2256" height="1242" alt="image" src="https://github.com/user-attachments/assets/06e10b44-de9c-4df3-9fb4-e60036de9450" />

<img width="2210" height="1238" alt="image" src="https://github.com/user-attachments/assets/3b5b6fa0-5c2d-4331-9e92-6d42f6cc8e1e" />

VirusTotal shows that the hash has been used under different names which would explain ckjg[.]exe along with Stub[.]exe

##

### Which URL was the file downloaded from?

Now, I have to step out of searching within Event ID: 1 and looking at the bigger picture.

To reconstruct the timeline simply (which there isn't much of a timeline because all of this happens almost simultaneously):

- Event 11 → .tmp
  - In-progress download file from google chrome browser
- Event 15 → ckjg.exe
  - Renames or re-writes contents from the .tmp file which leaves this log as a result
- Event 11 → ckjg.exe:Zone.Identifier
  - Windows creates Alternate Data Stream object
- Event 15 → ckjg.exe:Zone.Identifier
  - Stream being written to (contains zoneId so far)
- Event 15 → ckjg.exe
  - Regular file activity (i.e., hashing, metadata updates, reopened)
- Event 15 → ckjg.exe:Zone.Identifier (with contents)
  - Stream write completed, could be part of a open, write, close sequence
  - Contains Url of which the file was downloaded
  - @4:07:38
 - Next is a series of suspicious registry and process events along with suspicious dns queries to jumbled websites that end in .click (i.e., jfasdfsd[.]click)

<img width="1522" height="1008" alt="image" src="https://github.com/user-attachments/assets/7e27d5f8-71fe-4db3-8111-86bfebea131d" />

- http[:]//gettsveriff[.]com/bgj3/ckjg[.]exe

##

# SYSMON: FILES AND NETWORK

To begin, in order to answer questions going forward, I have to restructure this in a way that starts from the process creation logs (Event ID 1) to either network creation or DNS query events. The value of interest to establish this structure is ProcessId

- Going back to the event where the file was downloaded by Sarah @4:08:43 :

<img width="561" height="652" alt="Screenshot 2026-03-11 at 6 43 42 PM" src="https://github.com/user-attachments/assets/dd2ccb89-4964-4f89-99af-e727dbef9907" />

- ProcessId: 1460

|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||


- **Event ID 3 (Network Connection):**
<img width="564" height="652" alt="Screenshot 2026-03-11 at 7 00 34 PM" src="https://github.com/user-attachments/assets/0dc6ae8a-2d7e-4818-b7a3-bcd985d18a7f" />

- @4:08:58
Here, there is an established network connection with external IP: 193.46.217.4

|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||

- **Event ID 22 (DNS Query):**
<img width="561" height="652" alt="Screenshot 2026-03-11 at 6 48 44 PM" src="https://github.com/user-attachments/assets/511b774d-9952-4afe-a364-d9907283240f" />

- @4:08:58
Here, the suspicious .click query name is displayed, the QueryStatus is 0 which is a success, followed by the IP its querying. Note that before this, there were two other queries prior @4:08:48 where the queries failed and there was no IP. 

|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||


- **Event ID 11 (File creation):**
<img width="587" height="652" alt="Screenshot 2026-03-11 at 7 22 58 PM" src="https://github.com/user-attachments/assets/cbc02414-882b-46c8-a1ad-d4048034f13c" />
- @ 4:08:36

|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||


**RECONSTRUCTION:**

04:07:38
- Chrome finishes download
- ckjg.exe written
- Zone.Identifier written

04:08:43
- User executes ckjg.exe
- Event 1

04:08:46
- Malware persistence
- Startup\DeleteApp.url created

04:08:58
- Malware DNS query
- hkfasfsafg.click

04:08:58
- Network connection to C2

##

### Which file was created by the downloaded malware to persist on the host?

- C:\Users\sarah.miller\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\DeleteApp.url

##

### What is the Command & Control server malware connected to?

Referencing the network connection event (3), the IP address was:
- 193.46.217.4 on port 7777

##

### Finally, which domain does the malicious IP correspond to?



##



# Logging Commands
- Review the Administrator's PS history on the attached VM.
### Which PowerShell command was executed first?


<img width="733" height="359" alt="Screenshot 2026-03-18 at 9 28 18 PM" src="https://github.com/user-attachments/assets/2c8ee9cf-7c75-4cf4-8786-8a9225056829" />

Seen here is the directory for the poweshell history log. I'll cat the file from here:

<img width="733" height="419" alt="Screenshot 2026-03-18 at 9 33 17 PM" src="https://github.com/user-attachments/assets/a8bd2a31-b759-40a4-a10f-d19b7a3da0de" />

- Get-ComputerInfo

##

### When did the Administrator run the first PS command? (Format: April 18, 2025)
- The hint for this is that I need to identify the file through the file GUI and explore the Properties

<img width="491" height="505" alt="Screenshot 2026-03-18 at 9 42 42 PM" src="https://github.com/user-attachments/assets/17808b9a-04c2-443c-bbcd-75f04984fdf8" />

- May 18, 2025

##

### Can you find the flag stored in the PowerShell history? (Format: THM{...})

> **thm.alex attempt:**

<img width="768" height="379" alt="Screenshot 2026-03-18 at 9 48 04 PM" src="https://github.com/user-attachments/assets/c7ce7e68-cc15-41d9-a707-48079f63c4c2" />


> **thm.bob attempt:**

<img width="768" height="379" alt="Screenshot 2026-03-18 at 9 51 12 PM" src="https://github.com/user-attachments/assets/722200fd-1b75-49db-85b6-007be17c0efc" />
- THM{it_was_me!}

##

