🛡️ Wazuh SOC Lab: File Integrity Monitoring & Custom Detection Rule


## 📖 Overview

This project demonstrates hands-on experience with **Wazuh SIEM**, focusing on **File Integrity Monitoring (FIM)** and **Custom Detection Engineering** within a Security Operations Center (SOC) lab environment.

The objective was to configure real-time monitoring for critical files, create custom detection rules, validate alert generation, and perform troubleshooting to ensure accurate security event detection.

---

## 🎯 Project Goals

- Configure File Integrity Monitoring (FIM) on Windows and Linux endpoints
- Detect file modifications, creations, and deletions in real time
- Create a custom Wazuh rule to detect Windows Guest account enablement
- Validate security alerts through controlled testing
- Practice detection engineering and rule tuning workflows

---

## 🏗️ Lab Environment

### Security Platform
- Wazuh Manager
- Wazuh Dashboard
- Wazuh Agents

### Endpoints
- Windows Virtual Machine
- Ubuntu Linux Server

---

## 🔍 File Integrity Monitoring (FIM)

### Windows Monitoring

Configured Wazuh to monitor:

```text
C:\CompanyData
```
<img width="1124" height="461" alt="image" src="https://github.com/user-attachments/assets/66c8da76-c1e9-4940-8984-1e231a6eb185" />

Created and monitored:

```text
C:\CompanyData\Payroll.txt
```
<img width="1422" height="302" alt="image" src="https://github.com/user-attachments/assets/12d8a01a-e815-4221-a050-128619cc8ed2" />

Enabled real-time monitoring:

```xml
<directories realtime="yes">C:\CompanyData</directories>
```

### Linux Monitoring

Configured Wazuh to monitor:

```text
/opt/company-data
```
<img width="762" height="173" alt="image" src="https://github.com/user-attachments/assets/984e8f2f-85fe-4137-9437-fae9ee7cdd5a" />

Enabled real-time monitoring:

```xml
<directories realtime="yes">/opt/company-data</directories>
```
<img width="1103" height="417" alt="image" src="https://github.com/user-attachments/assets/1d93f5d4-7890-496a-b826-c826e4565baa" />

### Validation Results

Performed the following tests:

- File creation
- File modification
- File deletion

Generated successful Wazuh alerts for:

✅ File integrity checksum changes

✅ File modifications

✅ File deletions

✅ Real-time monitoring events
<img width="1896" height="601" alt="image" src="https://github.com/user-attachments/assets/81ef80f5-33a3-4f7a-aa46-203b3032d7bb" />
<img width="1896" height="654" alt="image" src="https://github.com/user-attachments/assets/b8c92eb8-e5a4-42c7-b418-4b3bd52a2195" />

---

## 🚨 Custom Detection Engineering

### Objective

Detect when the built-in Windows **Guest** account becomes enabled.

### Windows Security Event

```text
Event ID 4722
```

Meaning:

```text
A user account was enabled.
```

### Detection Logic

Created a custom rule within:

```text
local_rules.xml
```

Custom rule monitors:

- Event ID 4722
- Guest account activity
- Windows Security events

Example rule logic:

```xml
<rule id="100500" level="12">
  <field name="win.system.eventID">4722</field>
  <field name="win.eventdata.targetUserName">Guest</field>
  <description>Windows Guest Account Enabled</description>
</rule>
```

---

## 🛠️ Troubleshooting & Rule Tuning

The initial custom rule failed to trigger correctly.

Investigation included:

- Reviewing Wazuh alert logs
- Analyzing event field mappings
- Comparing default Wazuh detection rules
- Validating rule groups and dependencies
- Testing multiple rule configurations

### Key Improvements

Added:

```xml
<if_sid>
```

Matched built-in Windows account management rules.

Aligned custom rule with:

- windows
- windows_security
- account_changed
- adduser

After rule tuning, the custom alert successfully triggered.

---

## ✅ Validation

### Test Procedure

1. Disable Windows Guest account
2. Re-enable Guest account
3. Generate Event ID 4722
4. Review alerts in Wazuh Dashboard

### Outcome

✅ Custom alert generated successfully

✅ Event visible in Wazuh Dashboard

✅ Detection rule validated

✅ Alert correlated with Windows Security logs

---

## 📚 Skills Demonstrated

### Security Operations Center (SOC)

- Alert Monitoring
- Event Investigation
- Security Analysis
- Log Correlation

### Detection Engineering

- Custom Wazuh Rules
- XML Rule Development
- Rule Tuning
- Alert Validation

### Endpoint Security

- Windows Security Monitoring
- Linux Security Monitoring
- File Integrity Monitoring

### SIEM Technologies

- Wazuh Manager
- Wazuh Dashboard
- Wazuh Agents

---

## 🎓 Key Takeaways

- Implemented File Integrity Monitoring across Windows and Linux systems.
- Built and validated custom SIEM detection logic.
- Learned how Wazuh rules inherit and correlate with built-in detection rules.
- Applied troubleshooting techniques to improve detection accuracy.
- Demonstrated practical SOC analyst and detection engineering skills.

---

## 📸 Screenshots

screenshots for:

- File modification alert
<img width="1175" height="458" alt="image" src="https://github.com/user-attachments/assets/3aa611f1-d5a8-462d-b78a-e20ff05a48fe" />


- File deletion alert
<img width="1169" height="439" alt="image" src="https://github.com/user-attachments/assets/b59a2a9b-2659-434f-91f6-9f8dcd6809dd" />


- Custom Guest Account Enabled alert
<img width="1465" height="242" alt="image" src="https://github.com/user-attachments/assets/6270192a-ebe3-4631-9dc5-8dff05415a2c" />

- Wazuh Dashboard overview
<img width="1511" height="739" alt="image" src="https://github.com/user-attachments/assets/3cc0b25b-39f4-4d2c-aa51-73bef1418b8e" />

- local_rules.xml configuration
<img width="1498" height="164" alt="image" src="https://github.com/user-attachments/assets/8468b1c9-95ee-4492-a429-9ea502f1a232" />

---

## 🚀 Project Outcome

Successfully deployed real-time File Integrity Monitoring (FIM) and developed a custom Wazuh detection rule capable of identifying Windows Guest account enablement events. This project demonstrates practical skills in SOC operations, SIEM administration, detection engineering, security monitoring, and alert validation.
