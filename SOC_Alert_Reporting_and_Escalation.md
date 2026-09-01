# SOC Alert Reporting & Escalation

## 1. Alert Funnel

Typical SOC alert flow:

```text
Security Alerts
     ↓
    L1
     ↓
  L2 Analyst
     ↓
 DFIR / Incident Response
     ↓
Incident handled & corporate data protected
```

### L1 Analyst
- Receives alerts from **SIEM, EDR, or ticketing platforms**.
- Most alerts are either:
  - Closed as **False Positive (FP)**
  - Handled at L1
- Complex or threatening alerts are **escalated to L2**.
- L1 should provide proper **reporting, escalation, and communication**.

### L2 Analyst
- Performs **deeper investigation**.
- Performs remediation when required.
- Starts/coordinates **Incident Response (IR)**.
- Works with DFIR and other teams when necessary.

---

## 2. Alert Reporting

An alert report documents the investigation before closing or escalating an alert.

### Why write reports?

| Purpose | Why it matters |
|---|---|
| **Provide context for escalation** | Helps L2 quickly understand what happened |
| **Save findings for records** | Preserves investigation context for future use |
| **Improve investigation skills** | Summarising alerts improves L1 investigation ability |

> A good report should allow another analyst to understand the incident **without starting the investigation from scratch**.

---

## 3. The Five Ws

A good SOC alert report should answer the **5 Ws**:

| Question | What to include |
|---|---|
| **Who** | User/account involved |
| **What** | Exact action/event that occurred |
| **When** | Start/end time and relevant timeline |
| **Where** | Device, IP, website, etc. |
| **Why** | Why the activity is suspicious + reasoning for verdict |

### Example

```text
Who: user@example.com
What: Downloaded a suspicious executable
When: 01-Sep-2026 14:30 UTC
Where: Workstation-23 / suspicious domain
Why: File showed malicious indicators and matched the alert
Verdict: True Positive
```

---

## 4. What to Include in an Alert Report

A useful report should contain:

- **Alert name**
- **Status**
- **Verdict** → True Positive / False Positive
- **Severity**
- **Assignee**
- **Investigation summary**
- **Timeline**
- **Evidence**
- **Indicators of Compromise (IOCs)**
- **Reasoning for verdict**
- **Actions taken**
- **Escalation reason**, if applicable

### Evidence & Indicators

Include relevant evidence such as:

- IP addresses
- Domains/URLs
- File hashes
- Usernames/accounts
- Hostnames
- Commands/processes
- Relevant logs
- Screenshots or supporting evidence

---

## 5. Alert Escalation

### When should an alert be escalated?

Escalate when:

1. The alert indicates a **major cybersecurity event** requiring deeper investigation.
2. **Remediation** is required, e.g.:
   - Malware removal
   - Host isolation
   - Password reset
3. **Communication with customers, partners, management, or law enforcement** is required.
4. L1 **does not fully understand the alert** and requires senior analyst assistance.

### Basic escalation flow

```text
L1 investigates
     ↓
Create/update alert report
     ↓
Determine verdict & severity
     ↓
Escalate to L2
     ↓
L2 investigates/remediates
     ↓
DFIR / Incident Response if required
```

---

## 6. SOC Dashboard Escalation

Typical process:

1. Move alert to **In Progress**.
2. Write the alert report and set verdict to **True Positive**.
3. Assign/reassign the alert to the **L2/on-shift analyst**.
4. L2 receives a notification and continues investigation.

---

## 7. Requesting L2 Support

L1 should **ask for help rather than blindly closing an alert** when something is unclear.

```text
L1 → Explain issue + provide alert details
L2 → Review / investigate
L2 → Provide guidance or take over
```

L2 may also conduct:
- Knowledge-sharing sessions
- Team calls
- Guidance for difficult alerts

---

## 8. SOC Communication

SOC work is not only technical. Analysts may communicate with:

- L1/L2 analysts
- SOC manager
- IT team
- HR
- Legal/PR
- DFIR / Incident Response
- Management
- Affected users

### Communication principles

- Use the **approved communication channel/procedure**.
- Provide enough context for the recipient to act.
- Escalate critical issues quickly.
- Don't hide mistakes or missed activity.
- Don't ignore alerts because logs are difficult to search.
- If unsure, **ask the L2 analyst**.

---

## 9. Critical Communication Cases

### L2 unavailable

If a **critical alert** needs escalation but L2 is unavailable, follow the organisation's escalation chain, typically:

```text
L2 → L3 → Manager
```

### Account compromise

- Validate the login/activity with the affected user.
- Use approved alternative communication methods if necessary.
- Never blindly trust a potentially compromised communication channel.

### Alert overload

If many alerts arrive in a short period:

- Prioritise according to **severity/workflow**.
- Identify critical alerts first.
- Inform the L2/on-shift analyst.

### Missed/misclassified attack

If you realise an attack was missed:

- Immediately contact L2.
- Explain what was missed.
- Re-investigate as necessary.
- Do not hide the mistake.

### SIEM/logging problems

If logs are incomplete, incorrect, or unavailable:

- Don't simply ignore/close the alert.
- Investigate what information is available.
- Report the logging/SIEM issue to L2 or the SOC engineer.

---

## 10. L2 Communication & Incident Response

For serious incidents:

```text
L1 Alert
   ↓
L2 Investigation
   ↓
Incident Response initiated
   ↓
Management / relevant teams contacted
```

Possible actions:
- **Initiate Incident Response**
- Contact **management**
- Contact **legal/PR**
- Coordinate with **DFIR**
- Perform containment/remediation

---

## Quick Revision

```text
L1 = Detect → Triage → Investigate → Report → Escalate

L2 = Deep Investigation → Remediation → Incident Response

Report = 5Ws + Evidence + IOCs + Verdict + Actions

5Ws:
Who → What → When → Where → Why

Escalate when:
✓ Major security incident
✓ Remediation required
✓ External/management communication required
✓ L1 needs deeper expertise

Never:
✗ Ignore unclear alerts
✗ Hide missed attacks
✗ Close alerts without understanding them
✗ Ignore logging/SIEM problems
```

## ⭐ SOC Interview Point

> A good L1 analyst doesn't just close alerts — they document what happened, preserve relevant evidence, explain their verdict, and escalate anything requiring deeper investigation or remediation.
