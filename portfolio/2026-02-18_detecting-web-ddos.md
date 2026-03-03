# Detecting Web DDoS
**Date:** 2026-02-18

**Objective:**



**Environment:** TryHackMe – “Detecting Web DDos” room  
**Tools Used:** Terminal


## Scenario
- Your bicycle parts website has undergone a denial-of-service attack. Open up the access.log file on the user's Desktop to begin your investigation. The logs include a mix of normal user-generated traffic and attacker traffic. 

##

### What is the attacker’s IP address?

<img width="1293" height="480" alt="Screenshot 2026-03-01 at 6 49 19 PM" src="https://github.com/user-attachments/assets/00e604e6-5f24-482c-a7f7-db8ca77adb89" />

Upon opening the log, I see what appears to be normal user traffic due to the characteristics of the host interaction.

I see an internal user (IP ending in .5) getting onto the web and requesting a products page of a web server and then another internal user (ending in .10) getting onto the web and navigating to a blog page. After, all traffic from these two hosts are consistent with their activity.

<img width="1285" height="618" alt="Screenshot 2026-03-01 at 6 59 52 PM" src="https://github.com/user-attachments/assets/aa54f1c3-a141-4139-9173-1602a1286a1c" />

About a minute later, an external IP sends many GET requests in the same second to the /login page

- 203.12.23.195

##

### Which page is repeatedly targeted by the attacker’s requests?

- /login

##

### After the attack, what error code do legitimate users receive?

<img width="1285" height="618" alt="Screenshot 2026-03-01 at 7 09 40 PM" src="https://github.com/user-attachments/assets/69db7f45-6f97-4373-b0b7-27c31b3cb465" />


After a series "200" response codes created by the attackers requests, the attacker starts to receive a "503" response code. Eventually, at 01:59:29 internal users begin to face the same error response as well.

- 503


## Scenario
- This time, your website experienced a suspected DDoS attack. You’ll use Splunk to investigate what happened. During this exercise, you’ll analyze web access logs collected during the suspected attack period. These logs contain a mix of normal user traffic and potentially malicious requests

##

### What was the most frequently requested uri?

<img width="1440" height="729" alt="Screenshot 2026-03-02 at 9 37 28 PM" src="https://github.com/user-attachments/assets/8c858bc0-2018-4d3b-8369-9df9899d9559" />
Splunk main screen

<img width="1440" height="729" alt="Screenshot 2026-03-02 at 9 38 42 PM" src="https://github.com/user-attachments/assets/5134c351-7bc6-43cd-930d-7110b6c82e5c" />
Shown are details from the uri field. Here, we can see that /search is the top value.

- /search

##

### Which clientip made the most requests to the target uri?

To filter for this, I'll include the uri value (/search) and see which client contains the highest value for this filter.

<img width="1440" height="729" alt="Screenshot 2026-03-02 at 9 59 38 PM" src="https://github.com/user-attachments/assets/9bd0e012-7572-43b5-8bfa-7a8d9c86f209" />

- 203.0.113.7

##

### Use the timechart command to visualize the requests. What is the peak number of requests made per second during the attack?

<img width="1440" height="729" alt="Screenshot 2026-03-03 at 6 39 26 AM" src="https://github.com/user-attachments/assets/3aaa883a-f619-45b2-af5c-f9313adc665b" />

I'll begin by clicking the green column that contains the total amount of events.

<img width="1440" height="729" alt="Screenshot 2026-03-03 at 6 43 09 AM" src="https://github.com/user-attachments/assets/13f4c09b-1658-4419-9113-7f6077ee1b2c" />

To avoid redundancy with the pictures, eventually, I come across a column that contains all of the events for the day -> hour -> minute -> second.

They are all divided up evenly.

<img width="1440" height="729" alt="Screenshot 2026-03-03 at 6 48 15 AM" src="https://github.com/user-attachments/assets/cbcc1a06-4a23-4780-a7c8-1be0df26bb8f" />

- 207

##

### Which legitimate (non-attacking) clientip received the first 503 response status post-attack?

What I'll be filtering for is the response status and utilizing a filter that sorts the results from earliest to latest

<img width="1440" height="729" alt="Screenshot 2026-03-03 at 7 08 12 AM" src="https://github.com/user-attachments/assets/efabacec-fa34-4584-a949-614cea0a59e5" />

Maybe there couldve been a better way to filter or go about it but this is what allowed me to reverse the results and sift through until I found an internal IP. If any better options, I'll include as an edit.

<img width="1440" height="729" alt="Screenshot 2026-03-03 at 7 19 32 AM" src="https://github.com/user-attachments/assets/d18a123a-9ccc-4955-9c29-2ad180c56567" />


- 10.10.0.27

##



