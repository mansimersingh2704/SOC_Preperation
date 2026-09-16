# MITRE ATT&CK — TryHackMe Notes

> **Room:** TryHackMe — MITRE  
> **Focus:** MITRE ATT&CK, ATT&CK Matrix, CTI, CAR, D3FEND, Caldera, AADAPT and ATLAS  
> **Purpose:** Quick-reference notes for SOC / Blue Team / Threat Intelligence work.

---

## 1. MITRE

**MITRE** is a not-for-profit organisation that conducts research and development across multiple domains, including cybersecurity.

In cybersecurity, MITRE provides knowledge bases and frameworks that help defenders, threat-intelligence analysts, detection engineers, incident responders and red teams describe and understand adversary behaviour.

### Important MITRE resources

| Resource | Purpose |
|---|---|
| **ATT&CK** | Knowledge base of adversary tactics, techniques and procedures |
| **ATT&CK Navigator** | Visualise, annotate and compare ATT&CK techniques |
| **CAR** | Detection analytics mapped to ATT&CK |
| **D3FEND** | Knowledge graph of defensive countermeasures |
| **Caldera** | Automated adversary-emulation platform |
| **AADAPT** | Threat framework for digital asset payment technologies |
| **ATLAS** | Threat framework for AI/ML systems |

---

# 2. MITRE ATT&CK

**ATT&CK = Adversarial Tactics, Techniques, and Common Knowledge**

MITRE ATT&CK is a globally accessible knowledge base of adversary tactics and techniques based on real-world observations.

It is used by both **red teams and blue teams**.

### Main idea

ATT&CK helps answer:

> **What is the adversary trying to achieve?**  
> **How are they achieving it?**  
> **How can defenders detect or mitigate it?**

---

## 3. TTP — Tactics, Techniques and Procedures

### Tactic
The **goal/objective** of an adversary.

Think:

> **WHY?**

Examples:
- Initial Access
- Execution
- Persistence
- Privilege Escalation
- Defense Evasion
- Credential Access
- Discovery
- Lateral Movement
- Collection
- Command and Control
- Exfiltration
- Impact

### Technique
The **method** used to achieve a tactical objective.

Think:

> **HOW?**

Example:

**Tactic:** Reconnaissance  
**Technique:** Active Scanning

### Procedure
The **specific implementation** of a technique by an adversary/group.

Think:

> **HOW EXACTLY?**

### Easy memory trick

```text
TACTIC     = WHY?
TECHNIQUE  = HOW?
PROCEDURE  = HOW EXACTLY?
```

---

# 4. ATT&CK Matrix

The ATT&CK Matrix is a visual representation of tactics and techniques.

- **Columns / top-level categories → Tactics**
- **Techniques → methods under a tactic**
- **Sub-techniques → more specific versions of techniques**

### Example from the room

```text
Tactic
  Reconnaissance
      ↓
Technique
  Active Scanning
      ↓
Sub-techniques
  T1595.001  Scanning IP Blocks
  T1595.002  Vulnerability Scanning
  T1595.003  Wordlist Scanning
```

### Important IDs

**Active Scanning → T1595**

Sub-techniques:

| ID | Sub-technique |
|---|---|
| **T1595.001** | Scanning IP Blocks |
| **T1595.002** | Vulnerability Scanning |
| **T1595.003** | Wordlist Scanning |

---

# 5. How to use ATT&CK in SOC work

ATT&CK provides a common language for describing attacker behaviour.

### SOC Analyst
Use ATT&CK to:
- Investigate alerts
- Understand attacker behaviour
- Link activity to tactics/techniques
- Add context to incidents
- Prioritise investigation

### Detection Engineer
Use ATT&CK to:
- Map SIEM/EDR rules to techniques
- Identify detection gaps
- Build coverage matrices
- Improve detection engineering

### Incident Responder
Use ATT&CK to:
- Build incident timelines
- Map observed activity to adversary behaviour
- Understand attack progression

### Threat Intelligence Analyst
Use ATT&CK to:
- Map threat reports to TTPs
- Profile threat actors/groups
- Compare adversary behaviour
- Build threat-informed defensive strategies

### Red / Purple Team
Use ATT&CK to:
- Emulate adversaries
- Plan attack scenarios
- Test detection coverage
- Validate defensive controls

---

# 6. ATT&CK Navigator

**ATT&CK Navigator** is used to visually explore and annotate ATT&CK matrices.

Useful for:

- Threat actor profiling
- Incident mapping
- Detection coverage
- Purple-team exercises
- Comparing groups
- Creating ATT&CK layers

### Typical workflow

