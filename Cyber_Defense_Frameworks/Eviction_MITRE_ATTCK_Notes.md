# TryHackMe — Eviction Room Notes

## Overview

**Room:** Eviction  
**Platform:** TryHackMe  
**Focus:** Mapping an APT attack path to the MITRE ATT&CK framework.

This room follows an APT investigation and asks you to identify techniques across the attack lifecycle using the MITRE ATT&CK Navigator.

---

## Attack Lifecycle / MITRE ATT&CK Mapping

### 1. Reconnaissance

The threat actor begins by gathering information about the target.

**Key idea:**
- Reconnaissance is used to collect information before attempting access.
- Look for ATT&CK techniques related to gathering information about the target.

---

### 2. Resource Development

After reconnaissance, an APT may prepare resources needed for the operation.

**Example from the room:**
- The attacker may compromise or create **email accounts** while developing resources.

**Important concept:**
> Resource Development covers activities used to establish resources that support later stages of an operation.

---

### 3. Initial Access

The room identifies **social engineering** as part of the initial access chain.

Important techniques to recognize:

- **Spearphishing Link**
- User interaction may be required for the attack to succeed.

The important distinction is that a phishing link can be used to both:
- Gather information / perform reconnaissance
- Obtain initial access

---

## 4. Execution

The attacker attempts to get the victim to execute malicious content.

### User Execution

Two important ATT&CK techniques/sub-techniques highlighted by the room are:

- **Malicious File**
- **Malicious Link**

These rely on the victim/user interacting with malicious content.

---

## 5. Scripting Interpreters

After successful user execution, investigate **Scripting Interpreter** techniques.

### How to investigate

In MITRE ATT&CK:

1. Search for **Scripting Interpreter**.
2. Open the technique.
3. Examine its **sub-techniques**.
4. Identify the interpreters relevant to execution on the target system.
5. Compare the exact ATT&CK terminology with the TryHackMe question.

### Key lesson

Do not stop at the parent technique. ATT&CK frequently stores the specific interpreter as a **sub-technique**.

---

## 6. Persistence — Registry

The room describes obfuscated scripts modifying the Windows Registry for persistence.

### What to investigate

Search MITRE ATT&CK for:

**Boot or Logon Autostart Execution**

Then inspect the relevant registry-persistence sub-technique.

The important investigation clue is:

> Registry changes + maintaining persistence

When investigating, look for:
- Registry locations
- Run/Startup mechanisms
- Values that cause programs/scripts to execute automatically

### Investigation tip

Use `Ctrl + F` on the ATT&CK page and search for:

- `Registry`
- `Run`
- `Startup`

---

## 7. Defense Evasion — System Binary Proxy Execution

The room next identifies execution of legitimate system binaries to evade defenses.

### Technique

**System Binary Proxy Execution**

### Why it matters

Attackers can abuse trusted operating-system binaries to execute malicious content while attempting to blend in with legitimate activity.

### Investigation workflow

1. Search ATT&CK for **System Binary Proxy Execution**.
2. Open the technique.
3. Examine its **sub-techniques**.
4. Read the descriptions of the binaries.
5. Match the question's wording to the relevant system binary.

---

## 8. Discovery

The room then moves into discovery activity.

A compromised host may be used to gather information about:
- Network environment
- Hosts
- Accounts
- Services
- Other useful infrastructure

### Network Discovery

A key ATT&CK concept is **Network Service Scanning**.

Use ATT&CK to determine exactly which discovery technique matches the behavior described in the room.

---

## 9. Lateral Movement

APT activity can involve exploiting remote services to move between systems.

### Investigation approach

Search ATT&CK for:

**Remote Services**

Then examine the relevant sub-techniques.

Examples of remote-service concepts include:
- Remote Desktop Protocol (RDP)
- SMB/Windows Admin Shares
- Other remote administration mechanisms

The exact answer should be taken from the ATT&CK terminology corresponding to the behavior in the question.

---

## 10. Collection

Once the attacker has access to useful systems, they may collect information.

The room references stealing intellectual property from information repositories.

