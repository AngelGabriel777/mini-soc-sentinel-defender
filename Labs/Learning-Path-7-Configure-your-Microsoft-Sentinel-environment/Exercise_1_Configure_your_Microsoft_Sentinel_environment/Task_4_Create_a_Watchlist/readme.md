# 🧪 SC-200 Lab — Task 4: Create a Watchlist in Microsoft Sentinel

## 🎯 Objective
Create a Watchlist in Microsoft Sentinel containing a list of high-value hosts and validate its successful ingestion.

---

# 🏗️ Environment

| Item | Value |
|------|------|
| Platform | Microsoft Azure |
| Service | Microsoft Sentinel |
| Workspace | defenderWorkspace |
| Experience | Defender Portal (new Sentinel UI) |

---

# 🪜 Procedure

## 1️⃣ Create the CSV File (HighValue.csv)

On the Windows VM:

1. Open **Notepad**
2. Add header:

   ```
   Hostname
   ```

3. Add host values:

   ```
   Host1
   Host2
   Host3
   Host4
   Host5
   ```

4. Click **File → Save As**
5. Configure:
   - Name: `HighValue.csv`
   - Type: All files
   - Location: Documents

✅ File created successfully

---

## 2️⃣ Open Microsoft Sentinel

1. Go to **Azure Portal**
2. Search **Microsoft Sentinel**
3. Select workspace:

   ```
   defenderWorkspace
   ```

📌 The portal shows the message:

> This page has been moved to the Defender portal for the optimal SecOps experience

4. Click **Go to Defender portal**

---

## 3️⃣ Navigate to Watchlists

Inside Defender portal:

Path followed:

```
Microsoft Sentinel → Configuration → Watchlists
```

Screen displayed:

- Watchlists dashboard
- Watchlists = 0
- Watchlist Items = 0

---

## 4️⃣ Start Watchlist Creation

Click:

```
Add new
```

Watchlist Wizard opens with steps:

- General
- Source
- Review + create

---

## 5️⃣ General Configuration

| Field | Value |
|------|------|
| Name | HighValueHosts |
| Description | High Value Hosts |
| Alias | HighValueHosts |

Click **Next**

---

## 6️⃣ Source Configuration

| Setting | Value |
|--------|------|
| Source type | Local file |
| File type | CSV with header |
| Lines before header | 0 |
| Upload file | HighValue.csv |
| SearchKey | Hostname |

📊 File preview showed:

| Hostname |
|---------|
| Host1 |
| Host2 |
| Host3 |
| Host4 |
| Host5 |

Click **Next**

---

## 7️⃣ Review and Create

1. Validate configuration
2. Click **Create**

---

## 8️⃣ Watchlist Validation

After creation:

Dashboard shows:

| Metric | Value |
|-------|------|
| Watchlists | 1 |
| Watchlist Items | 5 |
| Status | Succeeded |
| Rows | 5 |

Details panel shows:

- Source: HighValue.csv
- SearchKey: Hostname
- Created by: user account

✅ Watchlist successfully created

---

# 🔍 Validation in Logs

Open Logs and execute:

```kql
_GetWatchlist('HighValueHosts')
```

Expected result:

```
Hostname
Host1
Host2
Host3
Host4
Host5
```

📸 **Evidence Collected**

✔ Sentinel workspace selected  
✔ Defender portal redirection observed  
✔ Watchlist wizard completed  
✔ CSV preview validated  
✔ Watchlist created with 5 rows  
✔ Status succeeded  

---

# 🧠 Technical Notes

- Watchlists are stored as name-value pairs  
- Cached for fast query performance  
- Can be used in:  
  - Analytics rules  
  - Hunting queries  
  - Workbooks  
  - Playbooks  
  - Incident investigation  

---

# 🏁 Conclusion

The **HighValueHosts** watchlist was successfully created in Microsoft Sentinel using the Defender portal experience.  
The dataset was validated, ingested, and is ready for detection and hunting scenarios.
