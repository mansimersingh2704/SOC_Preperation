# SOC L1 Alert Triage

## 1. Introduction

A **security alert** is a notification generated when a security
monitoring system detects activity that matches a detection rule or
appears suspicious.

**Alert triage** is the process of reviewing, prioritising, and
investigating these alerts to determine whether they represent a genuine
security threat.

A SOC may receive hundreds or thousands of alerts. An L1 analyst
therefore needs to quickly identify which alerts deserve attention,
investigate the relevant evidence, and decide whether the alert should
be closed or escalated.

### Main Objectives of Alert Triage

-   Understand what triggered the alert.
-   Review the alert's important properties.
-   Prioritise alerts based on severity and context.
-   Determine whether the activity is malicious, benign, or a false
    positive.
-   Investigate the relevant logs and surrounding activity.
-   Escalate serious or confirmed threats.
-   Document the investigation and final verdict.

------------------------------------------------------------------------

# 2. Events and Alerts

## What is an Event?

An **event** is a recorded activity that occurs on a system, network,
application, or security device.

Examples:

-   Successful login
-   Failed login
-   Process execution
-   File creation
-   DNS request
-   Network connection
-   PowerShell execution
-   Firewall connection

An event by itself does not necessarily mean that something malicious
happened.

### Example

``` text
User: Alice
Action: Login
Source IP: 10.10.10.25
Result: Success
```

This is an event.

------------------------------------------------------------------------

## What is an Alert?

An **alert** is generated when a security system detects activity that
matches a rule, threshold, or suspicious behaviour pattern.

Example:

``` text
Multiple failed logins
        ↓
Same source IP
        ↓
Multiple accounts targeted
        ↓
Detection rule triggered
        ↓
Security Alert
```

An alert is therefore a signal that something requires investigation. It
is **not automatically proof of an attack**.

### Event vs Alert

  Event                       Alert
  --------------------------- -------------------------------------------
  Records an activity         Indicates potentially suspicious activity
  May be completely normal    Requires investigation
  Raw security telemetry      Result of detection logic
  Example: successful login   Example: repeated failed logins

### Basic Flow

``` text
Event → Detection Rule / Correlation → Alert → Analyst Investigation
```

------------------------------------------------------------------------

# 3. From Events to Alerts

Security systems continuously generate events from endpoints, servers,
applications, and network devices.

A detection system processes these events and looks for suspicious
patterns.

``` text
Endpoint / Network / Server
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

The purpose of detection is to reduce the amount of raw data that
analysts need to manually review.

For example, instead of an analyst manually checking thousands of
authentication events, a detection rule can identify a pattern such as:

``` text
Many failed logins
+
Multiple accounts
+
Short time period
+
Same source
=
Potential Password Spraying Alert
```

------------------------------------------------------------------------

# 4. Alert Management Platforms

Different security platforms support different parts of the SOC
workflow.

  -----------------------------------------------------------------------
  Solution                Purpose                 Examples
  ----------------------- ----------------------- -----------------------
  **SIEM**                Collect, search,        Splunk, Elastic
                          correlate and analyse   
                          security logs           

  **EDR**                 Monitor and investigate Microsoft Defender,
                          endpoint activity       CrowdStrike

  **NDR**                 Monitor network         Network detection
                          activity and detect     platforms
                          network threats         

  **SOAR**                Automate investigation  Splunk SOAR, Cortex
                          and response workflows  XSOAR

  **ITSM / Ticketing**    Track incidents, tasks  Jira, ServiceNow
                          and analyst work        
  -----------------------------------------------------------------------

## SIEM

**SIEM = Security Information and Event Management**

A SIEM collects logs from many sources and provides a central location
for searching and analysing security events.

Common log sources include:

-   Windows and Linux systems
-   Firewalls
-   VPNs
-   Active Directory
-   Cloud services
-   Applications
-   EDR solutions
-   Network devices

A SIEM can:

-   Collect logs
-   Normalise data
-   Correlate events
-   Search historical activity
-   Apply detection rules
-   Generate alerts
-   Support investigations

------------------------------------------------------------------------

# 5. L1 Role in Alert Triage

A **SOC L1 Analyst** is commonly the first person to review security
alerts.

The L1 analyst's job is not simply to close alerts. The goal is to
determine whether an alert represents a real security issue and provide
enough evidence for the next level when escalation is required.

### Typical L1 Responsibilities

-   Review incoming alerts.
-   Validate the basic alert information.
-   Check severity and priority.
-   Determine whether the alert has already been investigated.
-   Review the alert description and fields.
-   Investigate relevant logs and activity.
-   Identify suspicious indicators.
-   Classify the alert.
-   Add investigation notes.
-   Escalate when necessary.
-   Close benign or false-positive alerts.

### L1 vs L2

``` text
L1
 ↓
