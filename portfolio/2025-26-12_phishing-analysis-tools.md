**Date:** 2025-12-09

**Objective:** 
- Gain familiarity with tools that aid in examining email header info
- Utilize techniques to obtain hyperlinks in emails and expand shortened URLs
- Use OSINT tools to obtain information on potentially malicious links without directly interacting
- Obtain malicious attachments from phishing emails and use malware sandboxes to detonate attachments

**Environment:** TryHackMe – “Phishing Analysis Tools” room  
**Tools Used:** CyberChef, AnyRun


# Case 1
**You are a Level 1 SOC Analyst. Several suspicious emails have been forwarded to you from other coworkers. You must obtain details from each email for your team to implement the appropriate rules to prevent colleagues from receiving additional spam/phishing emails. # Analyzing Phishing Emails**

As a brief statement for understanding, Thunderbird is being used which is an open-source email client that CAN connect to Microsoft and Yahoo email accounts. You can configure the IMAP mail client (Thunderbird) to access emails on those accounts. 

## What brand was this email tailored to impersonate?

<img width="733" height="570" alt="Screenshot 2025-12-26 at 5 42 53 PM" src="https://github.com/user-attachments/assets/1a0b5602-d0f2-434f-ba20-b82bfb44da85" />

- Netflix



## What is the From email address?

Because the VM I'm working in isn't allowing web searching, I have to manually retreive the information.

I've selected to view the source code and search for "From:" 

<img width="1273" height="712" alt="Screenshot 2025-12-26 at 5 58 02 PM" src="https://github.com/user-attachments/assets/f276e334-b4b2-4adc-be8f-120600923066" />

- JGQ47wazXe1xYVBrkeDg-JOg7ODDQwWdR@JOg7ODDQwWdR-yVkCaBkTNp[.]gogolecloud[.]com



## What is the originating IP? Defang the IP address. 

<img width="725" height="706" alt="Screenshot 2025-12-26 at 6 01 45 PM" src="https://github.com/user-attachments/assets/9b84f171-5ac9-4322-8bec-981312e37bea" />

For this, I'll search for "X-Originating-IP"
- 209[.]85[.]167[.]226



## From what you can gather, what do you think will be a domain of interest? Defang the domain.

To search for the domain, I'll enter "Smtp.mailfrom

<img width="731" height="680" alt="Screenshot 2025-12-26 at 6 06 05 PM" src="https://github.com/user-attachments/assets/e4d38609-c766-4459-a32f-60a86a9d000e" />

- etekno[.]xyz



## What is the shortened URL? Defang the URL.

<img width="795" height="683" alt="Screenshot 2025-12-26 at 6 13 22 PM" src="https://github.com/user-attachments/assets/51a60743-2dce-458a-8f3e-a9f04effe4aa" />

While using cyberchef to extract URLs, I see that there are 3 other shortened URLs so this question isn't really specific and while submitting the answer, it automatically changes into an adjusted answer which is subtly off from what's displayed here. (Maybe creator / cyberchef issue).

