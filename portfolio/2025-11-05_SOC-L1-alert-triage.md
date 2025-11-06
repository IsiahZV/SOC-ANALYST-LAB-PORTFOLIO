# SOC L1 Alert Triage

**Date:** 2025-11-05

**Objective:** Gain familiarization in triaging alerts as an L1 analyst through SOC simulations. 

**Environment:** TryHackMe – “SOC L1 Alert Triage” room  
**Tools Used:** Simulated SIEM tool


## Events & Alerts
**Question 1:** What is the number of alerts you see in the SOC dashboard?

- Five alerts are displayed from the SIEM dashboard
<img width="1440" height="717" alt="Screenshot 2025-11-05 at 1 39 35 PM" src="https://github.com/user-attachments/assets/ea087045-b09f-4cb1-9240-ed820ab90f94" />

**Question 2:** What is the name of the most recent alert you see?

- Referencing previous picture, it shows "Double-Extension File Creation"


## Alert Properties
**Question 1:** What was the verdict for the "Unusual VPN Login Location" alert?

- Under the "Alert Verdict" column, it states "**False Positive**"
<img width="1248" height="708" alt="Screenshot 2025-11-05 at 2 00 12 PM" src="https://github.com/user-attachments/assets/5f277834-6d2f-4165-ba05-085edfd00374" />

**Question 2:** What user was mentioned in the "Unusual VPN Login Location" alert?

- Referencing the previous picture, the **Source User** is listed as M.Clark


## Alert Prioritization
**Question 1:** Should you first prioritise medium over low severity alerts? (Yea/Nay)
- Yea
  - This is because the most criticial is always the most important as the detection engineers has determined which alerts are more likely to be major and have more impact

**Question 2:** Should you first take the newest alerts and then the older ones? (Yea/Nay)
- Nay
  - This is because the older ones means that the attacker could have made more progress with in their attack than a newer one where it may be simple discovery or attempts that the attacker is performing


## Alert Triage
**Question 1:** Which flag did you receive after you correctly triaged the first-priority alert?

The first-priority alert would be the alert labeled as critical.

**BREAKDOWN:**
- The **description** states that 5> gigs are being sent from one device to one destination within a day which could indicate data exfil
- The **destination** contains a url that has "*.zoom.us", although it'd be of benefit to see the rest of it, zoom is a online steraming service for meetings
- The **source IP** shows a private IP address while the **source network** shows "UK04/MEETINGROOM"
- Lastly, the **sent and received data** seems to be almost equivalent.


<img width="1253" height="768" alt="Screenshot 2025-11-05 at 2 42 55 PM" src="https://github.com/user-attachments/assets/1caa0712-d578-436a-ac20-00585c3f86da" />

- To promote good practice, one should always begin to take accountability for the alert and avoid confusion with other analysts with setting the status to "In Progress".
<img width="1253" height="768" alt="Screenshot 2025-11-05 at 2 54 20 PM" src="https://github.com/user-attachments/assets/17556fe1-8b4f-40f9-a475-2d7c67381985" />

- To finish, the **severity** should be changed to "Low" and the **verdict** should be changed to "False Positive", along with the **status** changed to "Closed"

+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=

**Question 2:** Which flag did you receive after you correctly triaged the second-priority alert?

The second-priority alert will be that listed with a severity of "High"
<img width="1253" height="768" alt="Screenshot 2025-11-05 at 3 00 51 PM" src="https://github.com/user-attachments/assets/2d831640-55c5-4eb5-8140-ec7370691ae4" />


**BREAKDOWN:**
- The **description** states detection of use of a double-extension file which is a manipulation tactic regarding phishing, where attackers attempt to trick users in opening the executable
- The **host** seems to have been a work computer
- The **process name** states "chrome.exe", so althought this can actually be a legitimate process of the Google Chrome browser, attackers can try to trick people with legit file names.
- The **File MotW** contains a url containing ".mp4.exe". Addtionally, a File MotW (Mark of the Web) is a metadata identifier used by Microsoft Windows to mark files downloaded from the Internet as potentially unsafe.
- Lastly, I'll check the MD5 hash (14d8486f3f63875ef93cfd240c5dc10b) by using VirusTotal.
  
<img width="1440" height="818" alt="Screenshot 2025-11-05 at 3 24 24 PM" src="https://github.com/user-attachments/assets/823655cf-8819-4ae1-93d8-6b3af7971efa" />

- This MD5 hash has been marked as malicious by many vendors including Malwarebytes Palo Alto Networks, and Microsoft

- To start, I changed Status to "In Progress", Verdict as "True Positive", and Asignee as myself.
<img width="1440" height="818" alt="Screenshot 2025-11-05 at 3 27 50 PM" src="https://github.com/user-attachments/assets/35b83e7a-7f39-4acf-a96c-3eede4855fe6" />

- After this, I changed the status to "Closed" and completed the question.

+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=

**Question 3:** Which flag did you receive after you correctly triaged the third-priority alert?

The third-priority alert will be the lowest

<img width="1440" height="818" alt="Screenshot 2025-11-05 at 3 32 59 PM" src="https://github.com/user-attachments/assets/8c5bd6d7-5718-486a-93b3-13016bf9b001" />

**BREAKDOWN:**
- The **description** is very broad since GitHub can be used for many things whether productive or malicious
- The **accessed URL** only shows a link to a facebook REACT library, nothing more

To avoid redundancy, I set the Status as "Closed", the Verdit as "False Positive", Asignee as myself, and for the comment, I entered "User accessed facebook react library repo."

## END
