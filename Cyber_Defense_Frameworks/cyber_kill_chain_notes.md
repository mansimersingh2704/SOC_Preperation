# Cyber Kill Chain — TryHackMe Notes

> **Room:** TryHackMe — Cyber Kill Chain  
> **Focus:** Cyber Defense Frameworks / SOC fundamentals  
> **Framework:** Lockheed Martin Cyber Kill Chain

---

## 1. What is the Cyber Kill Chain?

The **Cyber Kill Chain** is a framework used to describe the stages an adversary typically moves through during a cyberattack.

It helps defenders:

- Understand how an attack progresses.
- Identify where an attack can be detected or disrupted.
- Find missing security controls.
- Structure incident investigation and threat hunting.
- Understand an adversary's objectives and techniques.

The traditional Lockheed Martin Cyber Kill Chain contains **7 stages**:

1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command & Control (C2)
7. Actions on Objectives

### Core idea

> **Break the chain at any stage to prevent the attacker from reaching the objective.**

---

# 2. Cyber Kill Chain — Stage by Stage

## 2.1 Reconnaissance

### Purpose

The attacker researches the target before launching the attack.

They may collect:

- Infrastructure information
- Employee information
- Business processes
- Publicly exposed technologies
- Domains and subdomains
- IP addresses
- Email addresses
- Information from previous breaches

### OSINT

**OSINT = Open-Source Intelligence**

Common sources:

- Search engines
- Social media
- Online forums and blogs
- Public records
- News/media
- WHOIS and technical data
- Breach information

### Passive vs Active Recon

| Type | Meaning | Examples |
|---|---|---|
| Passive Recon | No direct interaction with the target | WHOIS, social media research, breach-data searches |
| Active Recon | Direct interaction with the target | Port scanning, banner grabbing, service probing |

### Useful OSINT tools

- `theHarvester` — gathers emails, names, subdomains, IPs and URLs.
- `Hunter.io` — finds contact information associated with a domain.
- `OSINT Framework` — collection of OSINT resources organised by category.

### SOC perspective

Look for:

- Unusual scanning activity
- Repeated connection attempts
- Enumeration against exposed services
- Suspicious collection of public information

---

# 3. Weaponization

## Purpose

The attacker converts information from reconnaissance into an **attack-ready payload**.

This can involve:

- Malware
- Exploits
- Payloads
- Malicious documents
- Backdoors
- C2 infrastructure
- Phishing templates

### Important terminology

**Malware**  
Software designed to damage, disrupt, or gain unauthorised access to a system.

**Exploit**  
Code or a program that takes advantage of a vulnerability or flaw.

**Payload**  
The malicious code that executes on the target.

### Examples

- Malicious Microsoft Office documents containing macros/VBA.
- Malicious payloads distributed through removable media.
- Backdoors designed to maintain access.
- Infrastructure prepared for command and control.
- Phishing templates designed to look legitimate.

### SOC perspective

Focus on:

- Malicious files
- Suspicious macros/scripts
- Payload creation or modification
- Malware samples
- Suspicious infrastructure preparation

---

# 4. Delivery

## Purpose

The attacker **transmits the payload** to the target environment.

Common delivery methods:

### Phishing

A malicious email may contain:

- A malicious attachment
- A malicious link
- A fake document
- A social-engineering lure

### Spearphishing

A targeted phishing attack against a specific person or group.

### USB Drop

Malicious USB devices may be deliberately placed where employees can find or receive them.

### Watering Hole

The attacker compromises a website frequently visited by a particular target group.

The victim may then be redirected to malicious content or tricked into downloading malware.

### SOC perspective

Monitor:

- Suspicious email attachments
- Malicious URLs
- Newly downloaded executables
- USB activity
- Browser redirects
- Phishing indicators

---

# 5. Exploitation

## Purpose

The attacker **executes code or abuses a vulnerability** on the target.

Common techniques include:

- Malicious macro execution
- Known CVE exploitation
- Zero-day exploitation
- Exploitation of exposed applications
- Exploitation for privilege escalation
- Exploitation of server/software vulnerabilities

### Zero-day

A **zero-day** is a vulnerability that is unknown to the vendor or has not yet received an available fix/patch at the time it is exploited.

