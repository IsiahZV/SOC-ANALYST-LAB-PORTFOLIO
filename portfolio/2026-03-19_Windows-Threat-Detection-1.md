# Windows Threat Detection (Initial Access & Discovery)
**Date:** 2026-03-19

**Objective:**
- Explore how threat actors access and breach Windows machines
- Learn common Initial Access techniques via real-world examples
- Practice detecting every technique using Windows event logs


**Environment:** TryHackMe – “Windows Threat Detection 1” room  
**Tools Used:** Windows Event Viewer
##

## Scenario
> The IT admin exposed RDP on a production server so that it could be accessed from home on weekends. The credentials were set to Administrator:Summer2025. Reconstruct what happened next, just in a few hours, and try to detect it in logs by using Event Viewer (C:\Users\Desktop\Administrator\Practice\RDP Case\RDP-Security.evtx file):
##

<img width="1440" height="791" alt="Screenshot 2026-03-19 at 4 23 28 PM" src="https://github.com/user-attachments/assets/e125433a-9685-4b98-9de4-c19f00dc7c89" />

### Which user seems to be most actively brute-forced by botnets?

I'll begin by filtering for Event ID 4625, whether its type 3 or 10 doesnt (it does) matter, and while looking through the results of the events, I notice that the user "ADMINISTRATOR" seems to have the most attempts. One thing to point out is that the user is attempted by many different public IPs. There's a ton of events so I dont think sharing multiple screenshots would be useful here.

##

### Which IP managed to breach the host via RDP (Logon Type 10)?

Now I need to identify which IP breached the user and for this, I'll filter for successful logins as well as Type 10. 

<img width="1440" height="749" alt="Screenshot 2026-03-19 at 4 54 27 PM" src="https://github.com/user-attachments/assets/916ff618-0171-400a-83fd-42974e7ece17" />
