# 🖥️ Splunk & Sysmon Home SOC Lab: Centralized Log Collection and Endpoint Monitoring
 
## 📖 Overview
 
This project demonstrates the deployment of a centralized logging solution using **Splunk Enterprise** and **Sysmon** within a Windows-based home SOC lab.
 
The objective was to collect Windows security telemetry from multiple endpoints, forward the logs to a centralized Splunk server, and build the foundation for future threat hunting, detection engineering, and SOC investigations.
 
By the end of the project, both the Windows workstation and Windows Server successfully forwarded Sysmon and Windows Event Logs to Splunk for centralized analysis.
 
---
 
## 🎯 Project Objectives
 
- Deploy a dedicated Splunk Enterprise server
- Install and configure Sysmon on Windows endpoints
- Configure Splunk Universal Forwarders
- Centralize Windows event collection
- Create a custom Splunk index for endpoint telemetry
- Validate end-to-end log ingestion
- Build a foundation for threat hunting and detection engineering
 
---
 
## 🏗️ Lab Environment
 
### Virtual Infrastructure
 
- VMware Workstation
- NAT Network Configuration
 
### Systems
 
| System | Role | IP Address |
|----------|----------|----------|
| Ubuntu Server | Splunk Enterprise Server | 192.168.10.10 |
| Windows Server | Active Directory Server | 192.168.10.7 |
| Windows Workstation | Target Endpoint | 192.168.10.100 |
| Kali Linux | Attack Platform | 192.168.10.x |
 
---
 
## 🔧 Splunk Server Deployment
 
### Splunk Enterprise Installation
 
Installed:
 
```text
Splunk Enterprise (Linux)
```
 
Configured:
 
```text
Static IP Address
192.168.233.11
```
 
Enabled Splunk web access:
 
```text
https://192.168.233.11:8000
```
 
Configured automatic startup:
 
```bash
splunk enable boot-start
```
<img width="1281" height="341" alt="image" src="https://github.com/user-attachments/assets/76872835-4cce-4289-874b-7d604d1c796c" />


---
 
## 📡 Splunk Data Collection Configuration
 
### Receiver Port
 
Configured Splunk to receive forwarded events on:
 
```text
TCP 9997
```
 
Path:
 
```text
Settings → Forwarding and Receiving
```
 
---
 
### Custom Index
 
Created a dedicated endpoint telemetry index:
 
```text
endpoint
```
 
Purpose:
 
- Store Windows logs
- Store Sysmon events
- Separate endpoint telemetry from default indexes
 
---
 
## 🛡️ Sysmon Deployment
 
Downloaded and installed:
 
```text
Sysmon
```
 
Configuration:
 
```text
Olaf Hartong Sysmon Configuration
```
 
Benefits:
 
- Process Creation Logging
- Network Connection Logging
- Registry Monitoring
- File Creation Monitoring
- Service Creation Monitoring
- PowerShell Activity Visibility
 
---
 
## 🚀 Splunk Universal Forwarder Deployment
 
Installed on:
 
### Target-PC
 
```text
Windows Workstation
```
 
### ADDC01
 
```text
Windows Server
```
 
Configured forwarding destination:
 
```text
192.168.10.10:9997
```
 
---
 
## 📝 Log Collection Configuration
 
Created:
 
```text
inputs.conf
```
 
Collected log sources:
 
### Windows Security Logs
 
```text
WinEventLog:Security
```
 
### Windows System Logs
 
```text
WinEventLog:System
```
 
### Windows Application Logs
 
```text
WinEventLog:Application
```
 
### Sysmon Logs
 
```text
Microsoft-Windows-Sysmon/Operational
```
 
Assigned index:
 
```text
index=endpoint
```
 
---
 
## ✅ Validation
 
Verified successful ingestion using:
 
```spl
index=endpoint
```
 
Results:
 
✅ Sysmon events received
 
✅ Windows Security logs received
 
✅ Windows Application logs received
 
✅ Windows System logs received
 
✅ Multiple endpoint hosts reporting to Splunk
 
---
 
## 🔍 Sample Investigation
 
Search used:
 
```spl
index=endpoint
```
 
Observed:
 
### Hosts
 
```text
Target-PC
ADDC01
```
 
### Sources
 
```text
WinEventLog:Security
WinEventLog:System
WinEventLog:Application
XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
```
 
This confirmed successful communication between all monitored systems and the Splunk server.
 
---
 
## 📚 Skills Demonstrated
 
### SIEM Administration
 
- Splunk Enterprise Deployment
- Splunk Index Management
- Log Ingestion Configuration
- Event Collection Validation
 
### Endpoint Monitoring
 
- Sysmon Deployment
- Windows Telemetry Collection
- Event Log Analysis
- Security Visibility Enhancement
 
### System Administration
 
- Ubuntu Server Configuration
- Static IP Assignment
- Service Management
- Network Configuration
 
### Security Operations (SOC)
 
- Centralized Logging
- Log Aggregation
- Security Monitoring
- Threat Hunting Preparation
 
---
 
## 🎓 Key Takeaways
 
- Centralized logging is a core component of every SOC environment.
- Sysmon significantly improves endpoint visibility beyond native Windows logging.
- Splunk Universal Forwarders provide reliable event collection from distributed systems.
- Proper indexing and log organization improve investigation efficiency.
- End-to-end validation is critical when deploying security monitoring infrastructure.
 
---
 
## 📸 Screenshots
 
screenshots of:
 
- Splunk Enterprise Dashboard
- Splunk Search & Reporting App
- Endpoint Index Configuration
- Sysmon Service Running
- Splunk Universal Forwarder Configuration
- Search Results (`index=endpoint`)
- Multiple Hosts Reporting to Splunk
 
---
 
## 🚀 Project Outcome
 
Successfully deployed a centralized logging platform using Splunk Enterprise and Sysmon. Windows endpoint telemetry, Sysmon events, and Windows Event Logs were collected and forwarded to a dedicated Splunk server for centralized monitoring and analysis. This project demonstrates hands-on experience with SIEM deployment, endpoint visibility, log management, security monitoring, and SOC infrastructure implementation.
