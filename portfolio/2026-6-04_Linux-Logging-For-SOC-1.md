# Linux Logging for SOC
**Date:** 2026-04-06

**Objective:**
- Explore authentication, runtime, and system logs on Linux
- Learn the commands and pitfalls when working with logs
- Uncover how tools like auditd monitor and report the events


**Environment:** TryHackMe – “Linux Logging for SOC” room  
**Tools Used:** CLI

---

## WORKING WITH TEXT LOGS
> Use the /var/log/syslog file on the VM to answer the questions.
### Which time server domain did the VM contact to sync its time?

For this question, I'm going to grep for the words "time" and "server":
<img width="1440" height="160" alt="Screenshot 2026-04-06 at 3 03 01 PM" src="https://github.com/user-attachments/assets/58e084de-2705-45f6-a119-d438d19ab619" />
- ntp.ubuntu.com


##


### What is the kernel message from Yama in /var/log/syslog?

<img width="1440" height="160" alt="Screenshot 2026-04-06 at 3 05 03 PM" src="https://github.com/user-attachments/assets/fd95a9a1-c779-46b2-ae51-7bd0fe34ba86" />

- Becoming mindful


---

## AUTHENTICATION LOGS
> Continue with the VM and use the /var/log/auth.log file.
### Which IP address failed to log in on multiple users via SSH?

I'll be using grep to filter for "sshd" and "failed"
<img width="1278" height="176" alt="Screenshot 2026-04-06 at 3 34 40 PM" src="https://github.com/user-attachments/assets/636eb722-70fe-45b3-8645-aa134d8746b0" />
- 10.14.94.82


##


### Which user was created and added to the "sudo" group?

I'll be using "useradd" and "usermod" to figure out which user was first created (useradd) and then to which group the user was added (usermod)
<img width="1423" height="143" alt="Screenshot 2026-04-06 at 3 42 03 PM" src="https://github.com/user-attachments/assets/f5aceae3-5120-495e-91cd-bbfcb1137321" />
- Xerxes

---

## COMMON LINUX LOGS

### According to the VM's package manager logs, which version of unzip was installed on the system?
To find the answer, I have to access the directory where the package manager logs are located which can either be: 
- /var/log/apt
- /var/log/yum.log

I'll then cat the history.log file and search for "unzip":
<img width="1423" height="143" alt="Screenshot 2026-04-06 at 5 14 36 PM" src="https://github.com/user-attachments/assets/4b2c9cdb-77c7-4aee-a5fa-861a90912e74" />
- 6.0-28ubuntu4.1


##


### What is the flag you see in one of the users' bash history?
I'm not sure if I'm overthinking this or what but here's the list of users:

<img width="1423" height="744" alt="Screenshot 2026-04-06 at 5 27 41 PM" src="https://github.com/user-attachments/assets/096766ba-7847-4af6-98e0-474310112d02" />

#### 🔧 Problem Fix
Lab will only cover searching for the .bash_history file locally, user will have to "sudo grep" a word of context from the list of users in hopes that something was logged. Although I didn't begin with root (I should've), if the question was more specific, it could save users some time while having them still understand the process behind it.

<img width="601" height="81" alt="Screenshot 2026-04-06 at 5 44 17 PM" src="https://github.com/user-attachments/assets/c644e1b0-f3ac-4fbb-bd7c-722f83533ab8" />
- THM{note_to_remember}

---

## RUNTIME MONITORING

### 
