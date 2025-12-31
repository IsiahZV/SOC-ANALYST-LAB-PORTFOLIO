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

The email header will only show the SPF result (i.e., pass /fail), and from this, I know that the sending IP was not authorized, meaning that yahoo (check the orange highlight for context) checked DNS and enforced the policy. 

The SPF policy for the Return-Path domain (mutawamarine[.]com) lives in DNS, so I have to do a dns lookup

<img width="1386" height="414" alt="Screenshot 2025-12-30 at 7 10 58 PM" src="https://github.com/user-attachments/assets/7f1eb306-e507-4985-a56d-2617b4a10b22" />

- v=spf1 include:spf.protection.outlook.com -all