Initial Triage
 ↓
Basic Investigation
 ↓
Evidence Collection
 ↓
True Positive / Suspicious?
 ↓
L2 / Incident Response
```

L2 analysts generally perform deeper investigation and response when an
L1 analyst cannot resolve the case or the incident is serious.

------------------------------------------------------------------------

# 6. Alert Properties

An alert normally contains several properties that help an analyst
understand and investigate it.

  Property                Description
  ----------------------- ----------------------------------------------
  **Alert Time**          Time at which the alert was generated
  **Alert Name**          Name or summary of the detection
  **Alert Severity**      Indicates the urgency or potential impact
  **Alert Status**        Current state of the alert
  **Alert Verdict**       Analyst's conclusion about the activity
  **Alert Assignee**      Analyst responsible for the alert
  **Alert Description**   Explanation of what the detection identified
  **Alert Fields**        Additional data needed for investigation

------------------------------------------------------------------------

## 6.1 Alert Time

The **alert time** indicates when the security system generated the
alert.

Example:

``` text
Alert Time: 31 Aug 2026 11:53
```

### Important

The alert time may not be the same as the time when the actual event
occurred.

``` text
11:50 → Suspicious activity occurs
11:52 → Event reaches SIEM
11:53 → Detection rule processes event
11:53 → Alert generated
```

During an investigation, always consider the timeline and event
timestamps.

------------------------------------------------------------------------

## 6.2 Alert Name

The alert name gives a quick summary of what triggered the detection.

Examples:

-   Unusual Login Location
-   Suspicious PowerShell Activity
-   Potential Data Exfiltration
-   Possible Ransomware Activity
-   Multiple Failed Logins
-   Downloaded File from GitHub

The alert name is a starting point for investigation, not a final
conclusion.

------------------------------------------------------------------------

## 6.3 Alert Severity

Severity indicates how urgent or potentially impactful the alert may be.

  Severity       General Meaning
  -------------- ----------------------------------------------
  **Low**        Informational or low-risk activity
  **Medium**     Suspicious activity requiring investigation
  **High**       Serious potential threat
  **Critical**   Highly urgent or potentially severe activity

### Severity is not the same as confirmation

``` text
Critical Alert ≠ Confirmed Attack
```

Severity tells the analyst how much attention an alert may deserve. The
analyst still needs evidence.

### Context Matters

A Medium alert affecting a **Domain Administrator** may be more
important than a High alert affecting an isolated test machine.

Therefore:

``` text
Priority = Severity + Context + Impact + Evidence
```

------------------------------------------------------------------------

## 6.4 Alert Status

Status tells the analyst what stage the alert is currently in.

Common statuses:

-   **New** -- Not yet investigated.
-   **In Progress** -- An analyst is actively investigating it.
-   **Closed** -- Investigation has been completed.
-   **Resolved** -- The issue has been dealt with according to the
    team's workflow.
-   **Escalated** -- Sent to another analyst/team for further
    investigation or response.

------------------------------------------------------------------------

## 6.5 Alert Verdict

The verdict represents the analyst's conclusion.

### True Positive

The alert correctly identified malicious or unauthorised activity.

``` text
Malicious Activity
        +
Detection Triggered
        ↓
True Positive
```

Example:

``` text
Multiple failed logins
        ↓
Successful login
        ↓
Suspicious source
        ↓
Account activity confirmed as unauthorised
        ↓
True Positive
```

### False Positive

The alert was triggered, but investigation shows that the activity was
legitimate.

``` text
Legitimate Activity
        +
