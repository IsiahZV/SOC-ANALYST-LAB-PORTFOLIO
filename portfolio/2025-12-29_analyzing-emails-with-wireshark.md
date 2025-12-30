# Analyzing Emails With Wireshark
**Date:** 2025-12-23

**Objective:** 

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

