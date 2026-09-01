# SOC L1 Alert Triage

## 1. Alert Triage Basics

**Security Alert:** A notification generated when a security system detects activity matching a rule, threshold, or suspicious behaviour pattern.

**Alert Triage:** Reviewing, prioritising, and investigating alerts to determine whether they represent a genuine security threat.

> **Alert ≠ Attack.** An alert is a signal that requires investigation.

### Main Objectives
- Understand what triggered the alert.
- Check severity, context, and important fields.
- Investigate relevant logs and surrounding activity.
- Identify suspicious indicators.
- Decide **True Positive / False Positive / Benign**.
- Document findings.
- Escalate serious or unresolved threats.

---

## 2. Event vs Alert

| Event | Alert |
|---|---|
| Recorded activity | Detection generated from activity |
| May be completely normal | Requires investigation |
| Raw security telemetry | Result of detection logic |
| Example: successful login | Example: repeated failed logins |

```text
Event → Detection/Correlation → Alert → Analyst Investigation
```

### Example Events
- Login success/failure
- Process execution
- File creation
- DNS request
- Network connection
- PowerShell execution
- Firewall connection

---

## 3. How Events Become Alerts

```text
Endpoint / Server / Network / Application
              ↓
            Events
              ↓
        Log Collection
              ↓
             SIEM
              ↓
   Detection / Correlation Rules
              ↓
            Alert
              ↓
        SOC Analyst
```

Example detection:

```text
Many failed logins
+ Multiple accounts
+ Short time period
+ Same source
        ↓
Potential Password Spraying Alert
```

---

## 4. SOC Security Platforms

| Platform | Main Purpose | Examples |
|---|---|---|
| **SIEM** | Collect, search, correlate and analyse logs | Splunk, Elastic |
| **EDR** | Monitor/investigate endpoint activity | Microsoft Defender, CrowdStrike |
| **NDR** | Monitor network activity/threats | Network detection platforms |
| **SOAR** | Automate investigation/response workflows | Splunk SOAR, Cortex XSOAR |
| **ITSM/Ticketing** | Track incidents and analyst work | Jira, ServiceNow |

### SIEM
**SIEM = Security Information and Event Management**

Common log sources:
- Windows/Linux
- Firewalls
- VPN
- Active Directory
- Cloud services
- Applications
- EDR
- Network devices

SIEM capabilities:
- Collect and normalise logs
- Correlate events
- Search historical activity
- Apply detection rules
- Generate alerts
- Support investigations

---

# 5. L1 Analyst Role

L1 is commonly the **first analyst to review an alert**.

### Responsibilities
- Review and validate alerts.
- Check severity, priority, and status.
- Check whether it was already investigated.
- Review description and fields.
- Investigate relevant logs/activity.
- Identify suspicious indicators.
- Classify the alert.
- Add investigation notes.
- Close benign/false-positive alerts.
- Escalate when necessary.

```text
L1
 ↓
Initial Triage
 ↓
Basic Investigation
 ↓
Evidence Collection
 ↓
Verdict
 ↓
Close / Escalate to L2
```

**L2** generally handles deeper investigation and response when L1 cannot resolve the case or the incident is serious.

---

# 6. Important Alert Properties

| Property | Meaning |
|---|---|
| **Alert Time** | When the alert was generated |
| **Alert Name** | Detection summary |
| **Severity** | Urgency/potential impact |
| **Status** | Current workflow state |
| **Verdict** | Analyst's conclusion |
| **Assignee** | Analyst responsible |
| **Description** | Context about the detection |
| **Fields** | Technical investigation data |

### Alert Time

Alert time may differ from the actual event time:

```text
11:50 → Activity occurs
11:52 → Event reaches SIEM
11:53 → Detection processes event
11:53 → Alert generated
```

Always consider the **event timeline**, not just alert timestamp.

### Alert Name
Useful as a starting point, but **not a final conclusion**.

### Severity

| Severity | General Meaning |
|---|---|
| **Low** | Informational / low risk |
| **Medium** | Suspicious activity requiring investigation |
| **High** | Serious potential threat |
| **Critical** | Highly urgent / potentially severe |

> **Critical Alert ≠ Confirmed Attack**

Priority depends on more than severity:

```text
Priority = Severity + Context + Impact + Evidence
```

Example: a Medium alert involving a **Domain Administrator** may matter more than a High alert on an isolated test machine.

