# Windows Logging for SOC

**Date:** 2026-03-03

**Objective:**


**Environment:** TryHackMe – “Windows Logging for SOC” Room  
**Tools Used:** Event Viewer

##

## Security Log: Authentication

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


