# Assessing A Potential Malicious Email

**Date:** 2025-12-23

**Objective:** 

**Environment:** TryHackMe – “Phishing Analysis Fundamentals” room  
**Tools Used:** 


# 📖 Context

A Sales Executive at Greenholt PLC received an email that he didn't expect to receive from a customer. He claims that the customer never uses generic greetings such as "Good day" and didn't expect any amount of money to be transferred to his account. The email also contains an attachment that he never requested. He forwarded the email to the SOC (Security Operations Center) department for further investigation. 

Investigate the email sample to determine if it is legitimate.


## What is the Transfer Reference Number listed in the email's Subject?

To get this started, the Transfer Reference Number (relative to banking) is highlighted in the screenshot below

<img width="1004" height="787" alt="Screenshot 2025-12-30 at 5 35 01 PM" src="https://github.com/user-attachments/assets/960a67e5-e1c4-4b9f-aa58-df06532be432" />

- 09674321



## Who is the email from?

<img width="978" height="215" alt="Screenshot 2025-12-30 at 5 38 12 PM" src="https://github.com/user-attachments/assets/09de1f4d-334a-446d-850b-aa9465c7bfb2" />

- Mr. James Jackson



## What is his email address?
For this question, its important to acknowledge the difference between the "From" and "Reply-To" source.

<img width="1295" height="461" alt="Screenshot 2025-12-30 at 5 42 21 PM" src="https://github.com/user-attachments/assets/b7696b7a-f205-4cfd-a758-dd67c87afc8a" />

- info@mutawamarine@mail.com



## What email address will receive a reply to this email? 
This is the source where the "Reply-To" email will receive emails.

<img width="1295" height="461" alt="Screenshot 2025-12-30 at 5 45 10 PM" src="https://github.com/user-attachments/assets/82077ca0-aab7-443e-8c61-84183e62ce50" />

- info.mutawamarine@mail.com



## What is the Originating IP?

Before giving the answer, I should break down some of my findings.

<img width="1390" height="461" alt="Screenshot 2025-12-30 at 6 18 16 PM" src="https://github.com/user-attachments/assets/eca9f996-9265-4d3e-ad15-8b44c51a348a" />


Highlighted are three sections related to IP addresses. 
- X-Originating-IP: Unavailable
- Received: from 10.197.41.148
- Received: from 192.119.71.157

In this case when the X-Originating-IP is unavailable, the IP address from the bottom most "Received:" header. This is because it represents the first SMTP server that accepted the message which happens to the section highlighted in green. 
- The earliest (very bottom) Received header is the most trustworthy as attackers cant modify headers added after their server, especially if the IP is public and not internal to the recipient.

- 192.119.71.157



## Who is the owner of the Originating IP? (Do not include the "." in your answer.)

Since network communication is disabled in this lab, I have to find a open source resource instead of providing a whois lookup within the terminal.

<img width="1440" height="792" alt="Screenshot 2025-12-30 at 6 45 18 PM" src="https://github.com/user-attachments/assets/65994bff-342b-479e-82a0-63d8480f1724" />

- Hostwinds LLC



## What is the SPF record for the Return-Path domain?
**As a starter, SPF basically says who is allowed to send**
- (i.e., "This email domain should only be coming from Outlook, not Google (gmail), apple (icloud), etc. You should literally only be using Outlook interface to send this mail). 

The email header will only show the SPF result (i.e., pass /fail), and from this, I know that the sending IP was not authorized, meaning that yahoo (check the orange highlight for context) checked DNS and enforced the policy, causing it to fail.


The SPF policy for the Return-Path domain (mutawamarine[.]com) lives in DNS, so I have to do a dns lookup

<img width="1386" height="414" alt="Screenshot 2025-12-30 at 7 10 58 PM" src="https://github.com/user-attachments/assets/7f1eb306-e507-4985-a56d-2617b4a10b22" />

- v=spf1 include:spf.protection.outlook.com -all

This means that Microsoft Outlook mail servers are authorized and to reject anything else. 

The return path from the sender of the email shows (mutawamarine.com) however the sending IP address (192[.]119[.]71[.]157) does not come from Outlook infrastructure, its Hostwinds. This means that the domain was spoofed. 


## What is the DMARC record for the Return-Path domain?
**To start, DMARC decides what to do when the results from SPF and DKIM dont align**

The command to find the DMARC record for mutawamarine.com is: dig TXT _dmarc.mutawamarine.com

<img width="1386" height="414" alt="Screenshot 2025-12-30 at 8 35 01 PM" src="https://github.com/user-attachments/assets/0407a7d7-767a-4075-9ce0-d8a5fb0877ae" />

- v=DMARC1; p=quarantine; fo=1

This is saying that DMARC instructed Yahoo to quarantine the message instead of fully rejecting it after failing SPF, basically sending it to spam. 


## What is the name of the attachment?

<img width="649" height="414" alt="Screenshot 2025-12-30 at 8 42 53 PM" src="https://github.com/user-attachments/assets/c01c9f70-7d28-4852-bee1-d21f193e6e41" />

- SWT_#09674321____PDF__.CAB



## What is the SHA256 hash of the file attachment?

Since I'm using Thunderbird (if that matters much), I'll download the file and save it to the desktop (and not open / execute). 

<img width="909" height="433" alt="Screenshot 2025-12-30 at 9 04 58 PM" src="https://github.com/user-attachments/assets/bff1bfc0-fa01-4296-970c-fb6ec83f3d73" />

- 2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f



## What is the attachments file size? (Don't forget to add "KB" to your answer, NUM KB)

I'll now have to use an open source resource, in this case, virus total

<img width="1440" height="696" alt="Screenshot 2025-12-30 at 9 07 08 PM" src="https://github.com/user-attachments/assets/db0e4884-20a0-448c-b447-3073501eb3c8" />

Additionally, I can enter "ls -l" to show the byte size and divide it by 1024

- 400.26 KB



## What is the actual file extension of the attachment?

<img width="1440" height="696" alt="Screenshot 2025-12-30 at 9 13 11 PM" src="https://github.com/user-attachments/assets/2e3cc7e5-ccdb-4627-a02d-4a5ad83754f6" />

Virus total shows this as a RAR file.

Another approach within the terminal is "file <filename>"

<img width="917" height="418" alt="Screenshot 2025-12-30 at 9 19 33 PM" src="https://github.com/user-attachments/assets/867e1508-672c-4755-a572-7e1ce086195b" />


This is a sign of extension spoofing. 
What this does is:
- Trick users into thinking that the file is safe
- Bypass some security filters
- Obfuscation



## End


