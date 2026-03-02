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


##
