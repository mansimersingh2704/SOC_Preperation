# TryHackMe --- Pyramid of Pain

> **Room:** Pyramid of Pain\
> **Platform:** TryHackMe\
> **Purpose:** Understand how defenders can use Indicators of Compromise
> (IOCs) and higher-level behavioral indicators to increase the cost of
> an adversary's operations.

## 1. Core Concept

The **Pyramid of Pain**, introduced by David Bianco, ranks indicators
from the easiest for an attacker to change to the hardest.

The key defensive idea is:

> **The higher you detect in the Pyramid, the more pain you cause the
> attacker.**

### Pyramid at a glance

  Level   Indicator                  Defender value   Attacker pain
  ------- -------------------------- ---------------- ---------------
  1       Hash Values                Low              Trivial
  2       IP Addresses               Low--Medium      Easy
  3       Domain Names               Medium           Simple
  4       Network / Host Artifacts   High             Annoying
  5       Tools                      Very High        Challenging
  6       TTPs                       Very High        Tough

The first three levels are mostly **atomic IOCs**. The upper levels
become increasingly behavioral and contextual.

------------------------------------------------------------------------

# 2. Hash Values --- Trivial

A cryptographic hash produces a fixed-length value representing a file
or other input.

Common examples:

-   **MD5** --- Message-Digest Algorithm 5
-   **SHA-1** --- Secure Hash Algorithm 1
-   **SHA-256** --- Secure Hash Algorithm 2, 256-bit variant

Example:

``` text
File → SHA-256 → 9f86d081884c7d659a2feaa0c55ad015...
```

### Why hashes are useful

If a known malicious file has a known hash, defenders can:

-   Search endpoint telemetry for the hash
-   Block the known sample
-   Enrich an alert
-   Pivot through threat-intelligence platforms
-   Correlate incidents involving the same sample

Useful services:

-   VirusTotal
-   MetaDefender Cloud / OPSWAT
-   MalwareBazaar
-   Hybrid Analysis

### Why hashes are "trivial"

A malware author can change the hash simply by modifying the file.

Even appending harmless data can produce a completely different
cryptographic hash.

Therefore:

``` text
Same malware
    ↓
Small file modification
    ↓
Different hash
    ↓
Hash IOC may no longer match
```

### Practical lesson

**Hash = excellent for identifying an exact known sample, but weak as a
standalone behavioral detection.**

------------------------------------------------------------------------

# 3. IP Addresses --- Easy

An IP address identifies a network endpoint.

Defenders can use malicious IP addresses to:

-   Block outbound connections
-   Add firewall rules
-   Create SIEM detections
-   Search proxy/DNS/network logs
-   Correlate malware communication

Example:

``` text
Malware
   ↓
50.87.136.52:443
   ↓
C2 infrastructure
```

### Why IPs are higher than hashes

Changing an IP usually requires more effort than modifying a file, but
infrastructure can still be changed.

Attackers may use:

-   New servers
-   Bulletproof hosting
-   Fast Flux
-   Compromised infrastructure
-   Proxies
-   Cloud infrastructure

### Fast Flux

Fast Flux associates many changing IP addresses with a domain.

The defender blocks one IP:

``` text
C2 Domain
 ├── IP A
 ├── IP B
 ├── IP C
 └── IP D
```

The attacker can rotate the destination.

### Defensive lesson

**Blocking IPs is useful, but don't rely only on IP reputation. Combine
IP telemetry with DNS, domain, process, and network behavior.**

------------------------------------------------------------------------

# 4. Domain Names --- Simple

Domains are human-readable names used to reach infrastructure.

Example:

``` text
malicious-example[.]com
```

Domains are generally more persistent than individual IPs, but attackers
can still register new domains.

## Important concepts

### Domain-based detection

Defenders can monitor:

-   DNS queries
-   Newly registered domains
-   Domain reputation
-   DNS history
-   WHOIS information
-   Domain/IP relationships
-   Suspicious TLDs
-   DGA-like domain patterns

