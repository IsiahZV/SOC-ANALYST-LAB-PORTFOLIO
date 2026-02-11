# MITM Detection
**Date:** 2026-02-03

**Objective:**
- Identify indicators of compromise related to MITM attacks
- Utilze network monitoring tools for detecting suspicious traffic patterns
- Practice incident response procedures for MITM attack scenarios


**Environment:** TryHackMe – “Man-in-the-Middle Detection” room  
**Tools Used:** Wireshark

##

**📎 Utilized in this lab is "network_traffic.pcap", where I'll be performing the analysis.**
- Gateway: 192.168.10.1 -> Router

<img width="1440" height="794" alt="Screenshot 2026-02-04 at 5 32 56 PM" src="https://github.com/user-attachments/assets/bb6023f8-5780-4820-8495-1948f1041923" />


# Network Traffic Analysis (Detected ARP Spoofing)

### Narrowing Down ARP Traffic

<img width="1440" height="355" alt="Screenshot 2026-02-04 at 5 34 48 PM" src="https://github.com/user-attachments/assets/92ef3ed6-9364-4948-9deb-be73f6c68eeb" />

All ARP traffic will contain "who has" requests and "is at" responses. On this network, a device is looking to communicate with a specific host by searching for its IP. ARP will send the requesting host the MAC address associated with that IP. 


### Reviewing ARP Responses

<img width="1440" height="515" alt="Screenshot 2026-02-04 at 5 46 34 PM" src="https://github.com/user-attachments/assets/e10716de-2348-43fd-947c-e99c50c105aa" />

ARP poisoning implies the presence of unwanted "is at" responses (aka gratuitous).
- Packet 1 reveals a gratuitious reply to broadcast for 192.168.10.1 which would be the router, and then another reply actually comes in at packet 4, responding to a searching device.

Legitimate replies correlate with recent "who has" requests


### Traffic Associated With Gateway

<img width="1440" height="305" alt="Screenshot 2026-02-04 at 6 06 08 PM" src="https://github.com/user-attachments/assets/ab139942-9d3d-4578-b0f7-3493e3461965" />

This filter is searching for traffic where the IP claims to be from 192.168.10.1 and where the MAC claims to be 02:aa:... which can include requests and and replies sent by this source. 

##

<img width="1440" height="507" alt="Screenshot 2026-02-04 at 6 12 10 PM" src="https://github.com/user-attachments/assets/e102d1b4-3559-4bc5-b452-c407a7ae5459" />

I've annoted the photo to shift focus as packet 257 and further show that the original router MAC address goes from 02:aa... to 02[:]fe[:]fe[:]fe[:]55:55 and this is after ARP tells 02:fe:... where 192.168.10.1 resides in earlier packets such as 4 and 119.

##

<img width="1440" height="507" alt="Screenshot 2026-02-04 at 6 42 09 PM" src="https://github.com/user-attachments/assets/454277ab-4977-4a9b-8b0b-b1b3f7e0289f" />

For this, it's similar to the last command except this is searching based off of context from the "Info" column. (Can be seen similar to grep use case).

##

### Examining the malicious MAC address

<img width="1440" height="364" alt="Screenshot 2026-02-04 at 6 48 17 PM" src="https://github.com/user-attachments/assets/17d796db-24fd-4904-8121-d1887fadc381" />

This filter is searching for the MAC address thats now associating itself with the gateway IP address

##

### Checking for Duplicate-IP-to-MAC Mappings

<img width="1440" height="423" alt="Screenshot 2026-02-04 at 6 53 18 PM" src="https://github.com/user-attachments/assets/8fadf857-2499-4b7e-a898-cebe326b2857" />

This simply reveals what IP address was used to cover different MAC addresses. In other words "Your (physical) home address (Gateway IP) was used by a runaway cereal killer (physical/MAC) in order to find a job"



## How many ARP packets from the gateway MAC Address were observed?

This question is in reference to the OG gateway MAC address prior to poisoning. I'll use the "arp && eth.src==" filter to only display ARP packets from the OG gateway MAC address.

<img width="1440" height="793" alt="Screenshot 2026-02-04 at 7 05 01 PM" src="https://github.com/user-attachments/assets/9cb1b34b-c971-485f-9cd9-49584f2d7c00" />

- 10 Packets



