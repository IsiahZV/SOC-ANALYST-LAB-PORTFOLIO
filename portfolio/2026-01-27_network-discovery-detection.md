# Network Discovery Detection
**Date:** 2026-01-27

**Objective:**
-

**Environment:** TryHackMe – “Network Discovery Detection” room  
**Tools Used:** Elastic

##
In the attached VM, open a terminal and navigate to /home/ubuntu/Downloads/logs. There will be a few CSV files in this directory, that have been exported from a SIEM solution. While the logs contain one file with sanitized output, the others might feel hard to read. However, this is how real life log files will often look when extracted from security appliances. We can use the head command to get a preview of the contents of the files.
##


### Which file contains logs that showcase internal scanning activity?

<img width="567" height="155" alt="Screenshot 2026-01-27 at 8 04 05 PM" src="https://github.com/user-attachments/assets/515cd8eb-8040-43f2-899d-c16ba6e5ee2a" />

The keyword here is internal so I'd be looking for private IP addresses

<img width="1400" height="333" alt="Screenshot 2026-01-27 at 8 08 53 PM" src="https://github.com/user-attachments/assets/a7b9d975-e834-4aa2-be0a-b19c9737c9ad" />

This picture is showing 3 of the many other alerts in this .csv file. I see a host using just one connection (port 52424) showing scanning characteristics such as communication with various hosts in such a short timeframe. Other .csv files show external IP addresses as the source.

- log-session-2.csv

##

### How many log entries are present for the internal IP performing internal scanning activity?

For this, the question provides a hint relative to the command to apply: cat log-session-x.csv |cut -d "," -f3 |uniq -c
I'll apply it to the previous .csv log file
- For clarification, "-f3" in this command entails selecting the third field and uniq -c removes duplicate lines and groups them all into one.
- In the picture below, note how the first value is "source.port" and then the IP following. There may be something wrong with how the data rows are presented since "-f2" would be the accurate selection as the second variable after "@timestamp" is "source.ip".

<img width="834" height="318" alt="Screenshot 2026-01-28 at 5 49 13 PM" src="https://github.com/user-attachments/assets/7b61972b-113c-46a8-80c9-088a56eff7d2" />

- 192.168.230.127 has 2276 log entries

##

### What is the external IP address that is performing external scanning activity?

<img width="834" height="318" alt="Screenshot 2026-01-28 at 6 07 20 PM" src="https://github.com/user-attachments/assets/0dc0581f-fbae-45e7-97f4-1ffef6fd1cf2" />

In logs both 0 and 1, the generated alerts for the only external IP to be in play here is: 
- 203.0.113.25

##

### One of the log files contains evidence of a horizontal scan. Which IP range was scanned? Format X.X.X.X/X

Horizontal scanning has characteristics of:
- Same source IP
- Multiple destination IP
- Same port

After looking through the logs, and using the command: 
- cat log-session-2.csv | cut -d "," -f3,4,5,6 | uniq -c

I was able to filter for the source IP, the source port (which really doesn't matter), the destination IP, and the destination port).

<img width="1147" height="740" alt="Screenshot 2026-01-28 at 6 51 49 PM" src="https://github.com/user-attachments/assets/263a426b-65a6-4fb4-9400-841741d9cc11" />
<img width="1141" height="198" alt="Screenshot 2026-01-28 at 6 53 13 PM" src="https://github.com/user-attachments/assets/63a25248-0d4f-4685-9322-4d87f272a2c8" />


The IP range scanned in this scenario would be 203.0.113.0/24

##

### In the same log file, there is one IP address on which a vertical scan is performed. Which IP address is this?

Vertical scanning has the characteristics of:
- Same source IP
- Same destination IP
- Multiple ports

<img width="1141" height="736" alt="Screenshot 2026-01-28 at 7 16 43 PM" src="https://github.com/user-attachments/assets/2dde9f35-720f-4e28-9478-dc81a0cee016" />

This is shortly after the break I displayed in the last picture. The internal destination IP is the same and has various destination ports.

- 192.168.230.145

##

### On one of the IP addresses, only a few ports are scanned which host common services. Which are the ports that are scanned on this IP address? Format: port1, port2, port3 in ascending order.

Maybe I'm stupid or maybe the question is worded to vaguely (I'm going to go with vague because their hint gives you a command to help you rule out IP addresses based on some alert count) 

The command given is: cat log-session-x.csv |cut -d "," -f5|grep -v "203.0"|grep -v "230.145"|sort|uniq -c
- f5 in this case extracts the destination port row but instead it pulls from the destination IPs
- "grep -v" is **excluding** IPs that contain that value (anywhere in the IP really. Use "^" prior to use it as starting) that do not match whats entered in quotations

<img width="1041" height="152" alt="Screenshot 2026-01-29 at 4 42 53 PM" src="https://github.com/user-attachments/assets/7872af7a-6d01-453a-a322-f45e90ac621f" />

This internal destination IP address has appeared in 9 alerts 

The command I used is:
- cat log-session-2.csv | cut -d "," -f2,3,4,5,6 | grep -w "192.168.230.1" | sort

<img width="1122" height="258" alt="Screenshot 2026-01-29 at 4 58 17 PM" src="https://github.com/user-attachments/assets/b7281d3d-5c2e-43f0-b022-9e73543a7653" />

The ports scanned in ascending order are: 80, 445, 3389

##

### Which source IP performs a ping sweep attack across a whole subnet?

Ping sweeps are noted as the sending host sending ICMP packets to the destination. This question is emphasizing that the attacker is trying to see which host is running and will respond, not port services on a specific host.

<img width="1440" height="796" alt="Screenshot 2026-01-30 at 6 57 21 AM" src="https://github.com/user-attachments/assets/b5db6b6a-b3d7-4575-8ff7-1596d169b957" />

Here, I've noted three things.
- Short incriments of time in between each alert
- The destination IP being involved is scanned all the way until 192.168.230.254
- All protocols are ICMP

The source IP that performed this was:
- 192.168.230.127

##

### The zeek.conn.conn_state value shows the connection state. Using the information provided by this value, identify the type of scan being performed by 203.0.113.25 against 192.168.230.145

<img width="1440" height="796" alt="Screenshot 2026-01-30 at 7 35 54 AM" src="https://github.com/user-attachments/assets/989df2d3-7e62-4a98-9af8-39fdcb9916e1" />

Some characteristics of this scan is:
- Same source IP -> Same destination IP
- Source IP targeting different ports on destination IP
- Short incriments of of time in between each alert

This lab doesnt cover zeek connection values so I did some research and under "zeek.conn_state" the "SO" means a connection attempt seen with no reply.

This would be classified as a:
- TCP SYN scan

##

### Is there any UDP scanning attempt in the logs? Y/N

<img width="1440" height="796" alt="Screenshot 2026-01-30 at 7 41 14 AM" src="https://github.com/user-attachments/assets/95447868-447e-4256-9468-b70738cdadf8" />

There are no seen instances of scanning activity involving the UDP protocol
- N
