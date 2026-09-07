# Systems as Attack Vectors

## 1. System Attack Vectors

Attackers can target systems directly without needing the user's involvement.

Common targets:

* Servers
* Laptops/workstations
* Mail servers
* Cloud platforms
* Network infrastructure
* Web applications

The goal may be data theft, ransomware, gaining internal access, botnets or disruption.

---

## 2. Human-Led vs System Attacks

### Human-Led Attacks

Require or manipulate a user.

Examples:

* Malicious USB devices
* Reused/weak passwords
* Downloading malware

### System Attacks

Exploit weaknesses directly in systems.

Examples:

* Software vulnerabilities
* Misconfigurations
* Supply-chain attacks

---

## 3. Vulnerabilities

A **vulnerability** is a security weakness that can be exploited by an attacker.

Once publicly disclosed, vulnerabilities are assigned a **CVE (Common Vulnerabilities and Exposures)** identifier.

### CVE Format

**`CVE-YEAR-NUMBER`**

Example: `CVE-2025-53770`

* **CVE** → Common Vulnerabilities and Exposures
* **2025** → Year the CVE was assigned
* **53770** → Unique identification number

The number length is not fixed. For example:

* `CVE-2017-0144`
* `CVE-2021-34527`
* `CVE-2025-53770`

### Examples

* **EternalBlue** — CVE-2017-0144
* **PrintNightmare** — CVE-2021-34527
* **ToolShell / SharePoint vulnerability** — CVE-2025-53770

### Responding to Vulnerabilities

Primary response:

* Apply the vendor's **patch/update**
* Restrict access
* Apply temporary vendor mitigations
* Block known attack patterns using IPS/WAF
* Monitor for exploitation

---

## 4. Misconfigurations

A **misconfiguration** is an insecure system setup rather than a software bug.

Examples:

* Weak/default passwords
* Unrestricted access
* Exposed services
* Incorrect cloud permissions
* Poor firewall configuration

### Responding to Misconfigurations

* **Penetration testing** — authorized testing to discover security weaknesses
* **Vulnerability scanning** — automated detection of vulnerabilities/misconfigurations
* **Configuration audits** — compare systems against security standards such as CIS Benchmarks

---

## 5. Supply Chain Attacks

A **supply-chain attack** compromises software, libraries, vendors or dependencies that an organization trusts.

Example:
`Compromised dependency → Software update → Customer systems → Attacker access`

Because organizations depend on many third-party components, supply-chain attacks can affect many victims at once.

---

## 6. Mitigation

| Control              | Purpose                              |
| -------------------- | ------------------------------------ |
| Patch Management     | Keep vulnerable systems updated      |
| Network Protection   | Restrict access to trusted users/IPs |
| Antivirus / EDR      | Detect and prevent malware           |
| IT Security Training | Reduce configuration mistakes        |
| Configuration Audits | Identify insecure settings           |

### Key Principle

**Mitigation prevents/reduces attacks → Detection identifies attacks that get through → SOC investigates and responds.**

---

## 7. SOC Analyst Perspective

A SOC analyst should understand:

* Which systems are exposed
* Which vulnerabilities are being exploited
* Whether systems are patched
* Signs of exploitation
* Suspicious network activity
* Misconfigurations
* Third-party/supply-chain risks

### Important Terms

**Vulnerability:** Security weakness in software/system.

**Misconfiguration:** Insecure system setup.

**CVE:** Unique identifier assigned to a publicly known vulnerability.

**Patch:** Software update that fixes a vulnerability.

**Penetration Testing:** Authorized security testing designed to find exploitable weaknesses.

**Supply Chain Attack:** Attack through a trusted third-party component/vendor.

---

## 8. Key Takeaways

* Systems can be attacked without user interaction.
* Vulnerabilities are weaknesses in software or systems.
* Misconfigurations are insecure settings.
* Patch management is a major defensive control.
* Supply-chain attacks abuse trusted third-party software/components.
* SOC analysts detect and investigate attacks that bypass preventive controls.
