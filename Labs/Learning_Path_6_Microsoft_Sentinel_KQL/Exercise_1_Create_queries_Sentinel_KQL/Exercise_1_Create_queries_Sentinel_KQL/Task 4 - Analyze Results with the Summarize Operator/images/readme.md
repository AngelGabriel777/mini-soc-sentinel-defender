# 📊 Task 4 – Analyze Results in KQL with the `summarize` Operator  
### Microsoft Sentinel / Log Analytics – Hands-On Lab

---

## 🎯 Objective

In this lab, we analyze authentication and security data using the **`summarize` operator in KQL (Kusto Query Language)**.

You will learn how to:

- Aggregate data
- Count events
- Calculate distinct values
- Detect suspicious patterns
- Extract latest and oldest records
- Generate lists and sets
- Understand pipeline execution order

---

# 🏗️ Environment

## Platform
- Microsoft Azure
- Log Analytics Workspace
- Microsoft Sentinel

## Available Tables

### LogManagement
- `SigninLogs`
- `AADManagedIdentitySignInLogs`
- `AADNonInteractiveUserSignInLogs`
- `AuditLogs`
- `MicrosoftGraphActivityLogs`
- `Usage`

### Microsoft Sentinel
- `SecurityAlert`
- `SecurityIncident`

> ⚠ Note: This environment uses native Azure tables. We will primarily use `SigninLogs`.

---

# 🔎 Step 1 – Count Sign-ins per Application

## 🎯 Goal
Determine how many authentication events occurred per application in the last 7 days.

## 🧪 Query

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize count() by AppDisplayName
```

## 📝 Explanation

- `where TimeGenerated > ago(7d)` → Filters last 7 days.
- `summarize count()` → Counts total events.
- `by AppDisplayName` → Groups results by application.

## 🔐 Security Insight

Helps identify:
- Most used applications
- Abnormal spikes in authentication activity

---

# 🔎 Step 2 – Count Sign-ins by Client Type and Application

## 🎯 Goal
Analyze how users are authenticating (Browser, Mobile, etc.).

## 🧪 Query

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize cnt=count() by ClientAppUsed, AppDisplayName
```

## 📝 Explanation

- `ClientAppUsed` → Indicates authentication method.
- `cnt=count()` → Renames the count column.

## 🔐 Security Insight

Detect:
- Suspicious authentication methods
- Unexpected legacy authentication usage

---

# 🔎 Step 3 – Count Distinct IP Addresses

## 🎯 Goal
Identify how many different IP addresses were used for sign-ins.

## 🧪 Query

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize dcount(IPAddress)
```

## 📝 Explanation

- `dcount()` → Approximate distinct count.
- Measures number of unique IP addresses.

## 🔐 Security Insight

A high number of IP addresses may indicate:
- Account compromise
- Credential sharing
- Distributed attack attempts

---

# 🔎 Step 4 – Detect Disabled Account Login Attempts

## 🎯 Goal
Identify login attempts from disabled accounts across multiple applications.

## 🧪 Query

```kql
let timeframe = 30d;
let threshold = 1;
SigninLogs
| where TimeGenerated >= ago(timeframe)
| where ResultDescription has "disabled"
| summarize applicationCount = dcount(AppDisplayName) by UserPrincipalName, IPAddress
| where applicationCount >= threshold
```

## 📝 Explanation

- `let` → Defines reusable variables.
- Filters disabled account errors.
- Counts distinct applications involved.
- Applies threshold logic.

## 🔐 Security Insight

Possible indicators:
- Automated attack
- Misconfigured account
- Account abuse attempt

---

# 🔎 Step 5 – Retrieve Most Recent Sign-in (arg_max)

## 🎯 Goal
Get the latest authentication event per user.

## 🧪 Query

```kql
SigninLogs
| summarize arg_max(TimeGenerated, *) by UserPrincipalName
```

## 📝 Explanation

- `arg_max(TimeGenerated, *)`
- Returns the full row with the most recent timestamp.

## 🔐 Security Insight

Useful for:
- Incident response
- Timeline reconstruction
- Last activity verification

---

# 🔎 Step 6 – Retrieve Oldest Sign-in (arg_min)

## 🎯 Goal
Identify the first recorded authentication event per user.

## 🧪 Query

```kql
SigninLogs
| summarize arg_min(TimeGenerated, *) by UserPrincipalName
```

## 📝 Explanation

- Returns earliest recorded event per user.

## 🔐 Security Insight

Useful for:
- Historical analysis
- Baseline behavior studies

---

# 🔎 Step 7 – Understanding Pipeline Order

## 🧪 Query 1

```kql
SigninLogs
| summarize arg_max(TimeGenerated, *) by UserPrincipalName
| where ResultType == 0
```

### Meaning

1. Get latest event per user.
2. Filter only successful logins.

👉 Shows users whose last activity was successful.

---

## 🧪 Query 2

```kql
SigninLogs
| where ResultType == 0
| summarize arg_max(TimeGenerated, *) by UserPrincipalName
```

### Meaning

1. Filter successful logins first.
2. Get most recent successful login.

👉 Shows most recent successful login for each user.

---

## ⚠ Key Difference

| Query 1 | Query 2 |
|----------|----------|
| Last activity was login | Last login event |
| More restrictive | Broader |

---

# 🔎 Step 8 – Using make_list()

## 🎯 Goal
Generate full list of applications used by each user.

## 🧪 Query

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize make_list(AppDisplayName) by UserPrincipalName
```

## 📝 Explanation

- Returns JSON array.
- Includes duplicate entries.

## Example Output

```json
["Azure Portal","Azure Portal","Teams","Outlook"]
```

---

# 🔎 Step 9 – Using make_set()

## 🎯 Goal
Generate unique application list per user.

## 🧪 Query

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize make_set(AppDisplayName) by UserPrincipalName
```

## 📝 Explanation

- Removes duplicates.
- Returns distinct values only.

## Example Output

```json
["Azure Portal","Teams","Outlook"]
```

---

# 🔎 Bonus – Analyze Security Alerts

## 🧪 Query

```kql
SecurityAlert
| summarize count() by Severity
```

## 🎯 Goal

Analyze alerts distribution by severity level.

---

# 🧠 Key Concepts Learned

- `summarize`
- `count()`
- `dcount()`
- `arg_max()`
- `arg_min()`
- `make_list()`
- `make_set()`
- Importance of pipeline execution order

---

# 🏁 Conclusion

This lab demonstrates how the `summarize` operator enables:

- Efficient aggregation  
- Behavioral analysis  
- Threat detection logic  
- Security monitoring optimization  

Mastering these aggregation functions is essential for:

- Microsoft Sentinel analysts  
- SOC analysts  
- SC-200 certification preparation  
- Threat hunting  

---
