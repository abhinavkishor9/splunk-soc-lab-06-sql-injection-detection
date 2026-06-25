# Investigation Notes

## Overview

This document records findings from the SQL Injection Detection investigation.

---

# Dataset Information

| Field | Value |
|---------|---------|
| SIEM Platform | Splunk Enterprise |
| Log Source | Apache Access Logs |
| Source File | sql_Injection_Lab.log |
| Index | main |

---

# Investigation Queries

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

## SQL Injection Indicators

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

## Attack Timeline

```spl
index=main source="sql_Injection_Lab.log"
("UNION" OR "SELECT" OR "' OR" OR "--" OR "DROP")
| timechart count
```

---

# Findings

## Total Events

```text
Document Results
```

## SQL Injection Events Identified

```text
Document Results
```

## Top Attacking IP Addresses

```text
Document Results
```

## Most Targeted Resources

```text
Document Results
```

## HTTP Status Code Distribution

```text
Document Results
```

## Automated Tools Identified

```text
Document Results
```

---

# Analyst Assessment

Determine:

- Attack techniques observed
- Targeted applications
- Evidence of automated tooling
- Attacker infrastructure
- Potential impact

---

# Conclusion

Summarize findings, attack patterns observed, and recommended next steps.
