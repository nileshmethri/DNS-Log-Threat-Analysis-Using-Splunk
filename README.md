# 📘 DNS Log Threat Analysis Using Splunk — project

## 🔍 Project Overview

This project demonstrates DNS log threat analysis using Splunk SPL. The objective is to detect suspicious DNS behavior, phishing domains, and malware-related indicators by analyzing structured DNS log data. The project simulates SOC L1 investigation workflows using statistical and time-based queries.

---

## 🎯 Objectives

* Ingest DNS logs into Splunk
* Perform field extraction and validation
* Analyze DNS query behavior
* Detect suspicious domains and risky TLDs
* Identify NXDOMAIN spikes and DGA indicators
* Build SOC L1 investigation queries

---

## 🧰 Tools & Technologies

* Splunk Enterprise
* SPL (Search Processing Language)
* JSON DNS log dataset
* Search & Reporting App

---

## 📂 Dataset Details

* Format: JSON
* Log Type: DNS queries
* Fields used:

  * timestamp
  * src_ip
  * dst_ip
  * fqdn
  * query_type
  * response_code
  * bytes
* Dataset size: Large simulated dataset for SOC practice

---

## ⚙️ Data Ingestion Steps

1. Open Splunk → Settings → Add Data → Upload
2. Upload DNS JSON log file
3. Select sourcetype = `_json`
4. Set timestamp field
5. Index into `main`
6. Verify fields in Events view

---

## 🔎 Tasks Performed

### 1.Top Queried Domains
Description:
This query lists domains by total query count to identify the most frequently requested domains in DNS traffic. It helps establish a baseline of normal domain usage and highlights unusually high-frequency domains that may indicate beaconing or suspicious repeated connections.
```spl
index=main sourcetype=_json
| stats count by fqdn
| sort -count
```
Screenshot: ![Image Alt](https://github.com/nileshmethri/DNS-Log-Threat-Analysis-Using-Splunk/blob/af5923aca6d1c11649e3d740bedca83f01edf4d0/dns1.png)
### 2.High DNS Activity Hosts
Description:
This query identifies source IP addresses generating the highest number of DNS requests. It is used to detect potentially compromised or misconfigured systems that produce excessive DNS traffic, which is a common SOC L1 investigation starting point.
```spl
index=main sourcetype=_json
| stats count by src_ip
| sort -count
```

### 3.NXDOMAIN Detection
Description:
This query filters failed DNS resolutions (NXDOMAIN responses) and counts them per host. A high number of NXDOMAIN results may indicate DGA-based malware, failed command-and-control callbacks, or suspicious automated domain generation behavior.
```spl
index=main sourcetype=_json response_code=NXDOMAIN
| stats count by src_ip
```

### 4.Suspicious Domain Hunt
Description:
This query hunts for domains with high-risk or uncommon top-level domains. Such TLDs are frequently associated with phishing and malware infrastructure. It supports threat hunting and IOC-based investigations in SOC workflows.
```spl
index=main sourcetype=_json
| where like(fqdn,"%.xyz") OR like(fqdn,"%.ru") OR like(fqdn,"%.top")
```

### 5.DNS Traffic Trend
Description:
This query visualizes DNS query volume over time in 5-minute intervals. It helps analysts detect traffic spikes, beaconing patterns, or unusual activity windows and supports timeline-based incident triage.
```spl
index=main sourcetype=_json
| timechart span=5m count
```

---

## 🚨 Detection Use Cases

* DGA / malware beaconing detection
* Phishing domain identification
* Risky TLD monitoring
* NXDOMAIN spike analysis
* Abnormal DNS volume detection

---

## 📊 SOC L1 Skills Demonstrated

* Log ingestion & validation
* SPL query writing
* Threat hunting
* IOC pattern detection
* Security event triage support

---

## ✅ Outcome

Successfully performed DNS threat analysis using Splunk and created SOC L1 detection queries to identify suspicious DNS activity patterns.

---
