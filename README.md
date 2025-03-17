# SQL Data Warehouse Project  
Building a modern data warehouse with SQL Server, leveraging **Medallion Architecture** (Bronze, Silver, Gold) for scalable data management, ETL processes, and analytics.

---

## Project Overview  
This project demonstrates a complete data warehouse solution designed for analytical workloads. It includes:  
- **Medallion Architecture**: Organized into Bronze (raw), Silver (cleaned), and Gold (business-ready) layers.  
- **ETL Pipelines**: Automates data ingestion, transformation, and loading.  
- **Star Schema Modeling**: Optimized for fast querying and business intelligence.  
- **Analytics**: SQL-based dashboards and reports for actionable insights.  

---

## Architecture Design  
### **Bronze Layer**  
- **Purpose**: Store raw, unmodified data from source systems (CSV files).  
- **Details**:  
  - Data is ingested as-is into SQL Server tables.  
  - No transformations applied (e.g., `Sales_Raw`, `Customers_Raw`).  

### **Silver Layer**  
- **Purpose**: Clean, standardize, and normalize data.  
- **Transformations**:  
  - Handle missing values (e.g., `COALESCE(NULL, 'Unknown')`).  
  - Deduplicate records.  
  - Enforce data types and constraints.  
  - Example: `Silver.Sales` with cleaned `OrderDate` and standardized `ProductID`.  

### **Gold Layer**  
- **Purpose**: Business-ready data modeled into a **star schema**.  
- **Schema**:  
  - **Fact Tables**: e.g., `FactSales` with metrics like `Revenue` and `Quantity`.  
  - **Dimension Tables**: e.g., `DimCustomer`, `DimProduct`, `DimDate`.  
  - Optimized for joins and aggregations (e.g., `SELECT SUM(Revenue) FROM FactSales JOIN DimDate ON ...`).  

---

## ETL Process  
1. **Extract**: Load CSV files into Bronze tables using `BULK INSERT` or SQL Server Integration Services (SSIS).  
2. **Transform**:  
   - Clean data in Silver (e.g., `UPDATE Silver.Sales SET Region = 'West' WHERE Region = 'W'`).  
   - Enrich data (e.g., add `ProfitMargin` column in Gold).  
3. **Load**: Populate Gold tables via `INSERT INTO ... SELECT` statements or stored procedures.  

---

## Analytics & Reporting  
- **SQL Queries**:  
  ```sql
  -- Example: Monthly Sales by Region
  SELECT 
      d.Year, 
      d.Month, 
      SUM(f.Revenue) AS TotalRevenue
  FROM Gold.FactSales f
  JOIN Gold.DimDate d ON f.DateID = d.DateID
  GROUP BY d.Year, d.Month
  ORDER BY TotalRevenue DESC;
  ```
- **Dashboards**: Connect Power BI/Tableau to Gold tables for visualizations.  

---

## Getting Started  
### Prerequisites  
- SQL Server (2019+ recommended).  
- Source CSV files (place in `/data` directory).  

---

## Folder Structure  
```
sql-data-warehouse-project/
├── data/              # Source CSV files
├── etl/               # ETL scripts (Bronze/Silver/Gold)
├── analytics/         # Reporting queries
└── setup/             # Database setup scripts
```

---

## Technologies Used  
- **SQL Server**: Core database engine.  
- **SSIS** (optional): For advanced ETL workflows.  
- **Power BI/Tableau**: For dashboarding.  

---
