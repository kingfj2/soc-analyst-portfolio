# 📊 Wazuh SOC Lab: Security Monitoring Dashboard & Log Analysis

## 📖 Overview

This project demonstrates the creation of a custom Security Operations Center (SOC) dashboard using **Wazuh Dashboard**. The goal was to transform raw security telemetry into actionable visualizations that provide quick visibility into authentication events, account management activity, and potential security incidents.

The dashboard was designed to help analysts monitor both Windows and Linux systems through a centralized view of security events.

---

## 🎯 Project Objectives

- Build a custom SOC dashboard in Wazuh
- Visualize Windows authentication failures
- Monitor Windows account management activity
- Track failed SSH authentication attempts on Linux
- Create actionable security-focused visualizations
- Improve visibility into authentication-related events

---

## 🏗️ Lab Environment

### Security Platform

- Wazuh Manager
- Wazuh Dashboard
- Wazuh Agents

### Endpoints

- Windows Virtual Machine
- Ubuntu Linux Server

### Data Source

```text
wazuh-archives
```

---

## 📊 Dashboard Design

The dashboard consists of three primary panels designed to provide a basic SOC activity overview.

### Panel 1: Failed Windows Logon Attempts

**Visualization Type:** Metric

Purpose:

- Display the total number of failed Windows logon attempts
- Monitor authentication failures at a glance

Event Filter:

```text
Event ID 4625
```

Description:

```text
An account failed to log on
```

Key Benefit:

Provides immediate visibility into potential password spraying, brute-force attempts, or user authentication issues.

---

### Panel 2: Windows Account Changes Over Time

**Visualization Type:** Line Chart

Purpose:

Track account management activities over time.

Monitored Event IDs:

| Event ID | Description |
|-----------|------------|
| 4720 | User Account Created |
| 4722 | User Account Enabled |
| 4723 | Password Change Attempt |
| 4724 | Password Reset Attempt |
| 4725 | User Account Disabled |
| 4726 | User Account Deleted |
| 4732 | User Added to Security Group |
| 4733 | User Removed from Security Group |
| 4738 | User Account Changed |

Configuration:

- Y-Axis: Event Count
- X-Axis: Timestamp
- Split Series: Event ID

Key Benefit:

Allows analysts to identify unusual spikes in account management activity that may indicate administrative misuse or account compromise.

---

### Panel 3: Linux Failed SSH Authentication Activity

**Visualization Type:** Data Table

Purpose:

Monitor failed SSH login attempts against the Linux server.

Filter:

```text
"Failed password"
```

Displayed Fields:

- Agent Name
- Timestamp
- Source User
- Destination User
- Source IP Address

Key Benefit:

Provides visibility into unauthorized SSH access attempts and potential brute-force activity.

---

## 🔍 Investigation Workflow

Security events were analyzed using:

- Wazuh Discover
- Event Filtering
- Event Field Mapping
- Dashboard Visualizations

Example investigations included:

### Failed Windows Logons

```text
Event ID 4625
```

Used to identify failed user authentication attempts.

### Linux SSH Failures

```text
Failed password
```

Used to identify unsuccessful SSH login attempts.

### Account Management Events

```text
4720, 4722, 4725, 4726, 4732, 4733, 4738
```

Used to track modifications to local accounts and group memberships.

---

## ✅ Dashboard Validation

Generated test telemetry by:

### Windows

- Failed user login attempts
- Account management activity

### Linux

- Failed SSH authentication attempts

Verification Results:

✅ Authentication failures displayed correctly

✅ Windows account changes visualized successfully

✅ Linux SSH failed login activity captured

✅ Real-time event monitoring confirmed

---

## 📚 Skills Demonstrated

### Security Operations Center (SOC)

- Security Monitoring
- Log Analysis
- Event Investigation
- Dashboard Development

### Threat Detection

- Authentication Monitoring
- Account Management Monitoring
- SSH Activity Analysis
- Security Event Correlation

### SIEM Technologies

- Wazuh Dashboard
- Wazuh Discover
- Wazuh Archives
- Data Visualization

### Security Analytics

- Event ID Analysis
- Query Development
- Visualization Design
- Security Reporting

---

## 🎓 Key Takeaways

- Effective dashboards help analysts identify security issues faster.
- Authentication monitoring provides valuable visibility into potential attacks.
- Security visualizations improve detection and investigation workflows.
- Combining Windows and Linux telemetry creates a centralized monitoring experience.
- Proper event filtering reduces noise and highlights meaningful security activity.

---

## 📸 Screenshots

 Screenshots of:

- Complete SOC Dashboard
<img width="1539" height="750" alt="image" src="https://github.com/user-attachments/assets/09d4f52a-3348-4d52-a603-6c9e5f155560" />

- Failed Windows Logon Metric
<img width="774" height="340" alt="image" src="https://github.com/user-attachments/assets/53b25aa9-462d-4209-9520-07f2cf60b226" />
  
- Windows Account Changes Line Chart
<img width="759" height="417" alt="image" src="https://github.com/user-attachments/assets/73a8f428-1bb1-42f3-9d48-e8abb9806ffc" />

- Linux Failed SSH Authentication Table
<img width="758" height="255" alt="image" src="https://github.com/user-attachments/assets/8882032d-180c-4de6-b110-9fae288772e9" />

- Wazuh Discover Queries
<img width="1199" height="846" alt="image" src="https://github.com/user-attachments/assets/d3e1bc05-8810-464b-935f-9aa21012a926" />


---

## 🚀 Project Outcome

Successfully developed a custom SOC monitoring dashboard within Wazuh that visualizes authentication failures, account management activities, and Linux SSH login attempts. This project demonstrates practical experience with SIEM dashboards, log analysis, security monitoring, incident investigation, and security data visualization commonly performed by SOC Analysts and Blue Team professionals.
