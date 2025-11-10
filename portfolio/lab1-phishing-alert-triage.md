# Identifying Phishing Tactics and Escalation

**Date:** 2025-11-10

**Objective:**
- Monitor and analyze real-time alerts.
- Identify and document critical events such as suspicious emails and attachments.
- Create detailed case reports based on your observations to help your team understand the full scope of alerts and malicious activity.

**Environment:** TryHackMe – “Introduction to Phishing” room  
**Tools Used:** Microsoft Sentinel


## Foundation

I'll start off by recalling the report format:
- Who: Which user logs in, runs the command, or downloads the file
- What: What exact action or event sequence was performed
- When: When exactly did the suspicious activity start and ended
- Where: Which device, IP, or website was involved in the alert
- Why: the reasoning for your final verdict


## Alert Triage Playbook

**Initial Alert Review:**
Access the SOC Dashboard: Open the SOC dashboard and review the new alerts

Assign Alert to Yourself: Add the first (earliest) alert to the list of assigned alerts

Understand Alert Logic: Review the alert description and understand its logic

Review Alert Details: Look at the IOCs provided in the alert, like IPs and domains

**Investigate in the SIEM:**
Access the SIEM: Open the "SIEM" tool to access raw security events that triggered the alert

Query Related Logs: Perform searches to gather more context and build the activity timeline

Use Analyst VM: From the "Analyst VM", open the TryDetectThis app to check the threat score of found indicators

Correlate and Validate: Correlate the alert data with other data sources to validate the credibility of the alert

# Lab Begin

Starting off, I get my first alert

<img width="1440" height="818" alt="Screenshot 2025-11-10 at 5 55 42 PM" src="https://github.com/user-attachments/assets/d6e3b8d4-ba02-4d68-ad69-d0474f06482c" />
