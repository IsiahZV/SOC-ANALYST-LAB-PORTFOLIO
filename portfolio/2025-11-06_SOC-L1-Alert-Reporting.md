# SOC L1 Alert Reporting - Reporting, Escalating, and Communicating

**Date:** 2025-11-06

**Objective:** Identify and write reports at L1 standards before escalating to L2 analysts

**Environment:** TryHackMe – “SOC L1 Alert Reporting” room  
**Tools Used:** Simulated SIEM

# Reporting Guide
**Question 1:** According to the SOC dashboard, which user email leaked the sensitive document?

- In reference to the picture, the description states that the user shared an important document outside of the organization which violates corporate policies.
- **Source User Email** shows "m.boslan@tryhackme.thm"
<img width="1440" height="806" alt="Screenshot 2025-11-06 at 4 13 51 PM" src="https://github.com/user-attachments/assets/b04569de-97a7-45b6-b9aa-96665538aebe" />


**Question 2:** Looking at the new alerts, who is the "sender" of the suspicious, likely phishing email?

- The **Sender** source shows "support@microsoft.com"
- to validate the suspicion as well, the description provides some context, stating that it could be for used as "phishing post-delivery" after conducting analysis and that the email contians urgent / outrageous key words along with a .rar file

<img width="1440" height="806" alt="Screenshot 2025-11-06 at 4 22 19 PM" src="https://github.com/user-attachments/assets/28423c35-52c8-44d1-855f-c9ec74988980" />
