# Analyzing VPN logs with splunk

**Date:** 2025-11-24

**Objective:** Establish foundational knowledge with using Splunk by practically ingesting logs and analyzing

**Environment:** TryHackMe – “Splunk: The Basics” room  
**Tools Used:** Splunk Enterprise
##

**Personal Sidenote:**
The log file is available in the /root/Rooms/SplunkBasic/ directory.

To upload the data successfully, you must follow five steps, which are explained below:
Select Source: Choose the Log file and the data source.
Select Source Type: Select what type of logs are being ingested, e.g, JSON, syslog.
Input Settings: Select the index where these logs will be dumped and the HOSTNAME to be associated with the logs.
Review: Review all the configurations.
Done: Complete the upload. Your data will be uploaded successfully and ready to be analyzed.



## Question 1: Upload the data attached to this task and create an index "VPN_Logs". How many events are present in the log file?

<img width="1440" height="817" alt="Screenshot 2025-11-24 at 3 52 15 PM" src="https://github.com/user-attachments/assets/108304f9-cb68-431b-9f51-c42eac076f13" />

<img width="1440" height="817" alt="Screenshot 2025-11-24 at 4 09 45 PM" src="https://github.com/user-attachments/assets/6bf61e51-910e-459d-88f5-8d4652db1205" />

- The total number of alerts present are 2,862



## Question 2: How many log events are captured by the user Maleena?

Since we know the username and how the information is displayed in the alerts, I can filter the alerts where UserName="Maleena". It's important to keep the index relevant in this filter although (i.e., index=VPN_Logs)

<img width="1440" height="817" alt="Screenshot 2025-11-24 at 4 29 07 PM" src="https://github.com/user-attachments/assets/ee565a4d-2a8d-40d5-a8c8-8c40c0b24ba0" />

- 60



## Question 3: What is the username associated with IP 107.14.182.38?

In the filter I can enter: Source_ip=107.14.182.38 

<img width="1440" height="817" alt="Screenshot 2025-11-24 at 4 46 56 PM" src="https://github.com/user-attachments/assets/98065591-76fc-407f-a8e6-65fee9abf181" />

- Smith



## Question 4: What is the number of events that originated from all countries except France?

By entering my mind like Jimmy Neutron, I can remember the use of logical operators and enter !=France after Source_Country.

<img width="1440" height="817" alt="Screenshot 2025-11-24 at 4 59 48 PM" src="https://github.com/user-attachments/assets/04e50c0d-f949-414a-b25f-c93610ee431e" />

- 2,814


## Question 5: How many VPN events were associated with the IP 107.3.206.58?

In this, of course all I'm doing is ensuring that the IP of interest is equal to the Source_ip variable

<img width="1440" height="817" alt="Screenshot 2025-11-24 at 5 30 50 PM" src="https://github.com/user-attachments/assets/030b320b-997e-49d4-ae85-cc35629d20c5" />

- 14

##

