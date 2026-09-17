# TryHackMe — Phishing Analysis

> Notes for the **TryHackMe Phishing Analysis** room.  
> Focus: email anatomy, delivery, headers, body/HTML analysis, attachments, phishing techniques, and investigation workflow.

---

## 1. What is Phishing?

**Phishing** is a social-engineering attack where an attacker sends a deceptive message designed to make a victim:

- Click a malicious link
- Open a malicious attachment
- Reveal credentials or sensitive information
- Transfer money
- Perform an unauthorized action

### Spam vs Phishing

| Spam | Phishing |
|---|---|
| Usually unsolicited bulk email | Deliberately deceptive/social-engineering message |
| Often advertising or unwanted content | Attempts to steal information or cause harm |
| May be low risk | Can provide an attacker with initial access |

---

# 2. Anatomy of an Email Address

Example:

```text
david@tryhackme.com
```

An email address has three important parts:

```text
david       @       tryhackme.com
username           domain
```

### Username

Identifies the mailbox/user.

```text
david
```

### `@`

Separates the username from the domain.

### Domain

Identifies the mail domain/server responsible for receiving the message.

```text
tryhackme.com
```

### Easy analogy

Think of an email address like a postal address:

- **Domain** → building/street
- **Username** → specific person/mailbox inside that location

---

# 3. Email Delivery

Several protocols work together when an email travels from sender to recipient.

## SMTP — Simple Mail Transfer Protocol

Used to **send/transfer email** between a mail client and mail server, and between mail servers.

```text
Sender
   |
   | SMTP
   v
Sender Mail Server
   |
   | SMTP
   v
Recipient Mail Server
```

## POP3 — Post Office Protocol 3

Used to download emails to a device.

Typical characteristics:

- Emails are downloaded to a device
- Messages are commonly stored locally
- Traditionally associated with single-device access
- Messages may be removed from the server after download, depending on configuration

## IMAP — Internet Message Access Protocol

Used to access and synchronize email while keeping messages on the server.

Typical characteristics:

- Emails remain on the server
- Multiple devices can access the same mailbox
- Read/sent state can synchronize across devices
- Useful when the same mailbox is used from multiple devices

### Quick memory trick

```text
SMTP = Send
POP3 = Pull/download
IMAP = Internet/server synchronization
```

---

# 4. Email Journey

A simplified email journey:

```text
1. User sends email
        |
        | SMTP
        v
2. Sender mail server
        |
        | DNS query
        v
3. DNS finds recipient mail server
        |
        v
4. Email is delivered to recipient mail server
        |
        v
5. Recipient mail client checks mailbox
        |
        | POP3 / IMAP
        v
6. Email is retrieved
```

### Step-by-step

1. Sender's email client sends the message using **SMTP**.
2. The sending mail server uses **DNS** to determine where the recipient's domain receives mail.
3. DNS provides the relevant mail-server information.
4. The message is transferred to the recipient's mail server.
5. The recipient's email client connects to the mailbox.
6. The message is retrieved using **POP3 or IMAP**, depending on the mailbox configuration.

---

# 5. Email Structure

An email can broadly be divided into:

```text
Email
├── Headers
└── Body
```

## Email Headers

Headers contain metadata about the message and its delivery.

Important fields include:

### From

The sender's address/display name.

```text
From: Security Team <security@example.com>
```

**Important:** Do not trust the display name alone. Check the actual email address and domain.

### To

The intended recipient.

```text
To: user@example.com
```

### Reply-To

Specifies where replies should be sent.

```text
Reply-To: attacker@example.net
```

This can be suspicious if it differs from the expected sender.

### Subject

The email subject.

```text
Subject: Urgent Account Verification
```

### Date

Timestamp associated with the email.

---

# 6. Viewing the Raw Email Source

Normal email clients hide much of the technical information.

For investigation, view the **Message Source / raw email**.

In Thunderbird:

```text
View → Message Source
```

Shortcut:

```text
Ctrl + U
```

Raw source can reveal:

- Full headers
- Received path
- Originating IP
- Authentication results
- MIME information
- HTML source
- Encoded attachments
- URLs and domains

---

# 7. Important Email Headers for Phishing Analysis

## Received

