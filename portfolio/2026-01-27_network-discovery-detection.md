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
