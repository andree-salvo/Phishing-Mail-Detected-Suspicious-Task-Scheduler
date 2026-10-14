# 🛡️ SOC140 — Phishing Mail Detected: Suspicious Task Scheduler

### Security Operations Center (SOC) | Phishing Email & Malicious Attachment Investigation

![Platform](https://img.shields.io/badge/Platform-LetsDefend-00bcd4) ![Severity](https://img.shields.io/badge/Severity-Medium-orange) ![Verdict](https://img.shields.io/badge/Verdict-True%20Positive-success) ![Score](https://img.shields.io/badge/Playbook-15%2F15-brightgreen) ![Status](https://img.shields.io/badge/Case-Closed-blue)

**Analyst:** Andree Salvo  
**Event ID:** `82` · **Difficulty:** Beginner · **Detection Source:** Exchange  
**Delivery Outcome:** **Not Delivered** · **Investigation Score:** **100%**

> **Lab scope:** This case was investigated in the simulated LetsDefend SOC environment. Findings below distinguish observed evidence from possible attack behaviors and recommended follow-up actions.

## 📑 Contents

- [Executive Summary](#-executive-summary)
- [Incident Overview](#-incident-overview)
- [Investigation Workflow](#-investigation-workflow)
- [Indicators of Compromise](#-indicators-of-compromise-iocs)
- [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
- [Impact Assessment](#-impact-assessment)
- [Response & Recommendations](#-response--recommendations)
- [Final Results](#-final-results)
- [Skills Demonstrated](#-skills-demonstrated)
- [Lessons Learned](#-lessons-learned)
  
---

## 📌 Executive Summary

I investigated a medium-severity alert, **SOC140 — Phishing Mail Detected — Suspicious Task Scheduler**, involving a suspicious email titled **“COVID19 Vaccine”** sent to `mark@letsdefend.io`.

The email used a COVID-19-themed call to action and referenced the password `infected` for suspicious attachment content. I reviewed email metadata, examined the attachment's reputation in VirusTotal, correlated MD5 and SHA-256 file identifiers, and checked the email delivery outcome.

**Findings:**

- **30/62** VirusTotal engines detected the associated PDF as malicious or suspicious.
- An investigated download URL received **10/92** vendor detections.
- The original alert reported the device action as **Blocked**.
- The LetsDefend playbook confirmed **Not Delivered**.
- I closed the alert as a **True Positive**, scoring **15/15 (100%)**.

**Conclusion:** This was a verified phishing attempt that the email security controls stopped before successful delivery. No attachment execution, scheduled-task creation, or endpoint compromise was confirmed.

## 📊 Incident Overview

| Field | Details |
|---|---|
| Alert | SOC140 — Phishing Mail Detected — Suspicious Task Scheduler |
| Event ID | `82` |
| Event Timestamp | March 21, 2021 — 12:26:57 UTC+03:00 |
| Severity / Difficulty | Medium / Beginner |
| Detection Source | Exchange |
| Sender | `aaronluo@cmail.carleton.ca` |
| Sender IP | `189.162.189.159` |
| Recipient | `mark@letsdefend.io` |
| Subject | `COVID19 Vaccine` |
| Device Action | **Blocked** |
| Delivery Status | **Not Delivered** |
| Final Verdict | **True Positive** |
| Playbook Score | **15/15 (100%)** |
| Case Status | Closed |

<img width="1197" height="51" alt="01" src="https://github.com/user-attachments/assets/aca1fd62-7708-410f-80d5-9c69b1c92670" />

*Evidence 01 — Initial security alert and triage from the LetsDefend monitoring dashboard.*

---

## 🔍 Investigation Workflow

### Phase 01 — Alert Triage

**Objective:** Identify the message source, target, initial risk, and control action.

I reviewed Event ID `82` in the LetsDefend Monitoring dashboard. The alert identified a phishing-related email from `aaronluo@cmail.carleton.ca` (`189.162.189.159`) addressed to `mark@letsdefend.io`, with the subject `COVID19 Vaccine`.

The monitoring alert listed the device action as **Blocked**. That was an initial control signal, which I then checked against the playbook's delivery result.

<img width="1916" height="729" alt="1 - Initial Security Alert   Triage" src="https://github.com/user-attachments/assets/bba57684-f80e-4b6f-9d88-082c10cb5ac2" />

*Figure 1 — Monitoring alert showing the sender, recipient, subject, and blocked action.*

### Phase 02 — Email Content Review

**Objective:** Identify social engineering and attachment indicators.

The email stated:

> “Hey, did you read breaking news about Covid-19. Open it now!”

It also supplied the password `infected`, apparently associated with its attachment. The urgent COVID-19 theme, prompt to open content, and reference to a potentially password-protected file were sufficient to justify deeper inspection. Password protection alone is not proof of maliciousness.

<img width="1913" height="862" alt="2 - Suspicious Email Analysis" src="https://github.com/user-attachments/assets/e21242a3-4ef8-4e9a-9f0d-cd517be202b2" />

*Figure 2 — Email Security view showing the sender, recipient, message body, password reference, and suspicious attachment.*

### Phase 03 — Attachment Reputation Analysis

**Objective:** Assess the attachment using threat intelligence.

The associated file was reported as `Material.pdf`, approximately **350.14 KB**. VirusTotal recorded **30 detections from 62 security vendors**, including labels related to trojans, malicious PDFs, downloaders, and phishing.

**Analyst finding:** The detection ratio supported treating the attachment as malicious; it does **not** establish that the recipient downloaded or executed it.

<img width="1918" height="945" alt="3 - VirusTotal Hash" src="https://github.com/user-attachments/assets/fb62df3d-1481-4b4a-acff-84ff679ed1c1" />

*Figure 3 — VirusTotal file detection results showing 30 out of 62 security vendors flagged the attachment as malicious.*

### Phase 04 — Attachment Hash Verification

**Objective:** Match the analyzed file to the attachment identifier in LetsDefend.

The attachment's **MD5** shown in LetsDefend matched the **MD5** shown in VirusTotal:

```text
MD5
72c812cf21909a48eb9cceb9e04b865d

SHA-256
39fb927c32221134a423760c5d1f58bca4cbbcc87c891c79e390a22b63608eb4
```

The matching MD5 helped correlate the reported attachment with the analyzed PDF.

<img width="1917" height="943" alt="4 - Attachment Verification " src="https://github.com/user-attachments/assets/a359ed60-f87a-4405-9736-5af2e676550a" />

*Figure 4 — VirusTotal details tab showing the MD5 and SHA-256 values used to verify the malicious attachment.*

### Phase 05 — Suspicious Download URL

<img width="1912" height="950" alt="5 - Virustotal suspicious download" src="https://github.com/user-attachments/assets/50864a69-ac89-42ff-a406-290309753552" />

**Objective:** Assess related malicious content hosting.

I examined a URL associated with `download.cyberlearn.academy`, referencing an archive named:

```text
72c812cf21909a48eb9cceb9e04b865d.zip
```

VirusTotal reported **10/92 security vendor detections** for the URL. These results added suspicion to the linked resource, but did not confirm a download or execution on the recipient's device.

> **Note:** A distinct VirusTotal URL-reputation screenshot was not included in the uploaded evidence set, so this phase is documented from the investigation notes only.

### Phase 06 — Additional Infrastructure Review

I also identified `a.pomf.cat` and `69.39.225.3` as **unverified investigation leads**. I attempted further dynamic analysis through ANY.RUN, but the required feature was restricted to business accounts. I did not independently confirm command-and-control activity involving these indicators.

### Phase 07 — Delivery Verification & Verdict

**Objective:** Determine whether the attack reached the intended recipient and classify the alert.

The monitoring view reported **Blocked**. Although another Email Security field displayed **Unknown**, the completed playbook explicitly answered **“Check If Mail Delivered to User?” → “Not Delivered.”**

The malicious file reputation, suspicious email content, and blocked delivery supported a **True Positive** disposition for an attempted phishing attack. There was no confirmed recipient interaction with or execution of the payload.

<img width="1642" height="586" alt="6 - Completed True Positive" src="https://github.com/user-attachments/assets/b6874699-ff94-4555-b7af-b469b369096b" />

*Figure 5 — Closed investigation showing the True Positive verdict, Not Delivered result, and 15/15 playbook score.*

---

## 🚨 Indicators of Compromise (IOCs)

| Type | Indicator | Context |
|---|---|---|
| Sender Email | `aaronluo@cmail.carleton.ca` | Phishing sender |
| Sender IP | `189.162.189.159` | Source of suspicious email |
| Recipient | `mark@letsdefend.io` | Intended target, not a malicious IOC |
| Subject | `COVID19 Vaccine` | Message correlation |
| Attachment Filename | `Material.pdf` | Analyzed PDF |
| Attachment MD5 | `72c812cf21909a48eb9cceb9e04b865d` | Matched across tools |
| Attachment SHA-256 | `39fb927c32221134a423760c5d1f58bca4cbbcc87c891c79e390a22b63608eb4` | File identifier |
| Referenced Archive | `72c812cf21909a48eb9cceb9e04b865d.zip` | Related download reference |
| Download Domain | `download.cyberlearn.academy` | Suspicious URL reputation |
| Additional Domain | `a.pomf.cat` | **Unverified lead** |
| Additional IP | `69.39.225.3` | **Unverified lead** |

> Additional infrastructure indicators should not be treated as verified malicious command-and-control endpoints without corroborating evidence.

## 🧩 MITRE ATT&CK Mapping

| Technique | Assessment | Evidence / Limitation |
|---|---|---|
| [T1566.001 — Spearphishing Attachment](https://attack.mitre.org/techniques/T1566/001/) | Consistent with observed attack attempt | Suspicious email and malicious attachment |
| [T1204.002 — User Execution: Malicious File](https://attack.mitre.org/techniques/T1204/002/) | **Potential, not observed** | Message not delivered; no user execution shown |
| [T1053.005 — Scheduled Task](https://attack.mitre.org/techniques/T1053/005/) | **Unconfirmed lead** | Detection rule references scheduler activity; no task creation evidence |

## ⚠️ Impact Assessment

**Observed:** A suspicious email with a malicious attachment was detected and blocked; the completed playbook classified delivery as **Not Delivered**.

**Potential, not confirmed:** Malware execution, credential exposure, unauthorized activity, or task-based persistence if the attachment were delivered and run.

**Evidence boundary:** No successful delivery, attachment execution, or endpoint compromise was verified.

## 🛠️ Response & Recommendations

**Observed defensive outcome:** Email controls blocked the phishing attempt; the case was classified True Positive and closed.

| Priority | Recommended follow-up | Rationale |
|---|---|---|
| High | Block confirmed malicious file hashes where supported | Reduce exposure through alternate delivery routes |
| High | Hunt for related sender and subject indicators | Identify repeated messages or additional targets |
| Medium | Search email records for other recipients | Assess campaign breadth |
| Medium | Review endpoint telemetry if independent exposure is found | Check for execution or malicious files |
| Medium | Investigate scheduled-task activity if supported by endpoint alerts | Validate the scheduler-themed detection |
| Medium | Validate `a.pomf.cat` and `69.39.225.3` | Avoid treating unconfirmed leads as established malicious infrastructure |
| Ongoing | Reinforce phishing awareness | Reduce user susceptibility |

These are **recommendations**, not actions claimed as completed in the lab.

## ✅ Final Results

| Investigation Component | Outcome |
|---|---|
| Severity | Medium |
| Email Delivery | **Not Delivered** |
| Device Action | Blocked |
| Attachment / URL Present | Yes |
| File Reputation | **30/62 vendors** |
| Download URL Reputation | **10/92 vendors** |
| Final Classification | **True Positive** |
| Playbook Score | **15/15 (100%)** |
| Case Status | **Closed** |

**Closed:** October 13, 2026, at 23:39:28 UTC.

## 🧠 Skills Demonstrated

- SOC alert triage and incident prioritization
- Phishing email and social engineering analysis
- Email metadata and attachment investigation
- VirusTotal file and URL intelligence
- MD5 and SHA-256 verification
- IOC collection and threat correlation
- Delivery status verification
- MITRE ATT&CK technique mapping
- Evidence-based incident reporting

## 💡 Lessons Learned

A **True Positive** phishing verdict does not imply a successful compromise. In this case, security controls detected and blocked malicious content, while the playbook confirmed non-delivery. File hashes allowed correlation across tools, and the lack of sandbox access illustrated why unverified indicators should be documented as leads—not facts.

---

### Project Notes

- **Platform:** LetsDefend
- **Scenario:** SOC140 — Phishing Mail Detected — Suspicious Task Scheduler
- **Event ID:** 82
- **Analyst:** Andree Salvo
- **Result:** True Positive / Not Delivered / 15 out of 15

*This project documents a simulated training investigation for educational and professional portfolio purposes.*
