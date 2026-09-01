# Introduction to Phishing — SOC Simulator

## 1. Objective

Practise a basic SOC phishing investigation:

```text
Alert → Investigate → Enrich → Correlate → Verdict → Report → Close/Escalate
```

The focus is on analysing suspicious emails, gathering evidence, and deciding whether the alert is a **True Positive (TP)** or **False Positive (FP)**.

---

## 2. Phishing Alert — What to Check

### Email
- Sender and recipient
- Subject/body
- Suspicious or unusual wording
- Urgency/social engineering
- Requests for credentials, payment, or sensitive information

### Links
- URL/domain
- Lookalike or suspicious domains
- URL shorteners
- Destination mismatch
- Malicious reputation

### Attachments
- Unexpected files
- Executables/scripts
- Disguised file types
- Suspicious/malicious files

---

## 3. Investigation Workflow

```text
Receive Alert
     ↓
Review Email
     ↓
Analyse Sender / URL / Attachment
     ↓
Threat Intelligence Enrichment
     ↓
Search SIEM for Related Activity
     ↓
Check User / Host / Network
     ↓
Determine TP / FP
     ↓
Document Findings
     ↓
Close / Escalate
```

### Threat Intelligence

Check suspicious:
- URLs
- Domains
- IPs
- File hashes

> Reputation is supporting evidence; combine it with user, host, email, and network context.

---

## 4. SIEM Correlation

Check whether the recipient interacted with the phishing email.

Look for:

- Link access
- DNS requests
- Network connections
- File downloads
- Process execution
- Authentication activity
- Other alerts involving the same user/host/domain

```text
Phishing Email
     ↓
User Interaction
     ↓
Network / DNS Activity
     ↓
Download / Process Execution
     ↓
Possible Compromise
```

This helps determine whether the phishing attempt resulted in further activity.

---

## 5. True Positive vs False Positive

**True Positive (TP):**
- Alert correctly identifies malicious/unauthorised activity.
- Document evidence and respond/escalate as required.

**False Positive (FP):**
- Alert triggers but investigation confirms legitimate activity.

> Suspicious appearance alone is not enough for a final verdict.

---

## 6. Phishing Report

A report should allow another analyst to understand the investigation.

Include:

```text
Alert:
Severity:
Sender:
Recipient:
Subject:

Suspicious Indicators:
URL/Domain:
IP:
Attachment:

Related SIEM Activity:
User Activity:
Network/Host Activity:

Verdict:
Reason:
Action:
```

Use the **5 Ws** where relevant:

- **Who** — sender/recipient/user
- **What** — suspicious activity
- **When** — timestamps
- **Where** — URL/domain/IP/host
- **Why** — reason for the verdict

---

## 7. Phishing vs Spam

| Spam | Phishing |
|---|---|
| Unsolicited/unwanted bulk communication | Attempts to deceive the user |
| Not necessarily malicious | Usually aims to cause harmful action |
| May simply be unwanted | Can target credentials, malware, data, or money |

### Common Phishing Goals

- Credential theft
- Malware delivery
- Data theft
- Financial fraud

---

## 8. SOC Investigation Mindset

Don't stop at the email.

```text
Email
 ↓
Indicators
 ↓
User
 ↓
Host
 ↓
Network
 ↓
Related Events
 ↓
Impact
```

Ask:
- Did the user click/access the link?
- Was anything downloaded?
- Did a process execute?
- Could credentials have been exposed?
- Are other users affected?
- Is activity still ongoing?

---

## Quick Checklist

```text
[ ] Check sender + recipient
[ ] Read subject/body
[ ] Inspect URL/domain
[ ] Analyse attachment
[ ] Enrich indicators with threat intelligence
[ ] Search SIEM for related activity
[ ] Check user/host/network activity
[ ] Decide TP / FP
[ ] Document evidence + reasoning
[ ] Close or escalate
```

### Remember

> **Email → Indicators → Context → Evidence → Verdict → Action**
