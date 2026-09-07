# 🟢 Splunk --- The Basics

## 📌 What is Splunk?

**Splunk** is a platform used to **collect, index, search, analyze,
visualize, and monitor machine-generated data** such as:

-   System logs
-   Network events
-   Firewall logs
-   VPN logs
-   Application logs
-   Security events
-   Cloud data

In a SOC, Splunk can act as a **SIEM**, allowing analysts to search
large amounts of security data and detect suspicious activity.

------------------------------------------------------------------------

# 🏗️ Splunk Architecture

Splunk mainly consists of **three core components**:

``` text
                ┌───────────────┐
                │   Forwarder   │
                │ Data Collector│
                └───────┬───────┘
                        │
                        ▼
                ┌───────────────┐
                │    Indexer    │
                │ Store + Index │
                └───────┬───────┘
                        │
                        ▼
                ┌───────────────┐
                │   Search Head │
                │ Search/Analyze│
                └───────────────┘
```

## 1. 📤 Forwarder

The **Forwarder** collects data from different sources and sends it to
Splunk.

Examples:

-   Windows/Linux logs
-   Network devices
-   Firewalls
-   Applications
-   Files
-   Syslog

Its primary job is to **collect and forward data**.

## 2. 🗄️ Indexer

The **Indexer** receives data from forwarders and:

1.  Processes incoming data
2.  Breaks it into events
3.  Parses/normalizes relevant information
4.  Creates indexes
5.  Stores the data
6.  Makes the data searchable

> **Forwarder = Collect → Indexer = Store & Index → Search Head = Search
> & Analyze**

## 3. 🔎 Search Head

The **Search Head** provides the interface through which analysts search
and analyze indexed data.

It allows you to:

-   Search logs
-   Run SPL queries
-   Create reports
-   Build dashboards
-   Visualize events
-   Investigate security incidents

------------------------------------------------------------------------

# 🖥️ Splunk Interface

## Splunk Bar

The top navigation bar provides access to:

  Option         Purpose
  -------------- ------------------------------
  **Messages**   System-level notifications
  **Settings**   Configure Splunk
  **Activity**   Review search jobs/processes
  **Help**       Documentation and tutorials
  **Find**       Search across Splunk

The **Apps Panel** allows switching between installed Splunk
applications.

## 📦 Apps Panel

The Apps Panel displays installed applications.

The default application commonly used for searching data is:

> **Search & Reporting**

This is where most basic SPL investigation happens.

## 🌐 Explore Splunk

The **Explore Splunk** section provides shortcuts for:

-   Adding data
-   Managing Splunk apps
-   Accessing Splunk documentation

## 📊 Splunk Dashboard

Dashboards provide a visual representation of data.

They can contain:

-   Tables
-   Charts
-   Graphs
-   Statistics
-   Security metrics
-   Monitoring panels

In a SOC, dashboards can monitor:

``` text
Failed Logins
     ↓
Suspicious IPs
     ↓
Malware Alerts
     ↓
Network Traffic
     ↓
Authentication Activity
```

------------------------------------------------------------------------

# 📥 Data Sources in Splunk

Splunk can ingest many types of machine-generated data.

  Data Source           Example
  --------------------- -------------------------
  Files & Directories   Log files
  Network Events        Network/SNMP data
  IT Operations         Monitoring data
  Cloud Services        AWS, Azure, etc.
  Database Services     MySQL, MS SQL
  Security Services     AD, Endpoint Protection
  Virtualization        VMware, XenApp
  Application Servers   IIS, WebLogic, etc.
  Windows Sources       Windows Event Logs
  Other Sources         APIs, custom inputs

### 🔥 SOC Perspective

Security analysts commonly work with:

``` text
Firewall Logs
     ↓
VPN Logs
     ↓
Windows Event Logs
     ↓
EDR Logs
     ↓
DNS Logs
     ↓
Authentication Logs
     ↓
       Splunk
```

Splunk converts incoming data into **individual searchable events**.

------------------------------------------------------------------------

# 📤 Adding Data to Splunk

Splunk provides several ways to ingest data.

### Upload

Upload files directly from the local machine.

Useful for:

-   JSON
-   CSV
-   Log files
-   Sample datasets

### Monitor

Continuously monitor a file, directory, or network source.

Useful when logs are continuously generated.

### Forward

Receive data through a Splunk Forwarder or other forwarding mechanisms.

