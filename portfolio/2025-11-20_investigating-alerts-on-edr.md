# Endpoint Detection Response Alert Investigation

**Date:** 2025-11-20

**Objective:** Gain familiarity in triaging alerts with an EDR console and how endpoint security works

**Environment:** TryHackMe – “Endpoint Detection and Response” room  
**Tools Used:** Simulated EDR console

## EDR Console Preview

<img width="1440" height="816" alt="Screenshot 2025-11-20 at 6 14 36 PM" src="https://github.com/user-attachments/assets/6d1bc756-3f5f-4577-a36b-e8967f2ed022" />


## Question 1: Which tool was launched by CMD.exe to download the payload on DESKTOP-HR01? 

I'll begin by previewing in the details regarding DESKTOP-HR01 and review the Process Info section

<img width="1368" height="636" alt="Screenshot 2025-11-20 at 6 16 52 PM" src="https://github.com/user-attachments/assets/43b4b22c-6391-470b-90ce-8bf95bdf786a" />


<img width="1368" height="797" alt="Screenshot 2025-11-20 at 6 16 07 PM" src="https://github.com/user-attachments/assets/077ade5c-97ad-401d-a7ea-56b95dca130e" />

To summarize, the tool *launched by* CMD.exe was **cURL.exe** as seen in the process chain.


## Question 2: What is the absolute path to the downloaded malware on the DESKTOP-HR01 machine?

While remaining in the **Process Info** section, by clicking on the "Install.exe" part of the chain, it reveals on which path was this process ran.

<img width="1368" height="727" alt="Screenshot 2025-11-20 at 6 20 21 PM" src="https://github.com/user-attachments/assets/84b6bcff-ab70-4688-9bbe-e8661bf4ed2d" />

The path shows: C:\Users\Public\install.exe


## Question 3: What is the absolute path to the suspicious syncsvc.exe on the WIN-ENG-LAPTOP03 machine?

<img width="1368" height="800" alt="Screenshot 2025-11-20 at 6 22 12 PM" src="https://github.com/user-attachments/assets/712a7c0d-650a-4d7b-aaa7-acc89bef1134" />

The absolute path that syncsvc.exer ran on was: C:\Users\haris.khan\AppData\Local\Temp\syncsvc.exe


## Question 4: On which URL was the exfiltration attempt being made on WIN-ENG-LAPTOP03?

With the same picture provided, this can be found 