### Signs of exploitation

Look for:

- Unexpected process creation
- Suspicious child processes
- Registry modifications
- New services
- Suspicious command-line arguments
- Exploit-related crashes
- Unusual application behaviour

### SOC mindset

Ask:

> **What vulnerability was abused, and what executed immediately after exploitation?**

---

# 6. Installation

## Purpose

The attacker establishes **persistence** so that access can survive beyond the initial compromise.

Common persistence mechanisms discussed in the room:

### Web Shell

A malicious script placed on a web server that allows an attacker to maintain remote access.

Examples of server-side scripting environments include:

- PHP
- ASP
- ASP.NET
- JSP

### Backdoor

A mechanism that provides continued access to a compromised system.

### Windows Services

Attackers may create or modify services so malicious programs execute as part of persistence.

### Registry Run Keys / Startup Folder

Malicious programs can be configured to execute when a user logs in.

### Timestomping

A technique that modifies file timestamps to make malicious files or changes appear legitimate.

Important file timestamps include:

- Modified
- Accessed
- Created
- Changed

### SOC perspective

Monitor:

- New services
- Service configuration changes
- Registry Run Keys
- Startup-folder changes
- Web shells
- Unexpected scheduled/persistent execution
- Timestamp inconsistencies

---

# 7. Command & Control (C2)

## Purpose

After compromising the system, the attacker establishes communication with infrastructure they control.

This allows the attacker to:

- Send commands
- Receive information
- Control the compromised host
- Deliver additional payloads
- Maintain communication with the victim

### C2 Beaconing

The compromised endpoint periodically communicates with the C2 infrastructure.

This repeated communication is commonly called **beaconing**.

### Common C2 channels

#### HTTP / HTTPS

Attack traffic can blend with legitimate web traffic.

Common ports:

- HTTP → 80
- HTTPS → 443

#### DNS

An infected host can make repeated DNS requests to attacker-controlled infrastructure.

This can be used for **DNS tunnelling**.

### SOC perspective

Look for:

- Periodic outbound connections
- Unusual domains
- Rare destinations
- Suspicious DNS patterns
- Long or encoded DNS queries
- Unexpected external connections
- Beacon-like timing

---

# 8. Actions on Objectives

## Purpose

The attacker finally performs the activity they wanted to achieve.

Possible objectives include:

- Credential theft
- Privilege escalation
- Internal reconnaissance
- Lateral movement
- Data collection
- Data exfiltration
- Deleting backups
- Deleting or destroying data

### Exfiltration

Sensitive information is collected and transferred out of the environment.

Examples:

- Credentials
- Customer information
- Financial data
- Intellectual property
- Database records

### SOC perspective

Monitor:

- Large outbound transfers
- Unusual destinations
- Sensitive file access
- Archive creation
- Credential access
- Lateral movement
- Backup deletion
- Destructive activity

---

# 9. Cyber Kill Chain — Quick Revision

| Stage | Key Question | Examples |
|---|---|---|
| Reconnaissance | What does the attacker know about us? | OSINT, WHOIS, email harvesting |
| Weaponization | What are they preparing? | Malware, payload, exploit, macro |
| Delivery | How does it reach us? | Phishing, USB, watering hole |
| Exploitation | How do they execute/enter? | CVE, zero-day, malicious macro |
| Installation | How do they stay? | Backdoor, web shell, services, Run Keys |
| C2 | How do they communicate? | HTTP/S, DNS tunnelling, beaconing |
| Actions on Objectives | What do they want? | Credential theft, lateral movement, exfiltration |

---

# 10. TryHackMe Practice — Target Breach

The room uses the **2013 Target breach** as a practical exercise.

The lab provides six items and asks us to map them to the relevant Cyber Kill Chain stages.

### Correct mapping

| Cyber Kill Chain Stage | Practice Item |
|---|---|
| **Weaponization** | `powershell` |
| **Delivery** | `spearphishing attachment` |
| **Exploitation** | `exploit public-facing application` |
| **Installation** | `dynamic linker hijacking` |
| **Command & Control** | `fallback channels` |
| **Actions on Objectives** | `data from local system` |

### Reconnaissance

Not represented in this particular six-item scenario.

### Why these mappings make sense