------------------------------------------------------------------------

# 🧪 Practical: Uploading VPN Logs

The TryHackMe task uses a dataset called:

``` text
VPN_logs
```

The uploaded file is **newline-delimited JSON (NDJSON)**.

That means:

``` text
Line 1 → Event 1
Line 2 → Event 2
Line 3 → Event 3
...
```

Splunk treats each line as an individual event.

------------------------------------------------------------------------

# ⚙️ Import Settings

Basic process:

1.  Open **Add Data**
2.  Select **Upload**
3.  Select `VPN_logs`
4.  Keep the detected source type as **JSON**
5.  Create/select the index:

``` text
VPN_Logs
```

6.  After uploading, open **Search & Reporting**
7.  Set the time range to **All time**

### ⚠️ Important

If JSON fields aren't appearing correctly in search results, use:

``` spl
| spath
```

`spath` extracts fields from structured JSON data.

------------------------------------------------------------------------

# 🔎 SPL --- Searching the Data

**SPL = Search Processing Language**

It is Splunk's query language for searching and processing events.

Basic syntax:

``` spl
index=VPN_Logs
```

This searches all events inside the `VPN_Logs` index.

------------------------------------------------------------------------

## 📊 Count All Events

``` spl
index=VPN_Logs
| stats count
```

`stats count` returns the total number of matching events.

------------------------------------------------------------------------

## 🔍 Search for a Specific User

``` spl
index=VPN_Logs
| spath
| search UserName="Maleena"
| stats count
```

This:

1.  Searches the `VPN_Logs` index
2.  Extracts JSON fields with `spath`
3.  Filters events where `UserName = Maleena`
4.  Counts matching events

------------------------------------------------------------------------

## 🌍 Search by Source IP

``` spl
index=VPN_Logs
| spath
| search Source_ip="107.14.182.38"
| stats values(UserName) as UserName count
```

### What does this do?

-   `values(UserName)` → displays unique usernames associated with the
    IP
-   `count` → counts matching events
-   `as UserName` → gives the output field a readable name

This is useful during **IP-based investigations**.

------------------------------------------------------------------------

## 🇫🇷 Search by Country

``` spl
index=VPN_Logs
| spath
| search Source_Country="France"
| stats count
```

This counts VPN events originating from France.

------------------------------------------------------------------------

## 🌐 Search Another Source IP

``` spl
index=VPN_Logs
| spath
| search Source_ip="107.3.206.58"
| stats count
```

Useful for determining how many events were generated by a particular
source IP.

------------------------------------------------------------------------

# 🧠 Important SPL Commands

  Command      Purpose
  ------------ ----------------------------
  `index=`     Select an index
  `search`     Filter events
  `spath`      Extract JSON fields
  `stats`      Calculate statistics
  `count`      Count events
  `values()`   Return unique field values
  `as`         Rename output fields

------------------------------------------------------------------------

# 🛡️ SOC Investigation Example

Suppose an analyst receives a suspicious VPN IP:

``` text
107.14.182.38
```

They could investigate it using:

``` spl
index=VPN_Logs
| spath
| search Source_ip="107.14.182.38"
| stats values(UserName) as UserName count
```

This helps answer:

> **Which users were associated with this source IP and how many VPN
> events occurred?**

From there, an analyst can investigate:

``` text
Suspicious IP
     ↓
Associated User
     ↓
Login Frequency
     ↓
Country
     ↓
Time of Activity
     ↓
Other Security Events
```

This is the basic workflow behind **SIEM-based threat investigation**.

------------------------------------------------------------------------

# 🧩 Key Takeaways

> 🔹 **Forwarder** → Collects and forwards data\
> 🔹 **Indexer** → Processes, indexes and stores data\
> 🔹 **Search Head** → Searches and analyzes data\
> 🔹 **Index** → Logical storage/search location for events\
> 🔹 **SPL** → Splunk's search/query language\
> 🔹 `spath` → Extracts fields from JSON\
> 🔹 `stats` → Performs calculations/aggregation\
> 🔹 **Dashboard** → Visualizes security/operational data

## 🔥 Remember This

``` text
Data Sources
     ↓
Forwarder
     ↓
Indexer
     ↓
Search Head
     ↓
SPL Investigation
     ↓
Detection / Analysis
     ↓
SOC Response
```

### Splunk's Core Idea

> **Ingest → Index → Search → Analyze → Detect → Respond**