### Punycode

A **Punycode attack** abuses Unicode characters so a malicious domain
can visually resemble a legitimate domain.

Conceptually:

``` text
Legitimate:
apple.com

Look-alike:
Unicode/Punycode representation
```

Always inspect the actual encoded domain rather than trusting visual
appearance.

### URL shorteners

Attackers can hide the final destination behind services such as URL
shorteners.

Defenders should inspect:

``` text
Short URL
   ↓
Redirect
   ↓
Final destination
```

Useful analysis sources include URL expansion/preview tools, proxy logs,
and sandbox reports.

### Defensive lesson

**Domains are more annoying for attackers than IPs because changing a
domain requires acquisition, configuration, reputation building, and
infrastructure changes.**

------------------------------------------------------------------------

# 5. Network & Host Artifacts --- Annoying

This is where detection becomes much more behavioral.

## Host artifacts

Host artifacts are traces left on the endpoint.

Examples:

-   Registry modifications
-   Suspicious files
-   Dropped executables
-   Persistence entries
-   Mutexes
-   Scheduled tasks
-   Suspicious process execution
-   Unusual command lines
-   Malware-specific paths
-   Modified configuration files

Example:

``` text
WINWORD.EXE
    ↓
PowerShell.exe
    ↓
Downloads suspicious executable
    ↓
Creates persistence
```

This sequence is more valuable than simply blocking one file hash.

## Network artifacts

Network artifacts are distinctive characteristics visible in traffic.

Examples:

-   Unique User-Agent
-   Suspicious URI pattern
-   HTTP POST pattern
-   Unusual protocol behavior
-   Distinctive headers
-   Repeated beacon timing
-   Abnormal DNS requests
-   Specific request/response structures

Example:

``` text
User-Agent:
Mozilla/5.0 (...) unusual-malware-string
```

If the attacker changes the IP but keeps the same malware/network
behavior, the network artifact can still identify the activity.

### TryHackMe example: Emotet

The room uses a distinctive User-Agent as a network artifact.

The screenshot asks which browser uses the displayed User-Agent.

**Answer: Internet Explorer**

The PCAP screenshot contains:

**6 POST requests**

### Defensive lesson

Artifacts are stronger because attackers often have to modify the
malware itself or its communication logic rather than simply changing
infrastructure.

------------------------------------------------------------------------

# 6. Tools --- Challenging

At this level, defenders detect the **tooling** used by the attacker.

Examples:

-   Cobalt Strike
-   Mimikatz
-   PowerShell
-   PsExec
-   Rclone
-   Impacket
-   Metasploit
-   Sliver
-   Custom malware frameworks

## Why tools are harder to defeat

Suppose defenders recognize a C2 framework from:

-   Its protocol behavior
-   Beacon characteristics
-   Command patterns
-   Configuration
-   Artifacts
-   Network behavior

The attacker may need to:

1.  Replace the tool
2.  Reconfigure it
3.  Modify its implementation
4.  Build a custom tool
5.  Train operators on another tool

That costs time and resources.

### Example: Cobalt Strike

The TryHackMe room asks which commercial remote-access tool is
associated with Chimera for C2 beacons and data exfiltration.

**Answer: Cobalt Strike**

### Important nuance

PowerShell itself is not automatically malicious.

The correct detection approach is usually:

``` text
Tool
+
Parent process
+
Command line
+
User
+
Destination
+
Timing
+
Behavior
```

rather than simply:

``` text
"powershell.exe" = malicious
```

------------------------------------------------------------------------

# 7. TTPs --- Tough

**TTP = Tactics, Techniques, and Procedures**

This is the highest level of the Pyramid of Pain.

Instead of asking:

> "Which file/hash/IP/domain is malicious?"

we ask:

> "What is the attacker actually doing?"

Examples:

-   Phishing
-   Credential dumping
-   PowerShell execution
-   Discovery
-   Lateral movement
-   Command and control
-   Data staging
-   Data compression
-   Exfiltration

