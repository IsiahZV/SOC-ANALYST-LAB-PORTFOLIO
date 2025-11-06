# SOC L1 Alert Reporting - Reporting, Escalating, and Communicating

**Date:** 2025-11-06

**Objective:** Identify and write reports at L1 standards before escalating to L2 analysts

**Environment:** TryHackMe – “SOC L1 Alert Reporting” room  
**Tools Used:** Simulated SIEM

# Reporting Guide
This section covers converting data into information to write an accurate report before submitting to an L2 analyst
**Task 1:** According to the SOC dashboard, which user email leaked the sensitive document?

- In reference to the picture, the description states that the user shared an important document outside of the organization which violates corporate policies.
- **Source User Email** shows "m[.]boslan@tryhackme[.]thm"
<img width="1440" height="806" alt="Screenshot 2025-11-06 at 4 13 51 PM" src="https://github.com/user-attachments/assets/b04569de-97a7-45b6-b9aa-96665538aebe" />

##

**Task 2:** Looking at the new alerts, who is the "sender" of the suspicious, likely phishing email?

- The **Sender** source shows "support@microsoft[.]com"
- to validate the suspicion as well, the description provides some context, stating that it could be for used as "phishing post-delivery" after conducting analysis and that the email contians urgent / outrageous key words along with a .rar file

<img width="1440" height="806" alt="Screenshot 2025-11-06 at 4 22 19 PM" src="https://github.com/user-attachments/assets/28423c35-52c8-44d1-855f-c9ec74988980" />

##

**Task 3:** Open the phishing alert, read its details, and try to understand the activity. Using the Five Ws template, what flag did you receive after writing a good report?

The "Five W's" template consist of who, what, when, where, and why. I'll have to take this context and form a relevant report in relation to the Microsoft phishing alert from task 2.

- Who: <support@microsoft.com>
- What: Phishing e-mail
- When: 19:25
- Where: No listed device, IP, or website. Can state from fake/ malicious e-mail to IT Manager e-mail
- Why: Email consisting of urgency for user to open attached .rar file

I could say "At 19:25, support@microsoft.com sent a phishing e-mail to e.huffman@tryhackme.thm (Eddie Huffman, IT Manager) that consisted of urgency, containing a likely malicious file named "REPORT.rar".
- Upon doing so, the **Authenticity** score returns as 100% and receieved the flag, however, I won't be posting the flag

<img width="1440" height="806" alt="Screenshot 2025-11-06 at 4 51 28 PM" src="https://github.com/user-attachments/assets/13e9c997-14ff-4be8-b239-82ef8d7a67be" />

# Escalation Guide
This section goes over communication with L2 whether sending alerts or requesting assistance as a L1 analyst

**Task 1:** Who is your current L2 in the SOC dashboard that you can assign (escalate) the alerts to?

- E.Fleming
<img width="830" height="380" alt="Screenshot 2025-11-06 at 5 13 26 PM" src="https://github.com/user-attachments/assets/adfbf52e-acbe-47b7-80dc-8e8ca2f20119" />

##

**Task 2:** What flag did you receive after correctly escalating the alert from the previous task to L2? 

What this is saying is, because I wrote the report while assigning the alert to myself (L1 analyst), I now have to assign it to the L2 analyst, in this case, E.Fleming

<img width="830" height="486" alt="Screenshot 2025-11-06 at 5 16 30 PM" src="https://github.com/user-attachments/assets/155cbede-4915-45b1-b872-476ff8e7c547" />

##

**Task 3:** Now, investigate the second new alert in the queue and provide a detailed alert comment.Then, decide if you need to escalate this alert and move on according to the process.

