# Phishing Analysis

## 1. Overview

Phishing analysis is the process of examining a suspicious email to determine:

- Whether the message is legitimate or malicious
- Who actually sent it
- Who it was intended for
- What infrastructure was used
- Whether URLs or attachments are malicious
- What indicators of compromise (IOCs) should be reported
- What action should be taken with the email

A good investigation starts with **artifact collection** and then moves toward **reputation, behavioral, and malware analysis**.

---

## 2. Email Artifacts

### Header Artifacts

Important fields to collect from an email header:

| Artifact | What to check |
|---|---|
| **Sender email address** | Is the sender/domain legitimate? |
| **Sender IP address** | Where did the message originate? |
| **Subject** | Does it create urgency or request an action? |
| **Recipient** | Who received the message? Check To/CC/BCC where available |
| **Reply-To** | Does replying go to a different/suspicious address? |
| **Date/Time** | When was the message sent? |

### Body Artifacts

Look for:

- **URLs / hyperlinks** — extract every link and inspect the real destination.
- **Shortened URLs** — expand them before deciding whether they are safe.
- **Attachments** — record filename, extension, and type.
- **Attachment hashes** — calculate a hash such as SHA-256 for threat-intelligence lookups.
- **Embedded content** — inspect buttons, images, redirects, and suspicious HTML.

> Never assume the visible text of a link is the actual destination.

---

# 3. Mail Header Analysis

Email clients often expose only a small amount of header information. For deeper investigation, obtain the **full/raw email header**.

### Useful tools

- **Google Admin Toolbox – Messageheader**
- **Message Header Analyzer**

These tools can help identify:

- Sending IP
- Mail servers involved
- Routing path
- Timestamps
- Authentication results
- SPF/DKIM information
- Possible header inconsistencies

### Basic investigation flow

```text
Email
  ↓
Get full/raw headers
  ↓
Inspect From / Reply-To / Received
  ↓
Identify originating IP
  ↓
Check SPF / DKIM / DMARC
  ↓
Trace mail-routing path
  ↓
Investigate IP/domain reputation
```

### Important header fields

#### From
The displayed sender can be spoofed. Do not trust the `From` field alone.

#### Reply-To
A particularly useful phishing indicator.

Example:

```text
From: support@legitimate-company.com
Reply-To: attacker@random-domain.com
```

The visible sender may look legitimate while replies are redirected elsewhere.

#### Received
`Received` headers show the mail servers through which the message travelled.

Look for:

- External IP addresses
- Unexpected countries/regions
- Suspicious hosts
- Unusual routing
- Timestamp inconsistencies

#### SPF
Checks whether the sending server is authorized to send mail for a domain.

#### DKIM
Provides cryptographic authentication of selected email headers/content.

#### DMARC
Uses domain alignment and SPF/DKIM results to help protect against spoofing.

---

# 4. IP and URL Reputation Analysis

Once indicators are extracted, investigate them independently.

## IPInfo

IPInfo can provide information about an IP address such as:

- Approximate geographic location
- ASN
- Organization
- Hosting/network information

Do not treat geolocation alone as proof that an email is malicious.

---

## URLScan

URLScan can be used to investigate websites without directly browsing them in the normal way.

Useful information can include:

- Page screenshot
- Redirect chain
- Domains contacted
- IP addresses
- Technologies
- Network requests
- Indicators associated with the page

### Safe workflow

```text
Suspicious URL
      ↓
URLScan
      ↓
Observe redirects + contacted domains
      ↓
Extract IP/domain indicators
      ↓
Check reputation
```

---

## Talos Intelligence

Cisco Talos Intelligence can be used to investigate the reputation of:

- IP addresses
- Domains
- Networks

Check whether an indicator has a history of suspicious or malicious activity.

---

# 5. URL Extraction and Obfuscation

Attackers frequently hide URLs using:

- URL shorteners
- HTML links
- URL encoding
- Redirects
- IP addresses instead of domains
- Look-alike domains
- Subdomains
- Obfuscated strings

### What to compare

```text
Visible link text
        vs
Actual hyperlink destination
```

Example:

```text
Visible:
https://paypal.com

Actual destination:
https://paypa1-example.com/login
```

The second domain is what must be investigated.

### URL encoding

A URL may contain encoded characters.

Useful decoding can reveal:

- Real domain
- Redirect target
- Parameters
- Embedded URLs

---

# 6. Attachment Analysis

Attachments are a major phishing vector.

### Collect

For every suspicious attachment record:

