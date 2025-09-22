[README.md](https://github.com/user-attachments/files/22478778/README.md)
# SQL Equivalents for Microsoft 365 Defender Queries (MySQL)

This repository provides **MySQL-compatible SQL versions** of Microsoft 365 Defender hunting queries originally written in **KQL (Kusto Query Language)**.  
The goal is to make these queries accessible for environments where MySQL is the analysis backend instead of the Microsoft 365 Defender portal.

## ⚠️ Disclaimer

These SQL queries are a **rough conversion** from the original KQL.  
They are intended as a **starting point** for analysts and engineers.  
Before use in production, please review and adapt:
- Table names (KQL logical tables do not directly exist in MySQL).
- JSON parsing (`parse_json` in KQL → `JSON_EXTRACT` in MySQL).
- Any custom functions, variables (`let`), or case logic.

---

## 📂 Repository Structure

```
SQL/
├─ ASR/
│  ├─ ASR_BlockedEvents_Detail_7d.sql
│  └─ ASR_BlockedEvents_Summary_7d.sql
├─ Defender-Health-Reports/
│  ├─ Defender_AV_Signature_Engine_Report.sql
│  └─ Endpoint_Agent_Health_Status.sql
├─ Auth-Logons-Identity/
│  ├─ NTLM_Successful_Network_Logons.sql
│  └─ AD_Sensitive_Group_Modifications.sql
...
```

Each `.sql` file includes, at the top, a comment block with the **original KQL** for side-by-side reference.

---
## 📑 Query Index

### ASR/
- [ASR_BlockedEvents_Detail_7d.SQL](ASR/ASR_BlockedEvents_Detail_7d.SQL) — Detailed list of ASR block events in the last 7 days.
- [ASR_BlockedEvents_Summary_7d.SQL](ASR/ASR_BlockedEvents_Summary_7d.SQL) — Summarized count of ASR events by category.
- [ASR_Filename_Search_7d.SQL](ASR/ASR_Filename_Search_7d.SQL) — Search ASR events for specific file names.

### ExploitGuard-NetworkProtection/
- [Network_Protection_Exploit_Guard.SQL](ExploitGuard-NetworkProtection/Network_Protection_Exploit_Guard.SQL) — List of Exploit Guard / Network Protection events.

### Auth-Logons-Identity/
- [AD_Sensitive_Group_Modifications.SQL](Auth-Logons-Identity/AD_Sensitive_Group_Modifications.SQL) — Detect modifications to sensitive AD groups.
- [NTLM_Successful_Network_Logons.SQL](Auth-Logons-Identity/NTLM_Successful_Network_Logons.SQL) — Successful NTLM network logon attempts.

### Defender-Health-Reports/
- [Defender_AV_Health_Status_6h.SQL](Defender-Health-Reports/Defender_AV_Health_Status_6h.SQL) — Unified Defender AV health report (mode, signatures, engine, platform).
- [Endpoint_Agent_Health_Status.SQL](Defender-Health-Reports/Endpoint_Agent_Health_Status.SQL) — Status and health of Defender agents across endpoints.
- [Defender_SecureConfig_BestPracticeReport.SQL](Defender-Health-Reports/Defender_SecureConfig_BestPracticeReport.SQL) — Report on Defender secure configuration compliance vs. best practices (sensor, tamper protection, PUA, AV, cloud).

### Persistence-Registry/
- [Registry_New_Changed_Keywords.SQL](Persistence-Registry/Registry_New_Changed_Keywords.SQL) — Detect new or modified suspicious registry keys.
- [Registry_AttachmentManager_ScanWithAntiVirus_30d.SQL](Persistence-Registry/Registry_AttachmentManager_ScanWithAntiVirus_30d.SQL) — Monitor changes to the ScanWithAntiVirus flag under Attachment Manager.
- [Registry_Service_Modifications.SQL](Persistence-Registry/Registry_Service_Modifications.SQL) — Detect registry modifications within the Services hive.

### Persistence-Services/
- [Process_ServiceCreation_sc_powershell.SQL](Persistence-Services/Process_ServiceCreation_sc_powershell.SQL) — Detect service creation via sc.exe or PowerShell.
- [Process_ServiceName_CommandLine.SQL](Persistence-Services/Process_ServiceName_CommandLine.SQL) — Detect processes that include ServiceName in their command line.
- [Process_ServicesExe_Children.SQL](Persistence-Services/Process_ServicesExe_Children.SQL) — Detect child processes spawned by services.exe.

### TVM-KEV-Vulns/
- [CISA_Top_Vulns_2019_2023.SQL](TVM-KEV-Vulns/CISA_Top_Vulns_2019_2023.SQL) — Identify devices exposed to top CISA KEV vulnerabilities.

### SCCM-Integration/
- [SCCM_Process_Execution_and_Security_Alert_Join.SQL](SCCM-Integration/SCCM_Process_Execution_and_Security_Alert_Join.SQL) — Join SCCM process execution data with Defender security alerts.
- [SCCM_Process_AlertTimeAligned_15m_30d.SQL](SCCM-Integration/SCCM_Process_AlertTimeAligned_15m_30d.SQL) — Time-align SCCM process and alert data within 15m windows over 30 days.

### Utilities/
- [Filename_Search.SQL](Utilities/Filename_Search.SQL) — Search events for specific filenames.
- [Process_SuspiciousExe_UserPaths.SQL](Utilities/Process_SuspiciousExe_UserPaths.SQL) — Detect execution of .exe files from Temp, AppData, or Public directories.

### _Unsorted/
- *(any queries that don’t yet fit into a category)*
## 🔧 Engine Target

- **MySQL 8.0+** is recommended.  
- Common translations:

| KQL Example | MySQL Equivalent |
|-------------|------------------|
| `ago(7d)` | `NOW() - INTERVAL 7 DAY` |
| `where Col startswith "x"` | `Col LIKE 'x%'` |
| `where Col endswith "y"` | `Col LIKE '%y'` |
| `where Col contains "z"` | `Col LIKE '%z%'` |
| `Col has_any ("a","b")` | `Col REGEXP '(a|b)'` |
| `summarize count() by Col` | `SELECT Col, COUNT(*) FROM … GROUP BY Col;` |
| `countif(cond)` | `SUM(CASE WHEN cond THEN 1 ELSE 0 END)` |
| `dcount(Col)` | `COUNT(DISTINCT Col)` |

---

## ▶️ Usage

1. Navigate to the relevant `.sql` file in the `SQL/` directory.  
2. Replace placeholder `/* <TABLE_OR_SUBQUERY> */` with the actual table or subquery in your schema.  
3. Run in MySQL 8.0+ and refine as needed.  

---

## 📜 License

This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.
