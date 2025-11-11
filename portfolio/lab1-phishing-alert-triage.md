# Identifying Phishing Tactics and Escalation

**Date:** 2025-11-10

**Objective:**
- Monitor and analyze real-time alerts.
- Identify and document critical events such as suspicious emails and attachments.
- Create detailed case reports based on your observations to help your team understand the full scope of alerts and malicious activity.

**Environment:** TryHackMe – “Introduction to Phishing” room  
**Tools Used:** Microsoft Sentinel
##

📄 Task documentation regarding company info and asset inventory will be posted at the very end

Please note that although the day is accurate, the time of generated data will be significantly different in Microsoft Sentinel compared to real time dashboard.


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
Luckily, they apply a report format that I can fill out.










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



