# ⚡ Wazuh Active Response Lab: Automated SSH Brute Force Detection & Blocking

## 📖 Overview

This project demonstrates how to move beyond detection and implement **automated threat response** using Wazuh Active Response.

The objective was to detect multiple failed SSH login attempts against a Linux server, create a custom brute-force detection rule, and automatically block the attacker's IP address using firewall actions.

This simulates a real-world SOC workflow where security tools not only identify threats but also respond to them automatically.

---

## 🎯 Project Goals

- Detect repeated failed SSH login attempts
- Create a custom Wazuh detection rule
- Configure Wazuh Active Response
- Automatically block malicious IP addresses
- Validate automated containment actions
- Simulate a brute-force attack scenario

---

## 🏗️ Lab Environment

### Security Platform
- Wazuh Manager
- Wazuh Dashboard
- Wazuh Agents

### Endpoints
- Windows Workstation (Attack Simulation)
- Ubuntu Linux Server (Target System)

---

## 🔍 Attack Simulation

A brute-force style attack was simulated by repeatedly attempting SSH authentication with incorrect credentials.

Example:

```powershell
ssh user@192.168.x.x
```

Three consecutive failed login attempts were generated against the Ubuntu server.
<img width="632" height="240" alt="image" src="https://github.com/user-attachments/assets/44b02a19-eacc-489e-b295-f667e1657cae" />


---

## 🚨 Custom Detection Rule

### Objective

Generate a dedicated security alert when:

- 3 or more SSH login failures occur
- From the same source IP
- Within a defined time window

### Rule Logic

Created a custom rule in:

```text
local_rules.xml
```
<img width="1502" height="778" alt="image" src="https://github.com/user-attachments/assets/49930299-238b-423f-ad0e-4454a1a7e097" />


Detection criteria:

- Failed SSH authentications
- Same source IP address
- Threshold ≥ 3 failed attempts
- Timeframe: 120 seconds

### Alert Description

```text
Multiple SSH login failures observed from the same source IP
```
<img width="1195" height="368" alt="image" src="https://github.com/user-attachments/assets/87e95091-84da-43ef-a463-d453879473b1" />

---

## ✅ Rule Validation

Performed three failed SSH login attempts.

Result:

✅ Custom Wazuh rule triggered

✅ Alert visible in Wazuh Dashboard

✅ Rule successfully correlated multiple failed login events

Example alert:

```text
Multiple SSH login failures observed from the same source IP
```
<img width="1195" height="368" alt="image" src="https://github.com/user-attachments/assets/dfd5f218-2bf4-42f4-8268-e4924c410951" />

---

## 🤖 Active Response Configuration

Configured Wazuh Active Response to automatically react when the custom brute-force detection rule was triggered.

### Response Action

```xml
<active-response>
  <disabled>no</disabled>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>100101</rules_id>
</active-response>
```
<img width="723" height="182" alt="image" src="https://github.com/user-attachments/assets/3b1b01bb-c2af-401d-bbb4-3a701e7125f5" />

### Configuration Details

| Setting | Purpose |
|----------|----------|
| disabled = no | Enable active response |
| command = firewall-drop | Block attacker IP |
| location = local | Execute on affected endpoint |
| rules_id = 100101 | Trigger from custom SSH brute-force rule |

---

## 🛡️ Automated Containment

When the custom rule detected three failed SSH login attempts:

1. Wazuh generated an alert
2. Active Response executed automatically
3. The attacker's IP address was added to the Linux firewall deny list
4. Further communication was blocked

Generated alert:

```text
Host blocked by firewall-drop active response
```

---

## 🔬 Proof of Concept

### Before Attack

```bash
ping 192.168.x.x
```

Result:

```text
Reply from target
```

### Trigger Brute Force Rule

Performed three failed SSH login attempts.

### After Active Response

```text
Request timed out
```

Result:

✅ Network connectivity blocked

✅ Source IP automatically denied

✅ No manual analyst intervention required

---

## 🧹 Recovery Process

Verified blocked IP addresses using:

```bash
sudo iptables -L -n --line-numbers
```

Removed firewall entries:

```bash
sudo iptables -D INPUT 1
sudo iptables -D FORWARD 1
```

Connectivity was restored successfully.

---

## 📚 Skills Demonstrated

### Security Operations (SOC)

- Threat Detection
- Alert Investigation
- Incident Response
- Security Monitoring

### Detection Engineering

- Custom Rule Creation
- Rule Correlation
- Threshold-Based Detection
- Brute Force Detection

### Security Automation

- Wazuh Active Response
- Automated Containment
- Firewall Automation
- Defensive Security Controls

### Linux Security

- SSH Monitoring
- IPTables Management
- Host-Based Protection

---

## 🎓 Key Takeaways

- Security monitoring becomes more effective when detection and response are integrated.
- Wazuh Active Response can automatically contain threats without analyst intervention.
- Custom detection rules enable organizations to monitor attack patterns specific to their environment.
- Automated blocking can significantly reduce the impact of brute-force attacks.
- Testing and validation are critical before deploying automated response actions in production environments.

---

## 📸 Screenshots

 screenshots for:

- Custom SSH brute-force rule
<img width="883" height="421" alt="image" src="https://github.com/user-attachments/assets/89ab6274-1d14-4d51-afd0-7fab70f58b53" />

- Wazuh alert in Discover
<img width="1067" height="278" alt="image" src="https://github.com/user-attachments/assets/160a9d73-368e-4dc0-84c5-6386e90d7925" />

- Active Response configuration
  <img width="903" height="175" alt="image" src="https://github.com/user-attachments/assets/9dac3c6c-ec1d-43a0-9df8-35d06f7f8fcc" />

- Firewall-drop alert
<img width="1067" height="278" alt="image" src="https://github.com/user-attachments/assets/3711416c-d3cb-437b-86ca-e989c0d113c6" />

- Linux IPTables block entry
<img width="626" height="259" alt="image" src="https://github.com/user-attachments/assets/197ac828-5c52-446d-b9a5-f3718d5c0bbf" />

-Linux IPTables Recovery
<img width="548" height="258" alt="image" src="https://github.com/user-attachments/assets/ea80ff70-a651-4408-98f0-d1d46ea45a8b" />

<img width="410" height="143" alt="image" src="https://github.com/user-attachments/assets/5f60aecf-1a8f-4cb6-9e2b-db4808f8ad4b" />

- Connectivity test before and after blocking
<img width="941" height="702" alt="image" src="https://github.com/user-attachments/assets/c849996d-85ab-4f45-83e7-529ef949b894" />


---

## 🚀 Project Outcome

Successfully implemented a complete detection-and-response workflow using Wazuh. The solution detected repeated SSH authentication failures, generated a custom security alert, and automatically blocked the attacking source IP through Active Response. This project demonstrates hands-on experience in SOC operations, detection engineering, security automation, incident response, and defensive security monitoring.
