# TryHackMe — Summit

> **Path:** SOC Level 1  
> **Theme:** Defensive Security / Purple Team / Pyramid of Pain  
> **Room:** Summit  
> **Goal:** Chase a simulated adversary upward through the Pyramid of Pain by detecting and preventing increasingly difficult indicators.

---

## 1. Room Overview

Summit is a practical defensive-security challenge where you act as a SOC/security analyst for **PicoSecure**.

A simulated penetration tester, **Sphinx**, progressively changes the malware and its infrastructure. Your job is to analyse the supplied samples/logs and configure PicoSecure's security controls to detect or prevent them.

The room follows the **Pyramid of Pain** from easier-to-change indicators to harder-to-change attacker behaviour:

1. Hash Values
2. IP Addresses
3. Domain Names
4. Host / Network Artifacts
5. Tools
6. Tactics, Techniques & Procedures (TTPs)

The important lesson is that as you move upward, the detection becomes less dependent on a single easily changed IOC and more dependent on attacker behaviour.

---

# 2. Prerequisites

Recommended before starting:

- **The Pyramid of Pain**
- **MITRE ATT&CK**
- Basic malware-analysis concepts
- Basic firewall concepts
- Basic DNS concepts
- Basic Sigma/Sysmon concepts

---

# 3. PicoSecure Tools Used

The simulated PicoSecure environment provides several defensive controls.

| Tool | Main purpose |
|---|---|
| Malware Sandbox | Analyse suspicious samples |
| Manage Hashes | Block/identify files using hashes |
| Firewall Rule Manager | Block network traffic using IP-based rules |
| DNS Rule Manager | Block domains |
| Sigma Rule Builder | Create behavioural detections from Sysmon-style events |

---

# 4. Pyramid of Pain — Quick Revision

| Level | Indicator | Example in Summit | Typical response |
|---|---|---|---|
| 1 | Hash Values | MD5/SHA1/SHA256 of `sample1.exe` | Hash block |
| 2 | IP Addresses | `154.35.10.113` | Firewall rule |
| 3 | Domain Names | `emudyn.bresonicz.info` | DNS deny rule |
| 4 | Host/Network Artifacts | Registry modification disabling Defender | Sigma detection |
| 5 | Tools | Repeated 97-byte network connections | Sigma detection |
| 6 | TTPs | System discovery + writing output to `exfiltr8.log` | Behavioural Sigma detection |

### Core idea

**Lower pyramid = easier for attacker to change.**

**Higher pyramid = harder for attacker to change because it describes behaviour/methodology rather than one fixed IOC.**

---

# 5. Sample 1 — Hash Value

## Objective

Identify `sample1.exe` using its unique file hash.

### Sandbox information

| Field | Value |
|---|---|
| File | `sample1.exe` |
| Size | `202.50 KB` |
| Type | PE32+ executable (GUI) x86-64, for MS Windows |
| OS | Windows 10 x64 v1803 |
| Tag | `Trojan.Metasploit.A` |
| MIME | `application/x-dosexec` |
| MD5 | `cbda8ae000aa9cbe7c8b982bae006c2a` |
| SHA1 | `83d2791ca93e58688598485aa62597c0ebbf7610` |
| SHA256 | `9c550591a25c6228cb7d74d970d133d75c961ffed2ef7180144859cc09efca8c` |

### Detection workflow

1. Open **Malware Sandbox**.
2. Select `sample1.exe`.
3. Submit it for analysis.
4. Inspect the General Information section.
5. Copy one of the file hashes.
6. Open **Manage Hashes**.
7. Add the hash.
8. This detects/blocks the exact sample.

### Why hashes?

A cryptographic hash acts like a fingerprint of a file.

A tiny modification to the file can produce a completely different hash, so hash-based detection is easy for an attacker to evade by changing the binary.

### Pyramid level

**Hash Values — Trivial**

### Flag 1

```text
THM{f3cbf08151a11a6a331db9c6cf5f4fe4}
```

---

# 6. Sample 2 — IP Address

## Why move to IP detection?

Changing a file slightly can defeat hash detection. The attacker therefore changes the sample while communicating with infrastructure that can be identified by an IP address.

The important section is:

**Malware Sandbox → Network Activity**

### Important network activity

HTTP request:

| Process | Method | Destination | URL |
|---|---|---|---|
| `sample2.exe` | GET | `154.35.10.113:4444` | `http://154.35.10.113:4444/uvLk8YI32` |

Other connections include:

- `40.97.128.3:443` — Microsoft Corporation
- `40.97.128.4:443` — Microsoft Corporation