### Status
Common states:
- **New** — not investigated
- **In Progress** — actively investigated
- **Escalated** — sent to another analyst/team
- **Resolved** — dealt with according to workflow
- **Closed** — investigation completed

### Verdict

**True Positive (TP):** Alert correctly identified malicious/unauthorised activity.

**False Positive (FP):** Alert triggered, but investigation confirms legitimate activity.

**Benign/Expected:** Activity is not a security threat and can be closed after documentation.

### Assignee
The analyst responsible for the alert. Clear ownership prevents alerts being ignored or duplicated.

---

# 7. Alert Fields to Investigate

Common fields:

- Hostname
- Username
- Source/Destination IP
- Source/Destination port
- Protocol
- Process name
- Process ID
- Parent/child process
- Command line
- File name/hash
- URL/domain
- Registry key

Example:

```text
Host: WORKSTATION-01
User: alice
Process: powershell.exe
Parent: winword.exe
Command Line: powershell -enc <encoded-command>
Destination IP: 203.0.113.10
```

---

# 8. Alert Prioritisation

When many alerts exist, prioritise using:

1. **Severity**
2. **Asset criticality**
3. **User privileges**
4. **Alert age**
5. **Whether activity is ongoing**
6. **Attack type**
7. **Existing investigation/context**

High-value assets include:
- Domain Controllers
- Production servers
- Security appliances
- Critical databases

High-value accounts include:
- Domain Administrator
- System Administrator
- Security Administrator

Potential ransomware, credential theft, data exfiltration, and lateral movement generally require higher urgency.

---

# 9. L1 Alert Triage Workflow

```text
Prioritise
   ↓
Select Alert
   ↓
Assign
   ↓
Move to In Progress
   ↓
Read Name + Description
   ↓
Review Fields
   ↓
Check User / Host / Network / Process
   ↓
Review Related Events + Timeline
   ↓
Follow Runbook / Playbook
   ↓
Investigate in SIEM / EDR
   ↓
Determine Verdict
   ↓
Need Escalation?
 ┌───────────────┴───────────────┐
 YES                             NO
 ↓                                ↓
Escalate                    Document + Close
```

### Initial Checklist

- Check alert name, time, severity, status, assignee.
- Read description.
- Identify affected user and host.
- Extract IPs, domains, URLs, hashes, processes, commands.
- Check related activity.
- Follow the relevant runbook.

---

# 10. Investigation

## A. Understand the Detection

Ask:
- What triggered the rule?
- Which system generated the event?
- Which user was involved?
- What was the source/destination?
- Why might it be suspicious?

## B. Check the Host

- Hostname/type
- Operating system
- Criticality
- Whether the activity is expected
- Other suspicious activity on the host

## C. Check the User

- Which account performed the action?
- Is it legitimate?
- Is it privileged?
- Was the user expected to perform the action?
- Was the user logged in at that time?

## D. Check Network Activity

- Source/destination IP
- Ports
- Protocol
- Domain/URL
- Connection time
- Connection frequency

## E. Check Process Activity

```text
Process Name
Parent Process
Child Process
Command Line
Process ID
User
Execution Time
```

Example:

```text
WINWORD.EXE
    ↓
PowerShell.exe
    ↓
cmd.exe
    ↓
Suspicious executable
```

**Process chains provide context that the alert title may not show.**

---

# 11. Timeline & Related Events

Never investigate an alert in isolation.

Example:

```text
10:01 → Email received
10:03 → Document opened
10:03 → WINWORD.EXE starts
10:04 → PowerShell starts
10:04 → External connection
10:05 → Suspicious file downloaded
10:06 → Alert generated
```

Timeline helps determine:
- What happened before the alert?
- What happened after it?
- Is it part of a larger sequence?
- Is activity still ongoing?

For suspicious PowerShell, also check:
- Parent/child processes
- Network connections
- DNS requests
- File creation
- Authentication
- Registry changes
- Scheduled tasks
- Other alerts on the host

---

# 12. Indicators of Compromise (IOCs)

An **IOC** is information that may indicate malicious activity.

Common IOCs:
- IP addresses
- Domains
- URLs
- File hashes
- File names
- Email addresses
- Malicious processes
- Registry keys

```text
Source IP: 203.0.113.10
Domain: suspicious-example.com
File: payload.exe
SHA256: <hash>
```

Search IOCs across **SIEM, EDR, and threat-intelligence tools** to find related activity.

---

