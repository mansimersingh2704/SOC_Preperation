# Unified Kill Chain (UKC)

> **Source:** TryHackMe --- Unified Kill Chain\
> **Purpose:** Practical notes for cybersecurity / SOC learning

------------------------------------------------------------------------

## 1. What is the Unified Kill Chain?

The **Unified Kill Chain (UKC)** is a cybersecurity framework used to
describe the different phases an attacker may go through during a cyber
attack.

It was published by **Paul Pols in 2017** to complement existing attack
frameworks such as the **Lockheed Martin Cyber Kill Chain** and **MITRE
ATT&CK**.

The UKC expands the traditional kill-chain model to cover **18 phases**,
including initial access, post-exploitation activity, lateral movement,
data theft, and the attacker's final objectives.

### Why UKC is useful

-   Gives defenders a high-level view of an attack.
-   Helps identify where an attacker currently is in the attack
    lifecycle.
-   Covers both initial compromise and post-compromise activity.
-   Helps SOC and incident-response teams map attacker behaviour.
-   Makes it easier to identify defensive opportunities before the
    attacker reaches the final objective.

------------------------------------------------------------------------

# 2. The 18 Phases of the Unified Kill Chain

  ------------------------------------------------------------------------
  \#                      Phase                   Main idea
  ----------------------- ----------------------- ------------------------
  1                       **Reconnaissance**      Researching and
                                                  identifying targets
                                                  using active or passive
                                                  reconnaissance

  2                       **Weaponization**       Preparing the
                                                  infrastructure/payload
                                                  required for the attack

  3                       **Delivery**            Transmitting the
                                                  weaponized object to the
                                                  target environment

  4                       **Social Engineering**  Manipulating people into
                                                  performing actions that
                                                  help the attacker

  5                       **Exploitation**        Taking advantage of
                                                  vulnerabilities or
                                                  weaknesses to execute
                                                  code

  6                       **Persistence**         Maintaining access after
                                                  obtaining an initial
                                                  foothold

  7                       **Defence Evasion**     Avoiding or bypassing
                                                  defensive/security
                                                  controls

  8                       **Command & Control**   Establishing
                                                  attacker-controlled
                                                  communication with
                                                  compromised systems

  9                       **Pivoting**            Using a compromised
                                                  system to reach
                                                  otherwise inaccessible
                                                  systems

  10                      **Discovery**           Gathering information
                                                  about systems, users,
                                                  permissions, and the
                                                  network

  11                      **Privilege             Obtaining higher
                          Escalation**            privileges on a
                                                  compromised system

  12                      **Execution**           Running malicious code
                                                  or commands

  13                      **Credential Access**   Obtaining credentials
                                                  such as passwords or
                                                  tokens

  14                      **Lateral Movement**    Moving from one
                                                  compromised system to
                                                  another

  15                      **Collection**          Gathering data relevant
                                                  to the attacker's
                                                  objective

  16                      **Exfiltration**        Removing stolen data
                                                  from the target
                                                  environment

  17                      **Impact**              Manipulating,
                                                  interrupting, or
                                                  destroying systems/data

  18                      **Objectives**          Achieving the attacker's
                                                  final strategic goal
  ------------------------------------------------------------------------

------------------------------------------------------------------------

# 3. Initial Access / Entry

The first part of an attack is focused on getting into the target
environment.

## Reconnaissance

**Goal:** Gather information about the target.

Reconnaissance can be:

-   **Passive** --- collecting information without directly interacting
    with the target.
-   **Active** --- directly interacting with systems or services to
    gather information.

### Information attackers may collect

-   IP addresses
-   Domains and subdomains
-   Open services
-   Applications and technologies
-   Employee information
-   Email addresses
-   Potential credentials
-   Network information

**MITRE ATT&CK:** `TA0043`

------------------------------------------------------------------------

## Weaponization

**Goal:** Prepare the infrastructure or malicious payload needed for the
attack.

Examples:

-   Preparing malware
-   Preparing an exploit
-   Setting up command-and-control infrastructure
-   Preparing a malicious document or payload

**MITRE ATT&CK:** `TA0001`

------------------------------------------------------------------------

## Delivery

**Goal:** Get the weaponized object into the target environment.

Common delivery mechanisms include:

