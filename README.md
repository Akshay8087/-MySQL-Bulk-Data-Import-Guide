# 🚀 Importing Large CSV Files into MySQL via Command Line

This guide explains how to efficiently import large **CSV datasets** into **MySQL** using the command line (`mysql` CLI).  
It’s especially useful when GUI tools (like Workbench) fail or hang due to file size.  

---

## 📌 Prerequisites
- **MySQL Server 8.0+** installed  
- MySQL **bin** directory added to your `PATH` *(or navigate to it manually)*  
- CSV file ready (with headers in the first row)  
- Database and table already created with proper schema  

---

## ⚙️ Step 1: Open MySQL CLI
```bash
cd "C:\Program Files\MySQL\MySQL Server 8.0\bin"
mysql --local-infile=1 -u root -p
```
- **Replace root with your MySQL username.**

## - ⚙️ Step 2: Select Your Database

```SQl
SHOW DATABASES;
USE churn;
```


## ⚙️ Step 3: Import the CSV File
```sql
LOAD DATA LOCAL INFILE 'D:\\Games\\Telco-Customer-Churn.csv'
INTO TABLE `telco-customer-churn`
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\r\n'
IGNORE 1 ROWS;
```

## ✅ `LOAD DATA LOCAL INFILE` Command Breakdown

- This table explains the purpose of each clause used in the bulk CSV import command.

| Clause | Purpose | Rationale |
| :--- | :--- | :--- |
| **`LOCAL INFILE`** | Instructs MySQL to read the file directly from the **client's file system** (your computer). | Necessary for security and efficiency when importing from a local machine. |
| **`FIELDS TERMINATED BY ','`** | Defines the **delimiter** that separates values into columns. | Standard for **CSV** (Comma Separated Values) files. |
| **`ENCLOSED BY '\"'`** | Specifies the character that wraps text strings. | Essential for correctly reading data where a column value itself contains the delimiter (e.g., `"Smith, John"`). |
| **`LINES TERMINATED BY '\r\n'`** | Defines the character sequence that marks the end of a row. | This is the standard line ending used by **Windows** operating systems. |
| **`IGNORE 1 ROWS`** | Tells the server to skip a specified number of rows at the start of the file. | Used to efficiently **skip the header row** (column names) so it isn't imported as data. |



---

## ⚙️ Step 4: Verify Import

```sql
SELECT COUNT(*) FROM `telco-customer-churn`;
```

## If In case Show An Error 

### 🛠️ Troubleshooting

- local_infile disabled? → Enable it in CLI:

 ```bash
mysql --local-infile=1 -u root -p
```

## Warnings shown?

```sql
SHOW WARNINGS LIMIT 10;
```

## .File not found error?
- 👉 Use absolute path with double \\ in Windows

## 📂 Example Output

```bash
Query OK, 7043 rows affected, 21140 warnings (0.16 sec)
Records: 7043  Deleted: 0  Skipped: 0  Warnings: 21140
```


## 🌟 Pro Tips

- For very large files, consider splitting CSV into smaller chunks

- Use LOAD DATA INFILE (without LOCAL) if the file is already placed in MySQL server’s directory

- Optimize table schema (indexes, datatypes) before loading for speed


# 💻 Importing CSV into MySQL (Command Line Session)

```bash
Microsoft Windows [Version 10.0.26100.6584]
(c) Microsoft Corporation. All rights reserved.

C:\Users\sajan>cd C:\Program Files\MySQL\MySQL Server 8.0\bin

C:\Program Files\MySQL\MySQL Server 8.0\bin>mysql --local-infile=1 -u root -p
Enter password: **********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 17
Server version: 8.0.43 MySQL Community Server - GPL

Copyright (c) 2000, 2025, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```
###🔹 Show Available Databases


```sql
mysql> SHOW DATABASES;
```

### Output
```
+--------------------+
| Database           |
+--------------------+
| churn              |
| gym                |
| information_schema |
| mysql              |
| new_schema         |
| pan_validation     |
| performance_schema |
| sys                |
+--------------------+
8 rows in set (0.00 sec)
```

### 🔹 Select Database --using Database present in MySql use Your Database Here
```sql
mysql> USE Churn;
```

```sql
Database changed
```
