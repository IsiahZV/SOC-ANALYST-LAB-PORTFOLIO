# Elastic Stack

**Date:** 2025-12-09

**Objective:** 
- Understand the components and features of ELK and their use in SOC while learning to search and filter for data
- Investigate VPN logs to identify anomalies and create visualizations.

**Environment:** TryHackMe – “Elastic Stack: The Basics” room  
**Tools Used:** Elastic


## Question 1: Select the index vpn_connections and filter from 31st December 2021 to 2nd Feb 2022. How many hits are returned?

Although solved already, in the Time Filter, I clicked on the start / end date and adjusted the current date to the given date.

<img width="1440" height="818" alt="Screenshot 2025-12-09 at 5 39 40 PM" src="https://github.com/user-attachments/assets/36c237e7-36a0-4ca5-9fd5-9be87e8b3b52" />

In the top left of the Time Interval chart (green columns), it shows **2,861 hits**.



## Question 2: Which IP address has the maximum number of connections?

This question may be worded weird since maximum implies a ceiling, but when clicking on the **Source_Ip** variable and seeing the top 5 values, the top would contain the most number of connections.

<img width="1440" height="818" alt="Screenshot 2025-12-09 at 6 22 29 PM" src="https://github.com/user-attachments/assets/c0b77940-f3c8-4978-868d-2dc7e7b9b272" />

- 238.163.231.224



## Question 3: Which user is responsible for the overall maximum traffic?

This is similar to the previous question 

<img width="1440" height="490" alt="Screenshot 2025-12-09 at 6 51 29 PM" src="https://github.com/user-attachments/assets/15b0fe34-7802-4d26-b66c-d0bb5123c811" />

- **James**



## Question 4: Apply Filter on UserName Emanda; which SourceIP has max hits?

To find this, I have to filter for the username "Emanda" and see the data that correlates with it.

<img width="1440" height="198" alt="Screenshot 2025-12-09 at 7 05 27 PM" src="https://github.com/user-attachments/assets/72c92207-d6bc-4bf9-ae30-8ecaee7f3fbd" />

<img width="1440" height="679" alt="Screenshot 2025-12-09 at 7 06 54 PM" src="https://github.com/user-attachments/assets/61aad47d-d351-4e6d-a16f-26c3f720a8fb" />

After clicking on the Source_Ip variable, i see that **107.14.1.247** has the most hits



## Question 5: On 11th Jan, which IP caused the spike observed in the time chart?

For this, to refrain from redundancy (refer to picture from question 1), I selected the tallest green column that happened to be on January 11th. In this timeframe, I selected the **Source_Ip** variable and referenced the first IP address in the Top 5 values.

<img width="1440" height="679" alt="Screenshot 2025-12-10 at 4 38 27 PM" src="https://github.com/user-attachments/assets/20359ba9-398c-4c5f-a565-b6f93a530d47" />


- **172.201.60.191**



## Question 6: How many connections were observed from IP 238.163.231.224, excluding the New York state?

To approach this, I can search "source_ip : 238.163.231.224" and then look at the values for **source_state** and exclude New York.

<img width="1440" height="679" alt="Screenshot 2025-12-10 at 4 31 23 PM" src="https://github.com/user-attachments/assets/2d0695e1-2566-42f3-9445-47f7a5e2ea78" />

<img width="1440" height="371" alt="Screenshot 2025-12-10 at 4 34 06 PM" src="https://github.com/user-attachments/assets/c2787673-87da-4a1c-a48b-466bf882baa2" />

- **48**



##
# KQL Overview

## Question 1: Create a search query to filter the logs where Source_Country is the United States and show logs from User James or Albert. How many records were returned?

<img width="1440" height="679" alt="Screenshot 2025-12-10 at 5 23 21 PM" src="https://github.com/user-attachments/assets/bea0344b-f23a-4357-9a9d-40be234b311b" />

- **161**



## Question 2: A user Johny Brown was terminated on the 1st of January, 2022. Create a search query to determine how many times a VPN connection was observed after his termination.

The tricky part about this is the date formatting, as searching how its displayed in the descriptions wont help. It has to be listed as: YYYY-MM-DD. Similar to the last question, I'll be searching for the username "Johny Brown" and for the timestamp that shows the day of termination and going forward.

<img width="1440" height="679" alt="Screenshot 2025-12-10 at 5 37 10 PM" src="https://github.com/user-attachments/assets/c158ce9b-b8a7-47bf-a635-15d2569ddbe4" />
- 1



##
# Creating Visualizations

## Question 1: Which user was observed with the greatest number of failed attempts?
I'm going to begin by creating a visualization with Lens (drag and drop values), inputting the username value as well as action

Below is what establishes what I'm filtering against (think filtering for states within a country)
<img width="1440" height="679" alt="Screenshot 2025-12-10 at 6 26 15 PM" src="https://github.com/user-attachments/assets/0fc89abd-a3c7-4df5-a327-2c82959a1e18" />

- Next is to drag over the UserName value into the drop area

<img width="1440" height="679" alt="Screenshot 2025-12-10 at 6 30 47 PM" src="https://github.com/user-attachments/assets/58dcc593-bbdd-4d7a-ba6b-5ae8ea0d9975" />

Upon doing so, I see that Simon is the only one with this action (failed connection attempt). As a sidenote, the dropdown can be used to change the way the graphs / charts are presented.

- Simon



## Question 2: How many wrong VPN connection attempts were observed in January?
I wouldn't know how to create a visualization for months, so instead, I changed the timeframe date to January 1st to the 31st.

<img width="1440" height="679" alt="Screenshot 2025-12-10 at 6 39 37 PM" src="https://github.com/user-attachments/assets/c5df0cc3-879e-47bd-8f62-e02ff0b77626" />

- 274



# End
