# Introduction to EDR — Notes

## 1. What is EDR?

**EDR = Endpoint Detection and Response**

EDR monitors endpoint devices such as laptops, desktops, and servers. It collects endpoint activity, detects suspicious or malicious behaviour, helps analysts investigate incidents, and provides response capabilities.

### Three main pillars of EDR

```text
Visibility → Detection → Response
```

### Visibility
Provides detailed visibility into endpoint activity.

Examples:
- Process execution
- Network connections
- File changes
- Registry modifications
- User activity

### Detection
Analyzes collected telemetry to identify suspicious activity.

Common techniques:
- Signature-based detection
- Behavioural detection
- Anomaly detection
- IOC matching
- MITRE ATT&CK mapping
- Machine Learning

### Response
Allows analysts to take action after a threat is detected.

Examples:
- Isolate host
- Terminate process
- Quarantine file
- Remote access
- Collect forensic artifacts

---

## 2. EDR vs Antivirus

Antivirus mainly focuses on detecting and blocking known malware, while EDR provides broader endpoint visibility, investigation, and response capabilities.

### Antivirus

```text
File → Scan → Detect/Block
```

### EDR

```text
Endpoint
   ↓
Telemetry
   ↓
Detection
   ↓
Investigation
   ↓
Response
```

EDR can also detect suspicious behaviour even when an exact malware signature is not available.

---

## 3. Why EDR is Needed

Modern attacks are not limited to traditional malware files.

Attackers may:
- Abuse legitimate tools
- Use PowerShell
- Steal credentials
- Abuse existing processes
- Use remote access
- Perform multi-stage attacks

EDR provides endpoint activity and context that helps analysts understand what happened.

---

## 4. EDR Architecture

```text
Endpoint 1 ─┐
Endpoint 2 ─┤
Endpoint 3 ─┼──→ EDR Console
Endpoint 4 ─┤
Endpoint 5 ─┘
```

### EDR Agent

The EDR agent is installed on endpoints. It collects telemetry and sends it to the EDR platform/console.

### EDR Console

The central interface where analysts can:
- View alerts
- Investigate endpoint activity
- Review threat intelligence
- Perform response actions
- Search telemetry

---

## 5. EDR Telemetry

**Telemetry = Data/activity collected from endpoints.**

### Process Executions & Terminations
Records processes that start and stop.

Useful for:
- Suspicious processes
- Parent-child process relationships
- Malware execution

### Network Connections
Records connections made by endpoints.

Useful for:
- C2 detection
- Suspicious outbound connections
- Possible data exfiltration

### Command Line Activity
Records commands executed by processes.

Example:

```text
powershell.exe -enc ...
```

Useful for detecting:
- PowerShell abuse
- Suspicious scripts
- Living-off-the-Land activity

### Files and Folders
Tracks file creation, modification, and deletion.

Useful for:
- Malware drops
- Suspicious files
- Ransomware activity

### Registry Modifications
Tracks Windows Registry changes.

Useful for:
- Persistence
- Configuration changes
- Malware activity

---

## 6. Detection Techniques

### Behavioural Detection

Instead of only matching known malware signatures, behavioural detection examines what a process is doing.

Example:

```text
winword.exe
     ↓
powershell.exe
     ↓
Suspicious command
```

An unusual parent-child relationship can be an important detection signal.

### Anomaly Detection

EDR can establish a baseline of normal endpoint behaviour and identify deviations.

```text
Normal behaviour
      ↓
Deviation
      ↓
Possible anomaly
```

### IOC Matching

**IOC = Indicator of Compromise**

Examples:
- IP address
- Domain
- File hash
- File path
- Registry key
- URL

EDR can compare collected telemetry against known IOCs.

### MITRE ATT&CK Mapping

Detected activity can be mapped to MITRE ATT&CK tactics and techniques.

```text
Activity
   ↓
MITRE ATT&CK Technique
   ↓
Tactic + Technique
```

This helps analysts understand attacker behaviour.

### Machine Learning

EDR can use ML models to identify suspicious behaviour, especially when:
- A known signature is unavailable
- Behaviour is unusual
- The attack is multi-stage

---

## 7. Response Capabilities

### Isolate Host

Network-isolates a compromised endpoint to contain malicious activity.

```text
Compromised Host
       ↓
Network Isolation
       ↓
Limits further communication/spread
```

### Terminate Process

Stops a malicious or suspicious process.

