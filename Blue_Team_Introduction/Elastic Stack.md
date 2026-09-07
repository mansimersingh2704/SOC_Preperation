# 🔵 Investigating with Elastic Stack

## 📌 What is the Elastic Stack?

The **Elastic Stack** is a collection of open-source components used to **collect, process, store, search, analyze, and visualize large amounts of data**.

The main components are:

```text
Beats → Logstash → Elasticsearch ↔ Kibana
  │         │             │             │
Collect   Process      Store/Search   Analyze/
Data      & Filter     Data           Visualize
```

### Core Components

| Component | Purpose |
|---|---|
| **Beats** | Lightweight agents that collect data from endpoints and send it to Elasticsearch/Logstash |
| **Logstash** | Ingests, processes, filters, transforms, and forwards data |
| **Elasticsearch** | Distributed search and analytics engine that stores and indexes data |
| **Kibana** | Web interface for searching, analyzing, visualizing, and creating dashboards |

> **Easy memory:** `Beats = Collect → Logstash = Process → Elasticsearch = Store/Search → Kibana = Analyze/Visualize`

---

# 1. 🔎 Elasticsearch

**Elasticsearch** is a distributed, RESTful search and analytics engine.

It stores data in a structure optimized for fast searching and analysis.

### What Elasticsearch does

- Stores events/documents
- Indexes data
- Provides fast searches
- Supports aggregations
- Handles large datasets
- Exposes REST APIs

Think of Elasticsearch as the **data storage + search engine** of the Elastic Stack.

---

# 2. 🔄 Logstash

**Logstash** is a data processing pipeline.

It can receive data from different sources, process/filter it, and send the resulting data to a destination.

### Logstash pipeline

```text
Input
  ↓
Filter
  ↓
Output
```

### Input

Defines where Logstash receives data from.

Examples:

- Files
- Syslog
- Beats
- Network sources
- APIs

### Filter

Processes and transforms the incoming events.

Examples:

- Parse fields
- Modify values
- Remove unwanted data
- Extract structured information
- Normalize events

### Output

Defines where processed events are sent.

For example:

```text
Logstash → Elasticsearch
```

### Example concept

```text
Raw Log
  ↓
Logstash Input
  ↓
Filter / Parse
  ↓
Structured Event
  ↓
Elasticsearch
```

---

# 3. 📡 Beats

**Beats** are lightweight data collection agents designed to ship data to the Elastic Stack.

Different Beats are designed for different types of data.

### Common Beats

| Beat | Purpose |
|---|---|
| **Filebeat** | Collects log files |
| **Metricbeat** | Collects system/application metrics |
| **Packetbeat** | Collects network traffic information |
| **Winlogbeat** | Collects Windows Event Logs |
| **Auditbeat** | Collects audit/security data |
| **Heartbeat** | Monitors availability and uptime |
| **Functionbeat** | Collects data from serverless/cloud functions |

For SOC work, **Filebeat, Winlogbeat, Packetbeat, and Auditbeat** are particularly useful.

---

# 4. 📊 Kibana

**Kibana** is the web-based interface used to interact with data stored in Elasticsearch.

SOC analysts can use Kibana to:

- Search logs
- Investigate suspicious activity
- Filter events
- Analyze fields
- Create visualizations
- Create dashboards
- Identify spikes/anomalies

A common SOC workflow is:

```text
Logs → Elasticsearch → Kibana → Investigation
```

---

# 🔍 Kibana Discover

The **Discover** tab is one of the most important areas for a SOC analyst.

It allows analysts to:

- Search indexed logs
- View individual events
- Filter by fields
- Select time ranges
- Inspect event details
- Identify unusual activity

### Important Discover Elements

| Element | Purpose |
|---|---|
| **Logs / Events** | Individual records returned by the search |
| **Fields Pane** | Available and selected fields |
| **Index Pattern / Data View** | Determines which indexed data is searched |
| **Search Bar** | Enter search queries |
| **Time Filter** | Restrict results to a time period |
| **Timeline** | Shows event volume over time |
| **Top Bar** | Search, save, share, and other actions |
| **Add Filter** | Apply structured field-based filters |

