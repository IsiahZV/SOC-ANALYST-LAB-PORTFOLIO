# Identifying Phishing Tactics and Escalation

**Date:** 2025-11-10

**Objective:**
- Monitor and analyze real-time alerts.
- Identify and document critical events such as suspicious emails and attachments.
- Create detailed case reports based on your observations to help your team understand the full scope of alerts and malicious activity.

**Environment:** TryHackMe – “Introduction to Phishing” room  
**Tools Used:** Microsoft Sentinel / Elastic (unable to view firewall rules in Sentinel)
##

📄 Task documentation regarding company info and asset inventory will be posted at the very end

Please note that the day and times will change throughout the lab as THM has a time limit extension issue and faulty information with some of the optional SIEM tools.


## Foundation

I'll start off by recalling the report format:
- Who: Which user logs in, runs the command, or downloads the file
- What: What exact action or event sequence was performed
- When: When exactly did the suspicious activity start and ended
- Where: Which device, IP, or website was involved in the alert
- Why: the reasoning for your final verdict

- When dealing with alerts, prioritize the most critical severity alert and work down to the lowest
- Prioritize alert by earliest time


# Analysis

The alert queue generates five alerts, four of which are medium and one being high.


<img width="1440" height="652" alt="Screenshot 2025-11-10 at 6 30 37 PM" src="https://github.com/user-attachments/assets/ea414124-5b1b-4598-9c97-e514989e5746" />


To begin, I'll assign the highest one to myself and make an assessment of whether TP or FP based on the given alert data.


<img width="1100" height="537" alt="Screenshot 2025-11-10 at 6 33 11 PM" src="https://github.com/user-attachments/assets/1147a22d-f736-4cd3-a488-73740af40306" />


**ALERT DATA SHOWS:**
- User attempted to access external URL which was blocked by firewall rules
- To better understand the type of website the user tried to go to, I will use VirusTotal to 

- **WHO:** Source IP -> 10[.]20[.]2[.]17
  - To get more info on who's behind this Source IP, I used the Employee Information Doc and concluded that it belongs to Hannah Harris (h.harris@thetrydaily.thm) from the Human Resources department.
- **WHAT:** User attempted to go blocked website 
- **WHEN:** Nov 10th 23:28
- **WHERE:** Destination IP (Serving IP of URL) -> 67[.]199[.]248[.]11
  - Domain -> http://bit[.]ly/3sHkX3da12340
  - VirusTotal shows at least one vedor marking this URL / IP as malicious. The IP especially has a bunch of malicious files that communicate with this IP address.
- **Why:** To discover this, I have to utilze the MS Sentinel SIEM logs where Hannah Harris is in communication or a ricipient to give some context.


<img width="1440" height="783" alt="Screenshot 2025-11-10 at 6 53 29 PM" src="https://github.com/user-attachments/assets/192b21e5-c08c-4d1d-9d9f-c38c7d038d25" />

 
Here, I see a sender "urgents@amazon[.]biz" and the ricipient of interest being Hannah Harris. Additionally, the attached shortened bit.ly URL reads the exact same from the alert. The rest of the message tries to invoke a sense of urgency by stating that the package will be returned to sender if not heard from.

The **Why** could be stated as: Fake amazon account promotes sense of urgency and attaches malicious bit.ly url


- I'll confirm that the alert is a **true positive** and that the alert doesn't require escalation.
##
Note that I'm now attending this lab on a different day so date and time may be different in the report. Time will show November 12th, 00:10 (time of alert) although today's current date is the 11th.
The adjusted email time is Nov 12th, 12:07am.
##

I'll write:
"On November 12th, 00:07, Hannah Harris (h.harris@thetrydaily.thm) from the Human Resources department  received a phishing email from a fake amazon email account with the name "urgents@amazon.biz", containing a message that attempts to invoke a sense of urgency in reference to a missed package. The email contained a shortened link with the domain being " http://bit[.]ly/3sHkX3da12340" and hosted by a malicious IP address ( 67[.]199[.]248[.]11), in which Hannah clicked which redirected her to a blocked website due to our firewall rules. 

Ensure this user goes over phishing awareness."

<img width="1060" height="690" alt="Screenshot 2025-11-11 at 7 36 32 PM" src="https://github.com/user-attachments/assets/d20d1b7f-3b31-47d8-8d82-33b27f2bd6a0" />

##

Now that the alert is closed, I have to assign the earliest alert out of the four receieved. However, there are two alerts that come in at 00:12, they do have the same context in reference to the alert description, this means I have to assess which one is more important in terms of how likely it would impact the CIA triad.

With alert ID 8817, I identified multiple flaws with the e-mail which will be highlighted for easier documentation.


<img width="1158" height="736" alt="Screenshot 2025-11-11 at 7 48 22 PM" src="https://github.com/user-attachments/assets/84eb9fbc-6be6-413a-8ce8-8b886c674884" />

- **WHO:** Charlotte Allen
  - Web Development Department
  - IP: 10.20.2.25
  - Email: c.allen@thetrydaily.thm
  - Malicious Email: no-reply@m1crosoftsupport.co
- **WHAT:** User received malicious phsihing email
- **WHEN:** 00:10 (Time subject to change due to lab restarting - THM issue)
- **WHERE:** Destination URL: https://m1crosoftsupport[.]co/login
  - TryDetectThis confirms URL as malicious
**WHY:** Likely to get user to input user information into fake site by utilizing scare tactics by sending a fake compromised account alert with a domain and email similar to an authentic one.

**NOTICE**
- Going forward I'll be using Elastic due to easier customization and it allows me to view firewall activity while MS Sentinel didn't

To begin, I will say that its a **true positive** and that it **does require escalation.**

