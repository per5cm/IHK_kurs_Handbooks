## 0) Database Handbook (SQLite + CSV/Excel Workflow)
**What’s a Database?**
- A **file** that stores data in a structured way (`favorites.db`). 

---

## 1) Folder Structure
Keep order or drown in chaos. Suggested layout:

```
C:\Users\Eric\Databases\
├── CS50x\
│   ├── Lecture7\
│   │   ├── favorites.db
│   │   ├── favorites.csv
│   │   └── exports\
│   └── Lecture8\
├── Playground\       ← experiments
└── Archive\          ← backups/old versions
```

**Rules:**
- `.db` files = live database files.  
- `exports/` = CSV or SQL dumps for sharing/backup.  
- `archive/` = copies with date tags.  

---

## 2) Naming Conventions
- Database files: `favorites.db`, `sales_2025.db`  
- Backups: `favorites_2025-09-17.db`  
- Dumps: `favorites_schema.sql`, `favorites_backup.sql`  
- CSV exports: `favorites_2025-09-17.csv`  

- Lowercase, underscores, and date tags keep life simple.

---

## 3) Create a Database
From PowerShell:

```powershell
cd C:\Users\Eric\Databases\Playground
sqlite3 mytest.db
```

Inside SQLite:

```sql
.database     -- shows the current DB file
.quit         -- exit
```

A `.db` file is created automatically when you connect.

---

## 4) (optional) Import Data from CSV
```sql
.mode csv
.import "C:/Users/<You>/Databases/CS50x/Lecture7/favorites.csv" favorites
```

- `favorites` = table name created on the fly.  
- Use `/` instead of `\` in paths.  

Check in sqlite3:
```sql
.headers on
.mode column
SELECT COUNT(*) AS rows FROM favorites;
```

---

## 5) (optional) Export Data

**Full table → CSV**
```sql
.headers on
.mode csv
.output "C:/Users/Eric/Databases/CS50x/Lecture7/exports/favorites.csv"
SELECT * FROM favorites;
.output stdout
```

**Custom query → CSV**
```sql
.headers on
.mode csv
.output "C:/Users/Eric/Databases/CS50x/Lecture7/exports/movies.csv"
SELECT name, rating FROM favorites WHERE category = 'movie';
.output stdout
```

**SQL dump (schema + data)**
```sql
.output "C:/Users/Eric/Databases/CS50x/Lecture7/exports/favorites_dump.sql"
.dump
.output stdout
```

---

## 6) (optional) Backup Strategy 
- Keep **one master `.db`** per project.  
- Export CSV + SQL dump at milestones.  
- Tag files with dates.  
- Push CSV/dumps to GitHub (skip raw `.db` binaries).  

---

## 7) GUI Option
Install **DB Browser for SQLite**:  
- Open `.db`, run queries, edit rows.  
- Save/export CSV or Excel-friendly files.  

---

[← Back to Index](../index.md)