---

# 🗂️ Index Patterns / Data Views

An **index pattern** (called a **data view** in modern Kibana) tells Kibana which Elasticsearch indices should be queried.

For example:

```text
vpn_connections
```

can be selected to investigate VPN logs.

An index represents a collection of related documents/events.

### Why this matters

Different log sources may use different structures.

For example:

```text
vpn_connections
windows_logs
firewall_logs
dns_logs
```

The analyst selects the appropriate data view/index before investigating.

---

# 🧩 Fields Pane

The **Fields Pane** displays fields available in the selected data.

Examples:

```text
UserName
Source_ip
Source_Country
action
source_state
timestamp
```

Clicking a field can reveal:

- Top values
- Percentage of occurrence
- Available values
- Filtering options

### Quick filtering

Kibana allows analysts to add a field as a filter directly from the Fields Pane.

This is useful when investigating a specific:

- User
- IP address
- Country
- Host
- Event type

---

# ⏱️ Time Filter

The time filter restricts the investigation to a specific period.

Common options include:

- Relative time
- Absolute time
- Quick ranges

Example:

```text
Jan 1, 2022 00:00 → Jan 31, 2022 23:59
```

### Why time filtering matters in SOC

Without a suitable time range, an investigation can contain unrelated events.

A good investigation normally follows:

```text
Identify incident time
        ↓
Set appropriate time range
        ↓
Search relevant events
        ↓
Correlate activity
```

---

# 📈 Timeline

The timeline provides an overview of **how many events occurred over time**.

It is useful for spotting:

- Activity spikes
- Unusual login bursts
- Beaconing patterns
- Scanning activity
- Suspicious periods
- Sudden changes in event volume

### SOC Tip

A sudden spike does **not automatically mean an attack**.

It is an investigation lead that should be correlated with:

- Source IP
- User
- Country
- Event type
- Other logs
- Time of occurrence

---

# 🧮 Creating Tables

Kibana can display selected fields in a table instead of showing complete raw events.

This reduces noise and makes important information easier to compare.

For example:

| UserName | Source_Country | Source_ip |
|---|---|---|
| James | United States | 238.163.231.224 |
| ... | ... | ... |

A table is particularly useful when investigating many events.

---

# 🔎 KQL — Kibana Query Language

**KQL (Kibana Query Language)** is used to search and filter data in Kibana.

KQL is primarily used for **filtering and searching documents**, not for performing complex aggregations like a full query language.

Kibana provides two useful search approaches:

1. **Free-text search**
2. **Field-based search**

---

# 📝 Free-Text Search

Free-text search looks for a term across the available document content.

Example:

```text
United States
```

This can return documents containing that term.

### Wildcards

KQL supports wildcards for broader matching.

Example:

```text
United*
```

This can match terms beginning with `United`, such as:

```text
United States
United Kingdom
```

> Wildcards are useful when the exact value is uncertain.

---

# 🎯 Field-Based Search

Field-based searches specify both the field and the value.

Syntax:

```text
field:value
```

Example:

```text
Source_ip:238.163.231.224
```

Multiple conditions can be combined.

Example:

```text
Source_ip:238.163.231.224 AND UserName:Suliman
```

This searches for documents where:

- `Source_ip` matches `238.163.231.224`
- AND `UserName` matches `Suliman`

---

# 🔗 Logical Operators

KQL supports logical operators such as:

```text
AND
OR
NOT
```

## AND

Both conditions must match.

```text
"United States" AND "Virginia"
```

Conceptually:

```text
Condition A
    AND
Condition B
    ↓
Only matching documents
```

---

## OR

Either condition can match.

```text
"United States" OR "England"
```

Useful when investigating multiple possible values.

---

## NOT

Excludes a value from the results.

```text
"United States" AND NOT "Florida"
```

This returns United States-related results while excluding Florida.

---

# 🧪 Practical Investigation Queries

### Search for a country

```text
Source_Country:"United States"
```