**Weaponization → PowerShell**  
PowerShell can be used as part of a malicious payload/tooling chain.

**Delivery → Spearphishing attachment**  
The attachment is the mechanism used to deliver the malicious content.

**Exploitation → Exploit public-facing application**  
The attacker abuses a vulnerable exposed application.

**Installation → Dynamic linker hijacking**  
Hijacking the dynamic linker/loading process can be used to establish malicious execution/persistence.

**C2 → Fallback channels**  
Fallback channels provide alternate communication paths when the primary C2 path is unavailable or disrupted.

**Actions on Objectives → Data from local system**  
Collecting data from the local system represents the attacker pursuing their final objective.

### Practice flag

```text
THM{7HR347_1N73L_12_4w35om3}
```

---

# 11. Important Terms From the Room

| Term | Meaning |
|---|---|
| OSINT | Open-Source Intelligence |
| C2 / C&C | Command and Control |
| Beaconing | Repeated communication between an infected host and C2 |
| DNS Tunnelling | Using DNS communication to carry C2/data |
| Zero-day | Previously unknown/unpatched vulnerability being exploited |
| Web Shell | Malicious server-side script used to maintain access |
| Timestomping | Modifying file timestamps to hide activity |
| Watering Hole | Compromising a site frequently visited by targets |
| Spearphishing | Targeted phishing |
| Payload | Malicious code delivered/executed on a target |
| Backdoor | Access mechanism that bypasses normal authentication/security |
| Exfiltration | Transfer of data out of the environment |

---

# 12. Defender's View — Breaking the Kill Chain

The Cyber Kill Chain is most useful when treated as a **defensive model**, not just an attack sequence.

### Reconnaissance

- Minimise public exposure.
- Monitor scanning.
- Reduce unnecessary information disclosure.

### Weaponization

- Email/file security.
- Malware analysis.
- Application allowlisting.
- Script controls.

### Delivery

- Secure email gateways.
- Attachment scanning.
- URL filtering.
- Security awareness.

### Exploitation

- Patch management.
- Vulnerability management.
- EDR.
- Exploit protection.

### Installation

- Persistence monitoring.
- File integrity monitoring.
- Registry/service monitoring.
- EDR telemetry.

### C2

- DNS monitoring.
- Egress filtering.
- Network detection.
- Domain/IP reputation.
- Beacon detection.

### Actions on Objectives

- DLP.
- Least privilege.
- Data access monitoring.
- Network segmentation.
- Backup protection.
- Incident response.

---

# 13. Limitations of the Traditional Cyber Kill Chain

The traditional framework was created in **2011** and was heavily focused on:

- Perimeter defence
- Malware delivery
- Network intrusion

Modern attacks are more complex.

Adversaries can:

- Combine many TTPs.
- Abuse legitimate tools.
- Modify indicators such as hashes/IPs.
- Use valid credentials.
- Operate through cloud environments.
- Exploit insider access.
- Move laterally without traditional malware.

### Important limitation

The traditional Cyber Kill Chain does not provide complete coverage of modern attack behaviour, especially **insider threats** and many post-compromise techniques.

### Use it with other frameworks

A stronger defensive approach is to combine it with:

- **MITRE ATT&CK**
- **Unified Kill Chain**
- Threat intelligence
- EDR telemetry
- SIEM detections
- Network monitoring

---

# 14. SOC Analyst Mental Model

When investigating an incident, think:

```text
What happened?
      ↓
How did the attacker get in?
      ↓
How was the payload delivered?
      ↓
What vulnerability or execution method was used?
      ↓
How did they maintain persistence?
      ↓
How did they communicate?
      ↓
What was the final objective?
```

Then ask:

> **At which stage could we have detected or broken the chain?**

This turns the Cyber Kill Chain from a memorisation exercise into a practical SOC investigation model.

---

## Quick Memory Trick

```text
R → W → D → E → I → C → A

Recon
Weaponize
Deliver
Exploit
Install
Command & Control
Act on Objectives
```

**Think:**

> **Find → Build → Send → Execute → Stay → Control → Achieve**

---

## References

- TryHackMe — Cyber Kill Chain room
- Lockheed Martin Cyber Kill Chain
- MITRE ATT&CK
- CISA cybersecurity guidance

