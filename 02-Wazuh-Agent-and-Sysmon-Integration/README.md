# Wazuh Agent and Sysmon Integration

## Project Overview

This project demonstrates how to connect a Windows endpoint to a Wazuh SIEM server and configure Sysmon for enhanced endpoint visibility.

## Skills Demonstrated

- SIEM Administration
- Endpoint Monitoring
- Windows Logging
- Threat Detection
- Sysmon Configuration
- Security Monitoring

## Lab Environment

| Component | Description |
| ---------- | ---------- |
| Wazuh Server | Ubuntu Server |
| Endpoint | Windows 10 |
| Telemetry | Sysmon |
| Platform | VirtualBox |


## Installation Process

### Step 1: Install Ubuntu and Install Wn10 Pro

<img width="1281" height="836" alt="image" src="https://github.com/user-attachments/assets/6e1bb245-6915-4fee-892a-4eefb16cd368" />
<img width="1219" height="865" alt="image" src="https://github.com/user-attachments/assets/d8368824-d37f-45e3-b486-70ee12839750" />


### Step 2: Register Agent

Agent for windows
<img width="1525" height="568" alt="image" src="https://github.com/user-attachments/assets/f6ec68bb-bb3b-4be6-9be3-2bdb54e828dc" />

Deploying agent for Linux

<img width="1095" height="614" alt="image" src="https://github.com/user-attachments/assets/a3261375-c092-4726-8c37-cf8621a264eb" />
<img width="1519" height="612" alt="image" src="https://github.com/user-attachments/assets/f2460cbf-975a-4374-b870-222be5a92eef" />


### Step 3: Install Sysmon

Installing Sysmon in Windows 10 VM

<img width="974" height="774" alt="image" src="https://github.com/user-attachments/assets/8f43d853-8fe4-498d-baff-9151fe76a60d" />

Installing Sysmon in Linux

<img width="1109" height="621" alt="image" src="https://github.com/user-attachments/assets/feef5e3a-cc47-4252-b340-8a3d7e5658bf" />


Logs in Wazuh 

<img width="1906" height="947" alt="image" src="https://github.com/user-attachments/assets/e904f9a7-01fb-4a50-805d-b518e356be7a" />



## Validation

### Endpoint Connected

Screenshot

### Sysmon Events Detected

<img width="1906" height="947" alt="image" src="https://github.com/user-attachments/assets/74830d4d-aaed-4261-a1b2-ea6b18401835" />


### Process Creation Logs

<img width="1215" height="768" alt="image" src="https://github.com/user-attachments/assets/450d2bb3-cd8d-4086-8c25-e6c55d3220af" />


## Lessons Learned

- Learned Wazuh agent deployment.
- Learned Sysmon event collection.
- Learned endpoint visibility concepts.
- Learned log analysis workflows.
