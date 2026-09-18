# TryHackMe — Phishing Emails in Action

> **Room:** Phishing Emails in Action  
> **Focus:** Identifying phishing indicators, analysing email headers/body, links, tracking pixels, attachments and malicious redirects.

---

## 1. What is Phishing?

**Phishing** is a social-engineering attack where an attacker pretends to be a trusted person, company or service to trick a victim into performing an action.

Common goals:

- Steal usernames/passwords
- Capture payment/card information
- Deliver malware
- Redirect victims to malicious websites
- Collect information through tracking pixels
- Create urgency so the victim acts without verifying

### Basic phishing flow

```text
Attacker
   ↓
Fake / spoofed email
   ↓
Victim trusts the message
   ↓
Click link / open attachment
   ↓
Fake website or malicious file
   ↓
Credentials / information / malware
```

---

# 2. Phishing Indicators — Quick Checklist

When analysing an email, **never trust the display name alone**.

Check:

### Sender
- Does the sender address match the organisation?
- Is the domain suspicious or slightly misspelled?
- Is the display name pretending to be a known brand?
- Does the sender use a strange/random domain?

### Subject
Look for:
- `Urgent`
- `Action Required`
- `Account Suspended`
- `Payment Failed`
- `Recent Purchase`
- `Track Your Package`
- `Your account is on hold`

Urgency is commonly used to stop the victim from thinking.

### Recipient
Check:
- Was the email actually sent to you?
- Is the recipient hidden using **BCC**?
- Are there unusual To/CC/BCC addresses?

### Body
Look for:
- Poor grammar/spelling
- Unusual wording
- Fake branding
- Threats or deadlines
- Requests for passwords/payment
- Unexpected attachments
- Buttons that ask you to log in or verify

### Links
**Do not judge a link by its visible text.**

Example:

```text
Visible text:
https://legitimate-looking-site.com

Actual destination:
https://attacker-domain.example/login
```

Always inspect the real hyperlink destination.

### Attachments
Be suspicious of:
- Unexpected documents
- Office files
- PDF files
- Archive files
- Files with unusual extensions
- Files that ask you to enable macros/content
- Documents containing external links

---

# 3. Spoofed Email Address

A phishing email may display a trusted brand as the sender while the actual email address belongs to another domain.

Example:

```text
Display Name:
Netflix Support

Actual address:
support@random-domain.example
```

### Key rule

```text
Display Name ≠ Proof of authenticity
```

Always inspect the **actual From address and domain**.

---

# 4. URL / Link Manipulation

Attackers frequently manipulate hyperlinks.

### Common techniques

- URL shorteners
- Redirects
- Look-alike domains
- Subdomains
- Long URLs
- URL encoding
- Hidden destination behind a button
- Typo-squatting

Example:

```text
https://trusted-brand.example
        ↓
redirect
        ↓
https://malicious-domain.example/login
```

### Analyst workflow

```text
Email
 ↓
Hover / inspect link
 ↓
Extract actual URL
 ↓
Check domain
 ↓
Check redirects
 ↓
Determine final destination
```

**Never assume the visible button text is the real destination.**

---

# 5. Tracking Pixels

A **tracking pixel** is usually a tiny/invisible image embedded in an email.

When the image is requested from the attacker's server, the attacker may learn that the email was opened.

Conceptually:

```text
Victim opens email
       ↓
Email loads image
       ↓
Request goes to attacker-controlled server
       ↓
Attacker knows the message was accessed
```

### Why attackers use tracking pixels

They can help determine:

- Whether the email was opened
- Approximate time of opening
- Potential recipient activity
- Sometimes technical/network information exposed by the request

### Important distinction

A tracking pixel is primarily an **information-gathering mechanism**. It is different from a malicious link that directly sends the victim to a phishing page.

---

# 6. Email Body Analysis

Do not analyse only the subject and sender.

Look at the entire message.

### Things to inspect

```text
Sender
  ↓
Subject
  ↓
Recipient
  ↓
Body text
  ↓
Branding
  ↓
Buttons / hyperlinks
  ↓
Attachments
  ↓
Images / tracking pixels
```

