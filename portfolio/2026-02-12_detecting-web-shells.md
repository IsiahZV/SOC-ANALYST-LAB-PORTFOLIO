# Detecting Web Shells
**Date:** 2026-02-11

**Objective:**



**Environment:** TryHackMe – “Detecting Web Shells” room  
**Tools Used:** Terminal


### 📎 Context
A webshell has been deployed at a certain server with the URL (i.e., http://MACHINE_IP:8080/files/awebshell.php)

<img width="1032" height="462" alt="Screenshot 2026-02-12 at 7 46 22 PM" src="https://github.com/user-attachments/assets/395dab32-03ad-4dd9-89bb-9dbe7594b4a5" />


## Web Shell Anatomy


### Access the shell and determine which account you have access to by running the whoami command.

<img width="1032" height="462" alt="Screenshot 2026-02-12 at 7 47 03 PM" src="https://github.com/user-attachments/assets/9295902e-fd93-4d93-a35a-26fc50fcc76c" />

- www-data


### List the directory contents and find the flag using the ls and cat commands.

<img width="1032" height="316" alt="Screenshot 2026-02-12 at 7 55 24 PM" src="https://github.com/user-attachments/assets/89bf0242-7b1d-465c-97cb-2012bb2bda9a" />
- Using the "ls" command

<img width="1032" height="325" alt="Screenshot 2026-02-12 at 7 59 13 PM" src="https://github.com/user-attachments/assets/6c0bed34-457a-42e2-a171-001beefd6bf0" />
- Using "cat "flag.txt"


## Investigation

**📎 You have been provided Apache access logs to help conduct your investigation.**
- /var/log/apache2/access.log


When reviewing the logs, remember to be on the lookout for:
- repeated or suspicious requests (especially ones to .php files)
- strange request patterns (look for different response codes)
- unusual user-agents (e.g. curl/0.00.0)


### Which IP address likely belongs to the attacker?

<img width="1372" height="748" alt="Screenshot 2026-02-17 at 6 18 56 PM" src="https://github.com/user-attachments/assets/99ec677b-a4a6-46fa-b377-170d666e65e8" />

To begin, this is what the log looks like. In this screenshot, it contains GET requests for a wordpress server.

The user activity seems to be normal at first glance. The status codes also seem to be in line with normal activity as well as other variables.

<img width="1372" height="748" alt="Screenshot 2026-02-17 at 6 26 32 PM" src="https://github.com/user-attachments/assets/8eb195f0-bddb-4201-ade8-d2f2a11b407c" />

Scrolling down, I see suspicious activtity coming from IP 203.0.113.66.
- Within the same second / minute, there are multiple "GET" requests on the web server
- Many status codes returning 404
- Suspicious User-Agent


### What is the first directory that the attacker successfully identifies?

<img width="1372" height="530" alt="Screenshot 2026-02-17 at 6 39 13 PM" src="https://github.com/user-attachments/assets/1c6066d9-7fca-4162-b1a5-010efd2c4612" />

<img width="1372" height="530" alt="Screenshot 2026-02-17 at 6 39 13 PM" src="https://github.com/user-attachments/assets/80f7fc79-06f3-4549-abd1-cf31a411adff" />

Technically it is HTTP, but besides that, its "wordpress"


### What is the name of the .php file the attacker uses to upload the web shell?
The key word here is "upload", so in relation to the suspicious IP address, I know I'm looking for "POST" related requests

<img width="1403" height="477" alt="Screenshot 2026-02-18 at 4 12 42 PM" src="https://github.com/user-attachments/assets/3a44f0ed-23ff-40bb-bd0c-9749856fc55b" />

After honing down and using some choppy filters, I've came across a POST request coming from the suspicious IP with the .php file being:
- upload_form.php

**Tip: Using "awk '{print $1, $6, $7}' access.log" will filter for the IP addresses, the request method, and the directory**


### What is the first command run by the attacker using the newly uploaded web shell?

By filtering for the IP, the request method, and "cmd" (which is used in GET requests when attackers are running commands through the uploaded .php file), I was able to see what commands were ran.

<img width="1403" height="477" alt="Screenshot 2026-02-18 at 4 28 19 PM" src="https://github.com/user-attachments/assets/e12da2ce-c95e-4d0d-8e84-2548d7893c34" />

The upload time (reference last question) was 06:09:27. 
- To correlate with this picture, the attackers first command was at 06:14:55 being "**whoami**"


### After gaining access via the web shell, the attacker uses a command to download a second file onto the server. What is the name of this file?

<img width="1403" height="477" alt="Screenshot 2026-02-18 at 4 32 42 PM" src="https://github.com/user-attachments/assets/60904abe-8d94-475a-a889-0cac0146c37c" />

When seeing a command containing "wget", its safe to assume that the attacker is attempting to download a file
- linpeas.php


### The attacker has hidden a secret within the web shell. Use cat to investigate the web shell code and find the flag.

Before answering this question, I want to establish that this lab is where I am acting on the machine that is hosting the compromised web server. Because the web server is compromised does not mean the machine is fully compromised but should be treated as such until proven otherwise (although it is only running as www-data which should contain minimal permissions pertaining to the system itself).

- First, I know what the .php filename is used in for webshell
- Since I'm working on the machine that hosts the web server, I can copy the .php file in the directory that it was uploaded into a directory where I can cat it

<img width="1415" height="494" alt="Screenshot 2026-02-18 at 5 19 02 PM" src="https://github.com/user-attachments/assets/cd2a3ef4-618f-4724-b228-37b1d21aa4be" />


## Likely Incident

An attacker performs directory discovery scanning on a website where they find /wordpress returns a 200 status code. The attacker is now aware that it is installed on the machine that hosts the web server. The attacker now has to discover the version of wordpress, what plugins are installed, and if its outdated (can be done by viewing page source) and utilize a CVE to exploit a known vulnerability. They also discover /wordpress/wp-content/uploads/uploads.php (which a .php file shouldnt be there anyway.. but this could mean that a vulnerable plugin placed it, a custom theme added it, or its for the sake of the lab). The attacker sends a POST request using curl and the server saved the shadyshell.php file. The server returns a status code of 200 and the attacker then executes commands via GET


## Incident Response Suggestions

- The web server is compromised
- Credentials on that system are considered exposed
- Lateral movement may have occurred
- The system may need full rebuild

===
- Isolate the machine
- Take forensic image
- Rebuild from clean backup
- Rotate all credentials
- Patch vulnerability that allowed upload

