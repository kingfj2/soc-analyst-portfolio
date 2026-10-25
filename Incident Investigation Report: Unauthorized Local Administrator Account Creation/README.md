# 🔍 Incident Investigation Report: Unauthorized Local Administrator Account Creation

## Project Overview

This project simulates an attacker creating a local account, escalating privileges by adding the account to the Administrators group, and deleting the account to remove traces of activity. The investigation was conducted using Wazuh SIEM and Windows Security Event Logs.

---

# Findings

During the investigation, a local user account named **HACKER** was created on the endpoint, added to the local Administrators group, and later deleted.

The following Windows Security Events were identified:

| Event ID | Description |
|-----------|-------------|
| 4720 | User Account Created |
| 4738 | User Account Changed |
| 4732 | User Added to Local Administrators Group |
| 4726 | User Account Deleted |

The activity demonstrates a complete account lifecycle that resembles attacker behavior used for persistence and privilege escalation.

---

# Investigation Summary

A new local account was created and granted administrative privileges. Shortly afterward, the account was deleted.

Analysis of Windows Security Logs collected by Wazuh confirmed:

1. Account creation
2. Privilege escalation
3. Account deletion

This attack chain is commonly observed during unauthorized access and persistence activities.

---

### Account Created
```
HACKER
```

### Account Performing Activity
```
Administrator
```

### Affected Endpoint
```
DESKTOP-222R39R
```

---

A local user account was created and added to the Administrators group. The account was subsequently removed from the system.

Observed Activities:

- User Account Creation
- Privilege Escalation
- Account Deletion

---
### Timezone
```
GMT+8 (Philippine Standard Time)
```

| Timestamp | Event |
|------------|---------|
| 2026-10-25 18:35:32 | Account Created |
| 2026-10-25 18:35:32 | Account Changed |
| 2026-10-25 18:36:15 | Added to Administrators |
| 2026-10-25 18:36:42 | Account Deleted |

### Current Status

✅ Activity has stopped

✅ Account has been removed

✅ No additional suspicious account activity observed

---

### Hostname
```
DESKTOP-222R39R
```

### Operating System
```
Windows 10
```

### Log Source
```
Windows Security Logs
```

### Monitoring Platform
```
Wazuh SIEM
```

---
This activity was performed within a home lab environment to simulate unauthorized account creation and privilege escalation.

In a real-world attack scenario, adversaries may perform these actions to:

- Establish persistence
- Maintain unauthorized access
- Gain administrator privileges
- Evade detection

---

### Account Creation

```cmd
net user HACKER Password123! /add
```

### Privilege Escalation

```cmd
net localgroup administrators HACKER /add
```

### Account Deletion

```cmd
net user HACKER /delete
```

Windows Security Logs generated corresponding events, which were collected and analyzed through Wazuh.

---

# Evidence

## Event ID 4720 - User Account Created

**Screenshot**

<img width="1006" height="419" alt="image" src="https://github.com/user-attachments/assets/d3a17be2-a951-4561-b7f4-be6cefa87f41" />


---

## Event ID 4720 - User Account Created

<img width="961" height="293" alt="image" src="https://github.com/user-attachments/assets/a12380dc-846f-41e7-9c42-2ca65b5d6178" />

----

## Event ID 4732 - Added to Administrators Group

**Screenshot**

<img width="973" height="295" alt="image" src="https://github.com/user-attachments/assets/ee295587-8a99-4edc-993a-bbf6ef2d6307" />


---

## Event ID 4726 - User Account Deleted

**Screenshot**

<img width="1003" height="407" alt="image" src="https://github.com/user-attachments/assets/572a5c58-c9a3-482f-a43b-211fe34e2021" />


---

## Wazuh Investigation View

**Screenshot**

screenshots/wazuh-discover.png

---

# MITRE ATT&CK Mapping

| Technique | ID |
|------------|------------|
| Create Account | T1136 |
| Local Account | T1136.001 |
| Valid Accounts | T1078 |
| Account Manipulation | T1098 |
| Indicator Removal on Host | T1070 |

---

# Recommendations

### Detection Improvements

Monitor the following Event IDs:

- 4720 (User Account Created)
-	4738 (User Account Changed)
- 4732 (Added to Administrators Group)
- 4726 (User Account Deleted)

### Security Controls

- Audit administrator group membership regularly.
- Alert on new local user creation.
- Alert when users are added to privileged groups.
- Implement least privilege principles.
- Enable centralized log monitoring.

### SIEM Correlation Rule

Generate a High Severity Alert when:

```text
Account Created
+
Added to Administrators Group
+
Account Deleted

Within 15 Minutes
```

---

# Lessons Learned

This exercise improved my understanding of:

- Windows Security Event Logs
- Wazuh SIEM Investigation
- Event Correlation
- Privilege Escalation Detection
- MITRE ATT&CK Mapping
- Incident Documentation

---

# Analyst Conclusion

The investigation successfully identified the creation of a local account, elevation of privileges through membership in the Administrators group, and deletion of the account. Although conducted in a controlled home lab environment, the activity closely mirrors real-world attacker behavior used to establish persistence and privileged access on Windows systems.

The exercise demonstrates the ability to detect, investigate, document, and report suspicious account management activity using Wazuh SIEM and Windows Security Logs.
