# SOC Analyst Lab: Windows & Linux Log Analysis Using Wazuh

## Overview

This project demonstrates hands-on Security Operations Center (SOC) analyst skills using Wazuh SIEM. The objective was to generate and investigate security events from Windows and Ubuntu endpoints to understand how user activity, authentication events, privilege changes, and SSH sessions are logged and analyzed.

By performing administrative and authentication-related actions on both operating systems, I learned how to identify and investigate common security events that SOC analysts encounter during daily monitoring and incident investigations.

---

## Lab Environment

| Component | Technology |
|------------|------------|
| SIEM Platform | Wazuh |
| Windows Endpoint | Windows 10 VM |
| Linux Endpoint | Ubuntu VM |
| Virtualization | VMware |
| Remote Access | RDP & SSH |
| Log Sources | Windows Security Logs, Linux Auth Logs, Sysmon |

---

## Objectives

- Analyze Windows Security Event Logs
- Understand account creation and deletion activities
- Investigate successful user logins
- Track privilege escalation events
- Monitor failed and successful SSH authentication attempts
- Correlate Linux SSH sessions with command execution activity
- Build practical SOC investigation skills

---

# Windows Investigation Activities

## User Enumeration

Executed the following commands:

```cmd
whoami
ipconfig /all
net user
net localgroup administrators
```

Purpose:

- Identify current logged-in user
- Review system network configuration
- Enumerate local user accounts
- Identify administrative users

---

## Enable Guest Account

```cmd
net user guest /active:yes
```

Observed Activity:

- Guest account enabled
- Account status change recorded in Windows logs

---

## Change Guest Account Password

```cmd
net user guest PASSWORD
```

Purpose:

- Generate account modification events
- Observe credential-related logging

---

## Create Local User

```cmd
net user wilma012 password123 /add
```
<img width="983" height="702" alt="image" src="https://github.com/user-attachments/assets/a5ac6c7b-2afa-4995-a8f6-1d9de7399bd8" />

### Windows Event ID 4720

**A user account was created**

Investigation Findings:

- Account Created: wilma012
- Created By: Bob
- Event Source: Windows Security Log

---

## Add User to Administrators Group

```cmd
net localgroup administrators wilma012 /add
```

### Windows Event ID 4732

**A member was added to a security-enabled local group**

Investigation Findings:

- User Added: wilma012
- Target Group: Administrators
- Action Performed By: Bob

Security Significance:

This activity represents a privilege escalation event because administrative rights were granted to a standard user account.
<img width="1478" height="348" alt="image" src="https://github.com/user-attachments/assets/08f218f0-f094-4ecb-9f19-5d32f4b03b44" />

---

## Delete User Account

```cmd
net user wilma012 /delete
```

### Windows Event ID 4726

**A user account was deleted**

Investigation Findings:

| Field | Value |
|---------|---------|
| Deleted Account | student1 |
| Performed By | Bob |
| Event ID | 4726 |

Go to https://www.ultimatewindowssecurity.com/ to verify the EVENTID
https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-identifiers - this is the information of each Idenfiyers

Analysis:

The account wilma012 was successfully deleted by user Bob.

---

## Successful Logon Investigation

### Windows Event ID 4624

**An account was successfully logged on**

Observed Values:

| Field | Value |
|---------|---------|
| Event ID | 4624 |
| User | Bob |
| Logon Type | 2 |

Logon Type Interpretation:

| Logon Type | Description |
|------------|-------------|
| 2 | Interactive Login |
| 3 | Network Login |
| 10 | Remote Desktop (RDP) |

Analysis:

The generated event confirmed a successful local interactive login performed through the VM console.
<img width="1469" height="518" alt="image" src="https://github.com/user-attachments/assets/d931fb8a-1381-431c-9846-6dcacf7e5dc2" />

---

# Linux Investigation Activities

## Failed SSH Authentication

Attempted SSH login using an invalid user account:

```bash
ssh fakeuser@192.168.136.129
```


Observed Log:

```text
Failed password for invalid user
```


Captured Information:

- Source IP Address
- Invalid Username
- Source Port
- Authentication Failure

Security Relevance:

Failed authentication attempts are commonly associated with brute-force attacks, credential misuse, or reconnaissance activity.

<img width="1486" height="871" alt="image" src="https://github.com/user-attachments/assets/43a8cfe5-33be-4d58-bf7a-8d87e7992a1b" />

---

## Successful SSH Authentication

Successful login:

```bash
ssh mydfir@192.168.136.129
```

Observed Log:

```text
Accepted password for mydfir
```

Captured Fields:

- Username
- Source IP Address
- Source Port
- Authentication Status

Analysis:

Successful authentication generated SSH session records that could later be correlated with user activity.
<img width="1480" height="492" alt="image" src="https://github.com/user-attachments/assets/5c319418-3fd9-4dec-940c-e8312c90870c" />

---

## Command Execution Monitoring

Commands Executed:

```bash
whoami
ip a
cat /etc/passwd
```

Observed Activity:

Wazuh captured command execution details, including:

- User executing the command
- Command line arguments
- Working directory
- Timestamp

Example:

```bash
ip a
```

Purpose:

Demonstrate Linux command auditing and user activity monitoring.

---

## SSH Session Correlation

Observed Events:

```text
New session 18
```

```text
Session closed
```

Analysis Process:

1. Identify session creation event.
2. Capture associated session ID.
3. Review commands executed during the session.
4. Locate session termination event.
5. Correlate activity across the entire login session.

SOC Relevance:

Session correlation allows analysts to determine:

- When a user logged in
- What actions occurred during the session
- When the session ended

---

# Key Event IDs Investigated

| Event ID | Description |
|------------|------------|
| 4624 | Successful Logon |
| 4720 | User Account Created |
| 4726 | User Account Deleted |
| 4732 | User Added to Local Administrators Group |

---

# Wazuh Search Queries Used

```kql
data.win.system.eventID:4624
```

```kql
data.win.system.eventID:4720
```

```kql
data.win.system.eventID:4726
```

```kql
data.win.system.eventID:4732
```

Linux SSH Investigation:

```kql
fakeuser
```

```kql
accepted password
```

```kql
session
```

---

# Skills Demonstrated

- Security Monitoring
- SIEM Investigation
- Wazuh Administration
- Windows Event Log Analysis
- Linux Log Analysis
- User Account Lifecycle Investigation
- Authentication Monitoring
- Privilege Escalation Detection
- SSH Session Correlation
- Incident Investigation
- Threat Detection Fundamentals

---

# Key Takeaways

This lab provided practical experience analyzing Windows and Linux security events through Wazuh SIEM. By generating real endpoint activity and investigating the resulting logs, I developed foundational SOC analyst skills including event analysis, authentication investigation, privilege escalation tracking, and session correlation.

The knowledge gained from this project is directly transferable to enterprise SIEM platforms and real-world SOC environments.

---

## Author

**King FJ P. Tolentino**

Aspiring SOC Analyst | IT Support Engineer | Cybersecurity Enthusiast

LinkedIn: www.linkedin.com/in/king-fj-p-tolentino  
GitHub: https://github.com/kingfj2