-   Phishing emails
-   Malicious attachments
-   Malicious links
-   Drive-by downloads
-   Removable media
-   Exploit delivery

------------------------------------------------------------------------

## Social Engineering

**Goal:** Manipulate a victim into performing an action useful to the
attacker.

Examples:

-   Phishing
-   Impersonation
-   Malicious attachment
-   Fake login page
-   Calling an employee and requesting credentials
-   Pretending to be IT support

**MITRE ATT&CK:** `TA0001`

### Easy way to remember

> **Social Engineering attacks the human.**

------------------------------------------------------------------------

## Exploitation

**Goal:** Abuse a vulnerability or weakness to execute code or gain
access.

Examples:

-   Exploiting a vulnerable web application
-   Executing malicious scripts
-   Exploiting software vulnerabilities
-   Using a vulnerability to obtain code execution

**MITRE ATT&CK:** `TA0002`

------------------------------------------------------------------------

# 4. Maintaining and Hiding Access

Once the attacker obtains a foothold, they often try to keep access and
avoid detection.

## Persistence

**Goal:** Maintain access to the target system.

Examples:

-   Creating a persistent service
-   Adding persistence mechanisms
-   Startup execution
-   Scheduled execution
-   Backdoors
-   Adding the target system to attacker-controlled infrastructure

**MITRE ATT&CK:** `TA0003`

### SOC perspective

If malware is removed but a persistence mechanism remains, the attacker
may simply regain access.

------------------------------------------------------------------------

## Defence Evasion

**Goal:** Avoid detection or bypass defensive controls.

Possible targets include:

-   Web application firewalls
-   Network firewalls
-   Antivirus
-   Endpoint security
-   Intrusion detection systems

**MITRE ATT&CK:** `TA0005`

### SOC perspective

Defence evasion explains **how attackers attempt to stay hidden**.

------------------------------------------------------------------------

## Command & Control (C2)

**Goal:** Establish communication between the compromised system and
attacker-controlled infrastructure.

Possible attacker actions:

-   Execute commands
-   Receive instructions
-   Send information
-   Control compromised systems
-   Use the compromised host to interact with other systems

**MITRE ATT&CK:** `TA0011`

### Key idea

> **C2 = communication/control channel between attacker and compromised
> system.**

------------------------------------------------------------------------

# 5. The "Through" Section

After obtaining an initial foothold, attackers may move **through** the
environment to reach systems containing valuable information or having
higher privileges.

The UKC describes this stage using several phases.

------------------------------------------------------------------------

## Pivoting

**Goal:** Use a compromised system as a staging point to reach another
system or network.

Example:

``` text
Internet
   |
   v
Compromised Server
   |
   v
Internal Network
   |
   +----> Internal Web Server
   |
   +----> Database Server
```

Pivoting is particularly important when internal systems are not
directly accessible from the Internet.

**MITRE ATT&CK:** `TA0008`

------------------------------------------------------------------------

## Discovery

**Goal:** Learn about the environment.

Attackers may discover:

-   Users
-   Accounts
-   Permissions
-   Applications
-   Running services
-   Network shares
-   Directories
-   System configurations
-   Other hosts

**MITRE ATT&CK:** `TA0007`

### SOC perspective

Discovery activity can reveal that an attacker has moved beyond the
initial compromised host.

------------------------------------------------------------------------

## Privilege Escalation

**Goal:** Obtain higher privileges.

Examples of privilege levels:

``` text
Normal User
     |
     v
Administrator
     |
     v
SYSTEM / Root
```

Attackers may exploit:

-   Vulnerable software
-   Misconfigured permissions
-   Weak access controls
-   Privileged accounts
-   Operating-system weaknesses

**MITRE ATT&CK:** `TA0004`

------------------------------------------------------------------------

## Execution

**Goal:** Execute malicious code or commands.

Examples:

-   Malicious scripts
-   Remote commands
-   Trojans
-   Scheduled tasks
-   Command interpreters

**MITRE ATT&CK:** `TA0002`

------------------------------------------------------------------------

## Credential Access

**Goal:** Obtain credentials that can be used for access or privilege
escalation.

Attackers may target:

-   Passwords
-   Password hashes
-   Tokens
-   Authentication material
-   Stored credentials

**MITRE ATT&CK:** `TA0006`

