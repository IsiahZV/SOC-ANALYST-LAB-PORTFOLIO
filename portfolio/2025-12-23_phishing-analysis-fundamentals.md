# Phishing Analysis Fundamentals

**Date:** 2025-12-23

**Objective:** 
- Gain familiarity with analyzing email components such as the email address, the protocols involved in sending an email, the header and body of an email, and the types of phishing involved. 

**Environment:** TryHackMe – “Phishing Analysis Fundamentals” room  
**Tools Used:**



# Introduction Recap

To avoid going over simple topics, I'll summarize my learnings into one section before introducing lab work.
What makes up an email address is:
- **User mailbox** (username)
- **@** , followed by the **domain**

Three protocols involved in facilitating outgoing and incoming emails are:
- **SMTP (Simple Mail Transfer Protocol)** -> Handles the sending of emails
- **POP3 / IMAP** -> Both responsible for transferring emails between client and mail server (how the mail is stored)
  - POP3 involves storing and downloading emails on a single device (meaning you can't universally access the mail on other devices unless able to specify that the user is keeping the email on the server)
  - With IMAP, emails are stored on a server and can be universally downloaded and synced to any device (think gmail, icloud, etc.), sent mail is also stored on the server
 


# Email Body

## In the given screenshots, what is the URI of the blocked image?

<img width="705" height="297" alt="image" src="https://github.com/user-attachments/assets/9799aeca-d1b0-4543-8684-2a841daec752" />

// Resort to viewing source code

<img width="800" height="523" alt="Screenshot 2025-12-23 at 2 36 48 PM" src="https://github.com/user-attachments/assets/aadb8bbf-f40a-4816-a1e0-b64c4d19bfe5" />

- **https://i.imgur.com/LSWOtDI.png**

