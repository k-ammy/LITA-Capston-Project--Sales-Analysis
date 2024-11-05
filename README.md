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
     - Total Sales by Product: Found that **Product Shoes** had the highest sales at **$613,380**.
     - Total Sales by Region: **Region South** had the highest sales at **$927,820**.
     - Total Sales by Month: **February** had peak sales of **$546,300**.

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

### SalesData Analysis Queries
- The following queries analyze the SalesData table for various insights into sales performance, customer behavior, and regional contributions.

- Query 1: Retrieve the total sales for each product category
 ```sql
   SELECT Product, SUM(Sales) AS TotalSales
   FROM SalesData
   GROUP BY Product;
 ```
- Rename the column 'TotalSales' to 'Sales' (if needed in the database structure)
   EXEC sp_rename 'SalesData.TotalSales', 'Sales', 'COLUMN';

- Query 2: Find the number of sales transactions in each region
 ```sql
   SELECT Region, COUNT(OrderID) AS NumOfTransactions
   FROM SalesData
   GROUP BY Region;
 ```

- Query 3: Find the highest-selling product by total sales value
 ```sql
   SELECT TOP (1) Product, SUM(Sales) AS TotalSales
   FROM SalesData
   GROUP BY Product
   ORDER BY TotalSales DESC;
 ```

- Query 4: Calculate total revenue per product
 ```sql
   SELECT Product, SUM(Sales) AS TotalRevenue
   FROM SalesData
   GROUP BY Product;
 ```

- Query 5: Calculate monthly sales totals for the current year
 ```sql
   SELECT MONTH(OrderDate) AS Month,
   SUM(Sales) AS MonthlySalesTotal
   FROM SalesData 
   WHERE YEAR(OrderDate) = 2024
   GROUP BY MONTH(OrderDate)
   ORDER BY Month;
 ```

- Query 6: Find the top 5 customers by total purchase amount
 ```sql
   SELECT TOP (5) Customer_Id,
   SUM(Sales) AS TotalPurchaseAmount 
   FROM SalesData
   GROUP BY Customer_Id
   ORDER BY TotalPurchaseAmount DESC;
 ```
- Query 7: Calculate the percentage of total sales contributed by each region
 ```sql
   SELECT Region, 
   SUM(Sales) AS RegionTotalSales,
   FORMAT(ROUND((SUM(Sales) / CAST((SELECT SUM(Sales) FROM SalesData) AS DECIMAL(10, 2)) * 100), 1), '0.#') 
   AS PercentageOfTotalSales
   FROM SalesData
   GROUP BY Region
   ORDER BY PercentageOfTotalSales DESC;
 ```
- Query 8: Identify products with no sales in the last quarter
 ```sql
   SELECT Product 
   FROM SalesData
   GROUP BY Product
   HAVING SUM(CASE 
   WHEN OrderDate BETWEEN '2024-06-01' AND '2024-08-31' 
   THEN 1 ELSE 0 END) = 0;
 ```

## Power BI Dashboard
### Dashboard Layout:
- Sales Overview: Displays monthly sales trends.
- Top-Performing Products: Shows top 5 products by sales.
- Regional Sales Breakdown: Map visualization highlighting sales by region.

## Results
### Excel
Top Product: Product S (Shoes) with $613,380 in total sales.
Highest Sales Region: South, generating $927,820 of total sales.
Monthly Trends: February was the highest sales month with $546,300.
### SQL
- Query 1: List of products with total sales.
- Query 2: Number of transactions per region.
- Query 3: Top-selling product.
- Query 4: Total revenue per product.
- Query 5: Monthly sales totals for 2024.
- Query 6: Top 5 customers by purchase amount.
- Query 7: Sales contribution percentage by region.
- Query 8: Products with no sales in the last quarter.