The suspicious download connection is:

```text
154.35.10.113:4444
```

### Firewall rule

Open:

**Firewall Rule Manager → Create Firewall Rule**

Use:

```text
Type:           Egress
Source IP:      Any
Destination IP: 154.35.10.113
Action:         Deny
```

### Egress vs Ingress

- **Egress** = traffic leaving the protected host/network
- **Ingress** = traffic entering the protected host/network

Since the malware is making an outbound connection, use **Egress**.

### Common mistake

Do **NOT** enter:

```text
Source IP: 154.35.10.113
Destination IP: Any
```

For this challenge the correct direction is:

```text
Source: Any
Destination: 154.35.10.113
```

### Why not block the Microsoft IPs?

The analysis also shows Microsoft-owned IPs. Those connections can be legitimate. The suspicious infrastructure is the hosting IP used to download the payload.

### Pyramid level

**IP Addresses — Low**

### Flag 2

```text
THM{2ff48a3421a938b388418be273f4806d}
```

---

# 7. Sample 3 — Domain Name

## Why domain detection?

An attacker can change an IP address relatively easily. Domain-based detection lets defenders block the infrastructure name instead of depending on one IP.

Analyse `sample3.exe` in the Malware Sandbox.

### Network activity

HTTP requests include:

```text
http://emudyn.bresonicz.info:1337/kzn293la
http://emudyn.bresonicz.info/backdoor.exe
```

### Connections

Suspicious infrastructure:

```text
Domain: emudyn.bresonicz.info
IP:     62.123.140.9
```

A Microsoft connection also appears:

```text
services.microsoft.com → 40.97.128.4
```

### DNS request

```text
emudyn.bresonicz.info → 62.123.140.9
```

### DNS rule

Open:

**DNS Rule Manager → Create DNS Rule**

Use:

```text
Rule Name:   Deny Phishing Domain
Category:    Phishing
Domain Name: emudyn.bresonicz.info
Action:      Deny
```

The exact rule name is not important; the domain/category/action are.

### Important DNS concept

A DNS deny rule can prevent the domain from resolving/being used according to the simulated environment's filtering logic.

### Pyramid level

**Domain Names — Annoying**

### Flag 3

```text
THM{4eca9e2f61a19ecd5df34c788e7dce16}
```

---

# 8. Sample 4 — Host / Network Artifact

## Why move higher?

Changing domains is more expensive than changing an IP, but an attacker can still do it.

The next useful indicator is a **host artifact** — something the malware does to the victim system.

Analyse `sample4.exe`.

This time pay attention to:

**Registry Activity**

### Suspicious registry modification

```text
Key:
HKEYLOCALMACHINE\SOFTWARE\Microsoft\Windows Defender\Real-Time Protection

Name:
DisableRealtimeMonitoring

Value:
1

Operation:
write
```

This indicates that the sample is attempting to disable Windows Defender real-time monitoring.

### Sigma rule

Open:

**Sigma Rule Builder → Create Sigma Rule**

Select:

```text
Focus:
Sysmon Event Logs

Sysmon Event:
Registry Modifications
```

Fill:

```text
Registry Key:
HKEYLOCALMACHINE\SOFTWARE\Microsoft\Windows Defender\Real-Time Protection

Registry Name:
DisableRealtimeMonitoring

Value:
1

ATT&CK ID:
Defense Evasion (TA0005)
```

Then:

**Validate Rule**

### Why use an alert instead of simply blocking?

A registry modification that disables real-time protection is suspicious, but in some environments there can be legitimate administrative reasons for registry changes.

Therefore a behavioural detection can **alert analysts** so they can investigate rather than automatically blocking every matching event.

### Pyramid level

**Host & Network Artifacts — Annoying / Challenging**

### Flag 4

```text
THM{c956f455fc076aea829799c0876ee399}
```

---

# 9. Sample 5 — Tools

At this stage the attacker is relying more heavily on their tooling and changes the actual malware behaviour.

Instead of a normal sandbox report, the challenge provides network logs.

## Important observation

A repeated connection pattern appears:

```text
Source:      10.10.15.12
Destination: 51.102.10.19
Port:        443
Size:        97 bytes
```

The same pattern repeats roughly every **30 minutes**.

This is the key behavioural pattern.

The attacker appears to be fragmenting communication into repeated 97-byte messages.

---

## Sigma detection

Open:

**Sigma Rule Builder → Create Sigma Rule**

Select:

```text
Focus:
Sysmon Event Logs

Sysmon Event:
Network Connections
```

Configure:

```text
Remote IP:          Any
Remote Port:        Any
Size (bytes):       97
Frequency (seconds): 1800
ATT&CK ID:          Command and Control (TA0011)
```

### Why 1800 seconds?

```text
1800 seconds = 30 minutes
```

The log shows the repeated 97-byte activity approximately every 30 minutes.

### Why Remote IP = Any?

The attacker can change infrastructure/IP addresses.

If the detection depended on one fixed IP, the attacker could simply move to another server.

The behavioural pattern is more valuable:

```text
Same message size + repeated frequency
```

### Why alert instead of block?

Network traffic matching this pattern could potentially be legitimate.

A SOC analyst can investigate repeated alerts and correlate:

- Source host
- Destination
- Frequency
- Payload/size
- Process
- Other telemetry

### Pyramid level

**Tools — Challenging**

### Flag 5

```text
THM{46b21c4410e47dc5729ceadef0fc722e}
```

---

# 10. Final Stage — TTPs

The final level is:

**Tactics, Techniques & Procedures**

At this level we are no longer relying primarily on:

- One hash
- One IP
- One domain
- One registry value
- One tool signature

Instead, we detect **how the attacker operates**.

---

## Commands observed

The final attachment contains commands such as:

```text
dir c:\ >> %temp%\exfiltr8.log
dir "c:\Documents and Settings" >> %temp%\exfiltr8.log
dir "c:\Program Files\" >> %temp%\exfiltr8.log
dir d:\ >> %temp%\exfiltr8.log
net localgroup administrator >> %temp%\exfiltr8.log
ver >> %temp%\exfiltr8.log
systeminfo >> %temp%\exfiltr8.log
ipconfig /all >> %temp%\exfiltr8.log
netstat -ano >> %temp%\exfiltr8.log
net start >> %temp%\exfiltr8.log
```

### What is happening?

The commands gather information about the system:

- Directory contents
- Installed/program directories
- Local administrator information
- Windows version
- System information
- Network configuration
- Network connections
- Running services

The output is redirected into:

```text
%temp%\exfiltr8.log
```

---

# 11. Final Sigma Rule

Open:

**Sigma Rule Builder → Create Sigma Rule**

Select:

```text
Focus:
System Event Logs

Sysmon Event:
File Creation and Modification
```

Configure:

```text
File Path:
%temp%

File Name:
exfiltr8.log

ATT&CK ID:
Collection (TA0009)
```

Then validate the rule.

### Why this is a TTP-level detection

The detection is based on the attacker's operational behaviour:

1. Discover system information.
2. Collect the output.
3. Store it in a temporary log.
4. Potentially prepare it for later exfiltration.

This is much harder to evade than simply changing a hash or IP.

### Pyramid level

**TTPs — Tough**

### Final Flag

```text
THM{c8951b2ad24bbcbac60c16cf2c83d92c}
```

---

# 12. All Flags — Quick Reference

| # | Sample / Stage | Pyramid Level | Flag |
|---|---|---|---|
| 1 | `sample1.exe` | Hash | `THM{f3cbf08151a11a6a331db9c6cf5f4fe4}` |
| 2 | `sample2.exe` | IP Address | `THM{2ff48a3421a938b388418be273f4806d}` |
| 3 | `sample3.exe` | Domain | `THM{4eca9e2f61a19ecd5df34c788e7dce16}` |
| 4 | `sample4.exe` | Host/Network Artifact | `THM{c956f455fc076aea829799c0876ee399}` |
| 5 | `sample5.exe` | Tool | `THM{46b21c4410e47dc5729ceadef0fc722e}` |
| 6 | Final Sphinx stage | TTP | `THM{c8951b2ad24bbcbac60c16cf2c83d92c}` |

---

# 13. Full Attack → Detection Mapping

```text
SAMPLE 1
   ↓
Hash
   ↓
Manage Hashes
   ↓
Block exact binary


SAMPLE 2
   ↓
IP Address
   ↓
Firewall
   ↓
Egress → Any → 154.35.10.113 → Deny


SAMPLE 3
   ↓
Domain
   ↓
DNS Filter
   ↓
emudyn.bresonicz.info → Deny


SAMPLE 4
   ↓
Registry Artifact
   ↓
Sigma
   ↓
Detect DisableRealtimeMonitoring


SAMPLE 5
   ↓
Tool / Network Behaviour
   ↓
Sigma
   ↓
97-byte connection every 1800 seconds


FINAL
   ↓
TTP
   ↓
Sigma
   ↓
Detect creation of %temp%\exfiltr8.log
```

---

