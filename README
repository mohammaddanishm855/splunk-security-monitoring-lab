# 🛡️ Splunk Security Monitoring Lab

A hands-on **Security Operations Center (SOC) home lab** built with **Splunk Enterprise** to collect, search, analyze, investigate, and visualize security telemetry generated from a Linux environment.

The project focuses on practical **SIEM operations, SPL-based detection, security log analysis, network reconnaissance monitoring, authentication monitoring, alert investigation, event correlation, and MITRE ATT&CK mapping**.

![Splunk](https://img.shields.io/badge/SIEM-Splunk-black?logo=splunk)
![Ubuntu](https://img.shields.io/badge/OS-Ubuntu%20Linux-E95420?logo=ubuntu&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-red)
![Focus](https://img.shields.io/badge/Focus-Blue%20Team%20%7C%20SOC-blue)

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Objectives](#-objectives)
- [Lab Environment](#-lab-environment)
- [Security Tools](#-security-tools)
- [Detection Pipeline](#-detection-pipeline)
- [Attack and Detection Scenarios](#-attack-and-detection-scenarios)
- [Alert Investigation Workflow](#-alert-investigation-workflow)
- [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
- [Dashboards and Visualization](#-dashboards-and-visualization)
- [Repository Structure](#-repository-structure)
- [Troubleshooting](#-troubleshooting)
- [Skills Demonstrated](#-skills-demonstrated)
- [Key Takeaways](#-key-takeaways)
- [Conclusion](#-conclusion)
- [Author](#-author)

---

# 🔎 Overview

This project simulates a small-scale SOC monitoring environment using **Splunk Enterprise as the central SIEM platform**.

The lab generates and analyzes security telemetry from a Linux system and uses **Splunk Search Processing Language (SPL)** to investigate authentication activity, HTTP activity, network-related events, and security-testing activity.

The project follows a practical SOC workflow:

**Log Collection → Search → Detection → Investigation → Visualization → Response**

Rather than focusing only on Splunk installation, the lab emphasizes how a SOC analyst can use collected telemetry to identify patterns, investigate suspicious activity, build timelines, correlate related events, and extract useful security information.

---

# 🎯 Objectives

The primary objectives of this project are to:

- Build a practical Splunk-based SIEM environment.
- Collect and analyze Linux security logs.
- Monitor authentication and SSH-related activity.
- Analyze HTTP and web-related logs.
- Practice network reconnaissance monitoring.
- Develop SPL queries for security investigations.
- Identify repeated and suspicious activity.
- Correlate related security events.
- Perform timeline-based investigations.
- Map observed behaviors to MITRE ATT&CK techniques.
- Create dashboards for security visibility.
- Develop practical SOC and blue-team skills.

---

# 🖥️ Lab Environment

| Component | Purpose |
|---|---|
| **Splunk Enterprise** | Central SIEM platform for log collection, search, analysis, and visualization |
| **Ubuntu / Linux** | Monitored system and source of security telemetry |
| **Authentication Logs** | Monitoring login and authentication activity |
| **HTTP / Web Logs** | Monitoring web-related activity |
| **Nmap** | Network reconnaissance and security testing |
| **SPL** | Searching, filtering, aggregating, and analyzing security events |
| **Virtual Machines** | Isolated environment for cybersecurity testing |

The environment is designed as an isolated home lab so that security-testing activity can be generated and investigated without affecting production systems.

---

# 🛠️ Security Tools

## 🔹 Splunk Enterprise

Splunk serves as the central SIEM platform for the project.

It is used for:

- Log collection
- Security event searching
- Event filtering
- Pattern identification
- Security analysis
- Detection development
- Dashboard creation
- Event visualization
- Investigation support

The project focuses on using Splunk from a **SOC analyst perspective**, rather than simply using it as a log storage platform.

---

## 🔹 Splunk Search Processing Language (SPL)

SPL is used extensively throughout the lab to search, filter, aggregate, and investigate security events.

The project uses SPL for:

- Searching authentication events
- Identifying failed login attempts
- Grouping activity by source IP
- Counting security events
- Identifying event frequency
- Filtering relevant activity
- Performing basic event correlation
- Creating time-based analysis
- Supporting investigation workflows

### Example — Authentication Analysis

```spl
index=* "Failed password"
| stats count by src_ip, user
| sort - count
```

This search groups failed authentication events by source IP and username, making repeated authentication activity easier to identify.

---

## 🔹 Ubuntu / Linux

Linux provides the monitored environment and generates several types of security-related telemetry.

The lab uses Linux logs to investigate:

- Authentication attempts
- SSH activity
- System events
- Network activity
- Web activity

These logs provide realistic telemetry for practicing SIEM-based security monitoring.

---

## 🔹 Nmap

Nmap is used to generate network reconnaissance activity within the lab environment.

The resulting activity can be examined through available security telemetry.

Investigation focuses on:

- Source address
- Destination address
- Target host
- Destination ports
- Services
- Connection patterns
- Event frequency

The objective is to understand how reconnaissance-related activity can appear within security monitoring data.

---

## 🔹 Authentication Logs

Authentication logs provide visibility into login-related activity.

The lab uses these logs to analyze:

- Successful authentication
- Failed authentication
- Repeated login attempts
- Source addresses
- Usernames
- Timestamps
- Authentication patterns

### Example SPL

```spl
index=* "Failed password"
| stats count by src_ip, user
| sort - count
```

This search helps identify systems or accounts associated with repeated authentication failures.

---

## 🔹 HTTP / Web Logs

HTTP and web logs provide visibility into web-related activity.

The lab can be used to examine:

- HTTP requests
- Client/source information
- Requested resources
- HTTP response status
- Request frequency
- Abnormal web activity

### Example SPL

```spl
index=*
| stats count by status
| sort - count
```

This provides a basic distribution of HTTP response activity.

---

# 🔄 Detection Pipeline

The project follows a basic SIEM monitoring lifecycle:

```text
              Security Activity
                      │
                      ▼
                Log Generation
                      │
                      ▼
                Log Collection
                      │
                      ▼
                    Splunk
                      │
                      ▼
                 SPL Searching
                      │
                      ▼
                   Detection
                      │
                      ▼
                Alert / Analysis
                      │
                      ▼
                Investigation
                      │
                      ▼
                 Visualization
                      │
                      ▼
                   Response
```

This workflow demonstrates how raw security telemetry can be transformed into actionable information for SOC investigation.

---

# ⚔️ Attack & Detection Scenarios

The lab was used to generate and investigate multiple types of security activity within an isolated environment. The scenarios are designed to simulate common SOC investigation cases and demonstrate how security telemetry can be identified, searched, correlated, and analyzed using Splunk.

The focus is not only on generating activity, but on understanding **how the activity appears in logs and how an analyst can investigate it using SIEM telemetry**.

---

## 1. SSH / Authentication Brute-Force Activity

Repeated SSH authentication failures are used to simulate suspicious login activity and practice detection of potential brute-force behavior.

### Detection Objectives

- Identify repeated failed authentication attempts
- Identify the source IP generating the attempts
- Identify targeted usernames
- Determine the frequency of authentication failures
- Establish the activity timeline
- Search for successful authentication following repeated failures

### Investigation Data

An analyst reviews:

- Source IP
- Destination host
- Username
- Authentication result
- Timestamp
- Number of attempts
- Event frequency
- Related authentication events

### Example SPL

```spl
index=* "Failed password"
| stats count by src_ip, user
| sort - count
```

This search groups failed authentication events by source IP and username, helping identify sources associated with repeated authentication failures.

### Investigation Workflow

```text
Failed Authentication Events
            ↓
Identify Source IP
            ↓
Count Authentication Attempts
            ↓
Identify Targeted Account
            ↓
Review Timeline
            ↓
Search Related Events
            ↓
Investigate Activity
```

**MITRE ATT&CK:**  
[T1110 — Brute Force](https://attack.mitre.org/techniques/T1110/)

---

## 2. Network Reconnaissance / Port Scanning

Network reconnaissance is performed using Nmap to generate network activity that can be investigated from a defensive perspective.

The objective is to understand how scanning activity can be identified through available telemetry and how an analyst can determine the source and scope of the activity.

### Detection Objectives

- Identify the scanning source
- Identify targeted hosts
- Identify destination ports
- Identify exposed services
- Analyze connection patterns
- Determine the frequency and volume of activity
- Establish a timeline of reconnaissance activity

### Investigation Data

The investigation focuses on:

- Source IP
- Destination IP
- Destination port
- Service
- Protocol
- Connection frequency
- Related network events

### Example SPL

```spl
index=*
| stats count by src_ip
| sort - count
```

This provides a starting point for identifying systems generating significant amounts of network activity.

Further investigation can then be performed by filtering the relevant source and examining the surrounding events.

### Investigation Workflow

```text
Reconnaissance Activity
          ↓
Identify Source
          ↓
Identify Target
          ↓
Analyze Ports / Services
          ↓
Review Event Frequency
          ↓
Correlate Related Events
          ↓
Build Activity Timeline
```

**MITRE ATT&CK:**  
[T1046 — Network Service Scanning](https://attack.mitre.org/techniques/T1046/)

---

## 3. HTTP / Web Activity Monitoring

HTTP activity is monitored to understand how web requests and responses appear within SIEM telemetry.

The scenario provides a foundation for identifying unusual web activity and investigating request patterns.

### Detection Objectives

- Identify HTTP clients/sources
- Analyze requested resources
- Review HTTP methods
- Analyze response status codes
- Identify high-frequency requests
- Establish request timelines
- Search for related web events

### Investigation Data

The investigation focuses on:

- Source/client
- Requested resource
- HTTP method
- Response status
- Request frequency
- Timestamp
- Related events

### Example SPL

```spl
index=*
| stats count by status
| sort - count
```

This provides a high-level view of HTTP response activity.

Additional filtering can be applied to investigate specific clients, resources, response codes, or time periods.

---

## 4. Command-Line Activity

Command-line activity provides another source of security telemetry that can be investigated through SIEM data.

The objective is to identify command-line-related events and understand their relationship with other activity occurring on the monitored system.

### Investigation Focus

- Host
- User
- Command activity
- Timestamp
- Parent or related events where available
- Activity sequence
- Related authentication or system events

The investigation should consider command-line activity in context rather than treating every command as malicious.

**MITRE ATT&CK:**  
[T1059 — Command and Scripting Interpreter](https://attack.mitre.org/techniques/T1059/)

---

## 5. Suspicious Authentication Patterns

Beyond individual failed login events, authentication telemetry can be analyzed for patterns that require further investigation.

Examples include:

- Multiple failures from the same source
- Multiple usernames targeted by one source
- Repeated authentication attempts within a short period
- Authentication activity occurring at unusual times
- Failed attempts followed by successful authentication
- Repeated activity against the same service

### Example SPL

```spl
index=* "Failed password"
| stats count by src_ip
| sort - count
```

This can be used as an initial investigation query before narrowing the search to a specific source IP or time period.

---

## 6. Event Correlation and Timeline Analysis

Individual events often provide limited context. The lab therefore also focuses on correlating related events to understand the sequence of activity.

For example:

```text
Reconnaissance
      ↓
Authentication Attempts
      ↓
System Activity
      ↓
Related Events
      ↓
Timeline Reconstruction
```

The analyst can search around the relevant timestamp and correlate:

- Source IP
- Destination host
- Username
- Service
- Event type
- Authentication activity
- Network activity

This approach helps move the investigation from **individual log entries to an activity timeline**.

---

## 7. Additional Security-Testing Scenarios

Additional security-testing activities were performed to generate different types of telemetry and evaluate Splunk's ability to support investigation.

These exercises focused on:

- Log generation
- Event collection
- Event filtering
- Pattern recognition
- Source identification
- Timeline analysis
- SPL-based investigation
- Security monitoring
- Event correlation

The repository does not document every testing command or activity because the primary purpose of the project is to demonstrate the **defensive monitoring and investigation workflow**.

---

## 🔍 Detection-to-Investigation Workflow

The scenarios in this lab follow a common SOC investigation methodology:

```text
Security Activity
       ↓
Telemetry Generated
       ↓
Splunk Collection
       ↓
Initial Search
       ↓
Identify Source
       ↓
Analyze Events
       ↓
Correlate Related Activity
       ↓
Build Timeline
       ↓
Map Behavior
       ↓
Determine Response
```

This demonstrates the transition from **raw telemetry to a structured security investigation**.

---

# 🔍 Alert Investigation Workflow

When suspicious or interesting activity is identified, the investigation follows a structured SOC workflow.

## Step 1 — Identify the Event

Review:

- Event type
- Timestamp
- Source
- Destination
- Log message
- Related activity

---

## Step 2 — Identify the Source

Determine:

- Source IP
- Destination IP
- Username
- Host
- Service
- Network connection

---

## Step 3 — Understand the Activity

Determine what happened by examining the available telemetry:

- What occurred?
- When did it occur?
- Which system generated the event?
- Which source generated the activity?
- Which service was involved?
- Was the activity repeated?

---

## Step 4 — Search Related Events

Use Splunk to investigate activity around the relevant timestamp.

Review:

- Previous events
- Subsequent events
- Related source IP activity
- Related usernames
- Related services
- Repeated patterns

This helps build a timeline and provides additional context around the event.

---

## Step 5 — Analyze the Pattern

Use SPL and surrounding telemetry to determine whether the observed activity represents:

- Normal activity
- Repeated activity
- Abnormal behavior
- Suspicious behavior
- Security-testing activity

The conclusion should be based on the available evidence rather than the initial alert or event name alone.

---

## Step 6 — Determine Response

Depending on the investigation results, possible defensive actions include:

- Continuing to monitor the source
- Investigating affected accounts
- Reviewing additional logs
- Blocking suspicious activity
- Collecting additional evidence
- Searching for related events
- Continuing monitoring for similar activity

---

# 🧩 MITRE ATT&CK Mapping

Observed behaviors can be mapped to the **MITRE ATT&CK framework** to provide additional context during detection and investigation.

| Observed Activity | MITRE ATT&CK Technique |
|---|---|
| Network service scanning | [T1046 — Network Service Scanning](https://attack.mitre.org/techniques/T1046/) |
| Repeated authentication attempts | [T1110 — Brute Force](https://attack.mitre.org/techniques/T1110/) |
| Command-line activity | [T1059 — Command and Scripting Interpreter](https://attack.mitre.org/techniques/T1059/) |

Technique selection should be based on the behavior observed in the available telemetry rather than solely on the name of an alert or event.

This provides a connection between individual SIEM detections and broader adversary behavior.

---

# 📊 Dashboards and Visualization

Splunk dashboards are used to transform security events into visual information that can be reviewed efficiently during monitoring and investigation.

## HTTP Logs Dashboard

Provides visualization of HTTP and web-related activity.

![HTTP Logs Dashboard](screenshots/http-logs-dashboard.png)

---

## SSH Logs Dashboard

Provides visualization of authentication and SSH-related activity.

![SSH Logs Dashboard](screenshots/ssh-logs-dashboard.png)

---

### Dashboard Visibility

The dashboards provide visibility into:

- Event volume
- Activity trends
- Authentication activity
- HTTP activity
- Security events
- Investigation context

Dashboards make frequently reviewed security information easier to understand during SOC monitoring.

---

# 📁 Repository Structure

```text
Splunk-Home-Lab/
│
├── README.md
├── setup.md
├── .gitignore
│
├── dashboards/
│   └── README.md
│
└── screenshots/
    ├── http-logs-dashboard.png
    └── ssh-logs-dashboard.png
```

### Repository Components

| File / Directory | Purpose |
|---|---|
| `README.md` | Project documentation and overview |
| `setup.md` | Installation and lab configuration instructions |
| `.gitignore` | Prevents unnecessary or sensitive files from being committed |
| `dashboards/` | Dashboard-related documentation |
| `screenshots/` | Screenshots demonstrating the Splunk dashboards |

---

# 🧪 Troubleshooting

<details>
<summary><b>Splunk is not starting</b></summary>

Check the following:

- Splunk service status
- Splunk logs
- Available system resources
- Splunk installation directory
- Port availability
- Configuration errors

</details>

<details>
<summary><b>No logs are appearing in Splunk</b></summary>

Check:

- Log source configuration
- Monitored files
- File permissions
- Splunk monitoring configuration
- Log file activity
- Splunk service status

</details>

<details>
<summary><b>Search returns no results</b></summary>

Check:

- Index name
- Search time range
- SPL syntax
- Source type
- Field names
- Whether new events are being generated

A broad search such as the following can help confirm whether events are being indexed:

```spl
index=*
```

</details>

<details>
<summary><b>Authentication logs are not visible</b></summary>

Check:

- Authentication log location
- File permissions
- Splunk monitoring configuration
- Log generation
- User permissions

The Splunk service must have appropriate permissions to read the logs being monitored.

</details>

<details>
<summary><b>Fields are not extracted correctly</b></summary>

Review:

- Source type
- Field extraction
- Log format
- Search syntax
- Available event fields

Start with raw events to understand the structure of the collected data before creating more complex SPL searches.

</details>

---

# 🧠 Skills Demonstrated

## Security Monitoring

- SIEM monitoring
- Log collection
- Security event analysis
- Event filtering
- Activity monitoring
- Alert investigation

## Log Analysis

- Authentication log analysis
- SSH log analysis
- HTTP log analysis
- Event correlation
- Timeline analysis
- Source identification

## Detection Engineering

- Splunk searches
- SPL development
- Event filtering
- Pattern identification
- Detection logic
- Search optimization

## Network Security

- Network reconnaissance
- Source and destination analysis
- Port analysis
- Network activity investigation
- Security testing

## Visualization

- Splunk dashboards
- Security-event visualization
- Activity-trend analysis
- Authentication monitoring
- HTTP monitoring

## Incident Response

- Event investigation
- Evidence collection
- Event correlation
- Activity identification
- Timeline development
- Response planning

## Threat Intelligence and Frameworks

- MITRE ATT&CK
- Attack-technique identification
- Security-event classification
- Detection-to-technique mapping

---

# 📚 Key Takeaways

This project provided practical experience with the complete SIEM monitoring workflow:

```text
Security Activity
       ↓
Log Generation
       ↓
Telemetry Collection
       ↓
Splunk
       ↓
SPL Search
       ↓
Detection
       ↓
Investigation
       ↓
Visualization
       ↓
Response
```

The lab helped bridge the gap between cybersecurity theory and practical SOC operations by requiring the analysis of security telemetry rather than focusing only on individual tools.

Working with Splunk also provided hands-on experience with **SPL**, including searching, filtering, aggregation, event analysis, event correlation, and investigation.

---

# 🏁 Conclusion

The **Splunk Security Monitoring Lab** provides a practical environment for developing foundational SOC and SIEM skills.

By combining:

- Splunk Enterprise
- SPL
- Linux security logs
- Authentication telemetry
- HTTP logs
- Network-security testing
- Security investigations
- Event correlation
- MITRE ATT&CK mapping
- Splunk dashboards

the project demonstrates a practical workflow for moving from **raw security telemetry to structured investigation and security monitoring**.

The lab provides a foundation for further development in:

- Threat hunting
- Detection engineering
- Incident response
- SIEM engineering
- Cloud security
- Security automation
- SOC operations

---

# 👨‍💻 Author

**Mohammad Danish**

**B.Tech — Information Technology**

Cybersecurity | SOC | Blue Team | Cloud Security

---

⭐ **If you find this project useful, feel free to explore the repository and learn from the implementation.**
