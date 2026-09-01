# SOC Workbooks, Identity & Asset Lookup

## 1. Identity Inventory

**Identity Inventory** = catalogue of corporate identities such as:

- User accounts
- Service accounts
- Machine accounts
- Privileges/roles
- Contacts
- Relationships with the company

### Common Identity Sources

| Source | Examples | Use |
|---|---|---|
| **Active Directory** | On-prem AD, Entra ID | Identity + asset information |
| **SSO Providers** | Okta, Google Workspace | Manage/search users |
| **HR Systems** | BambooHR, SAP, HiBob | Employee information |
| **Custom** | CSV, Excel | Organisation-specific records |

### SOC Use

Identity information helps answer:

- Who is the user?
- What is their role?
- Is the activity expected?
- Does the user have privileged access?

---

## 2. Asset Inventory

**Asset Inventory** = list of computing resources in an organisation.

Assets may include:

- Servers
- Workstations
- Laptops
- Network devices
- Applications
- Software
- Cloud resources

### Common Asset Sources

| Source | Examples | Use |
|---|---|---|
| **Active Directory** | On-prem AD, Entra ID | Identity + asset information |
| **SIEM / EDR** | Elastic, CrowdStrike | Information about monitored hosts |
| **MDM** | Microsoft Intune, Jamf MDM | List/manage devices |
| **Custom** | CSV, Excel | Organisation-specific inventory |

### SOC Use

Asset information helps determine:

- What device is involved?
- Which subnet/network is it on?
- Is the asset critical?
- Is the activity expected for that asset?

---

# 3. Identity + Asset Context

During alert investigation, combine **identity + asset information**.

```text
Alert
 ↓
Who? → Identity Inventory
 ↓
What host? → Asset Inventory
 ↓
Where? → Network Diagram
 ↓
Expected or suspicious?
```

Example questions:

- Who owns/uses the affected system?
- What is the user's role?
- Is the account privileged?
- What type of host is involved?
- Which network/subnet contains it?
- Is the activity normal for that user/asset?

---

# 4. Network Diagrams

A **network diagram** is a visual representation of network locations, subnets, systems, and their connections.

It helps an L1 analyst understand suspicious network activity in context.

### Example Investigation

```text
External IP
    ↓
Firewall
    ↓
Internal Host
    ↓
Subnet
    ↓
Other Internal Systems
```

A network diagram can help determine:

- What service is running on a suspicious port?
- Which subnet an IP belongs to?
- Whether communication between subnets is expected.
- Possible attack paths.
- Whether an attacker is moving between network segments.

### Example Attack Path

```text
Threat Actor
    ↓
VPN Subnet
    ↓
Firewall
    ↓
Database Subnet
    ↓
Office Subnet
```

A suspicious actor may attempt to move from an externally accessible/VPN environment toward more sensitive internal systems.

---

# 5. SOC Workbooks

**SOC Workbook** (also called a **playbook, runbook, or workflow**) = structured steps for investigating and remediating a specific threat efficiently and consistently.

### Why Workbooks?

L1 analysts may not be expected to handle every possible attack scenario perfectly.

Workbooks:

- Provide standard investigation steps.
- Reduce missed investigation steps.
- Make investigations consistent.
- Help L1 analysts follow procedures.
- Can contain links to required resources/tools.

---

# 6. Workbook Structure

A workbook can be divided into three logical stages:

```text
Enrichment
    ↓
Investigation
    ↓
Escalation
```

### 1. Enrichment
Collect context before making a decision.

Examples:
- Identity information
- Asset information
- Threat intelligence
- IP/domain reputation
- Relevant historical data

### 2. Investigation
Use the gathered context and security logs to determine whether activity is expected or malicious.

Examples:
- SIEM searches
- EDR investigation
- Login history
- Process activity
- Network activity
- User confirmation

### 3. Escalation
If the activity is suspicious/confirmed or requires additional expertise:

- Write an alert report.
- Escalate to L2.
- Contact the affected user when required.
- Start further response according to the procedure.

---

# 7. Workbook Example — Unusual Login

Typical investigation logic:

```text
Receive Alert
     ↓
Find expected user location
     ↓
Check login IP in Threat Intelligence
     ↓
Check IP with anonymisation detection
     ↓
Confirm whether login IP is malicious
     ↓
Review actions after login
     ↓
Check login timeline/history
     ↓
Check VPN / unusual location
     ↓
Contact user if needed
     ↓
False Positive → Close
True Positive / Suspicious → Escalate
```

### Useful Investigation Questions

- Is the login location expected?
- Is the IP associated with VPN?
- Is the IP suspicious/malicious?
- Is anonymisation involved?
- What actions occurred after login?
- Does the timeline make sense?
- Can the user confirm the activity?

---

# 8. Workbook Example — External Email + Script/Binary Attachment

A structured email investigation may include:

```text
Gather Evidence
      ↓
Analyse Email
      ↓
Analyse Attachment
      ↓
Check Recipient / User Context
      ↓
Check Related Login Activity
      ↓
Determine Verdict
      ↓
Document
      ↓
Escalate if Required
```

### Evidence to Gather

- Recipient list
- Sender
- Email headers
- Email body/content
- Attachment
- Attachment indicators
- Sender/domain reputation

### Attachment Analysis

For a script or binary attachment:

- Perform sandbox analysis where available.
- Review file indicators.
- Determine whether the attachment is malicious, ignored, or unexpected.
- Use the results as evidence in the alert report.

### User & Environment Context

- Check the user's identity/role.
- Check whether recipients are expected to receive the email.
- Review relevant login events when required.
- Notify employees through approved channels if the investigation requires it.

---

# 9. Key SOC Workflow

```text
Alert
 ↓
Identity Lookup
 ↓
Asset Lookup
 ↓
Network Context
 ↓
Threat Intelligence
 ↓
SIEM / EDR Investigation
 ↓
Workbook / Runbook
 ↓
Verdict
 ↓
Document
 ↓
Close / Escalate
```

> **Context prevents premature conclusions.** Use identity, asset, network, and security telemetry together.

---

# Quick Revision

```text
IDENTITY INVENTORY
= Who is involved?

ASSET INVENTORY
= What system/device is involved?

NETWORK DIAGRAM
= Where is it and how is it connected?

WORKBOOK
= What investigation steps should I follow?

ENRICHMENT
= Gather context

INVESTIGATION
= Analyse evidence

ESCALATION
= Report + send to L2/response when required
```

### Remember

**Who → What → Where → Context → Evidence → Verdict → Action**