### Why credentials matter

Valid credentials can allow attackers to access systems while appearing
more like legitimate users.

------------------------------------------------------------------------

## Lateral Movement

**Goal:** Move from one compromised system to another.

Example:

``` text
Workstation A
     |
     v
Server A
     |
     v
Database Server
```

The attacker uses access obtained from one system to reach additional
systems.

**MITRE ATT&CK:** `TA0008`

### Important distinction

-   **Pivoting:** using a compromised system/network as a route to reach
    another system.
-   **Lateral Movement:** moving between systems inside the environment.

------------------------------------------------------------------------

# 6. The "Out" Section

The final part of the UKC focuses on obtaining, removing, and acting on
valuable information.

------------------------------------------------------------------------

## Collection

**Goal:** Gather information relevant to the attacker's objective.

Potential targets:

-   Documents
-   Databases
-   Emails
-   Credentials
-   Financial information
-   Personal information
-   Sensitive business data

**MITRE ATT&CK:** `TA0009`

------------------------------------------------------------------------

## Exfiltration

**Goal:** Remove stolen data from the target environment.

Attackers may attempt to:

-   Transfer data to external infrastructure
-   Compress stolen files
-   Encrypt data
-   Hide the transfer inside normal-looking traffic

**MITRE ATT&CK:** `TA0010`

### Easy distinction

``` text
Collection  = Gather the data
Exfiltration = Move the data OUT
```

------------------------------------------------------------------------

## Impact

**Goal:** Manipulate, interrupt, or destroy systems/data.

Examples:

-   Ransomware
-   Data destruction
-   Service disruption
-   Denial of Service
-   Data manipulation
-   Account disruption

**MITRE ATT&CK:** `TA0040`

### CIA connection

Impact commonly affects:

-   **Integrity** --- data is modified or destroyed.
-   **Availability** --- services/systems become unavailable.
-   **Confidentiality** may be affected earlier through data theft.

------------------------------------------------------------------------

## Objectives

**Goal:** Achieve the attacker's strategic objective.

The objective depends on the attacker.

Examples:

-   Financial gain
-   Data theft
-   Espionage
-   Disruption
-   Extortion
-   Destruction
-   Reputational damage

### Important point

The objective is the **reason behind the attack**, while the previous
phases describe the actions used to achieve it.

------------------------------------------------------------------------

# 7. Threat Modelling

Threat modelling is a process used to identify risks and improve the
security of a system.

It broadly involves:

1.  **Identify systems and applications**
    -   What needs to be protected?
    -   What function does it provide?
    -   Is it critical to operations?
    -   Does it contain sensitive information?
2.  **Assess vulnerabilities**
    -   What weaknesses exist?
    -   How could they potentially be exploited?
3.  **Create a security plan**
    -   Decide how identified risks and vulnerabilities should be
        addressed.
4.  **Prevent recurrence**
    -   Implement policies and processes that reduce the chance of the
        vulnerability happening again.

Examples include:

-   Secure Software Development Lifecycle (SDLC)
-   Phishing-awareness training
-   Security policies
-   Access controls
-   Patch management

### Threat-modelling frameworks

Some commonly used frameworks include:

-   **STRIDE**
-   **DREAD**
-   **CVSS**

------------------------------------------------------------------------

# 8. UKC vs Traditional Cyber Kill Chain

The traditional Lockheed Martin Cyber Kill Chain has fewer phases.

The **Unified Kill Chain** provides a more detailed model by including:

-   Social engineering
-   Persistence
-   Defence evasion
-   Pivoting
-   Discovery
-   Privilege escalation
-   Credential access
-   Lateral movement
-   Collection
-   Exfiltration
-   Impact
-   Objectives

This makes UKC particularly useful for understanding **post-compromise
activity**.

------------------------------------------------------------------------

# 9. Quick Attack Flow

A simplified way to remember the UKC:

``` text
RECON
  ↓
WEAPONIZE
  ↓
DELIVER
  ↓
SOCIAL ENGINEER
  ↓
EXPLOIT
  ↓
PERSIST
  ↓
EVADE DEFENCES
  ↓
C2
  ↓
PIVOT
  ↓
DISCOVER
  ↓
PRIVILEGE ESCALATION
  ↓
EXECUTE
  ↓
CREDENTIAL ACCESS
  ↓
LATERAL MOVEMENT
  ↓
COLLECT
  ↓
EXFILTRATE
  ↓
IMPACT
  ↓
OBJECTIVES
```