Detection Triggered
        ↓
False Positive
```

Example:

``` text
Administrator runs an approved PowerShell script
        ↓
Detection rule triggers
        ↓
Analyst confirms approved activity
        ↓
False Positive
```

### Benign / Expected Activity

Some teams distinguish benign activity from a technical false positive.
The important point is that the activity is not a security threat and
can be closed after proper documentation.

------------------------------------------------------------------------

## 6.6 Alert Assignee

The **assignee** is the analyst responsible for investigating the alert.

Assigning ownership helps prevent alerts from being ignored or
investigated by multiple analysts at the same time.

``` text
Alert
 ↓
Assigned to Analyst
 ↓
Analyst Investigates
 ↓
Notes + Verdict
 ↓
Close / Escalate
```

------------------------------------------------------------------------

## 6.7 Alert Description

The description provides additional context about the detection.

It may explain:

-   What happened
-   Why the activity was detected
-   Which host was involved
-   Which user was involved
-   What detection rule triggered
-   What behaviour may indicate an attack

Example:

``` text
A suspicious PowerShell process was detected on WORKSTATION-01.
The process executed an encoded command and attempted an
outbound connection to an external address.
```

------------------------------------------------------------------------

## 6.8 Alert Fields

Alert fields contain technical information that can be used during
investigation.

Common fields include:

-   Hostname
-   Username
-   Source IP
-   Destination IP
-   Source port
-   Destination port
-   Process name
-   Process ID
-   Parent process
-   Command line
-   File name
-   File hash
-   URL
-   Domain
-   Registry key

Example:

``` text
Host: WORKSTATION-01
User: alice
Process: powershell.exe
Parent Process: winword.exe
Command Line: powershell -enc <encoded-command>
Destination IP: 203.0.113.10
```

------------------------------------------------------------------------

# 7. Alert Prioritisation

A SOC can receive many alerts at the same time. **Alert prioritisation**
determines which alerts should be investigated first.

A basic prioritisation approach is:

1.  Filter out alerts that have already been investigated.
2.  Start with higher-severity alerts.
3.  Consider the age of the alert.
4.  Consider the affected user and asset.
5.  Consider whether the activity is ongoing.
6.  Investigate the most relevant alerts first.

------------------------------------------------------------------------

## Factors Affecting Alert Priority

### 1. Severity

Higher severity usually means higher urgency.

``` text
Critical
   ↓
High
   ↓
Medium
   ↓
Low
```

### 2. Asset Criticality

A suspicious event on a:

-   Domain Controller
-   Production server
-   Security appliance
-   Critical database

may deserve more attention than the same event on a test machine.

### 3. User Privileges

An alert involving a:

-   Domain Administrator
-   System Administrator
-   Security Administrator

may be more significant than an alert involving a normal user.

### 4. Alert Age

Older alerts should be considered in context.

The analyst should determine whether:

-   The alert has already been investigated.
-   The activity is still ongoing.
-   The alert is part of a larger incident.

### 5. Attack Type

Potential ransomware, credential theft, data exfiltration, and lateral
movement generally require greater urgency than low-risk informational
alerts.

------------------------------------------------------------------------

# 8. Alert Triage Process

A practical alert triage process can be represented as:

``` text
START
  ↓
Prioritise Alerts
  ↓
Select Alert
  ↓
Assign Alert
  ↓
Move to "In Progress"
  ↓
Read Alert Name & Description
  ↓
Review Alert Fields
  ↓
Check Related Activity
  ↓
Follow Available Runbook / Playbook
  ↓
Investigate Activity in SIEM / EDR
  ↓
Make Final Verdict
  ↓
Need Escalation?
 ┌───────────────┴───────────────┐
 ↓                               ↓
YES                              NO
 ↓                               ↓
Escalate                      Add Analyst Notes
                                 ↓
                            Close Alert