### Search for a specific user

```text
UserName:James
```

### Search for an IP

```text
Source_ip:238.163.231.224
```

### Combine IP + user

```text
Source_ip:238.163.231.224 AND UserName:Suliman
```

### Search multiple countries

```text
Source_Country:"United States" OR Source_Country:England
```

### Exclude a country

```text
Source_Country:"United States" AND NOT Source_Country:Florida
```

> Field names must match the fields available in the dataset.

---

# 📊 Creating Visualizations

Kibana can convert search results into visualizations.

Possible visualization types include:

- Bar charts
- Line charts
- Pie charts
- Tables
- Metric visualizations
- Area charts
- Other analytical visualizations

### Basic workflow

```text
Search Data
    ↓
Filter Data
    ↓
Select Fields
    ↓
Choose Visualization
    ↓
Save Visualization
```

---

# 🔗 Correlation

Kibana can correlate multiple fields to understand relationships in the data.

For example:

```text
Source IP
    ↓
Username
    ↓
Country
    ↓
Time
    ↓
Connection Count
```

This can help an analyst determine whether activity is normal or suspicious.

---

# 📊 Creating Dashboards

Dashboards combine multiple saved visualizations into one view.

### Basic workflow

```text
Discover
   ↓
Create/Search Data
   ↓
Create Visualization
   ↓
Save Visualization
   ↓
Add to Dashboard
   ↓
SOC Monitoring
```

A security dashboard might contain:

```text
┌─────────────────────────────────┐
│ Total Events                    │
├───────────────┬─────────────────┤
│ Top IPs       │ Top Users       │
├───────────────┼─────────────────┤
│ Countries     │ Event Timeline  │
├───────────────┴─────────────────┤
│ Suspicious Activity             │
└─────────────────────────────────┘
```

Dashboards are useful for **continuous monitoring and visibility**.

---

# 🛡️ SOC Investigation Workflow

A practical Elastic investigation can follow this process:

```text
1. Select the correct Data View
            ↓
2. Set the time range
            ↓
3. Inspect the timeline
            ↓
4. Identify unusual spikes
            ↓
5. Search using KQL
            ↓
6. Filter by IP / User / Country
            ↓
7. Inspect individual events
            ↓
8. Correlate related fields
            ↓
9. Create a table/visualization
            ↓
10. Document findings
```

---

# 🚨 SOC Takeaways

Elastic Stack is useful for investigating:

- 🔐 Authentication activity
- 🌐 VPN connections
- 🖥️ Endpoint activity
- 📡 Network events
- 🔎 Suspicious IP addresses
- 👤 User activity
- 🌍 Geographic anomalies
- 📈 Traffic spikes
- 📊 Security trends

### Example Investigation

```text
Suspicious VPN Activity
        ↓
Identify Source IP
        ↓
Find Associated User
        ↓
Check Country
        ↓
Check Timestamp
        ↓
Look for Activity Spikes
        ↓
Correlate Other Events
        ↓
Determine Severity
        ↓
Escalate / Respond
```

---

# 🧠 Quick Revision

| Concept | Remember |
|---|---|
| **Beats** | Collect data |
| **Logstash** | Process/filter data |
| **Elasticsearch** | Store, index & search data |
| **Kibana** | Search, analyze & visualize |
| **Discover** | Main investigation workspace |
| **Data View / Index Pattern** | Selects data to search |
| **Fields Pane** | Explore/filter fields |
| **Time Filter** | Restrict investigation period |
| **Timeline** | See event volume over time |
| **KQL** | Search/filter Elasticsearch data through Kibana |
| **Visualization** | Turn data into charts/tables |
| **Dashboard** | Combine visualizations |

## 🔥 Remember

```text
BEATS
  ↓
Collect

LOGSTASH
  ↓
Process / Filter

ELASTICSEARCH
  ↓
Index / Store / Search

KIBANA
  ↓
Investigate / Visualize / Dashboard
```

> **Elastic Stack = Collect → Process → Store → Search → Visualize → Investigate**
