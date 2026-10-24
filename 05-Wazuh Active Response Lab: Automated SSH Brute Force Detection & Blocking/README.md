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

Detection criteria:

- Failed SSH authentications
- Same source IP address
- Threshold ≥ 3 failed attempts
- Timeframe: 120 seconds

### Alert Description

```text
Multiple SSH login failures observed from the same source IP
```

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

Add screenshots for:

- Custom SSH brute-force rule
- Wazuh alert in Discover
- Active Response configuration
- Firewall-drop alert
- Linux IPTables block entry
- Connectivity test before and after blocking

---

## 🚀 Project Outcome

Successfully implemented a complete detection-and-response workflow using Wazuh. The solution detected repeated SSH authentication failures, generated a custom security alert, and automatically blocked the attacking source IP through Active Response. This project demonstrates hands-on experience in SOC operations, detection engineering, security automation, incident response, and defensive security monitoring.
