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

