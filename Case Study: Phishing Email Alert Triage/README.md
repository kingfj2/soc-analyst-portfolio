# Case Study: Phishing Email Alert Triage

**Platform:** TryHackMe SOC Simulator
**SIEM:** Splunk
**Alert type:** Phishing | **Severity:** Low| **Alert ID:** 8814
**Date:** 10/26/2026
**Analyst:** King FJ Tolentino

## Summary

The alert fired because an inbound email contained an external link. The email came from onboarding@hrconnex.thm, which an internal IT email sent four minutes earlier had announced as the company's new HR vendor. The sender and link domains match, there is no attachment, TryDetectThis found no malicious indicators, and no host contacted hrconnex.thm in the firewall logs for the 24 hours checked. I assessed the alert as a false positive.

## Alert details
<img width="1491" height="425" alt="image" src="https://github.com/user-attachments/assets/af3b404d-347d-4071-a438-abaf84d28f92" />

The Alert was flagged due to inbound email that contains one or more external links


| Field | Value |
|-------|-------|
| Alert rule | Inbound Email Containing Suspicious External Link |
| Timestamp | 10/26/2026 |
| Sender | onboarding@hrconnex.thm |
| Recipient | j.garcia@thetrydaily.thm |
| Subject | Action Required: Finalize Your Onboarding Profile |
| Attachment | none |
| Link domain | hrconnex.thm |

## Triage (the 5 Ws)
- **Who:** 
Sender : onboarding@hrconnex.thm
Recipient : j.garcia@thetrydaily.thm

- **What:** 
  - The sender asks the Receiver to complete the final profile setup to configure the user's access on this link https://hrconnex.thm/onboarding/15400654060/j.garcia

- **Where:** 

  - Mailbox: j.garcia@thetrydaily.thm (recipient of the email)
  - Internal hosts: none observed reaching hrconnex.thm
  - External destination:** hrconnex.thm (link in the email)
  - Logs reviewed: email logs in Splunk; firewall/proxy logs in Splunk
    (all unique URLs, last 24 hours)
    
- **When:** 
   -07:02:03, internal email from IT saying hrconnex.thm is the new HR
    vendor and that J. Garcia is expecting an onboarding email
    <img width="1612" height="316" alt="image" src="https://github.com/user-attachments/assets/aa7356ee-6751-47d7-b9cb-70c97f61d14a" />

  - 07:06:29, onboarding email arrives from onboarding@hrconnex.thm

  <img width="1603" height="307" alt="image" src="https://github.com/user-attachments/assets/0c86a1fe-b7ef-46e4-85d4-2ec6f03d2e7e" />

  - No firewall or proxy activity to hrconnex.thm in the last 24 hours
    (10/25 8:00 AM to 10/26 8:29 AM)
    <img width="1903" height="295" alt="image" src="https://github.com/user-attachments/assets/15ace98e-d8e1-4974-b710-308b4b761bc3" />

- **Why:** 
  - The email attached by the sender appears to be legitimate onboarding message from the company's new HR vendor. So, the alert is likely a false positive. And internal email said four minutes earlier (7:02:03) says hrconnex.thm is the new third-party HR partner and that J. Garcia, the recipient, was waiting for this onboarding link. The link domain matches the sender's domain, and there's no attachment. No firewall or proxy traffic to hrconnex.thm appeared in the last 24hours, so there is no sign anyone visited it. The rule most likely fired because the sender is external and the message contains an "Action Required" link. The vendor relationship is confirmed only by an email, so I recommend verifying it independently.

##  Timeline
| Time | Event | Source |
|------|-------|--------|
| 07:02:03 | Internal IT email announces hrconnex.thm as the new HR vendor | Splunk email logs |
| 07:06:29 | Onboarding email from onboarding@hrconnex.thm delivered to j.garcia | Splunk email logs |
| 10/25 8:00 AM to 10/26 8:29 AM | No firewall/proxy traffic to hrconnex.thm | Splunk firewall logs |

### Sender analysis

