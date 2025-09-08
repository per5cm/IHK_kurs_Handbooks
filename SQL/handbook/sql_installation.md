# SQLite Handbook (Windows + CSV Import)

## 0) What we're using

-   **OS:** Windows 10/11
-   **Shell:** PowerShell (or Windows Terminal)
-   **DB:** SQLite (`sqlite3` CLI)

------------------------------------------------------------------------

## 1) Install SQLite on Windows

**Option A --- Portable (fast):** 1. Download the **sqlite-tools** ZIP
from the official site (contains `sqlite3.exe`). 2. Extract to a folder
you own, e.g. `C:\Tools\sqlite`. 3. Add that folder to your **PATH**: -
Start -> *Edit the system environment variables* -> *Environment
Variables...* - Under *User variables* -> select **Path** -> *Edit* ->
*New* -> `C:\Tools\sqlite` -> OK.

**Verify:**

``` powershell
sqlite3 -version
```

------------------------------------------------------------------------

## 2) Create/Open a database

``` powershell
cd "C:\Users\Eric\Desktop\Projects\CS50x\Lecture\Lecture_7\database"
sqlite3 favoritesdb
```

Exit any time with:

``` sql
.quit
```

------------------------------------------------------------------------

## 3) Import a CSV

``` sql
.mode csv
.import "C:/Users/Eric/Desktop/Projects/CS50x/Lecture/Lecture_7/favorites.csv" favorites
```

-   Forward slashes `/` recommended in paths.
-   `favorites` = table name.
-   To skip headers (if supported):

``` sql
.import --skip 1 "C:/.../favorites.csv" favorites
```

**Check:**

``` sql
.headers on
.mode column
SELECT COUNT(*) AS rows FROM favorites;
SELECT * FROM favorite LIMIT 10;
```

------------------------------------------------------------------------

## 4) Define a schema first (optional)

``` sql
DROP TABLE IF EXISTS favorites;
CREATE TABLE favorites (
  id INTEGER PRIMARY KEY,
  name TEXT,
  category TEXT,
  rating INTEGER,
  created_at TEXT
);

.mode csv
.import --skip 1 "C:/.../favorites.csv" favorites
```

------------------------------------------------------------------------

## 5) Everyday query cheat-sheet

``` sql
PRAGMA table_info(favorites);
SELECT COUNT(*) FROM favorites;
SELECT * FROM favorites WHERE category = 'movie' LIMIT 20;
SELECT name, rating FROM favorites ORDER BY rating DESC LIMIT 10;
SELECT category, COUNT(*) FROM favorites GROUP BY category ORDER BY COUNT(*) DESC;
CREATE INDEX IF NOT EXISTS idx_favorites_category ON favorites(category);
UPDATE favorites SET rating = 10 WHERE name = 'Blade Runner';
DELETE FROM favorites WHERE rating < 3;
```

------------------------------------------------------------------------

## 6) Import/Export

**TSV:**

``` sql
.mode tabs
.import "C:/path/file.tsv" favorites
```

**Export:**

``` sql
.headers on
.mode csv
.output "C:/Users/Eric/Desktop/out.csv"
SELECT name, rating FROM favorites WHERE category = 'game';
.output stdout
```

------------------------------------------------------------------------

## 7) Path & Windows quirks

-   Use `/` or `\` in paths.
-   If `.import` fails, check if table exists or if headers are
    included.
-   Everything defaults to TEXT unless schema is pre-created.

------------------------------------------------------------------------

## 8) One-liner import

``` powershell
sqlite3 "C:\Users\Eric\Desktop\Projects\CS50x\Lecture\Lecture_7\database\favoritesdb" `
".mode csv" ".import --skip 1 'C:/Users/Eric/Desktop/Projects/CS50x/Lecture/Lecture_7/favorites.csv' favorites" `
".headers on" ".mode column" "SELECT COUNT(*) AS rows FROM favorites;" ".quit"
```

------------------------------------------------------------------------

## 9) GUI option

Install **DB Browser for SQLite** if you want a visual tool.

------------------------------------------------------------------------

## 10) Checklist

-   [x] `sqlite3 -version` works\
-   [x] `cd ...\Lecture_7\database`\
-   [x] `sqlite3 favoritesdb`\
-   [x] `.mode csv`\
-   [x] `.import ... favorites`\
-   [x] `SELECT COUNT(*) FROM favorites;`
