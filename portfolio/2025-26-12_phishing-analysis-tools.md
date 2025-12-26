**Date:** 2025-12-09

**Objective:** 
- Gain familiarity with tools that aid in examining email header info
- Utilize techniques to obtain hyperlinks in emails and expand shortened URLs
- Use OSINT tools to obtain information on potentially malicious links without directly interacting
- Obtain malicious attachments from phishing emails and use malware sandboxes to detonate attachments

**Environment:** TryHackMe – “Phishing Analysis Tools” room  
**Tools Used:** 


# Case 1
**You are a Level 1 SOC Analyst. Several suspicious emails have been forwarded to you from other coworkers. You must obtain details from each email for your team to implement the appropriate rules to prevent colleagues from receiving additional spam/phishing emails. # Analyzing Phishing Emails**

As a brief statement for understanding, Thunderbird is being used which is an open-source email client that CAN connect to Microsoft and Yahoo email accounts. You can configure the IMAP mail client (Thunderbird) to access emails on those accounts. 

## What brand was this email tailored to impersonate?

<img width="733" height="570" alt="Screenshot 2025-12-26 at 5 42 53 PM" src="https://github.com/user-attachments/assets/1a0b5602-d0f2-434f-ba20-b82bfb44da85" />

- Netflix



## What is the From email address?

Because the VM I'm working in isn't allowing web searching, I have to manually retreive the information.

I've selected to view the source code and search for "From:" 

<img width="1273" height="712" alt="Screenshot 2025-12-26 at 5 58 02 PM" src="https://github.com/user-attachments/assets/f276e334-b4b2-4adc-be8f-120600923066" />

- JGQ47wazXe1xYVBrkeDg-JOg7ODDQwWdR@JOg7ODDQwWdR-yVkCaBkTNp.gogolecloud.com



## What is the originating IP? Defang the IP address. 

<img width="725" height="706" alt="Screenshot 2025-12-26 at 6 01 45 PM" src="https://github.com/user-attachments/assets/9b84f171-5ac9-4322-8bec-981312e37bea" />

For this, I'll search for "X-Originating-IP"
- 209[.]85[.]167[.]226



## From what you can gather, what do you think will be a domain of interest? Defang the domain.

To search for the domain, I'll enter "Smtp.mailfrom

<img width="731" height="680" alt="Screenshot 2025-12-26 at 6 06 05 PM" src="https://github.com/user-attachments/assets/e4d38609-c766-4459-a32f-60a86a9d000e" />

- etekno[.]xyz



## What is the shortened URL? Defang the URL.

<img width="795" height="683" alt="Screenshot 2025-12-26 at 6 13 22 PM" src="https://github.com/user-attachments/assets/51a60743-2dce-458a-8f3e-a9f04effe4aa" />

While using cyberchef to extract URLs, I see that there are 3 other shortened URLs so this question isn't really specific and while submitting the answer, it automatically changes into an adjusted answer which is subtly off from what's displayed here. (Maybe creator / cyberchef issue).

- hxxps[://]t[.]co/yuxfZm8KPg?amp==1

**SIDENOTE:**
- I've figured that they are talking about the link in relation to the "Update account now" button, in which cyberchef didn't extract this. However, by right clicking the buttong and copying the link location, I was able to identify this specific URL

