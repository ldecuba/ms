# Basis van T-SQL in Microsoft Fabric

## 📌 Inleiding
T-SQL (Transact-SQL) is de uitbreidbare SQL-taal van Microsoft en wordt gebruikt in databasesystemen zoals **Microsoft Fabric**. In dit document leer je de basisprincipes van **T-SQL in Fabric**, inclusief hoe je **Lakehouses en SQL Endpoints** gebruikt.

---

## 🎯 1. Basisstructuur van een T-SQL-query
Een eenvoudige **SELECT**-query:

```sql
SELECT kolom1, kolom2
FROM tabelnaam
WHERE voorwaarde
ORDER BY kolom1 DESC;
```

Voorbeeld: Selecteer alle actieve klanten.

```sql
SELECT CustomerID, Name, Status
FROM Customers
WHERE Status = 'Active'
ORDER BY Name ASC;
```

---

## 🏗 2. Microsoft Fabric SQL-opties
Binnen Microsoft Fabric werk je met **Lakehouses en SQL Endpoints**:

- **Lakehouse**: Opslag van **Delta-tables** in een data lake.
- **SQL Endpoint**: Een laag bovenop het Lakehouse waarmee je **T-SQL** gebruikt.

Voorbeeld: **Lijst van tabellen in je Fabric SQL Endpoint opvragen**:

```sql
SELECT * FROM INFORMATION_SCHEMA.TABLES;
```

Bekijk kolommen van een specifieke tabel:

```sql
SELECT * 
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'Sales';
```

---

## 🔍 3. Werken met Delta-tabellen in Fabric

### 3.1 Data ophalen uit een Delta-table
```sql
SELECT * FROM lakehouse.dbo.Sales;
```

**Met filtering:**
```sql
SELECT CustomerID, TotalAmount
FROM lakehouse.dbo.Sales
WHERE TotalAmount > 500;
```

---

### 3.2 Data manipuleren (INSERT, UPDATE, DELETE)
**Toevoegen van data:**
```sql
INSERT INTO lakehouse.dbo.Sales (SaleID, CustomerID, TotalAmount, SaleDate)
VALUES (101, 5, 200.50, '2024-03-24');
```

**Wijzigen van data:**
```sql
UPDATE lakehouse.dbo.Sales
SET TotalAmount = 250.75
WHERE SaleID = 101;
```

**Verwijderen van data:**
```sql
DELETE FROM lakehouse.dbo.Sales
WHERE SaleID = 101;
```

---

## 📊 4. Geavanceerde functies in Fabric T-SQL

### 4.1 Window Functions
Geef iedere verkoop een rang op basis van omzet:
```sql
SELECT CustomerID, TotalAmount,
       RANK() OVER (ORDER BY TotalAmount DESC) AS SalesRank
FROM lakehouse.dbo.Sales;
```

---

### 4.2 Aggregatiefuncties
```sql
SELECT CustomerID, SUM(TotalAmount) AS TotalSpent
FROM lakehouse.dbo.Sales
GROUP BY CustomerID
ORDER BY TotalSpent DESC;
```

---

### 4.3 Joins in Microsoft Fabric
Combineer klantgegevens met verkoopdata:
```sql
SELECT c.CustomerID, c.Name, s.TotalAmount
FROM lakehouse.dbo.Customers c
JOIN lakehouse.dbo.Sales s ON c.CustomerID = s.CustomerID;
```

---

## 🕒 5. Microsoft Fabric Specifieke SQL-opties

### **Time Travel in Delta Lake**
Bekijk historische data op een specifieke tijd:
```sql
SELECT * 
FROM lakehouse.dbo.Sales 
TIMESTAMP AS OF '2024-03-20 10:00:00';
```

---

## ⚡ 6. Best Practices voor T-SQL in Fabric

✅ Gebruik **SQL Endpoints** om interactie met je data te verbeteren.  
✅ Optimaliseer queries met **WHERE**, **LIMIT**, en **PARTITION BY**.  
✅ Gebruik **Time Travel** voor historische data zonder te overschrijven.  
❌ Vermijd overmatig gebruik van `SELECT *`, dit kan prestaties beïnvloeden.  

---

## 📌 Samenvatting

- **T-SQL in Microsoft Fabric** werkt vergelijkbaar met SQL Server, maar is geoptimaliseerd voor **Delta-tables**.
- **SQL Endpoints** maken het eenvoudig om queries op je **Lakehouse** uit te voeren.
- **Gebruik Time Travel en aggregatiefuncties** voor krachtige data-analyse.

Wil je een specifieke query testen? Open een **Issue** of stuur een **Pull Request**! 🚀