```text
Threat Report
     ↓
Identify TTPs
     ↓
Map TTPs to ATT&CK
     ↓
Create Navigator Layer
     ↓
Identify coverage / gaps
     ↓
Improve detections
```

---

# 7. ATT&CK for Threat Intelligence

A CTI report can contain a lot of unstructured information.

ATT&CK turns this into structured intelligence:

```text
CTI Report
   ↓
Extract attacker behaviours
   ↓
Identify Tactics
   ↓
Identify Techniques
   ↓
Identify Sub-techniques
   ↓
Map to ATT&CK IDs
   ↓
Create threat profile
```

### Example

```text
Attacker scans the victim's infrastructure
        ↓
Reconnaissance
        ↓
Active Scanning
        ↓
T1595
```

---

# 8. Important ATT&CK IDs from the TryHackMe room

| Technique | ID | Tactic / Context |
|---|---|---|
| Active Scanning | **T1595** | Reconnaissance |
| Scanning IP Blocks | **T1595.001** | Reconnaissance |
| Vulnerability Scanning | **T1595.002** | Reconnaissance |
| Wordlist Scanning | **T1595.003** | Reconnaissance |
| Create Account | **T1136** | Persistence / account creation context |
| Phishing | **T1566** | Initial Access |

> **Tip:** ATT&CK IDs are extremely useful when doing SOC investigations because they let you search the exact technique instead of relying only on technique names.

---

# 9. ATT&CK in Operation — Threat Group Mapping

The room demonstrates mapping a threat group to ATT&CK.

### Example: Mustang Panda

**Mustang Panda**
- ATT&CK Group ID: **G0129**
- Can be profiled using its known ATT&CK techniques.
- ATT&CK Navigator can be used to visualise its TTP coverage.

### Why group mapping matters

Instead of only saying:

> "This looks malicious."

A defender can say:

```text
Observed behaviour
       ↓
ATT&CK Technique
       ↓
Known threat group TTP
       ↓
Threat intelligence context
       ↓
Detection / response
```

This makes investigation more structured.

---

# 10. Threat Intelligence Scenario — APT33

The TryHackMe scenario focuses on an aviation-sector organisation.

### Key answers from the room

| Question | Answer |
|---|---|
| APT group targeting aviation sector and active since at least 2013 | **APT33** |
| Sub-technique concerning Office 365 cloud accounts | **Cloud Accounts** |
| Tool linked to the APT group | **Ruler** |
| Mitigation for removing inactive/unused accounts | **User Account Management** |
| Detection Strategy ID for abused/compromised cloud accounts | **DET0546** |

### Investigation pattern

```text
Identify target sector
       ↓
Find relevant ATT&CK group
       ↓
Open group page
       ↓
Inspect techniques/sub-techniques
       ↓
Check tools
       ↓
Check mitigations
       ↓
Check detection strategies
```

---

# 11. Cyber Analytics Repository — CAR

**CAR = Cyber Analytics Repository**

CAR is a MITRE knowledge base of analytics based on the ATT&CK adversary model.

It provides detection analytics and examples that defenders can use to translate ATT&CK TTPs into actual detections.

### CAR can contain

- Hypothesis
- Data domain
- ATT&CK technique/tactic references
- Pseudocode
- Tool-specific implementations
- Unit tests
- Data model
- Sensors

### Common detection formats

CAR may provide examples for tools such as:

- Splunk
- EQL
- Other security analytics environments

### Important concept

```text
ATT&CK
"What behaviour are attackers using?"
             ↓
CAR
"How can we detect that behaviour?"
```

---

# 12. CAR Example — Scheduled Task

The room uses:

**CAR-2020-09-001 — Scheduled Task / FileAccess**

The example demonstrates how a defender can identify Windows Task Scheduler activity.

### Detection concept

Look for suspicious file creation/access related to Windows Task Scheduler locations.

The CAR example includes:

- ATT&CK mapping
- Pseudocode
- Splunk search
- Detection logic
- Coverage information

### Pseudocode vs actual query

**Pseudocode**

Human-readable description of the logic an analytic should perform.

**Splunk search**

An implementation of the detection logic in Splunk.

### SOC lesson

Do not stop at:

> "Technique = Scheduled Task."

Also ask:

```text
What telemetry do I need?
       ↓
What event/action should I search?
       ↓
What query detects it?
       ↓
How do I validate the detection?
```

---

# 13. D3FEND

**D3FEND = Detection, Denial, and Disruption Framework Empowering Network Defense**

D3FEND is a structured knowledge graph of **cybersecurity countermeasures**.

It complements ATT&CK from the defensive side.

### Simple comparison