# 14. SOC Analyst Lessons

## 14.1 IOC vs Behaviour

### IOC-based detection

Examples:

```text
Hash
IP
Domain
```

These are easy to operationalise but can be changed by attackers.

### Behaviour-based detection

Examples:

```text
Registry modification
Repeated network pattern
System discovery
File creation
```

These describe attacker behaviour and can remain useful even when infrastructure changes.

---

# 15. False Positives

A detection should not automatically become a block rule.

For example:

```text
DisableRealtimeMonitoring
```

may be suspicious but could theoretically occur during legitimate administration.

Similarly:

```text
97-byte network connections every 1800 seconds
```

might not always be malicious.

A SOC analyst should correlate the alert with:

- Process
- User
- Host
- Parent process
- Destination
- Frequency
- Other events
- Historical activity

Then determine whether the event is benign, suspicious, or malicious.

---

# 16. MITRE ATT&CK Connections

Important ATT&CK concepts used in the room include:

### Defense Evasion

Sample 4 attempts to disable Windows Defender real-time monitoring.

```text
TA0005 — Defense Evasion
```

### Command and Control

Sample 5 demonstrates a repeated network communication pattern.

```text
TA0011 — Command and Control
```

### Collection

The final stage collects system information and writes it to a log file.

```text
TA0009 — Collection
```

---

# 17. Key SOC Vocabulary

| Term | Meaning |
|---|---|
| IOC | Indicator of Compromise |
| Hash | Fingerprint of a file |
| IP | Network address |
| Domain | Human-readable network name |
| Artifact | Observable trace left by activity |
| TTP | Tactics, Techniques & Procedures |
| Egress | Outbound traffic |
| Ingress | Inbound traffic |
| Sigma | Generic detection-rule format |
| Sysmon | Windows system-monitoring telemetry |
| C2 | Command and Control |
| ATT&CK | MITRE knowledge base of adversary behaviour |
| False Positive | Benign event incorrectly detected as malicious |
| IOC-based detection | Detection using a specific observable indicator |
| Behavioural detection | Detection based on activity/patterns |

---

# 18. Interview/Viva Questions

### Q1. What is the Pyramid of Pain?

A model that ranks attacker indicators from easier-to-change indicators such as hashes to harder-to-change behaviours such as TTPs.

### Q2. Why are hashes weak as long-term detection?

Because changing the file can produce a completely different hash.

### Q3. Why is behavioural detection stronger?

It can continue detecting an attacker even when they change infrastructure or modify their malware.

### Q4. What is Egress?

Traffic leaving a host or network.

### Q5. Why was `154.35.10.113` blocked as Egress?

Because the malware was making an outbound connection to that destination.

### Q6. Why use `Any` as the source?

The goal is to prevent any internal source from making the malicious outbound connection.

### Q7. Why detect the domain instead of only the IP?

Domains provide another infrastructure-level indicator and can cover infrastructure where IPs may change.

### Q8. What does `DisableRealtimeMonitoring = 1` indicate?

An attempt to disable Windows Defender real-time monitoring.

### Q9. What is Sigma?

A generic, structured format for describing detection rules, allowing detections to be shared across security platforms.

### Q10. Why was the final detection based on file creation?

The attacker was using commands to collect system information and redirecting the output into `%temp%\exfiltr8.log`. Detecting creation/modification of that artifact provides a behavioural signal.

---

# 19. One-Minute Revision

```text
Pyramid of Pain:

HASH
  ↓
IP
  ↓
DOMAIN
  ↓
HOST / NETWORK ARTIFACT
  ↓
TOOL
  ↓
TTP
```

Summit mapping:

```text
Sample 1 → Hash → Manage Hashes

Sample 2 → IP → Firewall
             Egress / Any / 154.35.10.113 / Deny

Sample 3 → Domain → DNS
             emudyn.bresonicz.info / Deny

Sample 4 → Registry Artifact → Sigma
             DisableRealtimeMonitoring = 1
             Defense Evasion / TA0005

Sample 5 → Tool/Network Pattern → Sigma
             97 bytes / 1800 seconds
             Command & Control / TA0011

Final → TTP → Sigma
          %temp%\exfiltr8.log
          Collection / TA0009
```

---

# 20. Sources / Further Reading

- TryHackMe Summit room
- TryHackMe Pyramid of Pain
- MITRE ATT&CK
- Sigma detection rules
- Sysmon documentation

This note is based on the Summit room workflow and cross-checked against multiple public Summit walkthroughs. The room itself is the authoritative source for the live lab state; walkthroughs are useful as supplementary references.
