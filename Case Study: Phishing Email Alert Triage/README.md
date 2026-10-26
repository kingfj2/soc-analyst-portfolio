# Case Study: Phishing Email Alert Triage

**Platform:** TryHackMe SOC Simulator | **SIEM:** Splunk
**Alert type:** Phishing | **Alert ID:** 8814
**Severity:** Low (alert rule rated it Medium)
**Date:** 10/26/2026
**Analyst:** King FJ Tolentino

---

## 1. Summary

The alert fired because an inbound email contained an external link. The email came from onboarding@hrconnex.thm, which an internal email sent four minutes earlier had described as the company's new third-party HR partner. The sender and link domains match, there is no attachment, TryDetectThis found no malicious indicators, and no host contacted hrconnex.thm in the firewall logs for the period checked. I assessed the alert as a **false positive**.

## 2. Alert details

![Alert 8814 in the SOC Simulator alert queue](https://github.com/user-attachments/assets/af3b404d-347d-4071-a438-abaf84d28f92)

The alert was flagged because an inbound email contained one or more external links.

| Field | Value |
|-------|-------|
| Alert rule | Inbound Email Containing Suspicious External Link |
| Timestamp | 10/26/2026, 07:06:29 (email log time) |
| Sender | onboarding@hrconnex.thm |
| Recipient | j.garcia@thetrydaily.thm |
| Subject | Action Required: Finalize Your Onboarding Profile |
| Attachment | None |
| Link domain | hrconnex.thm |

## 3. Triage (the 5 Ws)

- **Who:**
  - Sender: onboarding@hrconnex.thm
  - Recipient: j.garcia@thetrydaily.thm

- **What:** The sender asks the recipient to complete her final profile setup and configure her access using this link: `hxxps://hrconnex[.]thm/onboarding/15400654060/j.garcia`

- **Where:**
  - **Mailbox:** j.garcia@thetrydaily.thm (recipient of the email)
  - **Internal hosts:** none observed reaching hrconnex.thm
  - **External destination:** hrconnex.thm (link in the email)
  - **Logs reviewed:** email logs and firewall logs in Splunk (searched for hrconnex, 10/25 8:00 AM to 10/26 8:29 AM)

- **When:**
  - **07:02:03:** internal email (h.harris to the IT team) says J. Garcia has not received her onboarding email from hrconnex.thm, the company's new third-party HR partner.

    ![Internal email to the IT team about the missing onboarding email from hrconnex.thm](https://github.com/user-attachments/assets/aa7356ee-6751-47d7-b9cb-70c97f61d14a)

  - **07:06:29:** onboarding email arrives from onboarding@hrconnex.thm.

    ![Onboarding email logged at 07:06:29](https://github.com/user-attachments/assets/0c86a1fe-b7ef-46e4-85d4-2ec6f03d2e7e)

  - **10/25 8:00 AM to 10/26 8:29 AM:** no firewall activity to hrconnex.thm.

    ![Firewall search showing no activity to hrconnex.thm](https://github.com/user-attachments/assets/15ace98e-d8e1-4974-b710-308b4b761bc3)

- **Why:** The rule most likely fired because the sender is external and the message contains an "Action Required" link. The evidence points to a legitimate, expected HR vendor email (see Verdict).

## 4. Investigation

### Sender analysis

- **Sender address:** onboarding@hrconnex.thm, an external domain (the company's own domain is thetrydaily.thm).
- **Recipient:** j.garcia@thetrydaily.thm.
- **Link domain vs sender domain:** the link points to hrconnex.thm, the same domain as the sender, so there is no mismatch. The logged email body shows that the link's underlying address also points to hrconnex.thm.
- **Message content:** a personalized onboarding message that addresses Ms. Garcia, references TheTryDaily and the HR Onboarding Team, and contains one link labeled "Set Up My Profile." The subject is marked "Action Required." There is no attachment.
- **Internal context:** an internal email at 07:02:03 (h.harris to the IT team) states that hrconnex.thm is the company's new third-party HR partner and that J. Garcia had not yet received her onboarding email.
- **Lookup tool (TryDetectThis):** hrconnex.thm returned no malicious indicators.

  ![TryDetectThis result for hrconnex.thm](https://github.com/user-attachments/assets/f1c764db-2b9e-4649-a476-c7758ff4d2c8)

### Attachment review

No attachment present.

### Splunk investigation

**Search 1: email logs** (10/26/26, 7:00 to 8:00 AM)

![Splunk email log search for hrconnex.thm returning 3 events](https://github.com/user-attachments/assets/213a0783-6612-4cb3-8d20-8cdca595599c)

```spl
*datasource=firewall URL="*hrconnex*"
```

**Search 2: firewall logs** (10/25 8:00 AM to 10/26 8:29 AM)

![Splunk firewall search for hrconnex](https://github.com/user-attachments/assets/0429f676-74fd-432e-92c8-135472f3ba31)

```spl
datasource=firewall URL="*hrconnex*"
```

Returned 0 events, so no host contacted hrconnex.thm and no traffic to it was allowed or blocked.

### Related alerts

None found for this alert. Two separate leads from the firewall logs are listed under Recommended actions.

## 5. Timeline

| Time | Event | Source |
|------|-------|--------|
| 07:02:03 | Internal email to IT reports J. Garcia has not received her onboarding email from hrconnex.thm, the new third-party HR partner | Splunk email logs |
| 07:06:29 | Onboarding email from onboarding@hrconnex.thm delivered to j.garcia | Splunk email logs |
| 10/25 8:00 AM to 10/26 8:29 AM | No firewall traffic to hrconnex.thm | Splunk firewall logs |

## 6. Verdict and reasoning

**Verdict:** False positive
**Confidence:** Medium
**Severity:** Low (alert rule rated it Medium)

**Why:** An internal email at 07:02:03 (h.harris to the IT team) described hrconnex.thm as the company's new third-party HR partner and said J. Garcia was still waiting for her onboarding email, which arrived at 07:06:29. The sender and link domains are identical, there is no attachment, TryDetectThis found no malicious indicators, and no host contacted hrconnex.thm in the firewall logs for the period checked. Confidence is Medium because the vendor relationship rests on a single internal employee's email rather than an official announcement, so I would verify it with IT or HR out of band.

## 7. Recommended actions

- Close the alert as a false positive.
- Confirm the HR vendor relationship with IT or HR.
- Consider allowlisting hrconnex.thm for this rule.
- Raise the blocked bit.ly URL and the lookalike domain m1crosoftsupport.co (both seen in the firewall logs) as separate tickets.

## 8. MITRE ATT&CK mapping

- **T1566.002 Phishing: Spearphishing Link:** the alert pattern (an inbound email with an external link asking the user to take action) matches this technique, which is why the rule fired. Not confirmed: the sender was an expected HR vendor, and no host contacted the link domain.
- **T1204.001 User Execution: Malicious Link:** considered as the next step if the user had clicked. Not observed: no firewall traffic to hrconnex.thm in the period checked.

## 9. Lessons learned

Checking internal context and the firewall before external tools answered the question fastest. Next time I would also confirm the vendor with IT or HR to raise confidence from Medium to High.