### Red flags

- Unexpected transaction
- Fake account problem
- Immediate deadline
- Threat of account closure
- Request to update payment details
- Strange contact information
- Mismatched branding
- Suspicious hyperlink
- Unexpected attachment

---

# 7. Case Study — Cancel Your Order

This example demonstrates several phishing techniques at once.

### Techniques observed

- **Spoofed email address**
- **Urgency**
- **URL shortening**
- **Brand impersonation**
- **Suspicious button/link**
- **HTML manipulation**

### Analyst mindset

Do not click the button just because it says:

```text
Cancel the order
```

Instead:

```text
Inspect hyperlink
        ↓
Find actual destination
        ↓
Check domain
        ↓
Check redirects
        ↓
Decide whether it is legitimate
```

### Lesson

A polished-looking email can still be malicious.

---

# 8. Case Study — Track Your Package

This email uses a fake package-tracking notification.

### Phishing techniques

- Spoofed sender address
- Tracking pixel
- Link manipulation
- Fake delivery/package theme

### Important observation

The **visible tracking number/link does not prove legitimacy**.

The hyperlink destination must be inspected.

### Tracking pixel vs phishing link

| Feature | Tracking Pixel | Phishing Link |
|---|---|---|
| Main purpose | Track email activity | Redirect victim |
| Usually visible? | No | Often appears as text/button |
| Requires clicking? | Usually no | Usually yes |
| Can collect activity data? | Yes | Yes |
| Can lead to credential theft? | Indirectly | Commonly |

---

# 9. HTML Link Analysis

Email HTML can hide the actual destination.

Conceptually:

```html
<a href="https://malicious.example/login">
    Track your package
</a>
```

The victim sees:

```text
Track your package
```

But the browser follows:

```text
https://malicious.example/login
```

### What to inspect

```text
Display Text
      ↓
href attribute
      ↓
Actual domain
      ↓
Redirect chain
      ↓
Final destination
```

---

# 10. Case Study — Download Document Here

This scenario demonstrates a phishing email that attempts to make the victim download a document.

### Techniques

- Artificial urgency
- Link manipulation
- Malicious/redirected download
- Credential harvesting
- Fake cloud-storage/document page

### Why this works

The attacker creates a familiar workflow:

```text
"Document available"
       ↓
"Download document"
       ↓
Fake legitimate-looking page
       ↓
Victim enters credentials
```

### Analyst rule

An email attachment or download request that is unexpected should be treated as suspicious until verified independently.

---

# 11. Credential Harvesting

**Credential harvesting** is the collection of authentication information such as usernames and passwords.

Typical phishing flow:

```text
Phishing email
     ↓
Fake login page
     ↓
Victim enters username/password
     ↓
Credentials sent to attacker
```

### Common disguises

- Microsoft/Outlook login
- Google login
- OneDrive document
- Payroll portal
- Banking portal
- Streaming service
- Company SSO

### Key rule

If an email asks you to log in, verify the website independently instead of following the email's link.

---

# 12. Case Study — Netflix Account on Hold

This example demonstrates brand impersonation.

### Indicators

- Account suspension/hold theme
- Urgency
- Spoofed sender
- Fake Netflix branding
- Unusual contact information
- Payment/account update request
- Suspicious attachment/link

### Important principle

Attackers copy:

- Logos
- Colours
- Fonts
- Layouts
- Email signatures
- Buttons

**Visual appearance is not authentication.**

---

# 13. Attachment Analysis

Attachments can be used to bypass the user's normal suspicion of links.

Potentially suspicious attachment types include:

```text
.doc / .docm
.xls / .xlsm
.pdf
.zip
.rar
.iso
.js
.vbs
.lnk
.exe
```

> File type alone does not prove that a file is malicious. Context and analysis matter.

### Red flags

- Unexpected attachment
- Urgent request to open it
- Invoice/receipt that you did not expect
- Document asking to enable macros
- File that redirects to a login page
- Extension mismatch
- Suspicious embedded links

---

# 14. Case Study — Recent Purchase

This example uses a fake purchase notification.

### Techniques observed

