# Network Discovery Detection
**Date:** 2026-01-27

**Objective:**
-

**Environment:** TryHackMe – “Network Discovery Detection” room  
**Tools Used:** 

##
In the attached VM, open a terminal and navigate to /home/ubuntu/Downloads/logs. There will be a few CSV files in this directory, that have been exported from a SIEM solution. While the logs contain one file with sanitized output, the others might feel hard to read. However, this is how real life log files will often look when extracted from security appliances. We can use the head command to get a preview of the contents of the files.
##


### Which file contains logs that showcase internal scanning activity?

<img width="567" height="155" alt="Screenshot 2026-01-27 at 8 04 05 PM" src="https://github.com/user-attachments/assets/515cd8eb-8040-43f2-899d-c16ba6e5ee2a" />

The keyword here is internal so I'd be looking for private IP addresses

<img width="1400" height="333" alt="Screenshot 2026-01-27 at 8 08 53 PM" src="https://github.com/user-attachments/assets/a7b9d975-e834-4aa2-be0a-b19c9737c9ad" />

This picture is showing 3 of the many other alerts in this .csv file. I see a host using just one connection (port 52424) showing scanning characteristics such as communication with various hosts in such a short timeframe. Other .csv files show external IP addresses as the source.

- log-session-2.csv



### How many log entries are present for the internal IP performing internal scanning activity?

For this, the question provides a hint relative to the command to apply: cat log-session-x.csv |cut -d "," -f3 |uniq -c
I'll apply it to the previous .csv log file
- For clarification, "-f3" in this command entails selecting the third field and uniq -c removes duplicate lines and groups them all into one.
- In the picture below, note how the first value is "source.port" and then the IP following. There may be something wrong with how the data rows are presented since "-f2" would be the accurate selection as the second variable after "@timestamp" is "source.ip".

<img width="834" height="318" alt="Screenshot 2026-01-28 at 5 49 13 PM" src="https://github.com/user-attachments/assets/7b61972b-113c-46a8-80c9-088a56eff7d2" />

- 192.168.230.127 has 2276 log entries



### What is the external IP address that is performing external scanning activity?

<img width="834" height="318" alt="Screenshot 2026-01-28 at 6 07 20 PM" src="https://github.com/user-attachments/assets/0dc0581f-fbae-45e7-97f4-1ffef6fd1cf2" />

In logs both 0 and 1, the generated alerts for the only external IP to be in play here is: 
- 203.0.113.25