`Received` headers show the path a message took through mail servers.

Example concept:

```text
Received: from mail.example.com
          by mx.example.net
```

Multiple `Received` headers can help reconstruct the delivery path.

---

## X-Originating-IP

May contain the IP address associated with the originating client/server.

Example:

```text
X-Originating-IP: [192.0.2.10]
```

### Investigation

You can:

1. Extract the IP
2. Defang it if necessary
3. Research it using approved threat-intelligence/reputation sources
4. Compare it with the claimed sender/domain

> Do not assume that an originating IP automatically identifies the attacker. Proxies, mail services, VPNs, compromised infrastructure, and other intermediaries can affect attribution.

---

## Authentication-Results

Contains results from email authentication checks performed by a receiving mail system.

Common mechanisms:

- **SPF**
- **DKIM**
- **DMARC**

Example:

```text
Authentication-Results: ...
    spf=pass
    dkim=pass
    dmarc=pass
```

A failure can be a useful phishing indicator, but authentication results should be interpreted in context.

---

# 8. SPF, DKIM and DMARC — Quick Revision

## SPF

**Sender Policy Framework**

Checks whether a sending server/IP is authorized to send mail for a domain.

```text
Domain → authorized sending servers/IPs
```

## DKIM

**DomainKeys Identified Mail**

Uses a cryptographic signature to help verify that a message was authorized by the domain and was not altered in transit.

## DMARC

**Domain-based Message Authentication, Reporting & Conformance**

Uses SPF/DKIM alignment and tells receiving systems how to handle messages that fail the domain's authentication policy.

### Memory trick

```text
SPF  → Is this sender/server authorized?
DKIM → Is there a valid cryptographic signature?
DMARC → What should happen when authentication/alignment fails?
```

---

# 9. Email Body

The email body contains the actual message content.

It may be:

- Plain text
- HTML

HTML emails can contain:

- Hyperlinks
- Images
- Forms
- Tracking content
- External resources
- Obfuscated or hidden content

---

# 10. HTML Email Analysis

A normal-looking button may actually contain a malicious URL.

Example:

```html
<a href="https://evil.example/login">
    Click Here
</a>
```

The visible text:

```text
Click Here
```

does **not** tell you where the link really goes.

### What to inspect

Look for:

```html
href="..."
src="..."
action="..."
```

Also search the raw HTML for:

```text
http://
https://
www.
href=
src=
```

---

# 11. Safe Analysis: Defanging

When handling suspicious links/IPs, **defang** them so they cannot be accidentally clicked.

### URL

Original:

```text
http://www.suspiciousdomain.com
```

Defanged:

```text
hxxp://www[.]suspiciousdomain[.]com
```

### IP

Original:

```text
192.168.1.10
```

Defanged:

```text
192[.]168[.]1[.]10
```

### Why defang?

Defanging makes URLs, domains, and IP addresses non-clickable and reduces the chance of accidental interaction.

### Common replacements

```text
http://  → hxxp://
https:// → hxxps://
.        → [.]
@        → [@]
```

---

# 12. Attachments and MIME

Emails can contain attachments such as:

- PDF
- DOC/DOCX
- XLS/XLSX
- ZIP
- Images
- Executables

The raw email uses **MIME** to describe different parts of the message.

Important headers:

```text
Content-Type
Content-Disposition
Content-Transfer-Encoding
```

---

## Content-Type

Describes the type of content.

Example:

```text
Content-Type: application/pdf
```

This indicates PDF content.

---

## Content-Disposition

Can indicate that the content is an attachment.

Example:

```text
Content-Disposition: attachment;
filename="document.pdf"
```

The `filename` value can reveal the attachment name.

---

## Content-Transfer-Encoding

Shows how the content has been encoded for email transport.

Example:

```text
Content-Transfer-Encoding: base64
```

This means the attachment data is Base64 encoded.

---

# 13. Base64 Attachments

A raw email may contain a large block such as:

```text
JVBERi0xLj...
```

This can be Base64-encoded PDF data.

Conceptually:

```text
PDF
 ↓
Base64 Encode
 ↓
Email
 ↓
Extract Base64
 ↓
Base64 Decode
 ↓
PDF
```

### Important

When decoding, copy **only the Base64 data**.