```text
Filename
Extension
File type
SHA-256
MD5 (if required)
Size
Embedded URLs
Macros/scripts
```

### Important extensions

Be especially careful with:

```text
.exe
.dll
.js
.vbs
.ps1
.bat
.cmd
.scr
.docm
.xlsm
.zip
.rar
.pdf
```

A dangerous payload may also be hidden inside an archive or document.

---

# 7. Hashing Attachments

A file hash provides a unique fingerprint that can be searched in threat-intelligence platforms.

Example:

```bash
sha256sum suspicious_file.pdf
```

Windows PowerShell:

```powershell
Get-FileHash .\suspicious_file.pdf -Algorithm SHA256
```

Then search the SHA-256 hash in a trusted malware-analysis/reputation service.

### Why hash?

If the exact file has already been analyzed, a hash lookup may reveal:

- Malware classification
- Detection names
- Previous submissions
- Related indicators
- Vendor detections

---

# 8. Malware Sandboxes

Do **not** open suspicious attachments directly on your normal machine.

Use a controlled malware-analysis sandbox.

Tools covered in the room include:

### ANY.RUN

Interactive sandbox for observing suspicious files and URLs.

Can provide:

- Process activity
- Network connections
- DNS requests
- HTTP requests
- Files created
- Registry activity
- Screenshots
- Indicators of compromise

### Hybrid Analysis

A malware-analysis platform that provides:

- Static analysis
- Dynamic analysis
- Network activity
- Process behavior
- Detection information
- IOC extraction

### Joe Sandbox

Provides deeper static and dynamic analysis of suspicious files and URLs.

---

# 9. Sandbox Analysis Method

A practical sandbox workflow:

```text
Suspicious attachment
        ↓
Calculate SHA-256
        ↓
Check existing reputation
        ↓
If necessary, submit to sandbox
        ↓
Observe execution
        ↓
Collect processes + network activity
        ↓
Extract IOCs
        ↓
Correlate with threat intelligence
        ↓
Document findings
```

### What to observe

#### Process activity
Look for:

- Suspicious child processes
- Office spawning PowerShell/cmd
- Script interpreters
- Persistence attempts

#### Network activity
Look for:

- Suspicious DNS queries
- Unknown domains
- HTTP/HTTPS connections
- IP addresses
- C2-like communication

#### Files
Look for:

- Dropped executables
- Temporary files
- Scripts
- Modified documents

#### Registry
Look for:

- Run keys
- Persistence
- Configuration changes

---

# 10. PhishTool

PhishTool is designed to help security analysts investigate phishing emails.

It can assist with:

- Email artifact extraction
- Header analysis
- URL analysis
- Attachment analysis
- IOC identification
- Investigation workflow
- Case documentation

### Identifying artifacts

Extract:

```text
Sender
Recipient
Subject
Reply-To
Sending IP
URLs
Domains
Attachments
Hashes
```

---

# 11. Further Analysis

After extracting artifacts, correlate them.

Example:

```text
Email
 ↓
Suspicious URL
 ↓
URLScan
 ↓
Redirected domain
 ↓
Domain/IP reputation
 ↓
Threat intelligence
```

For attachments:

```text
Attachment
 ↓
SHA-256
 ↓
VirusTotal / sandbox
 ↓
Malware behavior
 ↓
Network IOCs
 ↓
Threat intelligence
```

The goal is not to rely on a single indicator. Multiple independent indicators provide stronger evidence.

---

# 12. Resolving the Case

After completing the investigation, document the conclusion.

A phishing case should contain:

### Classification

Examples:

```text
Benign
Suspicious
Phishing
Malicious
Spam
```

### Evidence

Document the important indicators:

```text
Sender:
Reply-To:
Originating IP:
URL:
Domain:
Attachment:
SHA-256:
Sandbox verdict:
Observed behavior:
```

### Recommended SOC action

Depending on the evidence:

- Quarantine the email
- Block malicious domains/IPs
- Block malicious hashes
- Search for other recipients
- Identify additional affected users
- Reset credentials if compromise is suspected
- Escalate to incident response when necessary

---

# 13. TryHackMe Lab — Phishing Analysis

## Task: Identify Phishing Artifacts

The room demonstrates a phishing email investigation from the perspective of a SOC analyst.

The investigation involves:

1. Reviewing the email body
2. Inspecting headers
3. Finding the real recipient
4. Extracting the sender IP
5. Investigating the return-path/domain
6. Inspecting shortened URLs
7. Analyzing malicious attachments
8. Using sandbox reports
9. Identifying malicious infrastructure
10. Writing the final conclusion

