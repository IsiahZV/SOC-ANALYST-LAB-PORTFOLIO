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

## USING AUDITD

### When was the secret.thm file opened for the first time? (MM/DD/YY HH:MM:SS)
> File in question
<img width="1494" height="962" alt="image" src="https://github.com/user-attachments/assets/5c78e8b6-ddf1-499c-8574-a2b49ade616f" />

In the note attached states that access to the file is logged with the "file_thmsecret" key, this means that I have to incorporate this into  my ausearch command:
<img width="2222" height="698" alt="image" src="https://github.com/user-attachments/assets/978a8978-902b-447c-9ad1-9e0ce1571387" />

Highlighted is the file of interest but more importantly, the date and time that shows when it was opened:
- 08/13/25 18:36:54


#### 🧠 Suggestions
• Pull the earliest secret-file access quickly
  Use ausearch -i -k file_thmsecret | awk '/msg=audit\(/ {print $0}' | head -n1 to grab the earliest event quickly.


##


### What is the original file name downloaded from GitHub via wget?
> Wget process creation is logged with the "proc_wget" key.

Similar processs as the previous except I'll include "grep 'github'" so that I can hone down on the searches that might've been logged:
<img width="2225" height="248" alt="image" src="https://github.com/user-attachments/assets/b641757a-ae14-4458-b02b-132cbf710fca" />

- naabu_2.3.5_linux_amd64.zip


##


### Which network range was scanned using the downloaded tool?
> There is no dedicated key for this event, but it's still in auditd logs.

Given the context (I know I could research the file for confirmation sake, but to stay inside lab parameters), I'll assume that the previous question means that it was the tool of question.

Because there is no dedicated key / argument for this search, I'll remove -k from the command, use "ausearch -i" as a standalone and grep "naabu"

<img width="2212" height="474" alt="image" src="https://github.com/user-attachments/assets/5c212690-c673-4217-bd2b-27486c4a873a" />

This search alone, you can easily reconstruct the events where the threat actor:
- Downloads the tool from github with wget
- unzips the .zip file
- The file obtains write properties
- Then, the file is used to scan **192.168.50.0/24** and I'm assuming to display the top ports that are used in the results


#### 🧠 Future Fixes & Benefit
Tools may be renamed to evade simple keyword searches; correlate by exe=/path and parent process lineage (pid/ppid) rather than name alone. This supports reconstructing attacker reconnaissance targets from process execution logs.

Recovering command-line arguments that reveal reconnaissance scope is a practical way to turn raw Linux telemetry into investigation-ready findings. This kind of pivoting across audit logs is a strong signal of triage capability under time pressure.

Flow:
- Searched audit logs without relying on a key to locate tool execution
- Used grep filtering to surface argument-bearing records that include the scan target range

----