- hxxps[://]t[.]co/yuxfZm8KPg?amp==1

**SIDENOTE:**
- I've figured that they are talking about the link in relation to the "Update account now" button, in which cyberchef didn't extract this. However, by right clicking the buttong and copying the link location, I was able to identify this specific URL

##

# Case 2
**Task: Investigate the analysis and answer the questions below.**
- Link: https://app.any.run/tasks/8bfd4c58-ec0d-4371-bfeb-52a334b69f59


## What does AnyRun classify this email as?

In AnyRun, by clicking on the "Text Report" section in the right column, I can see the full report containing the General Info, Behavior Activities, Malware Configuration, Static Information, Videos & Screenshots, Processes, Behavior Graph, Process information, Registry activity, etc. 

<img width="1440" height="818" alt="Screenshot 2025-12-28 at 1 09 46 PM" src="https://github.com/user-attachments/assets/9fbfaf8a-376e-4b7b-b5a7-36c60fab22ed" />
<img width="1142" height="570" alt="Screenshot 2025-12-28 at 1 14 54 PM" src="https://github.com/user-attachments/assets/c40f0645-ab6f-44cd-b919-c435024a8848" />

- Suspicious Activity


## What is the name of the PDF file?

- Payment-updateid.pdf


## What is the SHA 256 hash for the PDF file?

<img width="1142" height="570" alt="Screenshot 2025-12-28 at 1 16 28 PM" src="https://github.com/user-attachments/assets/727fcc12-6edd-4cf7-8419-4c14d8673810" />

- CC6F1A04B10BCB168AEEC8D870B97BD7C20FC161E8310B5BCE1AF8ED420E2C24



## What two IP addresses are classified as malicious? Defang the IP addresses. (answer: IP_ADDR,IP_ADDR)

In the **connections** section, I get a list of processess that have made or attempted to make network connections.

As a brief description of this field, it displays processes that have ran and have made a network connection, in this case, would suggest a malicious or weaponized PDF. This field helps determine initial access, execution, and C2 behavior.

<img width="1142" height="570" alt="Screenshot 2025-12-28 at 1 16 28 PM" src="https://github.com/user-attachments/assets/7864fb93-a62a-44ea-847a-25c600abc052" />
- 2[.]16[.]107[.]24,2[.]16[.]107[.]83


In this context, the first process established / attempted a network connection to an Adobe related domain flagged as malicious by AnyRun. 

The second highlighted process marked as malicious utilzed the svchost.exe which is commonly abused.
- Adobe update and download traffic is handled by Adobe services, **not** by OS service hosts (svchost.exe -> ardownload3.adobe.com)
  -  In this scenario, something can be pretending to be real network traffic

**Context:**
- Adobe reader process enabling network connection -> now svchost.exe makes network connection
- User opens PDF file, Adobe Reader is exploited -> malicious code could exploit svchost.exe in some way (process injection) where the process makes outbound network traffic that utilizes an Adobe-like domain to blend in with real network traffic

Because these domains are legit, this could be marked as malicious because of behavior rather than destination, as this domain can be abused for larger purposes such as a malicious execution chain.



## What Windows process was flagged as Potentially Bad Traffic?

In the **Threats** section we see exactly that where svchost.exe was utilized in this process to initiate network communication

<img width="1142" height="468" alt="Screenshot 2025-12-28 at 2 13 05 PM" src="https://github.com/user-attachments/assets/cc9e776d-3acd-4018-b0c3-4dc1b8441e7a" />

- svchost.exe



# Case 3
**Task: Investigate the analysis and answer the questions below.**
Link: https://app.any.run/tasks/82d8adc9-38a0-4f0e-a160-48a5e09a6e83

## What is this analysis classified as?

<img width="1142" height="688" alt="Screenshot 2025-12-28 at 2 56 33 PM" src="https://github.com/user-attachments/assets/27401bc1-9935-420d-9fc2-1892389376f9" />

- Malicious Activity



## What is the name of the Excel file?
At the very top - below "General Info", lies the **File Name**: "CBJ200620039539.xlsx" 



## What is the SHA 256 hash for the file?
- 5F94A66E0CE78D17AFC2DD27FC17B44B3FFC13AC5F42D3AD6A5DCFB36715F3EB



## What domains are listed as malicious? Defang the URLs & submit answers in alphabetical order. (answer: URL1,URL2,URL3)

<img width="1142" height="688" alt="Screenshot 2025-12-28 at 2 56 33 PM" src="https://github.com/user-attachments/assets/73f8f6e7-f304-44b1-a979-2a2f4a4e888f" />

- biz9holdings[.]com,findresults[.]site,ww38[.]findresults[.]site



## What IP addresses are listed as malicious? Defang the IP addresses & submit answers from lowest to highest. (answer: IP1,IP2,IP3)
For this, I'm just referencing the previous screenshot

- 75[.]2[.]11[.]242,103[.]224[.]182[.]251,204[.]11[.]56[.]48



## What vulnerability does this malicious attachment attempt to exploit?
The text report offers the CVE number which is:
- cve-2017-11882 -> Microsoft Office Memory Corruption Vulnerability
  - "Microsoft Office 2007 Service Pack 3, Microsoft Office 2010 Service Pack 2, Microsoft Office 2013 Service Pack 1, and Microsoft Office 2016 allow an attacker to run arbitrary code in the context of the current user by failing to properly handle objects in memory-"



# End
