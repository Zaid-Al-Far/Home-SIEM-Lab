# 🔐 Home SIEM Lab - Brute Force Detection with Splunk

## Overview
Built a home SIEM lab using Splunk Enterprise to simulate and detect 
a brute force attack. This project demonstrates the full SOC analyst 
workflow: log ingestion, SPL query writing, and automated alert creation.

**Tools:** Splunk Enterprise 10.4.0 | Windows 10  
**Attack Simulated:** SSH Brute Force  
**Detection Method:** SPL Query + Automated Alert  

---

## What is a Brute Force Attack?
A brute force attack is when an attacker repeatedly tries different 
passwords to gain unauthorized access. In this lab, a simulated attacker 
made 4 failed login attempts before succeeding on the 5th try — 
a classic indicator of compromise (IoC).

---

## Lab Walkthrough

### Step 1: Create the Log Data
Created a custom log file `bruteforce.log` simulating a real attack:
2026-06-07 10:01:01 Failed password for admin from 192.168.1.100
2026-06-07 10:01:02 Failed password for admin from 192.168.1.100
2026-06-07 10:01:03 Failed password for admin from 192.168.1.100
2026-06-07 10:01:04 Failed password for admin from 192.168.1.100
2026-06-07 10:01:05 Accepted password for admin from 192.168.1.100

> 4 failed attempts followed by 1 successful login = brute force pattern

---

### Step 2: Ingest Logs into Splunk
Uploaded `bruteforce.log` into Splunk Enterprise.  
- Source Type: SIEM (custom)  
- Index: Default  

<img width="1920" height="1080" alt="log-ingestion png" src="https://github.com/user-attachments/assets/3221fe0d-5722-4e9f-a138-3b2ef81afeac" />


<img width="1920" height="1008" alt="upload-successful png" src="https://github.com/user-attachments/assets/c83d6bbf-a0f4-4594-a212-1883d5418e15" />


---

### Step 3: Write SPL Detection Query
Wrote a Splunk Processing Language (SPL) query to detect the attack:

```spl
source="bruteforce.log" "Failed password" 
| stats count by host 
| where count >= 3
What this does:

	•	Filters all “Failed password” events
	•	Counts failures grouped by host
	•	Flags any host with 3+ failures as suspicious

Result: Host echo flagged with 4 failed attempts ✅
<img width="1920" height="1008" alt="query png" src="https://github.com/user-attachments/assets/fec7c4f4-6952-402b-a81d-cd8fc6da7c3d" /> 

Step 4: Create Automated Alert
Saved the query as an automated alert:

	•	Name: Brute Force Detection Alert
	•	Condition: Triggers when results > 0
	•	Schedule: Weekly (configurable to real-time)
<img width="1920" height="1008" alt="alert png" src="https://github.com/user-attachments/assets/605157a2-27d4-4b1f-844e-a368b0e8d410" />
Key Takeaways

	•	Understood how SIEM tools ingest and parse log data
	•	Learned SPL basics for threat detection
	•	Experienced the SOC analyst workflow end-to-end: ingest → search → alert
	•	Understood what a brute force IoC looks like in real logs
