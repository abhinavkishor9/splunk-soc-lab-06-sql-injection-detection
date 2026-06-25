# Troubleshooting Notes

## Overview

This document records troubleshooting performed during the SQL Injection Detection investigation.

---

# Issue 1

## Problem

No results returned for:

```spl
index=sql_Injection_Lab
| stats count
```

## Cause

The dataset was not stored in a dedicated SQL Injection index.

## Resolution

Verify available indexes:

```spl
index=*
| stats count by index
```

Confirmed dataset was stored in:

```text
index=main
```

---

# Issue 2

## Problem

Unable to locate SQL Injection events.

## Cause

Investigation was initially performed against the wrong index.

## Resolution

Identify source location:

```spl
index=main
| stats count by source
```

Confirmed source:

```text
sql_Injection_Lab.log
```

Use:

```spl
index=main source="sql_Injection_Lab.log"
```

for all investigation queries.

---

# Issue 3

## Problem

Queries returned zero results.

## Cause

Incorrect index name and source selection.

## Resolution

Verify source availability:

```spl
index=main source="sql_Injection_Lab.log"
| stats count
```

Expected result:

```text
928 events
```

---

# Issue 4

## Problem

Fields such as clientip or uri may not appear.

## Cause

Field extraction differences based on sourcetype configuration.

## Resolution

Review extracted fields:

```spl
index=main source="sql_Injection_Lab.log"
| fieldsummary
```

Adjust SPL queries based on available field names.

---

# Verification Queries

## Verify Event Count

```spl
index=main source="sql_Injection_Lab.log"
| stats count
```

## Verify Source

```spl
index=main
| stats count by source
```

## Verify Sourcetype

```spl
index=main source="sql_Injection_Lab.log"
| stats count by sourcetype
```

## Review Sample Events

```spl
index=main source="sql_Injection_Lab.log"
| head 20
```

---

# Lessons Learned

- Always verify the target index after data ingestion.
- Confirm source names before beginning an investigation.
- Validate field extractions before creating detection logic.
- SQL Injection attacks can be identified through common SQL keywords and payload patterns.
- Proper data validation reduces troubleshooting time and improves investigation accuracy.
