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