---

# 14. Lab Example — Account on Hold

A phishing email was designed to impersonate **Netflix** and claimed that the recipient's account was on hold.

### Social-engineering indicators

- Brand impersonation
- Payment/billing problem
- Urgent action request
- "Update payment" call to action
- Suspicious sender/recipient details
- External link

### Investigation mindset

Do not simply ask:

> "Does this look like Netflix?"

Instead ask:

```text
Who actually sent it?
Where did it originate?
Where does the button lead?
Who receives replies?
What infrastructure does the URL use?
Are the associated indicators malicious?
```

---

# 15. Lab Example — Malicious PDF

The room uses ANY.RUN to analyze a suspicious payment-themed PDF attachment.

Record:

```text
Classification
Attachment filename
SHA-256
Associated IP
Associated domain
Observed process/network behavior
```

The important lesson is that a document that appears harmless can still trigger malicious behavior when opened.

---

# 16. Lab Example — Malicious Excel Attachment

The room also demonstrates a suspicious Excel attachment.

Investigation includes:

- ANY.RUN classification
- Excel filename
- SHA-256 hash
- Associated IP address
- Malicious domain
- Exploited vulnerability

### CVE-2017-11882

This vulnerability is associated with Microsoft Office's Equation Editor and can be exploited through malicious Office documents.

For a SOC analyst, the important workflow is:

```text
Malicious document
      ↓
Sandbox execution
      ↓
Observed exploit behavior
      ↓
Identify CVE / technique
      ↓
Extract IOCs
      ↓
Correlate with threat intelligence
```

---

# 17. Practical SOC Phishing Checklist

Use this checklist whenever a suspicious email is reported.

## Email

- [ ] Check sender address
- [ ] Check display name
- [ ] Check Reply-To
- [ ] Check recipient
- [ ] Check subject
- [ ] Check date/time
- [ ] Obtain full headers

## Headers

- [ ] Identify originating IP
- [ ] Inspect Received chain
- [ ] Check SPF
- [ ] Check DKIM
- [ ] Check DMARC
- [ ] Look for spoofing/misalignment

## Body

- [ ] Read for urgency/social engineering
- [ ] Extract every URL
- [ ] Compare visible URL with actual destination
- [ ] Expand shortened URLs safely
- [ ] Identify suspicious domains

## Attachments

- [ ] Record filename
- [ ] Record extension
- [ ] Calculate SHA-256
- [ ] Check reputation
- [ ] Submit to a sandbox if required
- [ ] Extract network/process IOCs

## Threat Intelligence

- [ ] IP reputation
- [ ] Domain reputation
- [ ] URL reputation
- [ ] Hash reputation
- [ ] Sandbox verdict
- [ ] Correlate multiple indicators

## Final Case

- [ ] Classify the email
- [ ] Record evidence
- [ ] Record IOCs
- [ ] Determine affected users
- [ ] Recommend containment
- [ ] Escalate if compromise is suspected

---

# 18. Key Takeaways

1. **Never trust the visible sender alone.**
2. **Reply-To can reveal where an attacker wants responses to go.**
3. **Raw email headers provide much more information than the normal email view.**
4. **The real URL destination matters more than the text displayed in the email.**
5. **Hash attachments before submitting or searching them.**
6. **Do not execute suspicious files on your normal machine.**
7. **Use sandboxes such as ANY.RUN, Hybrid Analysis, or Joe Sandbox for controlled analysis.**
8. **Correlate IPs, domains, URLs, hashes, and behavioral evidence.**
9. **A single reputation result should not be the entire investigation.**
10. **Document the evidence and IOCs clearly so another analyst can reproduce the investigation.**

---

## Useful Investigation Mental Model

```text
          SUSPICIOUS EMAIL
                 │
       ┌─────────┴─────────┐
       ↓                   ↓
    HEADERS              BODY
       │                   │
       ↓             ┌─────┴─────┐
 Sender/IP            URLs     Attachments
 Reply-To              │           │
 Received               ↓           ↓
 SPF/DKIM/DMARC      URL Intel    SHA-256
       │              /Sandbox      │
       └───────┬──────────┘          ↓
               ↓                 Malware
        THREAT INTELLIGENCE      Sandbox
               │                    │
               └────────┬───────────┘
                        ↓
                 CORRELATE IOCs
                        ↓
                  FINAL VERDICT
                        ↓
              CONTAIN / ESCALATE
```

> **Core SOC principle:** Collect evidence first, investigate safely, correlate indicators, and document the reasoning behind the final classification.
