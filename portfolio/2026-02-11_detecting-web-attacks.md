# Detecting Web Attacks
**Date:** 2026-02-11

**Objective:**



**Environment:** TryHackMe – “Detecting Web Attacks” room  
**Tools Used:** Wireshark



## Log-Based Detection

### What is the attacker's User-Agent while performing the directory fuzz?

<img width="1440" height="743" alt="Screenshot 2026-02-11 at 5 19 29 PM" src="https://github.com/user-attachments/assets/9748c8be-c894-429e-9a2e-a46c2622d8e3" />

Here, the first few lines are dedicated to directory fuzzing, where the attacker receives the 200 response code from 4 different directories. 

The User-Agent used is:
- FFUF v2.1.0

##

### What is the name of the page on which the attacker performs a brute-force attack?

<img width="1440" height="743" alt="Screenshot 2026-02-11 at 5 25 56 PM" src="https://github.com/user-attachments/assets/d6bb312c-1bb6-4e6e-99a3-193a4b7de78c" />

Brute force is signaled by repeated "POST" requests followed by a request that contains the 302 response code that signals a successful login. 

The name of the page is:
- /login.php

##

### What is the complete, decoded SQLi payload the attacker uses on the /changeusername.php form?

<img width="1440" height="126" alt="Screenshot 2026-02-11 at 5 31 54 PM" src="https://github.com/user-attachments/assets/aa758710-a363-498a-911b-cfb84c0e7851" />

Seeing a 200 response code, I can confirm that the payload sent through was accepted and that the attacker may have retreived information.

After identifying the payload, I can plug this into cyberchef and retreive the decoded version of it.

<img width="1440" height="532" alt="Screenshot 2026-02-11 at 5 32 53 PM" src="https://github.com/user-attachments/assets/b4b83e68-98c2-4de0-a1f8-dbda9e10cf2b" />

- %' OR '1'='1

## Network-Based Detection

**📎 The file traffic.pcap will be used for this analysis**

### What password does the attacker successfully identify in the brute-force attack?

Based off of the prior log, I know IP 192.168.1.10 to be the malicious user. 

In wireshark, I can use this IP as the source and see what traffic it generates to correlate it with the server generated log

<img width="1440" height="768" alt="Screenshot 2026-02-11 at 6 01 39 PM" src="https://github.com/user-attachments/assets/f3bc54d2-3b36-4152-8857-2f049ba9e21b" />

While I cant see the response codes in wireshark, and while the packet numbers dont correlate exactly, I can correlate action and events. Comparing this PCAP and the log, exactly 3 packets from the bottom is where the attacker made a successful login.

The data in this PCAP shows that the password was:
- astrongpassword123

##

### What is the flag the attacker found in the database using SQLi?

Located at the very bottom (highlighted) is the query made using SQLi

<img width="1440" height="284" alt="Screenshot 2026-02-11 at 6 08 10 PM" src="https://github.com/user-attachments/assets/3a19cc07-82ed-418c-9620-b910ef92613a" />

I can right click this packet and follow the HTTP stream to see the data the attacker received after making the GET request

<img width="1440" height="770" alt="Screenshot 2026-02-11 at 6 12 17 PM" src="https://github.com/user-attachments/assets/457610a6-d3a0-407c-abff-00700111558c" />

Listed are the names and emails (as well as the flag).

- THM{dumped_the_db}
