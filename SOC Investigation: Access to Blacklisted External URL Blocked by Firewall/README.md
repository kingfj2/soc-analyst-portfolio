# SOC Investigation: Access to Blacklisted External URL Blocked by Firewall

> **Simulation Platform:** TryHackMe
> **Room/Lab Name:** 8816
> **Role:** Tier 1 SOC Analyst

## 🚨 Alert Summary
* **Alert Name:** Access to Blacklisted External URL Blocked by Firewall
* **Severity:** HIGH
* **Date/Time:** 10/26/2026 10:08:36.369 AM
* **Data Source:** firewall
* **Description:**  alert was triggered when a user attempted to access an external URL that is listed in the organization's blacklist or threat intelligence feeds
<img width="1883" height="581" alt="image" src="https://github.com/user-attachments/assets/e0753873-1fab-49bd-a4a5-4d7965de60b9" />


## 🔍 Triage & Context
* **What happened:** The user clicked the link http://bit.ly/3sHkX3da12340 that's when the firewall blocked the outbound request
* **Victim/Host:**  10.20.2.17
* **Attacker/Source:**  67.199.248.11


## 🕵️ Investigation Steps
1. **Initial Assessment:** After reviewing the alert, I copied the URL http://bit.ly/3sHkX3da12340 and pasted it on the analysis tool. Once came back the URL reported to be malicious
2. **Log Analysis:**  after I found it malicious I searched the email in splunk and found 1 event with Action is blocked and the datasource is firewall
<img width="1897" height="617" alt="image" src="https://github.com/user-attachments/assets/146f4e6e-bfd3-485b-b8dc-63f1bdec820d" />

3. **OSINT / Threat Intelligence:** I pasted the URL in TRYDetectThis and found that the URL is malicious
<img width="1020" height="794" alt="image" src="https://github.com/user-attachments/assets/b7d2b00a-c903-44b6-8540-4aa460575458" />

4. **Verification:** The attack was confirmed block when I see it in the Event that the action indicates it was blocked. and the data source is firewall.
<img width="1892" height="578" alt="image" src="https://github.com/user-attachments/assets/864e7f4b-5adc-4170-b2ec-0bcc06e0c5df" />


## 🛡️ Conclusion & Verdict
* **Classification:** This incident is a true positive since even the firewall block the attack. the link is still in the user's email. 
* **Summary of Findings:** At 10/26/2026 10:08:36.369 AM alert was triggered when a user attempted to access an external URL. And when check in the SIEM tool the URL is listed as blocked website. 

## 🛠️ Remediation & Escalation
* Although the firewall blocked the attack. The phishing email still on the user's mailbox where it could be clicked again

* Advise the user to delete the email to prevent the link to being click again.
* Block the sender address at the mail gateway.
 

## 🎯 Indicators of Compromise (IoCs)
* **IP Addresses:** 67.199.248.11
* **Domains/URLs:** http://bit.ly/3sHkX3da12340
* **Email Addresses:**urgents@amazon.biz
