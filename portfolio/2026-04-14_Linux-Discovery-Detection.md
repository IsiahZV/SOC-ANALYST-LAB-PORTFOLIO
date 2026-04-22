# Detecting Discovery Techniques in Linux Systems
**Date:** 2026-04-014

**Objective:**
- Explore how to identify Discovery commands in logs
- Learn common threats endangering Linux servers
- Know how attackers upload malware onto their victims
- Uncover a real cryptominer attack

**Environment:** TryHackMe – “Linux Threat Detection 2” room  
**Tools Used:** CLI

---

## DISCOVERY OVERVIEW

### What is the command's output you discovered?
> Run systemd-detect-virt to detect the system's cloud. 

<img width="526" height="142" alt="Screenshot 2026-04-14 at 8 16 53 PM" src="https://github.com/user-attachments/assets/11ff6bcd-b0a5-48ff-b6fa-daf1f2258e41" />

- amazon

#### Whats the importance of this command?
"systemd-detect-virt" checks the environment and tells you if you're running on physical hardware or a virtual machine. It also displays what virtualization technology is being used (in this case, amazon or AWS virtual machine. In this case, if an attacker learns that this is a cloud-based VM, they may for valuable IAM roles and abuse services (EC2 is common to see in the world of lateral movements and S3 for data exfiltration).

This command may also be ran to avoid sandbox detection and adjust payloads for cloud environments. 

##

### What is the full path to the detected antimalware binary?
> Now run ps aux and look for EDR or antivirus processes.

<img width="1424" height="782" alt="Screenshot 2026-04-14 at 8 32 21 PM" src="https://github.com/user-attachments/assets/35211c03-80e5-4302-a1ef-6e6d4956720c" />


---


## DETECTING DISCOVERY
> For this task, imagine you received a SIEM alert about a spike in Discovery commands.
The first thing you see is that the itsupport user launched the hostname command.
Can you continue the investigation on the VM and find out what really happened?

### What is the path of the script that initiated the "hostname" command?

<img width="1440" height="417" alt="Screenshot 2026-04-15 at 11 55 16 AM" src="https://github.com/user-attachments/assets/b5956e8f-5922-457d-a080-6790ee6ef484" />

- /home/itsupport/debug.sh

##

### What was the last Discovery command launched by the script?

I'll be explaining this for my sake.

<img width="1440" height="783" alt="Screenshot 2026-04-15 at 12 10 07 PM" src="https://github.com/user-attachments/assets/f2932dcd-59d4-4c22-964f-c7386c73d4c0" />

Keyword is discovery command

<img width="1440" height="783" alt="Screenshot 2026-04-15 at 12 26 53 PM" src="https://github.com/user-attachments/assets/b6f02531-e319-45d1-aee8-9bc8db7cdaca" />
- ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu

##

### Looking at the script content, what's the email of the script author?

<img width="708" height="135" alt="Screenshot 2026-04-15 at 12 58 55 PM" src="https://github.com/user-attachments/assets/f480bb3b-b7ff-4010-8a87-de9c728b5dec" />

<img width="879" height="167" alt="Screenshot 2026-04-15 at 1 01 59 PM" src="https://github.com/user-attachments/assets/2ed52916-e7a0-4c6b-863c-668034f4f8c9" />

<img width="883" height="674" alt="Screenshot 2026-04-15 at 1 02 38 PM" src="https://github.com/user-attachments/assets/f6d6e4c2-a136-4e53-95b9-0ac662a9a522" />

- greg@tryhackme.thm


---


## MOTIVATION FOR ATTACKS

> For this task, look for commands on the VM that may indicate Ingress Tool Transfer.

### From which domain was the Elastic agent downloaded?

This question will prompt to either use ausearch for traces of "wget", "curl", or "scp". In this case, its "wget" thats shown to correlate with the command sent to download the Elastic agent.

<img width="1423" height="169" alt="Screenshot 2026-04-21 at 3 14 50 PM" src="https://github.com/user-attachments/assets/8b554360-59bd-465b-a3b9-bda24f162e63" />

- artifacts.elastic.co

##

### What is the full path to the downloaded "helper.sh" script?

<img width="1423" height="200" alt="Screenshot 2026-04-21 at 3 20 56 PM" src="https://github.com/user-attachments/assets/028bcd24-c3fa-4b94-90f2-d399a21092f4" />

In this scenario, curl was ran, making an HTTP request to download a remote file and saving it into the /var/tmp file path. The name "drobbox" could be used as a form of typosquatting as a means to look legit (or simply for identification sake due to the lab), with this specific action being directed to a temporary folder on the host device. 