```

------------------------------------------------------------------------

# 9. Initial Actions

The initial stage is about taking ownership of the alert and
understanding what happened.

### Step 1 --- Prioritise and Select

Choose the alert that should be investigated based on severity, age,
context, and potential impact.

### Step 2 --- Assign the Alert

Assign the alert to yourself or the appropriate analyst.

### Step 3 --- Change Status

Move the alert to **In Progress** so other analysts know that it is
being investigated.

### Step 4 --- Read the Alert

Review:

-   Name
-   Description
-   Severity
-   Timestamp
-   User
-   Host
-   Alert fields

### Step 5 --- Identify Key Indicators

Extract useful investigation data such as:

-   IP addresses
-   Domains
-   URLs
-   File hashes
-   File names
-   Usernames
-   Hostnames
-   Processes
-   Commands

------------------------------------------------------------------------

# 10. Investigation

Investigation is the stage where the analyst determines whether the
detected activity is legitimate or malicious.

## 10.1 Understand the Alert

First understand what the detection is actually reporting.

Ask:

-   What activity triggered the rule?
-   Which system generated the event?
-   Which user was involved?
-   What was the source?
-   What was the destination?
-   Why could this activity be suspicious?

------------------------------------------------------------------------

## 10.2 Check the Affected Host

Determine:

-   Hostname
-   Host type
-   Operating system
-   Criticality
-   Whether the host is expected to perform the activity
-   Whether other suspicious events occurred on the host

------------------------------------------------------------------------

## 10.3 Check the User

Determine:

-   Which account performed the activity?
-   Is the account legitimate?
-   Is it a privileged account?
-   Was the user expected to perform the action?
-   Was the user logged in at that time?

------------------------------------------------------------------------

## 10.4 Check Network Activity

For network-related alerts, investigate:

-   Source IP
-   Destination IP
-   Source port
-   Destination port
-   Protocol
-   Domain
-   URL
-   Connection time
-   Frequency of connections

------------------------------------------------------------------------

## 10.5 Check Process Activity

For endpoint alerts, examine:

``` text
Process Name
Parent Process
Child Process
Command Line
Process ID
User
Execution Time
```

### Example

``` text
WINWORD.EXE
      ↓
PowerShell.exe
      ↓
cmd.exe
      ↓
Suspicious executable
```

The process chain provides context that may not be visible in the alert
title.

------------------------------------------------------------------------

# 11. Timeline Analysis

A timeline shows how events are related over time.

Example:

``` text
10:01 → User receives an email
10:03 → Document opened
10:03 → WINWORD.EXE starts
10:04 → PowerShell starts
10:04 → External network connection
10:05 → Suspicious file downloaded
10:06 → Security alert generated
```

Timeline analysis helps answer:

-   What happened before the alert?
-   What happened immediately after it?
-   Was the alert part of a larger sequence?
-   Is the activity still happening?

### Key Idea

Do not investigate an alert in isolation. **Look at the surrounding
events.**

------------------------------------------------------------------------

# 12. Related Events

After identifying the alert, search for related activity.

For a suspicious PowerShell alert, investigate:

-   Previous process
-   Parent process
-   Child processes
-   Network connections
-   DNS requests
-   File creation
-   Authentication activity
-   Registry changes
-   Scheduled tasks
-   Other alerts on the same host

This helps build a complete picture of the activity.

------------------------------------------------------------------------

# 13. Indicators of Compromise (IOCs)

An **Indicator of Compromise (IOC)** is a piece of information that may
indicate malicious activity.

### Common IOCs

-   IP addresses
-   Domains
-   URLs
-   File hashes
-   File names
-   Email addresses
-   Malicious processes
-   Registry keys

Example:

``` text
Source IP: 203.0.113.10
Domain: suspicious-example.com
File: payload.exe
SHA256: <hash>
```

IOCs can be searched across SIEM, EDR, threat-intelligence, and other
security tools to find related activity.

------------------------------------------------------------------------

# 14. Behavioural Analysis

Not every attack has a known IOC.

An analyst should also look at **behaviour**.

### IOC-Based Detection

``` text
Known malicious IP
        ↓
Connection detected
        ↓
Alert
```

### Behaviour-Based Detection

``` text
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

Even when an IP or hash is unknown, the behaviour itself may be
suspicious.

------------------------------------------------------------------------

# 15. Investigation Using a Runbook