# 13. IOC vs Behaviour Analysis

### IOC-Based

```text
Known malicious IP
       ↓
Connection detected
       ↓
Alert
```

### Behaviour-Based

```text
Office Application
       ↓
PowerShell
       ↓
Encoded Command
       ↓
External Connection
       ↓
Suspicious Behaviour
```

> Not every attack has a known IOC. **Behaviour itself can be suspicious.**

---

# 14. Runbooks / Playbooks

A runbook provides standard investigation steps for a specific alert type.

Example: suspicious login

```text
Suspicious Login
      ↓
Check User
      ↓
Source IP
      ↓
Login History
      ↓
MFA
      ↓
VPN
      ↓
Geographic Location
      ↓
Verdict
```

Runbooks help analysts stay consistent and avoid missing important steps.

---

# 15. Final Verdict & Action

```text
False Positive → Document → Close
Benign Activity → Document → Close
True Positive → Document → Escalate
Needs More Investigation → Escalate
```

The verdict must be based on **evidence**, not the alert title or severity alone.

---

# 16. Documentation

A good investigation note explains:
- What happened
- When it happened
- Who was involved
- Which system was affected
- Evidence found
- Verdict
- Action taken

### Poor

```text
Looks malicious.
```

### Better

```text
Multiple failed authentication attempts were followed by a successful
login from an unusual external IP. The IP was not associated with the
corporate VPN. Related authentication events were reviewed and the
activity was considered suspicious. The alert was escalated for further
investigation.
```

---

# 17. Escalation

Escalate when deeper investigation or response is required.

### Common Examples
- Confirmed malware
- Ransomware activity
- Credential compromise
- Privileged account compromise
- Data exfiltration
- Lateral movement
- Multiple affected hosts
- Active attacker behaviour
- Critical systems affected

### Escalation Information

```text
Alert Name:
Alert Time:
Severity:
Affected Host:
Affected User:
Source/Destination:
Relevant Process:
Command Line:
IOCs:
Timeline:
Investigation Findings:
Initial Verdict:
Reason for Escalation:
```

> The goal is to give L2/IR enough context to continue **without repeating the entire initial triage**.

---

# 18. Alert Fatigue

**Alert fatigue** = excessive/repetitive/low-value alerts causing analyst overload.

### Causes
- Too many low-quality detections
- Poorly tuned rules
- Repetitive alerts
- Large amounts of benign activity

### Effects
- Slower investigations
- Reduced attention
- Missed important alerts
- More human error

### Reduce It
- Tune detection rules.
- Improve prioritisation.
- Remove unnecessary alerts.
- Use appropriate automation.
- Add useful context to alerts.

---

# 19. Analyst Mindset

> **An alert is a signal, not a conclusion.**

Use:

```text
Alert
 ↓
Understand Detection
 ↓
Form Hypothesis
 ↓
Collect Evidence
 ↓
Analyse Context
 ↓
Verdict
 ↓
Action
```

Instead of:

```text
"This looks malicious."
```

Ask:

```text
"What evidence supports or disproves malicious activity?"
```

---

# 20. Common Mistakes

1. Treating every alert as a confirmed attack.
2. Looking only at the alert title.
3. Ignoring the timeline.
4. Ignoring user/asset context.
5. Closing without documentation.
6. Escalating without useful evidence.

---

# 21. L1 Quick Checklist

```text
[ ] Check alert name/time/severity/status
[ ] Read description and fields
[ ] Identify user + host
[ ] Check source/destination/network data
[ ] Check process + command line
[ ] Review related events
[ ] Analyse timeline
[ ] Search IOCs
[ ] Check user + asset context
[ ] Follow runbook
[ ] Decide TP / FP / Benign
[ ] Document evidence + reasoning
[ ] Escalate if required
[ ] Close/update alert
```

---

# Quick Revision

```text
EVENT
  ↓
Detection / Correlation
  ↓
ALERT
  ↓
Prioritise
  ↓
Assign
  ↓
Initial Review
  ↓
Investigate
  ↓
Evidence + Context
  ↓
Verdict
  ↓
┌───────────────────────┴───────────────────────┐
↓                                               ↓
Benign / FP                                  True Positive
↓                                               ↓
Document + Close                           Document + Escalate
```

### Remember

**Alert → Evidence → Analysis → Verdict → Action**

**Severity ≠ Confirmation**

**Context + Timeline + Behaviour + IOCs = Better Triage**
