# Windows Threat Detection (Initial Access & Discovery)
**Date:** 2026-03-19

**Objective:**
- Explore how threat actors access and breach Windows machines
- Learn common Initial Access techniques via real-world examples
- Practice detecting every technique using Windows event logs


**Environment:** TryHackMe – “Windows Threat Detection 1” room  
**Tools Used:** Windows Event Viewer
##

# Initial Access Via RDP
## Scenario
> The IT admin exposed RDP on a production server so that it could be accessed from home on weekends. The credentials were set to Administrator:Summer2025. Reconstruct what happened next, just in a few hours, and try to detect it in logs by using Event Viewer (C:\Users\Desktop\Administrator\Practice\RDP Case\RDP-Security.evtx file):
##

<img width="1440" height="791" alt="Screenshot 2026-03-19 at 4 23 28 PM" src="https://github.com/user-attachments/assets/e125433a-9685-4b98-9de4-c19f00dc7c89" />

### Which user seems to be most actively brute-forced by botnets?

I'll begin by filtering for Event ID 4625, whether its type 3 or 10 doesnt (it does) matter, and while looking through the results of the events, I notice that the user "ADMINISTRATOR" seems to have the most attempts. One thing to point out is that the user is attempted by many different public IPs. There's a ton of events so I dont think sharing multiple screenshots would be useful here.

##

### Which IP managed to breach the host via RDP (Logon Type 10)?

Now I need to identify which IP breached the user and for this, I'll filter for successful logins (Event ID 4624) as well as Type 10. 

<img width="1440" height="749" alt="Screenshot 2026-03-19 at 4 54 27 PM" src="https://github.com/user-attachments/assets/916ff618-0171-400a-83fd-42974e7ece17" />

> @ 9:51:48
- 203.205.34.107

##

### Can you get the real Workstation Name (hostname) of the threat actor?

<img width="1440" height="749" alt="Screenshot 2026-03-19 at 5 14 40 PM" src="https://github.com/user-attachments/assets/80180680-f9c1-4c55-bfea-f4963b3d92a0" />

> @ 9:51:45
- DESKTOP-QNBC4UU


##


# Initial Access Via Phishing

> investigate three phishing attachment examples stored in: C:\Users\Administrator\Desktop\Practice\Phishing Case 1-3

### Run the www.skype.com file from the Phishing Case 1 folder, which flag do you get?
> play the role of the untrained user and mindlessly open the COM file.

<img width="1000" height="594" alt="Screenshot 2026-03-19 at 5 57 56 PM" src="https://github.com/user-attachments/assets/2465cdfb-d2dc-44b4-aad4-1fab8454ef56" />

As I opened the file, the taskbar opens and generates messages until revealing that its malware

- THM{misleading_extension}

##

### From which URL does the malicious LNK download the next stage malware?
> Case 2

<img width="1000" height="540" alt="Screenshot 2026-03-19 at 6 16 31 PM" src="https://github.com/user-attachments/assets/533ffc23-868f-4eb4-82f4-8b3a2e9e55e0" />

<img width="1000" height="540" alt="Screenshot 2026-03-19 at 6 21 00 PM" src="https://github.com/user-attachments/assets/57af8206-43e3-406a-8a1a-45a70af473c7" />

You wont be able to see the entirety of the URL, however, it does contain:
- http[://]wp16[.]hqwlqpa[.]thm[:]8000/cgi-bin/f

##

### What is the name of the double-extension file you see there?
> Case 3

<img width="1000" height="540" alt="Screenshot 2026-03-19 at 6 25 00 PM" src="https://github.com/user-attachments/assets/873f918d-88fa-453a-a47f-f28b3835257c" />

- best-cat.jpg.exe


##


# Detecting Malicious Download
> In this task, let's try to investigate the third phishing case by checking the attached Sysmon logs: C:\Users\Administrator\Desktop\Practice\Phishing Case 3\Phishing-Sysmon.evtx

### Which file did the user download via the web browser?

I've identified the file that was downloaded, however, I'll reconstruct the process:

<img width="921" height="643" alt="Screenshot 2026-03-23 at 6 41 15 PM" src="https://github.com/user-attachments/assets/40f6a10c-0358-43f7-8129-f287fe9e0a84" />

- @ 6:58:28 -> User "Administrator" opens web browser


<img width="921" height="643" alt="Screenshot 2026-03-23 at 6 43 42 PM" src="https://github.com/user-attachments/assets/874d29a3-b4de-4520-8ff0-59a1c2f661f5" />

- @ 6:58:28 -> archive file appears in downloads
- Note how the file ends in .zip


<img width="921" height="643" alt="Screenshot 2026-03-23 at 6 48 06 PM" src="https://github.com/user-attachments/assets/bcc4467c-f970-4b61-9d62-22a98fcc33f4" />

- @ 6:58:43 -> File gets unarchived in the "Pictures" directory
- Note that now unarchived, the file name ends in ".jpg.exe"


<img width="921" height="643" alt="Screenshot 2026-03-23 at 7 05 10 PM" src="https://github.com/user-attachments/assets/c6d344ad-7fce-40ab-b8cf-51074ba2e41f" />

- @ 6:59:06 -> The user clicks on the unarchived file

- **Downloaded filename from web browser:** C:\Users\Administrator\Downloads\top-cats.zip

## 

### In which folder did the user unarchive the suspicious file?

- C:\Users\Administrator\Pictures

##

### What is the process ID of the launched phishing malware?

- Refer to latest picture of the reconstruction: (5484)

##

### Finally, which malicious domain did the malware try to connect to?

<img width="921" height="643" alt="Screenshot 2026-03-23 at 7 13 28 PM" src="https://github.com/user-attachments/assets/fc6971c5-fcb2-44e2-b518-07486dfb618b" />

- @ 6:59:09 -> DNS queries are made to "rjj.store"
- Note the process ID (same processID of the launched malware, this is an extension of the launch)


##


# Initial Access Via USB
> For this task, you will investigate a typical attack chain via USB using the attached Sysmon logs:
C:\Users\Administrator\Desktop\Practice\USB Case\USB-Sysmon.evtx

##

### Which USB file was launched by the user?

To begin, it should now be understood that Event ID "11" pertains to file creation. In the screenshot below, I've highlighted an event where Event ID 1 (user opens file) comes before 11. This is fine as some events happen so fast that one event may "precede" another visually, just not logically. 

<img width="964" height="664" alt="Screenshot 2026-03-25 at 10 14 48 PM" src="https://github.com/user-attachments/assets/63aa8378-bcd5-4bbc-8936-c221199cf7d4" />

- Note TargetFileName (suspicious directory / naming)
- Launched USB file: E:\Open Sandisk 4GB USB.exe

##

### Which suspicious file did the malware drop to the disk?

- Refer to TargetFileName from previous screenshot:
  - C:\Users\Public\Documents\winupdate.exe

##

### To which other USB did the malware propagate?

<img width="964" height="664" alt="Screenshot 2026-03-25 at 10 44 39 PM" src="https://github.com/user-attachments/assets/6fe72d7d-d452-4f98-8234-4559a1fec14d" />

- F:

##