A **runbook/playbook** provides standard investigation steps for a
particular type of alert.

Example:

``` text
Suspicious Login Alert
        ↓
Check User
        ↓
Check Source IP
        ↓
Check Login History
        ↓
Check MFA
        ↓
Check VPN
        ↓
Check Geographic Location
        ↓
Determine Verdict
```

Using a runbook helps L1 analysts follow a consistent process and avoid
missing important investigation steps.

------------------------------------------------------------------------

# 16. Final Actions

After investigation, the analyst must determine the appropriate action.

Possible outcomes:

``` text
False Positive → Document → Close
Benign Activity → Document → Close
True Positive → Document → Escalate
Needs More Investigation → Escalate
```

The final verdict should be based on the evidence collected during the
investigation.

------------------------------------------------------------------------

# 17. Documentation

Good documentation explains **what happened, what was investigated, what
evidence was found, and why the analyst reached the final verdict**.

### Poor Note

``` text
Looks malicious.
```

### Better Note

``` text
Multiple failed authentication attempts were followed by a
successful login from an unusual external IP. The source IP was
not associated with the corporate VPN. Related authentication
events were reviewed and the activity was considered suspicious.
The alert was escalated for further investigation.
```

### A Good Investigation Note Should Answer

-   What happened?
-   When did it happen?
-   Who was involved?
-   Which system was affected?
-   What evidence was found?
-   What was the verdict?
-   What action was taken?

------------------------------------------------------------------------

# 18. Escalation

An L1 analyst should escalate an alert when it requires deeper
investigation or response.

### Examples

-   Confirmed malware
-   Ransomware activity
-   Credential compromise
-   Privileged account compromise
-   Data exfiltration
-   Lateral movement
-   Multiple affected hosts
-   Active attacker behaviour
-   Critical systems affected

### What to Include in Escalation

``` text
Alert Name:
Alert Time:
Severity:
Affected Host:
Affected User:
Source IP:
Destination IP:
Relevant Process:
Command Line:
IOCs:
Timeline:
Investigation Findings:
Initial Verdict:
Reason for Escalation:
```

The goal is to give the L2/IR team enough context to continue the
investigation without repeating the entire initial triage.

------------------------------------------------------------------------

# 19. Alert Fatigue

**Alert fatigue** occurs when analysts receive excessive numbers of
alerts, especially repetitive or low-value alerts.

### Causes

-   Too many low-quality detections
-   Poorly tuned rules
-   Repetitive alerts
-   Large amounts of benign activity

### Effects

-   Analyst overload
-   Slower investigations
-   Reduced attention
-   Missed important alerts
-   Increased risk of human error

### Reducing Alert Fatigue

-   Tune detection rules.
-   Improve alert prioritisation.
-   Reduce unnecessary alerts.
-   Use automation where appropriate.
-   Add useful context to alerts.

------------------------------------------------------------------------

# 20. Practical Example --- Unusual Login

Suppose the SOC receives:

``` text
Alert: Unusual Login Location
User: john
Source IP: 203.0.113.50
Severity: Medium
```

Do not immediately classify it as malicious.

### Investigation

#### 1. Check the User

Was John travelling or working remotely?

#### 2. Check Login History

``` text
09:00 → Login from Country A
09:15 → Login from Country B
```

Could this represent impossible travel?

#### 3. Check Authentication

Look for:

-   Failed logins
-   Successful login
-   MFA result
-   Device information
-   VPN usage
-   User-agent

#### 4. Check Source IP

Determine whether the IP belongs to:

-   Corporate VPN
-   Cloud provider
-   Known organisation
-   Unknown/suspicious infrastructure

#### 5. Determine Verdict

If the activity is legitimate VPN usage:

``` text
Benign / False Positive
        ↓
Document
        ↓
Close
```

If evidence suggests credential compromise:

``` text
True Positive
        ↓
Document Evidence
        ↓
Escalate
```

------------------------------------------------------------------------

# 21. Practical Example --- Suspicious PowerShell

Alert:

``` text
Alert: Suspicious PowerShell Activity
Severity: High
Host: PC-01
User: Alice
```

### Questions to Ask