## Why TTPs cause the most pain

Changing a TTP can require changing the entire attack strategy.

For example:

``` text
Detect:
Credential dumping

Attacker:
Cannot simply change one IP address.
May need a different credential-access technique.
```

This is why behavior-based detections can survive IOC rotation.

------------------------------------------------------------------------

# 8. Pyramid of Pain --- Mental Model

Remember this progression:

``` text
                 TTPs
              / Tough \
             /         \
           Tools
        / Challenging \
       /               \
 Network / Host Artifacts
      /    Annoying     \
     /                   \
  Domain Names
     / Simple \
    /          \
 IP Addresses
    / Easy \
   /        \
 Hash Values
   /Trivial\
```

### Bottom → Easy to change

``` text
Hash
  ↓
IP
  ↓
Domain
  ↓
Artifact
  ↓
Tool
  ↓
TTP
```

### Top → Harder to change

``` text
TTP
  ↑
Tool
  ↑
Artifact
  ↑
Domain
  ↑
IP
  ↑
Hash
```

------------------------------------------------------------------------

# 9. TryHackMe Room Answers / Checkpoints

> These are the room-specific answers visible in the supplied room
> screenshots and supporting room material.

## Hash Values

**Filename associated with hash:**

``` text
Sales_Receipt 5606.xls
```

## IP Address

**First IP contacted by malicious process PID 1632:**

``` text
50.87.136.52
```

## Domain Name

**First domain contacted by PID 1632:**

``` text
craftingalegacy.com
```

## Domain / URL Concepts

**Address used to access websites:**

``` text
Domain Name
```

**Unicode look-alike domain attack:**

``` text
Punycode attack
```

## URL Shortener

The room's preview example redirects to:

``` text
https://tryhackme.com/
```

## Host Artifacts

**Suspicious executable dropped by the actor:**

``` text
G_jugk.exe
```

## Network Artifacts

**Malware identified through the User-Agent:**

``` text
Emotet
```

**Browser associated with the displayed User-Agent:**

``` text
Internet Explorer
```

**POST requests in the PCAP screenshot:**

``` text
6
```

## Fuzzy Hashing

**Method used to determine similarity between files:**

``` text
Fuzzy Hashing
```

**Full/alternative term:**

``` text
Context Triggered Piecewise Hashes
```

A commonly encountered implementation is **ssdeep**.

------------------------------------------------------------------------

# 10. Fuzzy Hashing / SSDeep

Traditional hashes are excellent for exact matches:

``` text
File A
  ↓
SHA-256
  ↓
HASH-A

Small modification

File A'
  ↓
SHA-256
  ↓
HASH-B
```

`HASH-A != HASH-B`

Fuzzy hashing is designed to identify **similarity**, not exact
equality.

Conceptually:

``` text
Original malware
       ↓
Small modification
       ↓
Fuzzy hash similarity
       ↓
High similarity score
```

This is useful for:

-   Malware variants
-   Modified samples
-   Threat hunting
-   Clustering related files
-   Identifying near-duplicate malware

------------------------------------------------------------------------

# 11. ATT&CK Mapping --- TTP Level

The Pyramid of Pain's TTP level maps naturally to **MITRE ATT&CK**.

MITRE ATT&CK organizes adversary behavior into:

-   Tactics
-   Techniques
-   Sub-techniques
-   Procedures

For example:

``` text
Tactic:
Exfiltration

Technique:
T1041 — Exfiltration Over C2 Channel
```

The current ATT&CK Enterprise Exfiltration tactic contains **9
techniques**.

------------------------------------------------------------------------

# 12. Exfiltration --- Important for SOC / CTI

**Exfiltration** is the adversary's attempt to steal data from a victim
environment.

MITRE ATT&CK identifies Exfiltration as:

``` text
TA0010 — Exfiltration
```

Common examples include:

### T1041 --- Exfiltration Over C2 Channel

Data is sent through an existing command-and-control channel.

``` text
Victim
  ↓
Existing C2 channel
  ↓
Attacker
```

