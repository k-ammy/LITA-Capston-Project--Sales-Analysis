# Sales Performance Analysis for a Retail Store

## Project Summary
This project analyzes the sales performance of a retail store, focusing on identifying top-selling products, regional sales performance, and monthly sales trends. The final deliverable is an interactive Power BI dashboard that visualizes insights into sales trends and product performance.

## Tools Used
- Microsoft Excel
- SQL
- Power BI

## Table of Contents
1. [Data Cleaning](#data-cleaning)
   - [Excel Data Cleaning](#excel-data-cleaning)
   - [SQL Data Cleaning](#sql-data-cleaning)
   - [Power BI Data Cleaning](#power-bi-data-cleaning)
2. [Analysis](#analysis)
   - [Excel Analysis](#excel-analysis)
   - [SQL Analysis](#sql-analysis)
   - [Power BI Dashboard](#power-bi-dashboard)
3. [Results](#results)

---

## Data Cleaning

### Excel Data Cleaning

1. **Remove Duplicates**:
   - Used **Data > Remove Duplicates** to identify and remove duplicates based on `Product ID`, `Sale Date`, and `Region`.

2. **Handle Missing Values**:
   - Filtered for missing values in key columns (`Sales`, `Product`, and `Region`). For records missing essential data, we excluded them. Non-essential missing values (e.g., `Customer ID`) were left blank.

3. **Data Type Validation**:
   - Confirmed that `Sales` was formatted as numeric, and `Sale Date` was formatted as a date. Any inconsistencies were corrected.

4. **Standardize Text Values**:
   - Standardized values for `Region` (e.g., corrected "north" to "North") and `Product` names for consistency.

### SQL Data Cleaning

1. **Schema Setup**:
   - Created a SQL table with appropriate data types: `Sales` as `DECIMAL`, `SaleDate` as `DATE`, `Product` as `VARCHAR`.

2. **Handling Null Values**:
   - Queried for null values and replaced missing `Region` or `Product` values based on known patterns. Dropped records with essential missing data where necessary.

3. **Remove Duplicates**:
   - Used a common table expression (CTE) to identify and remove duplicates:
     ```sql
     WITH CTE AS (
         SELECT *, ROW_NUMBER() OVER(PARTITION BY Product, SaleDate, Region ORDER BY SalesID) AS RowDup
         FROM SalesData
     )
     DELETE FROM CTE WHERE RowDup > 1;
     ```

4. **Standardize Values**:
   - Used `UPDATE` statements to standardize inconsistent region names and product categories.

### Power BI Data Cleaning

1. **Load and Inspect Data**:
   - Imported the data and used **Power Query Editor** to inspect and clean the data.

2. **Handle Missing Values**:
   - Filled missing values where appropriate and removed rows with critical null data.

3. **Standardize Text Values**:
   - Ensured all text data had consistent formatting (e.g., `Region` names standardized).

---

## Analysis

### Excel Analysis

1. **Initial Data Exploration**:
   - **Pivot Tables** to summarize:
     - Total Sales by Product: Found that **Product A** had the highest sales at **$125,000**.
     - Total Sales by Region: **Region North** had the highest sales at **$200,000**.
     - Total Sales by Month: **March** had peak sales of **$30,000**.

2. **Formulas**:
   - Average Sales per Product:
     ```excel
     =AVERAGEIFS(Sales, Product, [Specific Product])
     ```
     - Total Revenue by Region:
     ```excel
     =SUMIFS(Sales, Region, [Specific Region])
     ```
  3. **Additional Report**:
   - Top 5 Products by Sales, showing **Product A** with the highest revenue.

### SQL Analysis

1. **Total Sales per Product Category**:
   ```sql
   SELECT ProductCategory, SUM(Sales) AS TotalSales
   FROM SalesData
   GROUP BY ProductCategory;
   ```
2.**Number of Sales Transactions in Each Region:
```sql
SELECT Region, COUNT(*) AS TransactionCount
FROM SalesData
GROUP BY Region;
```
3. Highest-Selling Product by Sales Value:
 ```sql
SELECT TOP 1 Product, SUM(Sales) AS TotalSales
FROM SalesData
GROUP BY Product
ORDER BY TotalSales DESC;
```
4. Monthly Sales Totals:
```sql
SELECT MONTH(SaleDate) AS Month, SUM(Sales) AS MonthlyTotal
FROM SalesData
GROUP BY MONTH(SaleDate);
```
## Power BI Dashboard
### Dashboard Layout:
- Sales Overview: Displays monthly sales trends.
- Top-Performing Products: Shows top 5 products by sales.
- Regional Sales Breakdown: Map visualization highlighting sales by region.