-   What command was executed?
-   Was the command encoded?
-   What process launched PowerShell?
-   Did PowerShell download a file?
-   Did it connect to an external IP?
-   Was a file created?
-   Was the activity expected for the user?

### Example Process Chain

``` text
OUTLOOK.EXE
     ↓
WINWORD.EXE
     ↓
POWERSHELL.EXE
     ↓
BITSADMIN.EXE
     ↓
External Server
```

This process chain provides much more context than simply seeing
`powershell.exe`.

------------------------------------------------------------------------

# 22. SOC Dashboard Workflow

When working inside a SOC dashboard, a common workflow is:

``` text
1. Filter and prioritise alerts
2. Select an alert
3. Assign it to yourself
4. Change status to In Progress
5. Read alert name and description
6. Review alert fields
7. Check the available runbook
8. Investigate activity in SIEM/EDR
9. Make the final verdict
10. Add investigation comments
11. Escalate if necessary
12. Move the alert to Closed
```

The exact workflow can differ between organisations and security
platforms.

------------------------------------------------------------------------

# 23. Analyst Mindset

The most important mindset during alert triage is:

> **An alert is a signal, not a conclusion.**

Use this approach:

``` text
Alert
  ↓
Understand the Detection
  ↓
Form a Hypothesis
  ↓
Collect Evidence
  ↓
Analyse Context
  ↓
Determine Verdict
  ↓
Take Action
```

Instead of saying:

``` text
"This looks malicious."
```

Ask:

``` text
"What evidence supports or disproves malicious activity?"
```

------------------------------------------------------------------------

# 24. Common Mistakes to Avoid

### 1. Treating every alert as a confirmed attack

An alert only indicates that something triggered a detection.

### 2. Looking only at the alert title

The description, fields, logs, and surrounding activity may contain the
important evidence.

### 3. Ignoring the timeline

Events before and after an alert can completely change its meaning.

### 4. Ignoring asset/user context

A suspicious action by a Domain Administrator on a Domain Controller is
very different from the same action on a test machine.

### 5. Closing without documentation

Every final decision should have a clear reason.

### 6. Escalating without useful evidence

An escalation should contain enough information for the next analyst to
continue the investigation.

------------------------------------------------------------------------

# 25. L1 Alert Triage Checklist

``` text
[ ] Review alert name
[ ] Check alert time
[ ] Check severity
[ ] Check status
[ ] Check assignee
[ ] Read description
[ ] Review alert fields
[ ] Identify affected host
[ ] Identify affected user
[ ] Check source/destination IP
[ ] Check processes and command line
[ ] Review related events
[ ] Analyse timeline
[ ] Search for IOCs
[ ] Check user and asset context
[ ] Follow the relevant runbook
[ ] Determine True Positive / False Positive / Benign
[ ] Add investigation notes
[ ] Escalate if required
[ ] Close or update the alert
```

------------------------------------------------------------------------

# 26. Key Takeaways

-   **Events** are recorded activities; **alerts** are generated when
    detection logic identifies potentially suspicious activity.
-   A SIEM helps collect, correlate, search, and detect security events.
-   L1 analysts are responsible for initial alert triage and
    investigation.
-   Alert properties provide the information needed to understand the
    alert.
-   Severity helps prioritise alerts, but **severity alone does not
    confirm a threat**.
-   Always investigate the **user, host, network activity, processes,
    and timeline**.
-   Use both **IOC-based and behavioural analysis**.
-   A detection can result in a **True Positive, False Positive, or
    benign activity**.
-   Serious or confirmed threats should be escalated with useful
    evidence.
-   Good documentation is an essential SOC skill.
-   The goal of alert triage is to **identify real threats quickly and
    accurately while reducing unnecessary analyst workload**.

------------------------------------------------------------------------

# Quick Revision

``` text
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
Collect Evidence
  ↓
Verdict
  ↓
┌───────────────┴───────────────┐
↓                               ↓
Benign / False Positive      True Positive
↓                               ↓
Document + Close             Document + Escalate
```

## Remember

``` text
Alert ≠ Attack

Alert → Evidence → Analysis → Verdict → Action
```
