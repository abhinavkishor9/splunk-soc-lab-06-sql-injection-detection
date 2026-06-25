# splunk-soc-lab-06-sql-injection-detection
## Overview

This lab demonstrates how to detect and investigate SQL Injection (SQLi) attacks using Splunk Enterprise and Apache access logs.

SQL Injection is one of the most common web application attacks and can allow attackers to bypass authentication, access sensitive data, manipulate databases, and potentially compromise entire applications.

The objective of this lab is to identify SQL Injection attempts, investigate attacker behavior, analyze targeted resources, and develop practical threat hunting skills using Splunk.

---

# Lab Environment

- Splunk Enterprise
- Search & Reporting App
- Apache Access Logs
- Windows Host

---

# Scenario

The SOC team has received reports of suspicious web requests targeting a public-facing application.

As a SOC Analyst, your task is to investigate Apache access logs, identify SQL Injection attempts, determine attack patterns, identify attacker infrastructure, and assess potential impact.

---

# Objectives

- Identify SQL Injection attempts
- Investigate malicious payloads
- Analyze attacker activity
- Identify targeted resources
- Detect automated attack tools
- Build SQL Injection detection logic
- Develop web application threat hunting skills

---

# Data Source

| Source | Events |
|----------|----------|
| sql_Injection_Lab.log | 928 |

---

# MITRE ATT&CK Mapping

| Technique | Description |
|------------|------------|
| T1190 | Exploit Public-Facing Application |
| T1595 | Active Scanning |
| T1595.002 | Vulnerability Scanning |
| T1071.001 | Application Layer Protocol: Web Protocols |

---

# Severity

**High**

SQL Injection attacks can result in authentication bypass, unauthorized database access, sensitive data exposure, and application compromise.

---

# Detection Logic

## Verify Dataset

```spl
index=main source="sql_Injection_Lab.log"
| stats count
```

## UNION-Based SQL Injection

```spl
index=main source="sql_Injection_Lab.log"
"UNION"
```

## SELECT Statements

```spl
index=main source="sql_Injection_Lab.log"
"SELECT"
```

## Authentication Bypass Attempts

```spl
index=main source="sql_Injection_Lab.log"
("' OR '1'='1" OR "OR 1=1")
```

## SQL Comments

```spl
index=main source="sql_Injection_Lab.log"
"--"
```

## Destructive SQL Commands

```spl
index=main source="sql_Injection_Lab.log"
("DROP TABLE" OR "INSERT INTO" OR "UPDATE ")
```

## Common SQL Injection Indicators

```spl
index=main source="sql_Injection_Lab.log"
("UNION" OR "SELECT" OR "' OR" OR "--" OR "DROP")
```

## Top Attacking IP Addresses

```spl
index=main source="sql_Injection_Lab.log"
("UNION" OR "SELECT" OR "' OR" OR "--" OR "DROP")
| stats count by clientip
| sort -count
```

## Targeted Resources

```spl
index=main source="sql_Injection_Lab.log"
("UNION" OR "SELECT" OR "' OR" OR "--" OR "DROP")
| stats count by uri
| sort -count
```

## SQL Injection Timeline

```spl
index=main source="sql_Injection_Lab.log"
("UNION" OR "SELECT" OR "' OR" OR "--" OR "DROP")
| timechart count
```

---

# False Positives

- Internal vulnerability assessments
- Security testing activities
- Penetration testing engagements
- Developer testing environments

---

# Recommended Containment

Block malicious IP addresses, implement parameterized queries, validate user input, review affected applications, and monitor for continued SQL Injection activity.

---

# Skills Demonstrated

- Splunk SPL
- Apache Log Analysis
- SQL Injection Detection
- Threat Hunting
- SIEM Operations
- Security Monitoring
- Web Application Security
- Incident Investigation

---

# Lessons Learned

This lab improved understanding of:

- SQL Injection attack techniques
- Web application threat hunting
- Apache access log analysis
- Detection engineering
- Attacker behavior analysis
- SOC investigation workflows
