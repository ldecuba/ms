# Basics of T-SQL in Microsoft Fabric

## 📌 Introduction
T-SQL (Transact-SQL) is Microsoft's extendable SQL language and is used in database systems like **Microsoft Fabric**. This document teaches the basic principles of **T-SQL in Fabric**, including how to use **Lakehouses and SQL Endpoints**.

---

## 🎯 1. Basic Structure of a T-SQL Query
A simple **SELECT** query:

```sql
SELECT column1, column2
FROM tablename
WHERE condition
ORDER BY column1 DESC;
```

Example: Select all active customers.

```sql
SELECT CustomerID, Name, Status
FROM Customers
WHERE Status = 'Active'
ORDER BY Name ASC;
```

---

## 🏗 2. Microsoft Fabric SQL Options
In Microsoft Fabric, you work with **Lakehouses and SQL Endpoints**:

- **Lakehouse**: Storage of **Delta tables** in a data lake.
- **SQL Endpoint**: A layer on top of the Lakehouse that lets you use **T-SQL**.

Example: **List tables in your Fabric SQL Endpoint**:

```sql
SELECT * FROM INFORMATION_SCHEMA.TABLES;
```

View columns of a specific table:

```sql
SELECT * 
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'Sales';
```

---

## 🔍 3. Working with Delta Tables in Fabric

### 3.1 Retrieving Data from a Delta Table
```sql
SELECT * FROM lakehouse.dbo.Sales;
```

**With filtering:**
```sql
SELECT CustomerID, TotalAmount
FROM lakehouse.dbo.Sales
WHERE TotalAmount > 500;
```

---

### 3.2 Data Manipulation (INSERT, UPDATE, DELETE)
**Insert data:**
```sql
INSERT INTO lakehouse.dbo.Sales (SaleID, CustomerID, TotalAmount, SaleDate)
VALUES (101, 5, 200.50, '2024-03-24');
```

**Update data:**
```sql
UPDATE lakehouse.dbo.Sales
SET TotalAmount = 250.75
WHERE SaleID = 101;
```

**Delete data:**
```sql
DELETE FROM lakehouse.dbo.Sales
WHERE SaleID = 101;
```

---

## 📊 4. Advanced Functions in Fabric T-SQL

### 4.1 Window Functions
Give each sale a rank based on revenue:
```sql
SELECT CustomerID, TotalAmount,
       RANK() OVER (ORDER BY TotalAmount DESC) AS SalesRank
FROM lakehouse.dbo.Sales;
```

---

### 4.2 Aggregate Functions
```sql
SELECT CustomerID, SUM(TotalAmount) AS TotalSpent
FROM lakehouse.dbo.Sales
GROUP BY CustomerID
ORDER BY TotalSpent DESC;
```

---

### 4.3 Joins in Microsoft Fabric
Combine customer data with sales data:
```sql
SELECT c.CustomerID, c.Name, s.TotalAmount
FROM lakehouse.dbo.Customers c
JOIN lakehouse.dbo.Sales s ON c.CustomerID = s.CustomerID;
```

---

## 🕒 5. Microsoft Fabric Specific SQL Options

### **Time Travel in Delta Lake**
View historical data at a specific point in time:
```sql
SELECT * 
FROM lakehouse.dbo.Sales 
TIMESTAMP AS OF '2024-03-20 10:00:00';
```

---

## ⚡ 6. Best Practices for T-SQL in Fabric

✅ Use **SQL Endpoints** to interact with your data.  
✅ Optimize queries with **WHERE**, **LIMIT**, and **PARTITION BY**.  
✅ Use **Time Travel** for historical data without overwriting.  
❌ Avoid overusing `SELECT *`, as it may impact performance.  

---

## 📌 Summary

- **T-SQL in Microsoft Fabric** works similarly to SQL Server, but is optimized for **Delta tables**.
- **SQL Endpoints** make it easy to run queries on your **Lakehouse**.
- **Use Time Travel and aggregate functions** for powerful data analysis.