## What MAC address was used by the attacker to impersonate the gateway?

As revealed in the walkthrough up until now, it was established that the MAC addressed used was:
- 02[:]fe[:]fe[:]fe[:]55:55



## How many Gratuitous ARP replies were observed for 192.168.10.1?

For this, I just need to apply the "arp.isgratuitous && arp.src.proto_ipv4" filter. If including it as a source is too strict, I believe "_ws.col.info contains "192.168.10.1"" may be another option with the gratuitous filter.

For better practice, using "arp.opcode== 2 && arp.isgratuitous && arp.src.proto_ipv4== 192.168.10.1" would've been a better filter as it emphasizes that I am looking for replies.

<img width="1440" height="793" alt="Screenshot 2026-02-04 at 7 10 50 PM" src="https://github.com/user-attachments/assets/b5f1af40-ff5a-4fd6-92ed-40fdaa6ffb74" />

- 2



## How many unique MAC addresses claimed the same IP (192.168.10.1)?

This may be self explanatory after the walkthrough, but for the sake of the question, I've used "_ws.col.info contains "192.168.10.1 is at " to display where this text may be presented.

<img width="1440" height="793" alt="Screenshot 2026-02-04 at 7 18 21 PM" src="https://github.com/user-attachments/assets/240671f7-919a-40db-b3a2-f74a47926d70" />

- 2 unique MAC addresses



## How many ARP spoofing packets were observed in total from the attacker?

This can be conuted manually from the previous picture, but again, for the sake of the question and for my brain to grow, I'll utilize the filter.

<img width="1440" height="793" alt="Screenshot 2026-02-04 at 7 20 55 PM" src="https://github.com/user-attachments/assets/751bf4f2-ed57-4b07-8526-3160132bf1ce" />

- 14 packets



# DNS Spoofing

**📎 The PCAP used in this is the same used in the previous section that went over ARP**
- DNS spoofing happens by the attacker doing something similar to ARP poisoning, where there may be real DNS responses sent mixed with a malicious response in an attempt of "DNS cache poisoning" after the victim makes a query to a website. Like ARP poisoning attempts, there may be traces of unsolicited responses or where responses don't come from the expected source.
##

### Narrowing DNS Traffic

<img width="1440" height="793" alt="Screenshot 2026-02-05 at 4 59 10 PM" src="https://github.com/user-attachments/assets/9c2ce7d4-ace9-4c0c-ab70-96a60a3da6cc" />

##

### Filtering Legit Traffic

<img width="1440" height="500" alt="Screenshot 2026-02-05 at 5 05 33 PM" src="https://github.com/user-attachments/assets/ee994104-1d30-4413-bd6c-fa973738eb8a" />
- What this filter is looking for is responses where the Google DNS server IP address is the source dishing out the responses to those querying

##

### Filtering For DNS Responses

<img width="1440" height="500" alt="Screenshot 2026-02-05 at 5 08 39 PM" src="https://github.com/user-attachments/assets/cc6d058a-565b-41c5-b441-6438e3d01b27" />
- Now filtering for all responses, on packet 1124 there's a private IP replying to another IP about the destination of an acme corp login page. One packet later (1125), theres a reply from the DNS server IP with a different IP for the acme corp login page.
- This now becomes a domain of interest

##

### DNS Requests For Domains

<img width="1440" height="500" alt="Screenshot 2026-02-05 at 5 41 48 PM" src="https://github.com/user-attachments/assets/bc3da334-9f99-4527-bf0c-6d0854228acd" />

This filter searches for the specific domain that a user has queried. Each box highlighted contains a request and a response to and from the DNS server.

##

### Filtering For DNS Responses Coming From Non-DNS Server

<img width="1440" height="242" alt="Screenshot 2026-02-05 at 5 49 32 PM" src="https://github.com/user-attachments/assets/c74e7194-a2ea-48cc-ba9b-40cf025bcd3f" />

Here, there's only 2 responses that originate from an IP other than the DNS server


## How many DNS responses were observed for the domain corp-login.acme-corp.local?

For starters, I'll begin with "dns.flags.response ==1" to filter for responses and use "dns.qry.name =='<given domain>'". I believe it doesn't matter if the response orginated from a legit source or if it was from the malicious user, just the total response count pertaining to the domain.