I need to see if Charlotte Allen went through with clicking on the malicious URL.

<img width="1440" height="769" alt="Screenshot 2025-11-12 at 5 41 15 PM" src="https://github.com/user-attachments/assets/8affc273-6a63-4507-a19a-634409ad787e" />

Note that the IP address matches Charlotte's and that the firewall allowed connectivity to the phishing site

To conclude, I'll write:
"November 12th @ 17:19, Charlotte Allen(c.allen@thetrydaily.thm) of the Web Development Department received an email from a fake microsoft email account (no-reply@m1crosoftsupport.co) where the subject described unusual sign-in activity and where the message is urgently telling the user to secure their account by clicking on a malicious link (https://m1crosoftsupport[.]co/login) verified by TryDetectThis. This is likely an attempt at obtaining personal and credential information. At 17:20, Charlotte clicked on the malicious URL. Recommend isolating user's account until credentials are changed and that no sensitive information was accessed."


##

Onto alert ID 8818, @22:22

<img width="1440" height="769" alt="Screenshot 2025-11-12 at 5 56 52 PM" src="https://github.com/user-attachments/assets/84a8e01a-b2d3-4a87-9684-2fc92e2ca459" />

The URL in question for the alert is "https://hrconnex[.]thm/onboarding/15400654060/j[.]garcia"
- TryDetectThis verifies URL as clean

<img width="1440" height="769" alt="Screenshot 2025-11-12 at 6 01 14 PM" src="https://github.com/user-attachments/assets/61b51b6b-d0a2-497b-971b-e5fda6f012a4" />

- Email content shows no sign of urgency or suspicious activity and seems to follow along the time where the HR email and j.garcia are finalizing information regarding onboarding.

- **WHO:** Julia Garcia
  - Content Department
  - Email: j.garcia@thetrydaily.thm
  - IP: 10.20.2.8
- **WHAT:**
  - Received email from HR department along with a URL that returns clear, verified by TryDetectThis.
- **WHEN:**
  - 17:20
- **WHERE:**
  - "https://hrconnex.thm/onboarding/15400654060/j.garcia"
- **WHY:**
  - Finalizing onboarding information

I can label this as a **False Positive** and that it **doesnt require escalation**.

I'll write:
"November 12 @ 17:20, Julia Garcia (j.garcia@thetrydaily.thm) of the Content Department received an onboarding email with a link that returns clear which has been verified by TryDetectThis. Recommend engineer to fix configurations to not alert when "hrconnex.thm" URL is being sent."

##

Onward, alert ID 8815 @ 22:19

<img width="1440" height="769" alt="Screenshot 2025-11-12 at 6 25 41 PM" src="https://github.com/user-attachments/assets/c3cc8f9d-ae04-417a-82f2-3f92526d5fb2" />

This email is relative to the blocked URL that was handled first, so I can skip the entire process of trying to figure out the 5 W's and alter the **When** to the time the email was received, acknowledge it as a **True Positive**, that it **doesnt need to be escalated** and explain that this event was in relation to a solved alert.

<img width="1240" height="258" alt="Screenshot 2025-11-12 at 6 30 17 PM" src="https://github.com/user-attachments/assets/8e0c2c43-06d7-4b28-afa0-bdf99cb7e25a" />

I'll say:
"On November 12 at 17:17, Hannah Harris (h.harris@thetrydaily.thm) of the Human Resources Department received a malicous email from a fake amazon account (urgents@amazon.biz). This email contained characteristics of pursuasion and urgency as well as the URL (http://bit[.]ly/3sHkX3da12340) and IP address serving the URL were previously verified as malicious from TryDetectThis, likely for being a C2 server. This alert is in relation to alert with the ID of 8816 where the attempt to connect to the site due to the user clicking the link - was blocked."

##

# Results

<img width="1440" height="819" alt="Screenshot 2025-11-12 at 6 40 03 PM" src="https://github.com/user-attachments/assets/dd384446-8709-4807-b865-eab6e3663d9b" />

## Reflection and What I Could Have Done Better

<img width="1161" height="323" alt="Screenshot 2025-11-12 at 6 43 37 PM" src="https://github.com/user-attachments/assets/d2b08ce8-b732-46bc-a182-0f6bebb744aa" />

- ✅ Including what possible solutions that the L2 analyst could utilize (proactive to remediation)
- ⚠️ Inlcude more technical details such as internal and destination IP address and network activity (could've included more firewall information)
- ⚠️ Clarify if the URL's provided were part of the organizations blacklist or threat intelligence feed and confirm that there was no impact such as data loss or credential compromise (In reference to the package alert). In short, confirm impact of situation whether there was or wasn't any. 



# Documentation

## Alert Triage Playbook

**INITIAL ALERT REVIEW:**

- Access the SOC Dashboard: Open the SOC dashboard and review the new alerts

- Assign Alert to Yourself: Add the first (earliest) alert to the list of assigned alerts

- Understand Alert Logic: Review the alert description and understand its logic

- Review Alert Details: Look at the IOCs provided in the alert, like IPs and domains

**INVESTIGATE IN THE SIEM:**

- Access the SIEM: Open the "SIEM" tool to access raw security events that triggered the alert

- Query Related Logs: Perform searches to gather more context and build the activity timeline

- Use Analyst VM: From the "Analyst VM", open the TryDetectThis app to check the threat score of found indicators

- Correlate and Validate: Correlate the alert data with other data sources to validate the credibility of the alert


## Employee Information
<img width="973" height="508" alt="Screenshot 2025-11-10 at 6 46 08 PM" src="https://github.com/user-attachments/assets/f4662f80-c8bc-4508-85de-9b6b322658d7" />