```text
ATT&CK
Offensive / adversary behaviour
        ↓
"What can the attacker do?"

D3FEND
Defensive countermeasures
        ↓
"How can defenders counter behaviour?"
```

### D3FEND model

The room shows seven high-level defensive categories:

```text
MODEL
  ↓
HARDEN
  ↓
DETECT
  ↓
ISOLATE
  ↓
DECEIVE
  ↓
EVICT
  ↓
RESTORE
```

---

# 14. D3FEND Example — Credential Rotation

The room gives **Credential Rotation** as an example.

Credential rotation changes authentication credentials at predetermined intervals or after relevant events.

The example links:

```text
Credential Rotation
       ↓
Password
Credential
Certificate
```

The point is to understand how a defensive technique relates to digital artifacts and offensive techniques.

---

# 15. D3FEND User Behavior Analysis Example

Question:

> Which sub-technique of User Behavior Analysis would you use to analyze the geolocation data of user logon attempts?

### Answer

**User Geolocation Logon Pattern Analysis**

### ID

**D3-UGLPA**

### Related digital artifact

**Network Traffic**

### Important distinction

```text
ATT&CK → adversary behaviour
D3FEND → defensive technique / countermeasure
Artifact → thing being observed or analyzed
```

---

# 16. D3FEND Mental Model

When using D3FEND, think:

```text
Threat / Attack
      ↓
ATT&CK technique
      ↓
Defensive response
      ↓
D3FEND technique
      ↓
Required artifacts / telemetry
```

This is useful for detection engineering and security architecture.

---

# 17. Other MITRE Projects

MITRE provides several specialised projects beyond ATT&CK.

---

## 17.1 MITRE Caldera

**Caldera** is an automated adversary-emulation platform.

It can help security teams:

- Simulate adversary behaviour
- Test detections
- Validate defensive controls
- Conduct red-team / blue-team exercises
- Automate attack simulations in controlled environments

### Basic idea

```text
ATT&CK TTP
    ↓
Adversary Emulation
    ↓
Caldera
    ↓
Generate controlled activity
    ↓
SOC / EDR / SIEM
    ↓
Validate detection
```

---

## 17.2 MITRE AADAPT

**AADAPT = Adversarial Actions in Digital Asset Payment Technologies**

AADAPT is a threat framework focused on digital asset payment technologies.

It is modelled after ATT&CK and covers adversarial tactics and techniques relevant to digital asset systems.

### Example from the room

**Scrape Blockchain Data**

Technique ID:

**ADT3025**

AADAPT is especially relevant to:

- Cryptocurrency security
- Blockchain systems
- Digital asset platforms
- Smart contracts
- Digital asset payment systems

---

## 17.3 MITRE ATLAS

**ATLAS = Adversarial Threat Landscape for Artificial-Intelligence Systems**

ATLAS focuses on adversarial behaviour targeting AI/ML systems.

It is modelled after ATT&CK.

### Example from the room

**LLM Prompt Obfuscation**

Tactic:

**Defense Evasion**

### Why ATLAS matters

AI systems introduce additional attack surfaces such as:

- Prompt manipulation
- Model abuse
- Data poisoning
- Adversarial techniques
- AI-specific evasion
- Attacks against ML pipelines

---

# 18. ATT&CK vs CAR vs D3FEND vs Caldera vs ATLAS vs AADAPT

| Framework / Tool | Main Purpose |
|---|---|
| **ATT&CK** | Describe adversary behaviour |
| **Navigator** | Visualise ATT&CK |
| **CAR** | Build / study detections |
| **D3FEND** | Describe defensive countermeasures |
| **Caldera** | Emulate adversary behaviour |
| **ATLAS** | AI/ML threat landscape |
| **AADAPT** | Digital asset / cryptocurrency threats |

### Easy memory

```text
ATT&CK  → ATTACKER
CAR     → DETECTION
D3FEND  → DEFENSE
CALDERA → EMULATION
ATLAS   → AI SECURITY
AADAPT  → DIGITAL ASSETS
```

---

# 19. SOC Analyst Workflow Using MITRE

A practical SOC workflow can look like:

```text
Alert
  ↓
Collect evidence
  ↓
Identify suspicious behaviour
  ↓
Map behaviour to ATT&CK
  ↓
Identify tactic + technique + sub-technique
  ↓
Check threat actor/group
  ↓
Check CAR / detection analytics
  ↓
Check D3FEND countermeasures
  ↓
Investigate / contain / remediate
  ↓
Document ATT&CK mapping
```

---

# 20. ATT&CK ID Hunting — Fast Method

When a TryHackMe question asks:

> "Which technique / tactic / ID?"

Use this workflow.

### Step 1 — Search the exact technique name

Example:

```text
Create Account MITRE ATT&CK
```

### Step 2 — Open the official ATT&CK page

Look for:

- Technique ID
- Tactic
- Description
- Sub-techniques
- Procedure examples
- Mitigations
- Detections

### Step 3 — If the question asks for a sub-technique

Check the **Sub-techniques** section.

### Step 4 — If the question asks for a tool

Open the relevant **Group / Software / Tool** page.

### Step 5 — If the question asks for detection

Check:

- Detection section
- CAR
- D3FEND where relevant

---

# 21. Important TryHackMe Answers From This Room

For revision only:

```text
Phishing tactic
→ Initial Access

Create Account ID
→ T1136

APT group targeting aviation sector
→ APT33

Office 365 concern
→ Cloud Accounts

Tool linked to APT33
→ Ruler

Mitigation
→ User Account Management

Detection Strategy
→ DET0546

D3FEND sub-technique for geolocation login analysis
→ User Geolocation Logon Pattern Analysis

D3FEND digital artifact
→ Network Traffic

AADAPT technique
→ Scrape Blockchain Data
→ ADT3025

ATLAS technique
→ LLM Prompt Obfuscation

LLM Prompt Obfuscation tactic
→ Defense Evasion
```

---

# 22. Quick Revision Sheet

## ATT&CK

```text
Tactic     = WHY
Technique  = HOW
Procedure  = HOW EXACTLY
```

## ATT&CK Matrix

```text
Tactics
  ↓
Techniques
  ↓
Sub-techniques
```

## SOC

```text
Alert
 ↓
Evidence
 ↓
Behaviour
 ↓
ATT&CK mapping
 ↓
Threat intelligence
 ↓
Detection
 ↓
Response
```

## Framework relationships

```text
            ATT&CK
         /     |      \
        /      |       \
      CAR    D3FEND   Caldera
       |        |        |
  Detection   Defense   Emulation

       ATLAS → AI/ML threats
       AADAPT → Digital asset threats
```

---

# 23. Important Official Links

### Core

- **MITRE ATT&CK:** https://attack.mitre.org/
- **ATT&CK Navigator:** https://mitre-attack.github.io/attack-navigator/
- **ATT&CK Groups:** https://attack.mitre.org/groups/
- **ATT&CK Software:** https://attack.mitre.org/software/
- **ATT&CK Techniques:** https://attack.mitre.org/techniques/

### Detection / Defense

- **MITRE CAR:** https://car.mitre.org/
- **CAR Analytics:** https://car.mitre.org/analytics/
- **MITRE D3FEND:** https://d3fend.mitre.org/
- **D3FEND Resources:** https://d3fend.mitre.org/resources/
- **D3FEND Tools:** https://d3fend.mitre.org/tools/

### Adversary Emulation

- **MITRE Caldera:** https://caldera.mitre.org/

### Specialised Frameworks

- **MITRE ATLAS:** https://atlas.mitre.org/
- **MITRE AADAPT:** https://aadapt.mitre.org/

### TryHackMe

- **TryHackMe MITRE Room:** https://tryhackme.com/room/mitre

---

# 24. What to Remember for a SOC Internship

You do **not** need to memorise the entire ATT&CK Matrix.

Focus on being able to:

1. Recognise common attacker behaviour.
2. Find the relevant ATT&CK technique.
3. Understand tactic → technique → sub-technique.
4. Read an ATT&CK technique page.
5. Map SIEM alerts to ATT&CK.
6. Use ATT&CK IDs in incident reports.
7. Understand detection opportunities.
8. Use CAR for detection ideas.
9. Understand D3FEND as the defensive counterpart.
10. Use Navigator to visualise coverage.

### High-value SOC skill

Instead of memorising:

> `T1059.001 = PowerShell`

learn to think:

```text
Suspicious PowerShell activity
        ↓
Execution
        ↓
Command and Scripting Interpreter
        ↓
PowerShell
        ↓
T1059.001
        ↓
Search SIEM telemetry
        ↓
Investigate process tree + command line + user + host
```

That workflow is more useful than memorising IDs in isolation.

---

## 25. One-Line Summary

> **MITRE ATT&CK tells you how adversaries behave, CAR helps turn that behaviour into detections, D3FEND describes defensive countermeasures, Caldera helps emulate attacks, ATLAS covers AI threats, and AADAPT covers digital-asset threats.**

---

## Sources

The notes above are based on the TryHackMe MITRE room screenshots provided for this study session and the official MITRE resources linked above.