Do NOT include:

```text
Content-Type:
Content-Disposition:
Content-Transfer-Encoding:
MIME boundary
```

For example:

```text
------boundary123--
```

is a MIME boundary, not part of the Base64 payload.

---

# 14. CyberChef for Decoding

Useful CyberChef operations:

```text
From Base64
```

For a PDF payload, the workflow is:

```text
Raw Email
   ↓
Extract Base64 payload
   ↓
CyberChef → From Base64
   ↓
Save/inspect decoded file
```

If necessary, use a PDF rendering/analysis operation after decoding.

### Practical troubleshooting

If you get:

```text
Invalid Base64
```

check:

- Did you copy the entire Base64 payload?
- Did you miss the first characters?
- Did you include MIME boundaries?
- Did you include header text?
- Did formatting add unwanted characters?

---

# 15. Common Phishing Types

## Spam

Unsolicited messages sent to many recipients, often in bulk.

---

## Phishing

A deceptive message designed to trick a victim into an action such as clicking a link, opening an attachment, or revealing information.

---

## Spear Phishing

A targeted phishing attack aimed at a specific individual or organization.

Usually more personalized than generic phishing.

---

## Whaling

A targeted attack against a high-value individual, such as:

- CEO
- Executive
- Senior manager
- Other high-privilege person

---

## Smishing

Phishing through **SMS/text messages**.

```text
SMS + Phishing = Smishing
```

---

## Vishing

Phishing through **voice/phone calls**.

```text
Voice + Phishing = Vishing
```

---

## Clone Phishing

A legitimate message is copied/cloned and modified so that the replacement content is malicious.

---

## Business Email Compromise (BEC)

BEC involves compromising or abusing a legitimate business email account or identity to trick employees into fraudulent or unauthorized actions.

Common objectives can include:

- Fraudulent payments
- Invoice manipulation
- Credential theft
- Sensitive information disclosure

### Memory

```text
BEC = compromised/abused business email + social engineering + fraud
```

---

# 16. Anatomy of a Phishing Email

Common indicators highlighted in the room include:

### 1. Spoofed From Address

The sender may impersonate a legitimate organization.

Example:

```text
Display name: Home Depot
Actual address: support@teckbe.com
```

The display name looks legitimate, but the domain does not necessarily match the claimed organization.

---

### 2. Urgent Subject

Examples:

```text
URGENT: Your account will be closed
Immediate action required
Password expires today
```

Urgency can pressure victims into acting without verification.

---

### 3. Brand Impersonation

Attackers may imitate:

- Company logos
- Colors
- Email templates
- Support messages
- Account notifications

---

### 4. Suspicious Links

The visible text can hide the real destination.

Always inspect the actual URL.

---

### 5. Grammar/Spelling/Formatting Issues

Poor wording, unusual formatting, or inconsistent branding can be indicators.

However, polished phishing emails can also contain none of these obvious errors.

---

### 6. Malicious Attachments

Be cautious with unexpected:

```text
.exe
.zip
.docm
.xlsm
.pdf
.html
```

and other attachment types.

File type alone is not enough to determine whether an attachment is malicious.

---

### 7. Credential Requests

Be suspicious of unsolicited requests for:

- Passwords
- MFA codes
- Banking information
- Recovery codes
- Personal information

---

# 17. Phishing Investigation Workflow

Use a repeatable workflow rather than looking at only one indicator.

```text
1. Identify sender
       ↓
2. Check actual email/domain
       ↓
3. Inspect recipient and Reply-To
       ↓
4. Read subject/body
       ↓
5. Inspect URLs
       ↓
6. Defang URLs/IPs
       ↓
7. View raw source
       ↓
8. Analyze Received headers
       ↓
9. Check X-Originating-IP
       ↓
10. Check Authentication-Results
       ↓
11. Analyze MIME/attachments
       ↓
12. Decode suspicious encoded content safely
       ↓
13. Correlate indicators
       ↓
14. Document findings
```

---

# 18. Questions to Ask During Analysis

## Sender

- Does the display name match the actual sender?
- Is the domain expected?
- Is there a look-alike domain?
- Does `Reply-To` differ from `From`?

## Headers