<img width="1440" height="790" alt="Screenshot 2026-02-05 at 5 55 07 PM" src="https://github.com/user-attachments/assets/6afa26d0-3813-4ffe-b1fb-c2654965f0f6" />


## How many DNS requests were observed from the IPs other than 8.8.8.8?

To filter for requests, I've set "dns.flags.response" to 0 and for IPs other than 8.8.8.8, I've used "ip.src !==8.8.8.8"

<img width="1440" height="790" alt="Screenshot 2026-02-05 at 6 00 45 PM" src="https://github.com/user-attachments/assets/389b9b70-d317-4e1b-abd3-65590f025541" />

### ⚠️ Mental Note:
Maybe I've made a mistake here or maybe its the question but the question states request, meaning that it isn't a response and does imply searching, as a response gives the user making the domain query (request) the information. Either way, I've set the flag to "2" and have gotten the supposed answer of 2. I love the ambiguity of tryhackme so much that it makes me want to rip my head off.

- 2 requests

## What IP did the attacker’s forged DNS response return for the domain?

I can just reference the previous picture and walkthrough to see that the IP's forged DNS response is:
- 192.168.10.55



# SSL Stripping

### Filtering for SSL Traffic

<img width="1440" height="790" alt="Screenshot 2026-02-09 at 7 47 51 PM" src="https://github.com/user-attachments/assets/bfe6ea30-7d60-46bd-80b0-1df830f2aea1" />
- Basic SSL traffic filtering

##

### Filtering for Specific SSL Traffic of a Server

<img width="1440" height="790" alt="Screenshot 2026-02-09 at 7 51 42 PM" src="https://github.com/user-attachments/assets/738f646f-7fe7-45e9-9069-c5350164ccb7" />

- Whats happening here is the filter is looking for all of the times the initial message was sent by the client to the server to start the TSL process.
  - TLS handshake type 1 refers to "Client Hello", where the client wants to establish connection which should expect in return from the server, a "Server Hello"
  - In the early stages of this packet with this filter applied, TLSv1.2 is the established protocol
 
##

### Confirmationn of DNS Redirection Prior to SSL Stripping

Ok so remember when the attacker was performing DNS spoofing which was to trick the DNS cache (cache poisoning, similar to ARP poisoning) where the attacker sends unwanted responses amidst query requests basically saying "the website you're looking for is at this malicious IP" in the midst of the real responses? 

This filter is to confirm by DNS - the suspicions that the malicious IP that was sending out those responses was able to later perform SSL stripping. 

<img width="1440" height="201" alt="Screenshot 2026-02-09 at 8 06 48 PM" src="https://github.com/user-attachments/assets/c89a1691-e304-472d-8522-ecb48206bb1a" />

- I see here that the IP ending in .55 is telling the requesting IP requesting that the domain resides at the attackers IP (.55)

##

### Verifying TLS Isn't Utilized

<img width="1440" height="201" alt="Screenshot 2026-02-09 at 8 16 33 PM" src="https://github.com/user-attachments/assets/90414818-9d53-4a6c-b05e-c4fca64299b0" />

<img width="1440" height="590" alt="Screenshot 2026-02-09 at 8 17 34 PM" src="https://github.com/user-attachments/assets/eb9fe616-68e7-4705-91ef-b2f419a7b6c9" />

- Here, I see the user IP ending in .10 is interacting with the malicious server from the IP ending in .55
- In the HTTP header, I see where the victim is at on the fake URL which happens to be the login page of the intended webpage
- The packets are consecutive, the following packet containing the user's credentials



## How many POST requests were observed for our domain corp-login.acme-corp.local?

To filter for this, the filter should contain POST being the http request method and then searching for the relative domain.

<img width="1440" height="763" alt="Screenshot 2026-02-09 at 8 33 25 PM" src="https://github.com/user-attachments/assets/78a8f33e-3fdf-412e-90d7-149d0449bc11" />

- 1



## What's the password of the victim found in the plaintext after successful ssl stripping attack. 

Staying where I am, because its "POST", it means information has been submitted. By expanding the HTML header, I'm able to see what the victim has entered.

<img width="1440" height="763" alt="Screenshot 2026-02-09 at 8 36 22 PM" src="https://github.com/user-attachments/assets/4b3e121d-7a30-4d47-a532-918d1e7a02fe" />

- Secret123!

## End