- Spoofed sender
- Recipient hidden using **BCC**
- Urgency
- Poor grammar/typos
- Fake Apple Support identity
- Suspicious `.dot` document attachment
- Redirect link

### Important lesson

A fake transaction is a strong social-engineering lure:

```text
"You were charged"
        ↓
Victim becomes worried
        ↓
Victim opens attachment/link
        ↓
Attacker gets credentials or executes payload
```

### BCC as an indicator

BCC itself is **not malicious**.

However, when combined with:
- spoofed sender
- unexpected purchase
- urgency
- suspicious attachment

it becomes useful contextual evidence during analysis.

---

# 15. Case Study — Scheduled Shipment

This scenario impersonates a courier/shipping company.

### Techniques

- Spoofed email address
- Brand impersonation
- Urgency
- Malicious Excel attachment
- Executable/payload execution

### Attack chain

```text
Fake courier email
       ↓
"Your shipment is scheduled"
       ↓
Open Excel attachment
       ↓
Embedded content / execution
       ↓
Malicious payload
```

### Why shipping emails work

Package notifications are common and create curiosity:

```text
"What package is this?"
        ↓
User opens attachment
```

---

# 16. Malicious Office Documents

Attackers may abuse Office documents to deliver malware.

Potential mechanisms include:

- Macros
- Embedded objects
- External links
- Exploit-based document attacks
- Scripts/payloads triggered by user interaction

### Defensive approach

Do not enable macros or other active content simply because an email tells you to.

---

# 17. Social Engineering Patterns

Across the room, several attacks rely on the same psychological triggers.

### Urgency

```text
"Act now"
"Your account will be closed"
"Cancel immediately"
```

### Fear

```text
"Suspicious purchase"
"Account suspended"
"Payment failed"
```

### Curiosity

```text
"Track your package"
"Recent purchase"
"Document attached"
```

### Authority

```text
Microsoft
Apple
Netflix
DHL
PayPal
```

### Trust

Attackers combine familiar branding with malicious infrastructure.

---

# 18. Email Investigation Method

Use this workflow when analysing a suspicious email:

```text
1. Check sender
       ↓
2. Check actual domain
       ↓
3. Check subject for urgency
       ↓
4. Check recipient / BCC
       ↓
5. Read body carefully
       ↓
6. Inspect hyperlinks
       ↓
7. Inspect redirects
       ↓
8. Inspect attachments
       ↓
9. Check images/tracking pixels
       ↓
10. Verify the request independently
```

---

# 19. SOC Analyst Perspective

When a phishing email is reported, a SOC analyst may collect:

### Email metadata

- From
- To
- CC
- BCC
- Subject
- Date/time
- Message-ID
- Received headers

### Indicators of Compromise (IOCs)

- Sender email
- Sender domain
- URLs
- Domains
- IP addresses
- File hashes
- Attachment names
- Malicious filenames

### Investigation

```text
Email
 ↓
Extract IOCs
 ↓
Analyse URLs/domains
 ↓
Analyse attachment
 ↓
Search SIEM / mail logs
 ↓
Identify affected users
 ↓
Contain
 ↓
Remediate
```

---

# 20. Useful Security Concepts

## Spoofing

Pretending to be another sender/entity.

```text
Fake sender identity
```

## Brand impersonation

Copying a legitimate company's identity to create trust.

## URL manipulation

Changing/hiding the real destination of a hyperlink.

## Redirect

A URL sends the victim through one or more intermediate locations before reaching the final destination.

## Tracking pixel

Tiny remote image used to track email activity.

## Credential harvesting

Stealing authentication information through fake login pages/forms.

## Spearphishing

Highly targeted phishing aimed at a particular person or organisation.

---

# 21. Phishing vs Spearphishing

| Phishing | Spearphishing |
|---|---|
| Broad targeting | Targeted victim |
| Generic message | Personalised message |
| Large-scale campaigns | Smaller targeted campaigns |
| Less victim-specific context | More victim-specific context |

---

# 22. Red Flags to Memorise

### The "10-second phishing check"

Ask:

```text
1. Do I know the sender?
2. Does the actual domain match?
3. Was I expecting this email?
4. Is it creating urgency?
5. Is it asking me to log in?
6. Is there an unexpected attachment?
7. Where does the link REALLY go?
8. Is the branding suspicious?
9. Are there grammar/spelling issues?
10. Can I verify this independently?
```

---

# 23. What NOT To Do

When you receive a suspicious email:

❌ Do not click the link immediately  
❌ Do not open unexpected attachments  
❌ Do not enable macros  
❌ Do not enter credentials into a page opened from the email  
❌ Do not trust the display name  
❌ Do not assume HTTPS means the site is legitimate  
❌ Do not let urgency override verification  

Instead:

✅ Verify through the official website/app  
✅ Contact the organisation using a trusted channel  
✅ Report the email  
✅ Preserve the message for analysis  
✅ Extract and investigate indicators safely  

---

# 24. Interview / Viva Questions

### Q1. What is phishing?
A social-engineering attack designed to trick a victim into revealing information, clicking a malicious link, opening a malicious attachment, or performing another unsafe action.

### Q2. Why is the display name not enough?
Because attackers can make the display name look like a trusted organisation while using a completely different email domain.

### Q3. What is a tracking pixel?
A small/hidden remote image that can notify a sender when an email is opened or loaded.

### Q4. Why should you inspect hyperlinks?
Because the visible text can be different from the actual URL destination.

### Q5. What is credential harvesting?
The process of tricking users into submitting usernames/passwords or other authentication information to an attacker-controlled system.

### Q6. Why is urgency a phishing indicator?
It attempts to reduce the time available for the victim to verify the request and encourages impulsive action.

### Q7. Is BCC automatically suspicious?
No. BCC has legitimate uses. It becomes an additional contextual indicator when combined with other suspicious characteristics.

### Q8. Why are attachments dangerous?
They may contain active content, malicious scripts, links, exploits, or malware.

### Q9. What is brand impersonation?
Using the identity, branding or appearance of a trusted organisation to make a malicious communication appear legitimate.

### Q10. What should a SOC analyst extract from a phishing email?
Sender/domain, URLs, domains, IPs, attachment names, hashes and relevant email/header metadata.

---

# 25. Quick Revision Sheet

```text
PHISHING
│
├── Sender
│   ├── Spoofed address
│   └── Look-alike domain
│
├── Subject
│   └── Urgency / fear
│
├── Body
│   ├── Poor grammar
│   ├── Fake branding
│   └── Suspicious request
│
├── Links
│   ├── Hidden destination
│   ├── Shortened URL
│   └── Redirect
│
├── Tracking
│   └── Tracking pixel
│
├── Attachments
│   ├── Office documents
│   ├── PDFs
│   ├── Archives
│   └── Executable/active content
│
└── Goal
    ├── Credential theft
    ├── Malware delivery
    └── Information gathering
```

---

# 26. SOC Cheat Sheet

```text
SUSPICIOUS EMAIL
      ↓
Identify sender
      ↓
Check domain
      ↓
Analyse headers
      ↓
Analyse subject/body
      ↓
Extract URLs
      ↓
Check final destination
      ↓
Analyse attachments safely
      ↓
Extract IOCs
      ↓
Search logs / SIEM
      ↓
Identify affected users
      ↓
Contain + remediate
```

---

## Key Takeaways

- **Never trust the display name alone.**
- **Always inspect the real sender domain.**
- **Visible link text can hide a malicious destination.**
- **Tracking pixels can reveal email activity.**
- **Urgency + authority + unexpected action is a major phishing pattern.**
- **Attachments should be treated cautiously, especially unexpected Office documents.**
- **Branding can be copied; verify through trusted channels.**
- **BCC is not inherently malicious.**
- **HTTPS does not automatically mean a website is trustworthy.**
- **For SOC work, learn to extract and correlate IOCs from phishing emails.**

---

## Practical SOC Mindset

> **Stop → Inspect → Verify → Report**

The objective is not to identify a phishing email from one clue.  
A good analyst **correlates multiple indicators** and determines whether the message, infrastructure and requested action are consistent with a legitimate communication.
