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

- **https://i[.]imgur[.]com/LSWOtDI[.]png**



## In the given screenshot, what is the name of the PDF attachment?

<img width="882" height="418" alt="image" src="https://github.com/user-attachments/assets/b804637a-8301-4b25-9e77-aa594b456898" />


// Viewing attachment in source code

<img width="488" height="217" alt="Screenshot 2025-12-23 at 2 45 19 PM" src="https://github.com/user-attachments/assets/13ba7bf5-3322-4b69-bca2-52c199b31e69" />



## In the attached virtual machine, view the information in email2.txt and reconstruct the PDF using the base64 data. What is the text within the PDF?

<img width="601" height="442" alt="Screenshot 2025-12-23 at 2 50 53 PM" src="https://github.com/user-attachments/assets/466cbaf8-8d99-465d-b354-3887558b6bf3" />

To convert to text from base64, I can decode by using the base64 decode command (Linux), this then has to be made into a pdf

<img width="739" height="338" alt="Screenshot 2025-12-23 at 3 35 24 PM" src="https://github.com/user-attachments/assets/a72df130-72a4-4b6a-9490-1923824c808b" />

<img width="739" height="338" alt="Screenshot 2025-12-23 at 3 35 53 PM" src="https://github.com/user-attachments/assets/5a9ff574-8c48-43f5-853f-ae38cec5e2a8" />

