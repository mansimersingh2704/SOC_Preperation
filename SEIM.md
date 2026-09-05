# SEIM / SIEM Notes

## What is SIEM?

**SIEM (Security Information and Event Management)** is a security solution that collects logs from different sources, normalizes them, correlates events, detects suspicious activity, and generates alerts for security analysts.

## SIEM Workflow

```text
Log Sources
    ↓
Log Collection / Ingestion
    ↓
Log Normalization
    ↓
Event Correlation
    ↓
Detection Rules
    ↓
Alerts
    ↓
Alert Investigation
    ↓
Response
```

---

## 1. Log Sources

A network continuously generates logs whenever activity occurs.

### Host-Centric Log Sources

These logs capture events occurring on or related to individual hosts.

Examples:

- User accessing a file
- Authentication attempts
- Process execution
- Registry modifications
- PowerShell execution

Common sources:

- Windows endpoints
- Linux servers
- Application servers

### Network-Centric Log Sources

These logs capture network-related activity.

Examples:

- SSH connections
- FTP file access
- Web traffic
- VPN activity
- Network file-sharing activity

Common sources:

- Firewalls
- IDS/IPS
- Routers
- Network devices

---

## 2. Why SIEM?

Organizations can generate thousands or millions of events, making manual analysis difficult.

### Major Challenges

- **Numerous Log Sources** — logs are distributed across many devices.
- **No Centralization** — analysts may need to check multiple systems individually.
- **Limited Context** — a single event may look harmless, while correlated events can reveal an attack.
- **Limited Analysis** — humans cannot efficiently inspect huge volumes of logs manually.
- **Different Formats** — different systems produce logs in different structures.

### SIEM Solution

SIEM centralizes logs and provides tools for:

- Searching
- Correlation
- Detection
- Alerting
- Investigation
- Reporting

---

## 3. Main Features of SIEM

### Centralized Log Collection

Collects logs from multiple sources into a central platform.

```text
Windows ──┐
Linux ────┤
Firewall ─┼──→ SIEM
Web Server┤
Database ─┘
```

### Log Normalization

Different systems produce logs in different formats.

Normalization converts them into a consistent structure so that events from different sources can be analyzed together.

### Event Correlation

SIEM correlates multiple events to identify suspicious activity.

Example:

```text
Multiple Failed Logins
        ↓
Successful Login
        ↓
Unusual Process Execution
        ↓
Suspicious Outbound Connection
        ↓
Possible Compromise
```

### Real-Time Alerting

Detection rules can generate alerts when suspicious conditions are met.

Examples:

- Multiple failed login attempts
- Successful login after many failures
- Suspicious USB activity
- Large outbound data transfer

### Dashboards and Reporting

SIEM dashboards can display:

- Security events
- Login activity
- Network connections
- Threat detections
- Event statistics
- Investigation data

---

## 4. Log Ingestion

SIEM systems can ingest logs using different methods.

### Agent / Forwarder

A lightweight agent runs on an endpoint and forwards logs to the SIEM.

```text
Endpoint
   ↓
Agent
   ↓
SIEM
```

### Syslog

A standard protocol commonly used to collect logs from network devices and servers.

### Manual Upload

Some SIEM platforms allow analysts to manually upload logs for analysis.

### Port Forwarding

Endpoints can forward logs or events to a SIEM listening on a specific network port.

---

## 5. Common Linux Logs

Common Linux log locations include:

```text
/var/log/httpd
/var/log/messages
/var/log/auth.log
/var/log/secure
```

These logs may contain:

- Authentication events
- SSH activity
- System events
- Errors
- Web-server requests
- Cron jobs

### Apache / Web Server Logs

Web logs can help detect:

- Web attacks
- Suspicious requests
- Reconnaissance
- Exploitation attempts
- Abnormal traffic

---

## 6. Windows Logs

Windows records events that can be viewed using **Event Viewer**.

Important events include:

- Authentication events
- Process creation
- Account activity
- PowerShell activity
- Security events

Example:

```text
Event ID 4688
    ↓
Process Creation
```

---

## 7. Detection Rules

A detection rule defines a condition that should trigger an alert.

### Example: Multiple Failed Logins

```text
IF failed login attempts > 5
WITHIN 10 seconds
THEN trigger:
"Multiple Failed Login Attempts"
```

### Example: Possible Data Exfiltration

```text
IF outbound traffic > 25 MB
THEN trigger:
"Potential Data Exfiltration Attempt"
```

---

## 8. Detection Rule Examples

### Use Case 1 — Event Log Clearing

Attackers may clear logs to remove evidence.

```text
IF Log Source = WinEventLog
AND Event ID = 104
THEN Alert = "Event Log Cleared"
```

### Use Case 2 — Suspicious Process Execution

A suspicious process can be detected using specific event fields.

```text
IF Log Source = WinEventLog
AND Event Code = 4688
AND NewProcessName contains "whoami"
THEN Alert = "WHOAMI Command Execution Detected"
```

---

## 9. Alert Investigation

When a SIEM generates an alert, a SOC analyst investigates the surrounding events.

The analyst determines whether the alert is a:

### False Positive

The activity is legitimate and does not represent a real attack.

### True Positive

The activity is genuinely suspicious or malicious.

### Investigation Questions

Ask:

- **Who** performed the action?
- **What** happened?
- **When** did it happen?
- **Where** did it originate?
- **Which host** was involved?
- **What happened before and after?**
- Are there related events?

---

## 10. Alert Response

A typical response process can look like:

```text
Alert
  ↓
Triage
  ↓
Investigation
  ↓
False Positive / True Positive
       ↓
    True Positive
       ↓
Containment
       ↓
Remediation
       ↓
Recovery
```

Possible actions include:

- Contacting the asset owner
- Isolating an infected host
- Blocking a suspicious IP
- Disabling a compromised account
- Removing malicious processes
- Collecting additional evidence

---

## 11. SIEM Analyst Mindset

Do not investigate an alert in isolation.

A single event may not be meaningful. A sequence of correlated events can reveal an attack.

### Example

```text
Failed Login Attempts
        +
Successful Login
        +
Suspicious Process
        +
Unusual Network Connection
        ↓
Potential Compromise
```

> **Key Principle:** One event may be harmless; a sequence of correlated events can reveal an attack.

---

## 12. SIEM in a SOC

SIEM is one of the core technologies used by SOC teams.

```text
Logs
 ↓
SIEM
 ↓
Detection Rules
 ↓
Alert
 ↓
SOC Analyst
 ↓
Triage
 ↓
Investigation
 ↓
Response
```

### Key Takeaway

Before learning tools such as **Splunk, Microsoft Sentinel, Wazuh, or Elastic SIEM**, understand these fundamentals:

1. Log sources
2. Log ingestion
3. Log normalization
4. Event correlation
5. Detection rules
6. Alert triage
7. Alert investigation
8. Incident response
