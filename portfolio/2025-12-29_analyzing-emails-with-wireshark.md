# Analyzing Emails With Wireshark
**Date:** 2025-12-23

**Objective:**
- Use protocol specific filters and identify where email content lives
- Gain familiarity with SMTP response codes and what they signal
- Understand security control validation
- Conduct artifact analysis by identifying attachments, identifying the encoding type, and client used

**Environment:** TryHackMe – “Phishing Prevention” room  
**Tools Used:** Wireshark


In this task, you will analyze a PCAP file with SMTP traffic

## Which Wireshark filter can you use to narrow down your results based on SMTP response codes?

By digging far in my brain, I can remember how to **search** for filters. 
- Hover over "Analyze"
  - Display Filter Expression
    - Type "smtp" and look for whats in relation to the question

<img width="1440" height="704" alt="Screenshot 2025-12-29 at 6 35 37 PM" src="https://github.com/user-attachments/assets/556789b2-37dc-473b-a6fb-5bda5497a6dc" />

- smtp.response.code



## How many packets in the capture contain the SMTP response code 220 Service ready?

For this, I'll apply the filter from the previous question and add "== 220" at the end

<img width="1440" height="704" alt="Screenshot 2025-12-29 at 6 37 15 PM" src="https://github.com/user-attachments/assets/fd90eef1-a9be-414e-8985-03cdbd87bbe5" />

- 19



## One SMTP response indicates that an email was blocked by spamhaus.org. What response code did the server return?

When removing a specific status code, I can see all of the other status codes in this pcap

<img width="1440" height="704" alt="Screenshot 2025-12-29 at 6 43 29 PM" src="https://github.com/user-attachments/assets/b99f8334-e4c4-40fd-ae36-bb159c2d59e7" />

- 553



## Based on the packet from the previous question, what is the full Response code: message?

<img width="1440" height="704" alt="Screenshot 2025-12-29 at 6 54 25 PM" src="https://github.com/user-attachments/assets/32bbe4ff-7352-4d43-81c1-cf43ee1aed9d" />

- Requested action not taken: mailbox name not allowed (553)



## Search for response code 552. How many messages were blocked for presenting potential security issues?

<img width="1440" height="704" alt="Screenshot 2025-12-29 at 6 57 39 PM" src="https://github.com/user-attachments/assets/72d6dd91-d3fd-4a5f-9671-d6a2d1a9bb4c" />

- 6



# Inspecting Emails and Attachments
**You will utilize the Internet Message Format (IMF) to examine the inner details of emails, such as sender and recipient fields, content type, and attachments.**


## How many SMTP packets are available for analysis?

<img width="1440" height="704" alt="Screenshot 2025-12-29 at 7 00 44 PM" src="https://github.com/user-attachments/assets/0995bb36-fbc4-4a84-911d-e8a3202b38d9" />

- 512



## What is the name of the attachment in packet 270?

<img width="1440" height="704" alt="Screenshot 2025-12-29 at 7 18 04 PM" src="https://github.com/user-attachments/assets/146ca18e-678d-44e1-bcca-2a6c765575ce" />

- document.zip



## According to the message in packet 270, which Host IP address is not responding, making the message undeliverable?

<img width="1440" height="704" alt="Screenshot 2025-12-29 at 7 18 50 PM" src="https://github.com/user-attachments/assets/a359662f-3983-4c92-ab1d-5008aabdd2c5" />

- 212[.]253[.]25[.]152



## By filtering for imf, which email client was used to send the message containing the attachment attachment.scr?

<img width="1440" height="704" alt="Screenshot 2025-12-29 at 8 00 20 PM" src="https://github.com/user-attachments/assets/304dbc1d-baf0-42ef-ba7a-702f98696ae8" />

- Microsoft Outlook Express 6.00.2600.0000



## Which type of encoding is used for this potentially malicious attachment?

<img width="1440" height="704" alt="Screenshot 2025-12-29 at 8 02 51 PM" src="https://github.com/user-attachments/assets/55d84812-bb04-4f1b-abc5-5a08d54ff1ce" />

- base64


# End

## Lessons Learned

This lab taught me how to SMTP codes reflect mail server enforcement decisions and how IMF headers and MIME encoding can be used to attribute tooling and analyze potentially malicious attachments.

