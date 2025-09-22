# SQL Equivalents for Microsoft 365 Defender Queries (MySQL)

This repository mirrors the structure of your original KQL collection and provides **MySQL-compatible SQL** versions of each query.

## 📂 Structure

```
SQL/
└── (mirrors original KQL folders and filenames, with `.sql` instead of `.kql`)
```

Each `.sql` file includes, at the top, a comment block with the **original KQL** for side-by-side reference.

## 🔧 Engine Target

**MySQL** (8.0+ recommended).

### Common Translations

| KQL | MySQL |
|-----|-------|
| `ago(7d)` | `NOW() - INTERVAL 7 DAY` |
| `where col startswith "x"` | `col LIKE 'x%'` |
| `where col endswith "y"` | `col LIKE '%y'` |
| `where col contains "z"` | `col LIKE '%z%'` |
| `has_any("a","b")` | `col REGEXP '(a|b)'` |
| `summarize count() by Col` | `SELECT Col, COUNT(*) FROM ... GROUP BY Col` |
| `countif(cond)` | `SUM(CASE WHEN cond THEN 1 ELSE 0 END)` |
| `dcount(Col)` | `COUNT(DISTINCT Col)` |

> JSON parsing from KQL (e.g., `parse_json(AdditionalFields)`) has no 1:1 translation. Use `JSON_EXTRACT(json_col, '$.path')` in MySQL.

## ⚠️ Manual Review Recommended

This conversion is heuristic. Please review and adjust:
- Table names (KQL logical tables vs. your actual MySQL table names)
- JSON field paths
- Complex `case(...)` expressions
- Any `let` variables or custom functions

## ▶️ Usage

1. Choose a `.sql` in `SQL/...` that matches your original KQL.
2. Replace `/* <TABLE_OR_SUBQUERY> */` with the correct table or subquery when present.
3. Run on MySQL 8.0+.

## 📜 License

MIT (see `LICENSE`).

