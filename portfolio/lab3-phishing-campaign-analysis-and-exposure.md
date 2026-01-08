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



## What is the URL to the .zip archive of the phishing kit? (defanged format)