```text
malware.exe
     ↓
Terminate
```

### Quarantine

Moves a malicious file to an isolated location so it cannot execute normally.

### Remote Access

Allows an analyst to remotely access an endpoint and run commands/scripts for investigation and response.

### Artifact Collection

Collects endpoint artifacts for forensic investigation.

Examples:
- Memory dump
- Event logs
- Specific files
- Registry hives

---

## 8. EDR + Antivirus

EDR and antivirus can work together.

```text
             Endpoint
                ↓
        ┌───────────────┐
        │  Antivirus    │
        │      +        │
        │      EDR      │
        └───────────────┘
                ↓
        Better Protection
```

Antivirus provides prevention/detection, while EDR adds deeper visibility, investigation, and response.

---

## 9. EDR Investigation — SOC Workflow

When investigating an alert, an analyst should examine:

- Hostname
- Username
- Process name
- Process path
- Command line
- Parent process
- Network connections
- IP address
- Domain
- File hash
- Registry changes
- Threat Intelligence
- Detection severity

### Investigation flow

```text
Alert
 ↓
Identify Host
 ↓
Identify Process
 ↓
Check Process Details
 ↓
Check Network Activity
 ↓
Check IOCs
 ↓
Check Threat Intelligence
 ↓
Determine Malicious/Benign
 ↓
Respond or Escalate
```

---

## 10. TryHackMe Investigation Example

### Suspicious `syncsvc.exe`

Observed path:

```text
C:\Users\haris.khan\AppData\Local\Temp\syncsvc.exe
```

A process running from a user's Temp directory can be suspicious and should be investigated in context.

Associated indicators included:

```text
IP Address:
100.42.28.64

Domain:
files-wetransfer.com

Registry:
HKCU\...\Run\syncsvc
```

### Why is this suspicious?

Multiple indicators together increase confidence:

```text
Suspicious File
      +
Suspicious Network Connection
      +
Persistence Registry Key
      ↓
High Suspicion
```

---

## 11. Data Exfiltration

EDR network telemetry can help identify attempts to send data outside the organisation.

Example:

```text
syncsvc.exe
     ↓
Network Connection
     ↓
files-wetransfer.com
     ↓
Possible Exfiltration
```

During investigation, check:
- Destination domain
- Destination IP
- Process responsible
- Data being transferred
- Timing
- Related processes

---

## 12. Threat Intelligence

Threat Intelligence helps determine whether an indicator is known malicious, suspicious, or legitimate.

Common indicators:
- IP
- Domain
- Hash
- File
- URL

### TryHackMe example

`UpdateAgent.exe` on `DESKTOP-DEV01` was labelled:

```text
Known internal IT utility tool
```

This demonstrates why context matters. A suspicious-looking executable is not automatically malware.

---

## 13. SOC Analyst's Role with EDR

A SOC analyst commonly follows this process:

```text
Monitor Alerts
     ↓
Triage
     ↓
Investigate
     ↓
Correlate Telemetry
     ↓
Check Threat Intelligence
     ↓
Determine Severity
     ↓
Respond / Escalate
```

An alert should not be judged in isolation.

```text
Context + Telemetry + Threat Intelligence
                    ↓
          Better Investigation
```

---

## 14. Important Terms

| Term | Meaning |
|---|---|
| EDR | Endpoint Detection and Response |
| Endpoint | Laptop, desktop, server, etc. |
| Telemetry | Endpoint activity/data collected by EDR |
| IOC | Indicator of Compromise |
| C2 | Command and Control |
| Exfiltration | Sending/stolen data outside the organisation |
| Persistence | Maintaining access after compromise |
| Threat Intelligence | Information about known/suspicious threats |
| EDR Agent | Software installed on an endpoint |
| EDR Console | Central management/investigation interface |
| Quarantine | Isolating a malicious file |
| MITRE ATT&CK | Framework for attacker tactics and techniques |

---

## 15. Interview Revision

### EDR in one line

> **EDR continuously collects endpoint telemetry, detects suspicious behaviour, provides investigation context, and enables response actions.**

### SOC + EDR flow

```text
Telemetry
   ↓
Detection
   ↓
Alert
   ↓
Triage
   ↓
Investigation
   ↓
Response
   ↓
Recovery / Lessons Learned
```

### Key point

**EDR is not just an alerting tool. It provides visibility, detection, investigation context, and response capabilities across endpoints.**