### T1048 --- Exfiltration Over Alternative Protocol

The attacker uses a protocol other than the existing C2 channel.

Examples can include:

-   DNS
-   Other network protocols
-   Encrypted alternate channels

### T1567 --- Exfiltration Over Web Service

The attacker uses a legitimate web service to move stolen data.

Sub-techniques include:

``` text
T1567.001 — Exfiltration to Code Repository
T1567.002 — Exfiltration to Cloud Storage
T1567.003 — Exfiltration to Text Storage Sites
T1567.004 — Exfiltration Over Webhook
```

------------------------------------------------------------------------

# 13. How to Find "Exfiltration" in MITRE ATT&CK

If a question asks:

> "How many techniques fall under the Exfiltration category?"

Go to the MITRE ATT&CK **Exfiltration tactic page** rather than
searching the entire website manually.

Useful page:

-   MITRE ATT&CK --- Exfiltration (TA0010)

This is the correct place to inspect the current technique count.

------------------------------------------------------------------------

# 14. Exfiltration Detection --- SOC Perspective

Don't rely only on an IP/domain IOC.

Look for behavioral sequences such as:

``` text
Sensitive file access
       ↓
Archive / staging
       ↓
Compression / encryption
       ↓
Unusual outbound connection
       ↓
Large outbound transfer
       ↓
Rare external destination
```

Useful telemetry:

### Endpoint

-   Process creation
-   File access
-   File creation
-   Archive utilities
-   PowerShell
-   Command line
-   User context

### Network

-   DNS
-   Proxy
-   HTTP/S
-   NetFlow
-   Firewall
-   TLS metadata
-   Upload/download byte ratios

### Cloud / SaaS

-   File upload logs
-   API activity
-   OAuth activity
-   Cloud storage events
-   Unusual account behavior

------------------------------------------------------------------------

# 15. Example Detection Logic

A useful analytic could look conceptually like:

``` text
IF
    sensitive_file_access = TRUE
AND
    unusual_process = TRUE
AND
    rare_external_destination = TRUE
AND
    outbound_bytes >> normal_baseline
THEN
    investigate_possible_exfiltration
```

The exact thresholds should be environment-specific.

------------------------------------------------------------------------

# 16. IOC vs Behavioral Detection

  Detection          Example                                      Attacker effort
  ------------------ -------------------------------------------- -----------------
  Hash IOC           SHA-256                                      Very low
  IP IOC             1.2.3.4                                      Low
  Domain IOC         evil-domain.com                              Low--Medium
  Network artifact   Unique User-Agent                            Medium
  Host artifact      Registry/file/process behavior               Medium--High
  Tool               Cobalt Strike behavior                       High
  TTP                Credential dumping / exfiltration behavior   Very High

### Key SOC lesson

A mature detection program should combine all levels.

``` text
IOC Detection
      +
Behavior Detection
      +
Threat Intelligence
      +
MITRE ATT&CK Mapping
      =
Stronger Detection
```

------------------------------------------------------------------------

# 17. Practical Workflow for a SOC Analyst

When you receive a suspicious sample:

### Step 1 --- Hash

Calculate:

``` text
MD5
SHA-1
SHA-256
```

Search the hashes in your threat-intelligence sources.

### Step 2 --- Infrastructure

Extract:

``` text
IP addresses
Domains
URLs
DNS records
```

### Step 3 --- Host Artifacts

Investigate:

``` text
Processes
Files
Registry
Persistence
Command lines
Mutexes
```

### Step 4 --- Network Artifacts

Investigate:

``` text
User-Agent
URI
HTTP methods
DNS patterns
Beacon timing
TLS metadata
Protocol behavior
```

### Step 5 --- Tooling

Identify:

``` text
Cobalt Strike?
PowerShell?
Mimikatz?
Rclone?
Impacket?
Custom malware?
```

### Step 6 --- TTP Mapping

Map behavior to MITRE ATT&CK.

Example:

``` text
Observed behavior
      ↓
MITRE ATT&CK technique
      ↓
Detection analytic
      ↓
SIEM / EDR rule
```

------------------------------------------------------------------------

# 18. Important Takeaways

### Hashes

> Exact sample identification.

### IPs

> Infrastructure identification.

### Domains

> Infrastructure and DNS intelligence.

### Network/Host Artifacts

> Detect the traces and characteristics left by malicious activity.

### Tools

> Detect attacker tooling and its behavioral fingerprints.

### TTPs

> Detect what the attacker is doing rather than what exact artifact they
> are using.

------------------------------------------------------------------------

# 19. One-Line Revision

``` text
Hash → IP → Domain → Artifact → Tool → TTP
```

Or:

``` text
Exact Indicator
      ↓
Infrastructure
      ↓
Behavioral Artifact
      ↓
Tooling
      ↓
Adversary Behavior
```

**Higher on the Pyramid = more attacker pain.**

------------------------------------------------------------------------

# 20. Useful External References

## Primary / High-Value References

-   David Bianco --- Pyramid of Pain presentation
-   MITRE ATT&CK --- Enterprise Techniques
-   MITRE ATT&CK --- Exfiltration (TA0010)
-   MITRE ATT&CK --- Exfiltration Over C2 Channel (T1041)
-   MITRE ATT&CK --- Exfiltration Over Web Service (T1567)
-   VirusTotal
-   OPSWAT MetaDefender Cloud
-   MalwareBazaar
-   ssdeep documentation

## TryHackMe

Room:

``` text
Pyramid of Pain
https://tryhackme.com/room/pyramidofpainax
```

------------------------------------------------------------------------

# 21. Quick Revision Sheet

``` text
PYRAMID OF PAIN

1. HASHES
   - MD5
   - SHA-1
   - SHA-256
   - Exact file identification
   - Trivial for attacker to change

2. IP ADDRESSES
   - C2 / malicious infrastructure
   - Easy to rotate
   - Firewall / SIEM / threat intel

3. DOMAIN NAMES
   - DNS infrastructure
   - Punycode
   - Fast Flux
   - URL redirection

4. NETWORK / HOST ARTIFACTS
   - User-Agent
   - URI patterns
   - Registry
   - Files
   - Processes
   - Persistence
   - Annoying for attacker

5. TOOLS
   - Cobalt Strike
   - Mimikatz
   - PowerShell
   - Impacket
   - Requires tooling changes

6. TTPS
   - Tactics
   - Techniques
   - Procedures
   - Hardest to change
   - Highest attacker pain
```

------------------------------------------------------------------------

# 22. Exam / Interview Questions

### Q: Why is a hash at the bottom?

Because a small modification to a file produces a different
cryptographic hash.

### Q: Why are TTPs at the top?

Because changing an established attack technique can require significant
changes to the attacker's workflow, tooling, and operational procedures.

### Q: What is SSDeep?

A fuzzy hashing implementation used to compare files based on similarity
rather than exact equality.

### Q: What is the alternative/full term associated with fuzzy hashing in the room?

**Context Triggered Piecewise Hashes (CTPH).**

### Q: What is TA0010?

**MITRE ATT&CK Exfiltration tactic.**

### Q: What is T1041?

**Exfiltration Over C2 Channel.**

### Q: What is T1567?

**Exfiltration Over Web Service.**

### Q: Why is behavioral detection stronger than a single IOC?

Because infrastructure and files can change while the underlying
attacker behavior may remain similar.

------------------------------------------------------------------------

## Final Mental Model

``` text
          TTPs
       "What?"
          ▲
          │
        Tools
       "How?"
          ▲
          │
 Host / Network Artifacts
      "Traces"
          ▲
          │
       Domains
      "Where?"
          ▲
          │
         IPs
      "Where?"
          ▲
          │
        Hashes
       "Which?"

     MORE PAIN ↑
```

**Defender goal:** move detection upward whenever possible.

**Attacker goal:** change indicators before they are detected.