> **Note:** Real-world attacks are not always strictly linear. Attackers
> can revisit phases, skip phases, or perform multiple phases
> simultaneously.

------------------------------------------------------------------------

# 10. SOC Analyst Perspective

The UKC is useful for analysing incidents because the same attack can be
viewed as a sequence of behaviours.

### Example

Suppose a SOC detects:

``` text
1. Phishing email
2. User opens attachment
3. Malware executes
4. Persistence created
5. C2 connection established
6. Internal hosts discovered
7. Credentials accessed
8. Lateral movement
9. Sensitive files collected
10. Data exfiltrated
```

Possible UKC mapping:

  Activity                        UKC Phase
  ------------------------------- --------------------------
  Reconnaissance                  Reconnaissance
  Malicious attachment prepared   Weaponization
  Email sent                      Delivery
  User manipulated                Social Engineering
  Malware execution               Exploitation / Execution
  Persistence mechanism           Persistence
  Hiding activity                 Defence Evasion
  Beaconing                       Command & Control
  Accessing internal network      Pivoting
  Network enumeration             Discovery
  Obtaining admin rights          Privilege Escalation
  Stealing passwords              Credential Access
  Moving to another host          Lateral Movement
  Finding sensitive files         Collection
  Sending files outside           Exfiltration
  Encrypting/destroying systems   Impact
  Attacker's final goal           Objectives

------------------------------------------------------------------------

# 11. Important Exam / Room Answers

### What is the technical term for a piece of software or hardware in IT?

**Asset**

### What is the Unified Kill Chain?

A framework describing the phases and techniques an attacker can use
throughout a cyber attack.

### How many phases are in the Unified Kill Chain?

**18**

### When was the Unified Kill Chain released?

**2017**

### What is a tactic to gain a foothold using emails?

**Social Engineering** / phishing-based social engineering.

### If suspicious data is being sent outside the network, which UKC phase?

**Exfiltration**

### If personally identifiable information is publicly released, which CIA property is affected?

**Confidentiality**

------------------------------------------------------------------------

# 12. High-Value Differences to Remember

  Concept                Remember it as
  ---------------------- ---------------------------------
  Reconnaissance         Find information
  Weaponization          Prepare attack
  Delivery               Send attack
  Social Engineering     Trick the human
  Exploitation           Abuse weakness
  Persistence            Stay inside
  Defence Evasion        Hide
  C2                     Communicate/control
  Pivoting               Use one system to reach another
  Discovery              Learn the environment
  Privilege Escalation   Gain more privileges
  Execution              Run code
  Credential Access      Steal credentials
  Lateral Movement       Move between hosts
  Collection             Gather data
  Exfiltration           Take data out
  Impact                 Damage/disrupt
  Objectives             Achieve the goal

------------------------------------------------------------------------

## 13. MITRE ATT&CK Tactic Mapping

Some UKC phases directly reference MITRE ATT&CK tactics:

``` text
Reconnaissance       → TA0043
Resource Development → TA0042 (related concept)
Initial Access        → TA0001
Execution             → TA0002
Persistence           → TA0003
Privilege Escalation  → TA0004
Defence Evasion       → TA0005
Credential Access     → TA0006
Discovery             → TA0007
Lateral Movement      → TA0008
Collection             → TA0009
Command & Control     → TA0011
Exfiltration          → TA0010
Impact                → TA0040
```

> UKC and MITRE ATT&CK are related but **not identical frameworks**. UKC
> describes an attack lifecycle, while ATT&CK provides a detailed
> knowledge base of adversary tactics and techniques.

------------------------------------------------------------------------

# 14. One-Line Revision

**Recon → Weaponize → Deliver → Trick → Exploit → Persist → Evade → C2 →
Pivot → Discover → Escalate → Execute → Steal Credentials → Move
Laterally → Collect → Exfiltrate → Impact → Achieve Objectives**

------------------------------------------------------------------------

## References

-   TryHackMe --- Unified Kill Chain room
-   Paul Pols --- Unified Kill Chain
-   MITRE ATT&CK
