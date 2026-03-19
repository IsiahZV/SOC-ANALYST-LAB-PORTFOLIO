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