- What are the `Received` hops?
- Is there an `X-Originating-IP`?
- What does `Authentication-Results` say?
- Do SPF/DKIM/DMARC results make sense?

## Body

- Is the message creating urgency?
- Is it asking for credentials/payment?
- Does the branding match the claimed organization?
- Are there suspicious links?

## Links

- What is the real destination?
- Is the domain suspicious or unrelated?
- Does the displayed text differ from the actual URL?

## Attachments

- What is the filename?
- What is the MIME type?
- What encoding is being used?
- Does the extension match the claimed file type?
- Does decoding reveal additional content?

---

# 19. Useful Thunderbird Workflow

When investigating an `.eml` file:

```text
Email Samples/
       ↓
Open .eml in Thunderbird
       ↓
Check visible email
       ↓
View → Message Source
       ↓
Ctrl + U
       ↓
Search important headers
```

Useful searches:

```text
From:
To:
Reply-To:
Subject:
Date:
Received:
X-Originating-IP:
Authentication-Results:
Content-Type:
Content-Disposition:
Content-Transfer-Encoding:
href=
http://
https://
```

---

# 20. SOC Analyst Perspective

When a phishing email reaches a SOC, the goal is not simply:

> "This looks suspicious."

Instead, collect **observable indicators** and build evidence.

### IOC examples

```text
Sender email
Sender domain
Reply-To address
IP address
URL
Domain
Attachment filename
File hash
MIME type
Authentication results
```

### Example investigation record

```text
Sender:
support@suspicious-domain.example

Display Name:
Legitimate Company Support

Reply-To:
different-address@example.net

URL:
hxxps://login[.]suspicious-domain[.]example

Attachment:
invoice.pdf

Authentication:
SPF = fail
DKIM = fail
DMARC = fail
```

Then correlate the indicators using your organization's approved threat-intelligence and security tools.

---

# 21. Key Takeaways

```text
Email Address
    ↓
Username + @ + Domain

Email Delivery
    ↓
SMTP + DNS + POP3/IMAP

Email Investigation
    ↓
Headers + Body + URLs + Attachments

Headers
    ↓
From / To / Reply-To / Received /
X-Originating-IP / Authentication-Results

HTML
    ↓
Inspect href/src and actual destinations

Safety
    ↓
Defang URLs and IPs

Attachments
    ↓
MIME → encoding → extract → decode safely

Phishing
    ↓
Spam / Phishing / Spear Phishing / Whaling /
Smishing / Vishing / Clone Phishing / BEC
```

---

# 22. Quick Revision Cheat Sheet

| Indicator | What to check |
|---|---|
| `From:` | Actual sender + domain |
| Display name | Can be spoofed |
| `Reply-To:` | Does it differ from sender? |
| `Received:` | Mail delivery path |
| `X-Originating-IP:` | Possible originating IP |
| `Authentication-Results:` | SPF/DKIM/DMARC results |
| `Content-Type:` | Type of email content |
| `Content-Disposition:` | Attachment + filename |
| `Content-Transfer-Encoding:` | Encoding such as Base64 |
| `href=` | Actual hyperlink destination |
| MIME boundary | Separates email parts |
| Base64 | Decode only the payload |
| Defanging | Prevent accidental clicks |
| BEC | Business Email Compromise |

---

# 23. Room Investigation Example

For the example phishing email shown in the room:

```text
Display name:
Thank you! Home Depot

Actual sender:
support@teckbe.com
```

The important lesson is that **the display name alone is not trustworthy**.

Always compare:

```text
Claimed organization
        vs
Actual sender/domain
```

This difference is one of the first things to check during phishing triage.

---

# 24. Further Learning

The room is part of the **TryHackMe SOC Level 1 → Phishing Analysis** learning path.

Suggested progression:

1. Phishing Analysis
2. Phishing Emails in Action
3. Phishing Analysis Tools
4. Phishing Prevention
5. The Greenholt Phish
6. Snapped Phish-ing Line

---

## References

- TryHackMe — Phishing Analysis: https://tryhackme.com/room/phishingemails1tryoe
- CyberChef: https://gchq.github.io/CyberChef/

---

## One-Line SOC Memory

> **Never trust the display name — inspect the actual sender, headers, URLs, body, and attachments before deciding what an email is.**
