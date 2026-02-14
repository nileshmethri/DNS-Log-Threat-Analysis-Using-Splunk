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

## 🔎 Key SPL Queries

### Top Queried Domains
Description:
This query lists domains by total query count to identify the most frequently requested domains in DNS traffic. It helps establish a baseline of normal domain usage and highlights unusually high-frequency domains that may indicate beaconing or suspicious repeated connections.
```spl
index=main sourcetype=_json
| stats count by fqdn
| sort -count
```

### High DNS Activity Hosts

```spl
index=main sourcetype=_json
| stats count by src_ip
| sort -count
```

### NXDOMAIN Detection

```spl
index=main sourcetype=_json response_code=NXDOMAIN
| stats count by src_ip
```

### Suspicious Domain Hunt

```spl
index=main sourcetype=_json
| where like(fqdn,"%.xyz") OR like(fqdn,"%.ru") OR like(fqdn,"%.top")
```

### DNS Traffic Trend

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