- /var/tmp/helper.sh

##

### Which of the downloaded files is more likely to be malicious: The one downloaded with curl or wget?

Without using virustotal or any other resource, I would say that the file that was downloaded into /var/tmp is more likely to be malicious. In this case, curl.


---


## DOTA3: FIRST ACTIONS
> For this task, try detecting a similar cryptominer infection chain in the VM. Open the /home/ubuntu/scenario folder and use the logs inside.

### Which IP address managed to brute-force the exposed SSH?
As stated in the previous lab (intial access), when logging in via SSH, trusted users typically access it via publickey (SSH keys) instead of a password which is a red flag.

<img width="1423" height="200" alt="Screenshot 2026-04-21 at 4 24 55 PM" src="https://github.com/user-attachments/assets/41eb4f32-3d00-4355-8a78-706224fc1d65" />

- 45.9.148.125

##

### Which command did the attacker use to list the last logged-in users?

I've tried hard to figure out where this answer would be and by checking online sources to verify, it seems that other learners are struggling at this point as well while those that do have an answer, dont explain how they got to it. Regardless, the command to list the last logged-in users is "last".

##

### Which three EDR processes did the attacker look for with "egrep"?
I've found that not using auditd and cat-ing the audit log instead has made searches faster. Plus, it simply wont pick up on "egrep" alone. This is also probably because I'm not too familiar yet with using ausearch.

<img width="1423" height="143" alt="Screenshot 2026-04-21 at 5 06 27 PM" src="https://github.com/user-attachments/assets/cc317560-ecb1-4e36-91a2-5920d9c10f3b" />

- ds_agent,falcon,sentinel

#### Research on problem:
- Executed program: /bin/sh
- Arguments passed: /usr/bin/egrep , --color=auto , falcon|sentinel|ds_agent
- A shell (/bin/sh) was launched, and it was told to run an egrep command searching for EDR-related process names.

- Dont use ausearch -i **-x** because egrep alone was never directly executed, it was invoked through the shell
  - What really was the executable was /bin/sh

After doing research, it still seems that using cat is just way more proficient since ausearch likes to be very specific. Ausearch can be very good to search for specific suspicious discovery and other well known commands to do process reconstruction, however, it was very time consuming for me to try and discover where and how this answer came to be. 


---


## DOTA3: MINER SETUP
> Continue the analysis of the cryptominer from the previous task and uncover its last steps
### What is the name of the malicious archive that was transferred via SCP?

<img width="1423" height="179" alt="Screenshot 2026-04-22 at 5 51 59 PM" src="https://github.com/user-attachments/assets/b825ea67-17c5-4d9d-8617-d2e008c937b9" />

Full command is: tar xzf kernupd.tar.gz -C /tmp/.apt
- The attacker is unarchiving and extracting (tar, x(extract), z(using gzip), f(file name)) kernupd.tar.gz into a directory in /tmp
- Research notes extracting archives is a sign of: staging, priv. escalation, persisistence

#### Ausearch Research / Fix
(I fucking hate ausearch)
- ausearch -i -if /home/ubuntu/scenario/audit.log | grep “PROCTITLE”
  - Skim through results and see what looks suspicious in the directory

##

### What was the full command line of the cryptominer launch?

<img width="1423" height="494" alt="Screenshot 2026-04-22 at 6 29 52 PM" src="https://github.com/user-attachments/assets/dc945e7a-2625-4d7a-93d1-ad6a5c634b63" />

- nohup /tmp/.apt/kernupd/kernupd

##

### Which IP address range did the attacker scan for an exposed SSH?

I have a bone to pick with THM due to the fact that the training made it seem like the scan would be prompted within the created directory (nohup /tmp/.X26-unix/.rsync/c/tsm -p 22 [...] /tmp/up.txt 192.168 >> /dev/null 2>1&); which made it seem as if you could search based off of context from something that was already built off of such as "nohup" or "/tmp/", additionally, theres no adequate training on ausearch and its use case. In the preceding lab, there seemed to be no use to utilize "-if </directory to audit log> | grep <filter>", in which the only time they introduced this kind of command is through a hint. Additionally, with the praise of AI, it was not able to help in this scenario.

<img width="1423" height="542" alt="Screenshot 2026-04-22 at 7 04 44 PM" src="https://github.com/user-attachments/assets/f973fd64-b44b-49b7-a01f-c9f64d554ecd" />

- 10.10.12.1-10.10.12.10

---

