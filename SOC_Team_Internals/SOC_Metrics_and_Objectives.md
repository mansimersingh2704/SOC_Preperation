# SOC Metrics & Objectives

## 1. Why SOC Metrics Matter

SOC metrics measure how effectively the team detects, acknowledges, investigates, and responds to threats.

The main goal is to protect the **Confidentiality, Integrity, and Availability (CIA)** of organisational digital assets.

---

## 2. Core SOC Metrics

| Metric | Formula | Measures |
|---|---|---|
| **Alerts Count (AC)** | `Total Count of Alerts Received` | Overall SOC analyst workload |
| **False Positive Rate (FPR)** | `False Positives / Total Alerts` | Amount of noise in alerts |
| **Alert Escalation Rate (AER)** | `Escalated Alerts / Total Alerts` | L1 analyst experience/decision-making |
| **Threat Detection Rate (TDR)** | `Detected Threats / Total Threats` | Reliability of SOC detection |

---

# 3. Alerts Count

**Alerts Count** = total number of alerts received.

It indicates the workload/volume handled by the SOC.

> A useful general range shown in the room is **5–30 alerts/day per L1 analyst**, depending on company size and environment.

Too many alerts can cause:
- Analyst overload
- Alert fatigue
- Important threats being hidden by noise

Too few alerts can also be a concern if the SIEM/EDR or detection system is not working correctly.

---

# 4. False Positive Rate (FPR)

**FPR = False Positives / Total Alerts**

A **False Positive** occurs when an alert triggers but investigation shows the activity is legitimate.

### Example

```text
80 alerts
75 False Positives
       ↓
FPR = 75 / 80 = 93.75%
```

High FPR means excessive alert noise and can cause analysts to lose vigilance.

### Target

- **0% FPR** = ideal but generally unrealistic
- **80% or higher** = serious problem according to the room

### Reducing High FPR

- Exclude trusted/expected activities from detection rules.
- Tune detection rules.
- Automate common alert handling using SOAR/custom scripts.

---

# 5. Alert Escalation Rate (AER)

**AER = Escalated Alerts / Total Alerts**

It indicates how often L1 analysts escalate alerts to higher-level analysts.

A very high rate may indicate L1 analysts are overly dependent on L2.

The room states it is generally aimed to be **below 50%, or even below 20%**.

### Key Idea

L1 should filter noise and escalate **actionable/threatening alerts**, while avoiding unnecessary escalation.

---

# 6. Threat Detection Rate (TDR)

**TDR = Detected Threats / Total Threats**

It measures how reliably the SOC detects actual threats.

### Example

```text
6 total attacks
4 detected
       ↓
TDR = 4 / 6 = 66.7%
```

A low TDR is a serious problem because missed threats can lead to major consequences such as:

- Ransomware
- Data exfiltration
- Other successful attacks

> The room states that the threat detection rate should **always be 100%**.

---

# 7. SOC Availability

A common SLA shown in the room is:

**SOC Team Availability → 24/7**

Some organisations may operate Monday–Friday (8/5), while others use 24/7 coverage.

---

# 8. Triage / Response Metrics

These metrics measure how quickly the SOC reacts to an attack.

| Metric | Common SLA shown | Meaning |
|---|---:|---|
| **MTTD** | 5 min | Average time between attack and detection |
| **MTTA** | 10 min | Average time for L1 to start triage |
| **MTTR** | 60 min | Average time for SOC to stop the breach from spreading |

### Timeline

```text
Malicious Event
      ↓
   MTTD
      ↓
SOC receives alert
      ↓
   MTTA
      ↓
L1 starts triage
      ↓
Internal processes / investigation
      ↓
   MTTR
      ↓
SOC mitigates breach
```

### Definitions

**MTTD — Mean Time to Detect**
- Time from attack/event to detection by SOC tools.

**MTTA — Mean Time to Acknowledge**
- Time from alert creation to L1 analyst starting triage.

**MTTR — Mean Time to Respond**
- Time taken by the SOC to actually stop/mitigate the breach.

> Definitions/formulas can vary between teams; use the organisation's stated metric definition when calculating.

---

# 9. Example MTTD / MTTA / MTTR Calculation

Scenario:

```text
Attack occurs
 ↓ 12 min
SOC receives alert
 ↓ 10 min
L1 starts triage
 ↓ 6 min
Alert escalated to L2
 ↓ 35 min
Malware cleaned
```

Therefore:

```text
MTTD = 12 min
MTTA = 10 min
MTTR = 6 + 35 = 41 min
```

**Answer: `12,10,41`**

---

# 10. Improving SOC Metrics

| Problem | Improvement |
|---|---|
| **FPR > 80%** | Tune rules, exclude trusted activity, automate common alerts |
| **MTTD > 30 min** | Make detection rules faster/higher-rate; ensure SIEM logs arrive in real time |
| **MTTA > 30 min** | Real-time analyst notifications; distribute queue/workload |
| **MTTR > 4 hours** | Escalate threats to L2 quickly; maintain documented response procedures |

### General Principle

```text
Better Detection
      ↓
Lower MTTD
      ↓
Faster Acknowledgement
      ↓
Lower MTTA
      ↓
Faster Investigation/Response
      ↓
Lower MTTR
```

---

# 11. Metric Interpretation

### High FPR
**Problem:** Too much alert noise.

**Fix:** Detection-rule tuning + trusted exclusions + automation.

### High MTTD
**Problem:** SOC detects threats too slowly.

**Fix:** Improve detection rules and ensure logs are collected in real time.

### High MTTA
**Problem:** L1 starts triage too late.

**Fix:** Real-time notifications and better workload distribution.

### High MTTR
**Problem:** SOC takes too long to contain/respond.

**Fix:** Escalate quickly to L2 and maintain documented response procedures.

---

# 12. Practice Scenario Logic

When analysing a SOC metric problem:

```text
Identify the problematic metric
          ↓
Understand why it is high/low
          ↓
Choose an improvement
          ↓
Assign the improvement to the appropriate team/person
```

Example:

```text
MTTR too high
    ↓
Need faster response
    ↓
Escalate threats to L2 quickly
    ↓
Improve/document response procedures
```

---

# Quick Revision

```text
AC  = Total Alerts

FPR = False Positives / Total Alerts
     → Measures alert noise

AER = Escalated Alerts / Total Alerts
     → Measures L1 escalation behaviour

TDR = Detected Threats / Total Threats
     → Measures detection reliability

MTTD = Attack → Detection
MTTA = Alert → L1 starts triage
MTTR = Response time → Breach mitigated
```

### Key Numbers From the Room

```text
Alerts/day/L1        → 5–30
FPR ≥ 80%            → Serious problem
AER target           → <50%, ideally <20%
TDR                  → 100%
SOC availability     → 24/7
MTTD SLA             → 5 min
MTTA SLA             → 10 min
MTTR SLA             → 60 min
```

### Remember

> **Detect fast → Acknowledge fast → Respond fast → Reduce attack impact.**