### Example

**SharePoint**

This is an example of information being collected from a repository used by an organization.

### Key idea

Collection techniques describe how attackers gather data of interest before attempting exfiltration or other objectives.

---

## 11. Exfiltration

The room introduces a major constraint:

> The APT could collect data but could not connect to the C2 for data exfiltration.

This means the attacker needs another way to move collected information out.

### Proxy-based exfiltration

Investigate ATT&CK's **Proxy**-related techniques.

The room specifically points toward:
- External Proxy
- Multi-hop Proxy

### Key lesson

A proxy can provide an alternate communication path when direct communication with the command-and-control infrastructure is unavailable.

---

# MITRE ATT&CK Investigation Cheat Sheet

| Behavior / Clue | ATT&CK area to investigate |
|---|---|
| Reconnaissance | Reconnaissance |
| Developing attacker resources | Resource Development |
| Phishing link | Initial Access |
| User executes malicious content | User Execution |
| Scripts/interpreters | Scripting Interpreter |
| Registry persistence | Boot or Logon Autostart Execution |
| Trusted system binary abused | System Binary Proxy Execution |
| Finding network services | Discovery |
| Remote access between systems | Remote Services |
| Stealing organizational data | Collection |
| Alternate path for exfiltration | Proxy / Exfiltration |

---

# How to Solve ATT&CK Navigator Questions

When a TryHackMe question gives a behavior instead of a technique name, use this workflow:

```text
Question
   ↓
Extract behavior / keywords
   ↓
Identify ATT&CK tactic
   ↓
Find parent technique
   ↓
Check sub-techniques
   ↓
Read description
   ↓
Match exact ATT&CK wording
```

## Useful keywords

### Initial Access
- phishing
- spearphishing
- malicious link
- malicious attachment

### Execution
- user execution
- scripting
- PowerShell
- command shell
- interpreter

### Persistence
- registry
- startup
- run keys
- logon
- autostart

### Defense Evasion
- proxy execution
- trusted binary
- system binary
- signed binary

### Discovery
- network
- services
- hosts
- scanning

### Lateral Movement
- remote service
- RDP
- SMB
- Windows admin shares

### Collection
- documents
- intellectual property
- repositories
- SharePoint

### Exfiltration
- proxy
- external proxy
- multi-hop
- alternate communication path

---

# Practical ATT&CK Navigator Tips

## Search

Use the browser's:

```text
Ctrl + F
```

Useful searches:

```text
Scripting Interpreter
Boot or Logon Autostart Execution
Registry
System Binary Proxy Execution
Network Service Scanning
Remote Services
SharePoint
External Proxy
Multi-hop Proxy
```

## Parent vs Sub-technique

A common mistake is entering the parent technique when TryHackMe expects a specific sub-technique.

Always check:

```text
Parent Technique
└── Sub-techniques
    ├── ...
    ├── ...
    └── ...
```

The question's wording usually gives clues about which level is required.

---

# Key Takeaways

- **Reconnaissance** happens before direct compromise.
- **Resource Development** involves preparing resources for an operation.
- **Initial Access** includes phishing-based techniques.
- **User Execution** depends on the victim interacting with malicious content.
- **Scripting Interpreter** techniques describe interpreters used to execute commands/scripts.
- Registry modifications can provide **persistence**.
- Legitimate system binaries can be abused for **proxy execution** and defense evasion.
- **Network Service Scanning** belongs to Discovery.
- **Remote Services** can enable lateral movement.
- **SharePoint** can be an information repository targeted during Collection.
- **External Proxy / Multi-hop Proxy** can provide alternate communication paths.

---

## Reference

- MITRE ATT&CK: https://attack.mitre.org/
- MITRE ATT&CK Navigator: https://mitre-attack.github.io/attack-navigator/
- TryHackMe Eviction: https://tryhackme.com/room/eviction

> **Note:** These notes intentionally focus on the methodology and ATT&CK concepts rather than simply copying room answers. Use the ATT&CK pages and the question wording to verify exact sub-technique names.
