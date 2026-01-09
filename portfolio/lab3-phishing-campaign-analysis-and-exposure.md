# Phishing Campaign Analysis and Exposure
**Date:** 2026-01-02

**Objective:**
- Analyze the email samples provided by your colleagues.
- Analyze the phishing URL(s) by browsing it using Firefox.
- Retrieve the phishing kit used by the adversary.
- Use CTI-related tooling to gather more information about the adversary.
- Analyze the phishing kit to gather more information about the adversary.


**Environment:** TryHackMe – “Snapped Phish-ing Line” room  
**Tools Used:**

**Disclaimer:** The phishing kit used in this scenario was retrieved from a real-world phishing campaign. Hence, it is advised that interaction with the phishing artefacts be done only inside the attached VM, as it is an isolated environment.

##

### Context
As an IT department personnel of SwiftSpend Financial, one of your responsibilities is to support your fellow employees with their technical concerns. While everything seemed ordinary and mundane, this gradually changed when several employees from various departments started reporting an unusual email they had received. Unfortunately, some had already submitted their credentials and could no longer log in.

## Who is the individual who received an email attachment containing a PDF?

After reviewing all .eml files given, the user "William McClean" is the only person who received a PDF file attached in the email.

<img width="1149" height="721" alt="Screenshot 2026-01-06 at 7 23 43 PM" src="https://github.com/user-attachments/assets/d1adac66-448f-4d1c-bc03-9b30b5464e61" />



## What email address was used by the adversary to send the phishing emails?

Using the previous picture as reference, by looking at the "From" part of the header, there is:
- Accounts.Payable@groupmarketingonline.icu



## What is the redirection URL to the phishing page for the individual Zoe Duncan? (defanged format)

In the email for Zoe Duncan, attached is a .html file, this is pure and basic readable information. I'll download this and cat the file so that I can grep the URL. 

<img width="1221" height="707" alt="Screenshot 2026-01-07 at 6 40 18 PM" src="https://github.com/user-attachments/assets/126ce540-0582-49bc-854c-98926da25594" />

<img width="1221" height="707" alt="Screenshot 2026-01-07 at 7 13 39 PM" src="https://github.com/user-attachments/assets/165b23df-d10d-4554-a96d-a740a662a013" />

Now that I retreivied the URL, I can input this into cyberchef for easy defanging

<img width="1438" height="562" alt="Screenshot 2026-01-07 at 7 15 31 PM" src="https://github.com/user-attachments/assets/c5b1e2d5-9300-42ae-ab82-6f955f8ca9f3" />

Here the defanged URL is:
- hxxp[://]kennaroads[.]buzz/data/Update365/office365/40e7baa2f826a57fcf04e5202526f8bd/?email=zoe[.]duncan@swiftspend[.]finance&error

**Sidenote**: The 40e7... is used for session tracking specific to the victim. 
- i.e., credential logging



## What is the URL to the .zip archive of the phishing kit? (defanged format)

-  hxxp[://]kennaroads[.]buzz/data/Update365.zip

After doing some research and upon discovery, attackers don't put kit references in emails / files anymore because they can be easily flagged and caught by email scanners. 

On a server, a phishing kit can be uploaded (Update365.zip) and extracted to /Update365/ and will display as "/Update365/office365/". 

The easy way to identify this is by carefuly analyzing the URL, noting what tries to blend in or what looks suspicious. In this case, "Update365" is not a Microsoft name, this is a campaign name. Additionally, its questionable seeing folders under /data/

Whats in these kits are just small fake web applications, shown as a copy of a legitimate page. Behind the interface are credential harvesters that send the information once submitted. They use these kits because of quick redeployment, reuse, and can be modified to mimic other real brands. 



## What is the SHA256 hash of the phishing kit archive?

After identifying the URL to the .zip archive, I have to now download the phishing kit archive and hash it locally.

The command I'll be using is: wget hxxp[://]kennaroads[.]buzz/data/Update365.zip (hashed on this platform for safety)

<img width="863" height="476" alt="Screenshot 2026-01-09 at 12 36 16 PM" src="https://github.com/user-attachments/assets/a96fa142-5e1c-436d-b9f2-d42b6a31bd9b" />

Sweet bippy, now I'll use the "sha256sum <filename"" command to get the hash

<img width="863" height="476" alt="Screenshot 2026-01-09 at 12 39 07 PM" src="https://github.com/user-attachments/assets/8748c195-5756-4694-a79b-f70449cffaff" />


**Unzipped content:**
<img width="1379" height="768" alt="Screenshot 2026-01-09 at 12 59 48 PM" src="https://github.com/user-attachments/assets/60c7863e-f78f-417a-a7bb-7afa74d1fb20" />



## When was the phishing kit archive first submitted? (format: YYYY-MM-DD HH:MM:SS UTC)

I'll be using VirusTotal to gather this information

<img width="1438" height="638" alt="Screenshot 2026-01-09 at 12 46 05 PM" src="https://github.com/user-attachments/assets/b2c108b9-1a36-4e4d-80d6-e2eccd9f202d" />

- 2020-04-08 21:55:50 UTC



## What was the email address of the user who submitted their password twice?

This question implies that a user did interact and submit credentials in the phishing site. 
- As a sidenote for better understanding, this means that **the credentials were logged and stored server-side.**

**Note: I decided to get help in this question, the solution wasn't that it was in the file itself, but on the web server that I needed to search for and use. This solution was found on open source resources**

<img width="1203" height="499" alt="Screenshot 2026-01-09 at 2 24 24 PM" src="https://github.com/user-attachments/assets/1edea78c-40e6-45c2-9ac3-c247fd1ed67f" />

<img width="1155" height="767" alt="Screenshot 2026-01-09 at 2 26 23 PM" src="https://github.com/user-attachments/assets/2715a5ee-3998-4472-abb4-7121571c35e6" />



**Sidenote** 
- Global search "grep -R -E "[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}" Update365" retreived attackers email only


