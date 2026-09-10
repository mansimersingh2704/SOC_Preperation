# TryHackMe --- Introduction to SOAR

## 1. SOAR

**SOAR = Security Orchestration, Automation and Response**

SOAR helps a SOC integrate security tools and automate repetitive
investigation/response workflows.

### Why SOAR?

-   Reduce alert fatigue
-   Reduce manual work
-   Speed up investigations and response
-   Make workflows consistent
-   Connect multiple security tools

------------------------------------------------------------------------

## 2. Traditional SOC

A traditional SOC continuously monitors the environment, investigates
suspicious activity, responds to incidents, uses threat intelligence,
and communicates with IT/management.

### Main challenges

-   **Alert fatigue:** Too many alerts, including false positives.
-   **Disconnected tools:** Analysts switch between SIEM, EDR, firewall,
    IAM, etc.
-   **Manual processes:** Repetitive investigation steps take time.
-   **Talent shortage:** Limited skilled analysts have to handle
    increasingly complex threats.

------------------------------------------------------------------------

## 3. Three Core Parts of SOAR

### Orchestration

Connects and coordinates different security tools in one workflow.

**Example:**

``` text
SIEM Alert → SOAR → Threat Intelligence → Investigation → Response
```

### Automation

Automatically performs predefined/repetitive tasks.

### Response

Carries out actions to contain or remediate an incident.

------------------------------------------------------------------------

## 4. Playbooks

A **playbook** is a predefined workflow for handling a particular
security event.

Think:

> **Playbook = step-by-step incident response procedure**

Typical flow:

``` text
Alert
 ↓
Collect information
 ↓
Enrich / investigate
 ↓
Decision
 ↓
Response
 ↓
Update / close ticket
```

------------------------------------------------------------------------

## 5. Phishing Playbook

A phishing playbook automates repetitive email investigation.

``` text
Suspicious Email
 ↓
Create Investigation Ticket
 ↓
Check Attachment / URL
 ↓
Attachment → Calculate Hash → Check Hash Reputation
URL → Check URL Reputation
 ↓
Determine Malicious or Benign
 ↓
Update Ticket with IOCs
```

Important IOC examples: - IP address - Domain - URL - File hash

------------------------------------------------------------------------

## 6. CVE Patching Playbook

**CVE = Common Vulnerabilities and Exposures**

A CVE workflow helps coordinate vulnerability remediation.

``` text
CVE Identified
 ↓
Create Ticket
 ↓
Assess Vulnerability
 ↓
Identify Affected Systems
 ↓
Patch / Remediate
 ↓
Verify
 ↓
Close Ticket
```

------------------------------------------------------------------------

## 7. Threat Intelligence + SOAR

SOAR can use threat-intelligence feeds to enrich indicators found in
alerts.

``` text
IOC
 ↓
SOAR
 ↓
Threat Intelligence Lookup
 ↓
Reputation / Context
 ↓
Investigation
 ↓
Response
```

------------------------------------------------------------------------

## 8. SOAR vs Traditional SOC

  Traditional SOC                 With SOAR
  ------------------------------- ------------------------
  Many manual steps               Automated workflows
  Analysts switch between tools   Tools are orchestrated
  High alert fatigue              Reduced alert fatigue
  Slower response                 Faster response
  Inconsistent processes          Standardized playbooks

------------------------------------------------------------------------

# Viva Questions

### What is SOAR?

SOAR is a security solution/framework that integrates security tools and
automates incident-response workflows.

### What does SOAR stand for?

**Security Orchestration, Automation and Response.**

### What is orchestration?

Coordinating multiple security tools and processes through a unified
workflow.

### What is automation?

Automatically executing predefined repetitive security tasks.

### What is a playbook?

A predefined sequence of steps used to investigate and respond to a
particular security event.

### Why is SOAR needed?

To reduce manual work and alert fatigue, improve consistency, and speed
up incident response.

### What is alert fatigue?

Analyst overload caused by a large number of security alerts,
particularly false positives.

### Does SOAR replace SIEM?

No. SIEM mainly collects/correlates/analyzes security events, while SOAR
focuses on orchestration, automation, and response.

### Does SOAR replace SOC analysts?

No. It automates repetitive work so analysts can focus on complex
investigations and decisions.

### What is an IOC?

An Indicator of Compromise is evidence that may indicate malicious
activity, such as an IP, domain, URL, or file hash.

### What is CVE?

Common Vulnerabilities and Exposures --- a system for identifying
publicly known vulnerabilities.

------------------------------------------------------------------------

# Practical Task --- Key Point

The practical task involved configuring a Threat Intelligence workflow
by deciding which workflow steps should be **manual** and which should
be **automated**, then running the workflow.

**Flag:**

``` text
THM{AUTOM@T1N6_S3CUR1TY}
```

------------------------------------------------------------------------

# ⚡ Last-Minute Revision

``` text
SOAR
 ├── Orchestration → Connect tools
 ├── Automation    → Automate repetitive tasks
 └── Response      → Take action
```

**Remember:**

> **SIEM detects/analyses → SOAR orchestrates/automates/responds →
> Analyst handles complex decisions.**
