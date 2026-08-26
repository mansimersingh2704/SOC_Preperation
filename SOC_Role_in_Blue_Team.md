# SOC Role in Blue Team

## 1. Blue Team

The **Blue Team** is responsible for the defensive side of cybersecurity.

Its main goal is to **prevent, detect, investigate and respond to cyber attacks**.

Typical Blue Team activities:

* Monitoring security alerts
* Analyzing logs
* Detecting suspicious activity
* Investigating incidents
* Responding to attacks
* Improving security controls

---

## 2. Security Hierarchy

A company's security structure can contain different levels:

### CEO / Executives

Focus on overall business objectives and risk.

### CISO

**Chief Information Security Officer**

* Leads the organization's security program
* Makes major cybersecurity decisions
* Reports security risks to management

### Security Managers

Examples:

* SOC Manager
* Red Team Lead

They manage security teams and make sure operations run effectively.

### Technical Roles

Examples:

* SOC Analyst
* SOC Engineer
* GRC Specialist
* Penetration Tester

These roles perform hands-on security tasks.

---

## 3. Security Departments

### Red Team

The offensive security team.

Responsibilities:

* Penetration testing
* Finding vulnerabilities
* Simulating attacks
* Testing security defenses

### Blue Team

The defensive security team.

Responsibilities:

* Detect attacks
* Monitor systems
* Investigate alerts
* Respond to incidents

### GRC Team

**Governance, Risk and Compliance**

Responsibilities:

* Security policies
* Risk management
* Compliance
* Security standards and regulations

Example: **PCI DSS** compliance.

---

# 4. Security Operations Center (SOC)

A **SOC (Security Operations Center)** is the central team responsible for monitoring and defending an organization's environment.

SOC is often considered the **first line of defense**.

### Main SOC activities

* Monitor security alerts
* Collect and analyze logs
* Investigate suspicious activity
* Create detection rules
* Respond to security incidents
* Work with IT and other teams
* Document and report incidents

---

## 5. SOC Roles

### SOC L1 Analyst

The entry-level SOC role and usually the first person to investigate an alert.

Responsibilities:

* Monitor alerts
* Perform initial investigation
* Check logs and indicators
* Identify false positives
* Escalate complex incidents to L2

**Typical flow:**

`Alert → Initial Investigation → Triage → Escalate`

### SOC L2 Analyst

Handles more advanced investigations.

Responsibilities:

* Perform deeper analysis
* Investigate complex attacks
* Analyze multiple sources of evidence
* Support L1 analysts

### SOC Engineer

Responsible for the technical infrastructure used by the SOC.

Examples:

* SIEM
* EDR
* Detection rules
* Log collection

They configure and maintain security tools.

### SOC Manager

Manages the SOC team.

Responsibilities:

* Team management
* Incident coordination
* Reporting
* SOC operations

---

# 6. CIRT / CSIRT / CERT

These terms generally refer to teams responsible for responding to significant cyber incidents.

**CIRT = Cyber Incident Response Team**

They are usually involved when an incident becomes serious or requires specialized response.

Responsibilities:

* Handle major incidents
* Perform digital forensics
* Identify hidden threats
* Investigate breaches
* Recover compromised systems

Think of CIRT as the **cybersecurity firefighters**.

---

# 7. Specialized Blue Team Roles

Large organizations may have specialized defensive roles.

### Digital Forensics Analyst

Investigates evidence from systems, disks and memory after an incident.

### Threat Intelligence Analyst

Collects and analyzes information about threat actors, malware and attack techniques.

### AppSec Engineer

Focuses on securing applications and the software development lifecycle.

### Threat Analyst

Studies threats and attacker behavior to improve detection.

### DevSecOps

Integrates security into the software development and deployment process.

### AI Researcher

Researches how AI can be used for security and how attackers may abuse AI.

---

# 8. Internal SOC vs MSSP

### Internal SOC

An organization operates its own SOC.

Example:

`Bank → Internal SOC → Protects Bank Systems`

Advantages:

* Deep knowledge of the organization
* Focuses on one environment
* Direct access to internal teams

### MSSP

**Managed Security Service Provider**

An MSSP provides cybersecurity services to other organizations.

Example:

`MSSP → Customer 1`
`    → Customer 2`
`    → Customer 3`

MSSP analysts may work with many customers and different security tools.

### Key Difference

**Internal SOC:** Protects its own organization.

**MSSP:** Provides security services to multiple organizations.

---

# 9. SOC Career Path

A common SOC progression is:

`SOC L1 → SOC L2 → Senior Analyst / Specialized Role`

Later, you can move into areas such as:

* Incident Response
* Threat Intelligence
* Digital Forensics
* Detection Engineering
* SOC Engineering
* Security Management

---

# 10. Important SOC Mindset

### Learn From Every Alert

Understand **why** an alert was triggered and what caused it.

### Think Like an Attacker

Ask:

> "How would an attacker perform this?"

This helps understand suspicious behavior.

### Verify Everything

Don't blindly trust alerts.

Always validate:

* Logs
* IP addresses
* Domains
* Users
* Processes
* Timestamps
* Other IOCs

### Get Involved in Incidents

Real incidents provide valuable experience and improve investigation skills.

---

# 11. Final Challenge

In the final challenge, different security incidents had to be assigned to the appropriate security professionals.

Examples included:

* Firewall brute-force alert → SOC Analyst
* Phishing malware → Threat/Incident investigation
* Ransomware → Incident Response
* PCI DSS audit → GRC
* Vulnerability checking → Security/technical role
* Threat group analysis → Threat Intelligence

**Flag:**

`THM{trysecureme_is_secured!}`

---

## Key Takeaways

* **Blue Team = Defensive Security**
* **SOC = Security Operations Center**
* **L1 = Initial alert investigation**
* **L2 = Advanced investigation**
* **SOC Engineer = Security tools and infrastructure**
* **CIRT = Major incident response**
* **GRC = Governance, Risk & Compliance**
* **MSSP = Security services for multiple organizations**
* **Red Team = Offensive Security**
* **Blue Team = Defensive Security**

**TryHackMe Room:** SOC Role in Blue Team
**Status:** ✅ Completed
