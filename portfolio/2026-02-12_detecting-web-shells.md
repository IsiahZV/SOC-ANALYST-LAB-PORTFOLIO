# Detecting Web Shells
**Date:** 2026-02-11

**Objective:**



**Environment:** TryHackMe – “Detecting Web Shells” room  
**Tools Used:** Wireshark


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


## 