- Sender address: onboarding@hrconnex.thm, an external domain
  (the company's own domain is thetrydaily.thm).
  
- Recipient: j.garcia@thetrydaily.thm.

- Link domain vs sender domain: the link in the email points to
  hrconnex.thm, the same domain as the sender, so there's no mismatch
  between the two.
  
- Message content: an onboarding message asking the recipient to
  click a link to finish setting up her profile; subject marked
  "Action Required". The wording is generic, with no attachment.
  
- Lookup tool (TryDetectThis): hrconnex.thm returned
  <img width="1031" height="830" alt="image" src="https://github.com/user-attachments/assets/f1c764db-2b9e-4649-a476-c7758ff4d2c8" />

  
### Attachment review
No attachment present

### Splunk investigation

*Caption: what this search shows.*

```spl
*datasource=firewall URL="*hrconnex*"
```
<img width="1904" height="447" alt="image" src="https://github.com/user-attachments/assets/0429f676-74fd-432e-92c8-135472f3ba31" />
*Firewall search for hrconnex over the 24 hours before the alert.*

What the results showed:
<img width="1568" height="753" alt="image" src="https://github.com/user-attachments/assets/213a0783-6612-4cb3-8d20-8cdca595599c" />


### Related alerts
None Found

## Verdict and reasoning
**Verdict:**  False positive
**Confidence:** Medium 
**Severity:** Low
Why: an internal email from IT at 07:02:03 announced hrconnex.thm as the new HR vendor and said J. Garcia was expecting an onboarding email, and the onboarding email arrived at 07:06:29. The sender domain and link domain are identical, there is no attachment, TryDetectThis found no malicious indicators, and no host contacted hrconnex.thm in the firewall logs for the 24 hours checked. Confidence is Medium because the vendor relationship was confirmed only by an internal email, so I would verify it with IT or HR out of band.

##  Recommended actions
- Close the alert as a false positive, confirm the vendor with IT or HR, consider allowlisting hrconnex.thm for this rule, and raise the bit.ly and m1crosoftsupport.co observations as separate tickets.

##  MITRE ATT&CK mapping
-  T1566.002 Phishing: Spearphishing Link: The alert pattern (an inbound email with an external link asking the user to take action) matches this technique, which is why the rule fired. Not confirmed: the sender was an expected HR vendor, and no host contacted the link domain.
- T1204.001 User Execution: Malicious Link: Considered as the next step if the user had clicked. Not observed: no firewall or proxy traffic to hrconnex.thm in the 24 hours checked.

## Lessons learned
Searching for internal context first (the 07:02:03 IT email announcing the vendor) answered the main question faster than the reputation lookup. Next time I would check internal announcements and the firewall for activity to the domain before running external tools, and I would confirm the vendor relationship with IT or HR to raise confidence from Medium to High.









# Case Study: Phishing Email Alert Triage

**Platform:** TryHackMe SOC Simulator | **SIEM:** Splunk
**Alert type:** Phishing | **Alert ID:** 8814
**Severity:** Low (alert rule rated it Medium)
**Date:** 10/26/2026
**Analyst:** King FJ Tolentino

---

## 1. Summary

The alert fired because an inbound email contained an external link. The email came from onboarding@hrconnex.thm, which an internal IT email sent four minutes earlier had announced as the company's new HR vendor. The sender and link domains match, there is no attachment, TryDetectThis found no malicious indicators, and no host contacted hrconnex.thm in the firewall logs for the 24 hours checked. I assessed the alert as a **false positive**.

## 2. Alert details

![Alert 8814 in the SOC Simulator alert queue](https://github.com/user-attachments/assets/af3b404d-347d-4071-a438-abaf84d28f92)

The alert was flagged because an inbound email contained one or more external links.

| Field | Value |
|-------|-------|
| Alert rule | Inbound Email Containing Suspicious External Link |
| Timestamp | 10/26/2026 |
| Sender | onboarding@hrconnex.thm |
| Recipient | j.garcia@thetrydaily.thm |
| Subject | Action Required: Finalize Your Onboarding Profile |
| Attachment | None |
| Link domain | hrconnex.thm |

## 3. Triage (the 5 Ws)

- **Who:**
  - Sender: onboarding@hrconnex.thm
  - Recipient: j.garcia@thetrydaily.thm

- **What:** The sender asks the recipient to complete their final profile setup and configure their access using this link: `hxxps://hrconnex[.]thm/onboarding/15400654060/j.garcia`

- **Where:**
  - **Mailbox:** j.garcia@thetrydaily.thm (recipient of the email)
  - **Internal hosts:** none observed reaching hrconnex.thm
  - **External destination:** hrconnex.thm (link in the email)
  - **Logs reviewed:** email logs and firewall/proxy logs in Splunk (all unique URLs, last 24 hours)

- **When:**
  - **07:02:03:** internal email from IT states that hrconnex.thm is the new HR vendor and that J. Garcia is expecting an onboarding email.

    ![Internal IT email announcing the new HR vendor](https://github.com/user-attachments/assets/aa7356ee-6751-47d7-b9cb-70c97f61d14a)

  - **07:06:29:** onboarding email arrives from onboarding@hrconnex.thm.

    ![Onboarding email logged at 07:06:29](https://github.com/user-attachments/assets/0c86a1fe-b7ef-46e4-85d4-2ec6f03d2e7e)

  - **10/25 8:00 AM to 10/26 8:29 AM:** no firewall or proxy activity to hrconnex.thm.

    ![Firewall search showing no activity to hrconnex.thm](https://github.com/user-attachments/assets/15ace98e-d8e1-4974-b710-308b4b761bc3)

- **Why:** The rule most likely fired because the sender is external and the message contains an "Action Required" link. The evidence points to a legitimate, expected HR vendor email (see Verdict).

## 4. Investigation

### Sender analysis

- **Sender address:** onboarding@hrconnex.thm, an external domain (the company's own domain is thetrydaily.thm).
- **Recipient:** j.garcia@thetrydaily.thm.
- **Link domain vs sender domain:** the link points to hrconnex.thm, the same domain as the sender, so there is no mismatch.
- **Message content:** an onboarding message asking the recipient to click a link to finish setting up their profile. The subject is marked "Action Required" and the wording is generic. There is no attachment.
- **Internal context:** an internal email at 07:02:03 states that hrconnex.thm is the company's new third-party HR partner and that J. Garcia was expecting this onboarding email.
- **Lookup tool (TryDetectThis):** hrconnex.thm returned no malicious indicators.

  ![TryDetectThis result for hrconnex.thm](https://github.com/user-attachments/assets/f1c764db-2b9e-4649-a476-c7758ff4d2c8)

### Attachment review

No attachment present.

### Splunk investigation

![Splunk search for hrconnex in the firewall logs](https://github.com/user-attachments/assets/0429f676-74fd-432e-92c8-135472f3ba31)
*Firewall search for hrconnex over the 24 hours before the alert.*

```spl
PASTE THE EXACT SEARCH FROM YOUR SCREENSHOT HERE
```

**What the results showed:** 0 events, so no host contacted hrconnex.thm and no traffic to it was allowed or blocked.

![Splunk results showing no events for hrconnex](https://github.com/user-attachments/assets/213a0783-6612-4cb3-8d20-8cdca595599c)

### Related alerts

None found for this alert. Two separate leads from the firewall logs are listed under Recommended actions.

## 5. Timeline

| Time | Event | Source |
|------|-------|--------|
| 07:02:03 | Internal IT email announces hrconnex.thm as the new HR vendor | Splunk email logs |
| 07:06:29 | Onboarding email from onboarding@hrconnex.thm delivered to j.garcia | Splunk email logs |
| 10/25 8:00 AM to 10/26 8:29 AM | No firewall/proxy traffic to hrconnex.thm | Splunk firewall logs |

## 6. Verdict and reasoning

**Verdict:** False positive
**Confidence:** Medium
**Severity:** Low (alert rule rated it Medium)

**Why:** An internal email from IT at 07:02:03 announced hrconnex.thm as the new HR vendor and said J. Garcia was expecting an onboarding email, and the onboarding email arrived at 07:06:29. The sender and link domains are identical, there is no attachment, TryDetectThis found no malicious indicators, and no host contacted hrconnex.thm in the firewall logs for the 24 hours checked. Confidence is Medium because the vendor relationship was confirmed only by an internal email, so I would verify it with IT or HR out of band.

## 7. Recommended actions

- Close the alert as a false positive.
- Confirm the HR vendor relationship with IT or HR.
- Consider allowlisting hrconnex.thm for this rule.
- Raise the blocked bit.ly URL and the lookalike domain m1crosoftsupport.co (both seen in the firewall logs) as separate tickets.

## 8. MITRE ATT&CK mapping

- **T1566.002 Phishing: Spearphishing Link:** the alert pattern (an inbound email with an external link asking the user to take action) matches this technique, which is why the rule fired. Not confirmed: the sender was an expected HR vendor, and no host contacted the link domain.
- **T1204.001 User Execution: Malicious Link:** considered as the next step if the user had clicked. Not observed: no firewall or proxy traffic to hrconnex.thm in the 24 hours checked.

## 9. Lessons learned

Checking internal announcements and the firewall before external tools answered the question fastest. Next time I would also confirm the vendor with IT or HR to raise confidence from Medium to High.
