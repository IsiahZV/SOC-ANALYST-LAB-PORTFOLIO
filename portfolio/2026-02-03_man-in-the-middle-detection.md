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
