# End-to-End-Sales-Performance-Analytics-Reporting

A comprehensive data analysis project examining sales, product performance, and customer ordering trends for a pizza retail business using **SQL Server**, **MS Excel**, **Power BI**, and **Tableau**.

---

## Executive Summary

This project analyzes transactional order data to identify key performance indicators (KPIs), optimal operational hours, high-performing menu items, and customer purchasing patterns. By converting raw transactional logs into actionable data visualizations, this repository provides data-driven strategies to streamline inventory management, optimize shift scheduling, and improve overall revenue generation.

---

## Key Performance Indicators (KPIs)

Based on the complete dataset analysis:

| Metric | Calculated Value | SQL Logic / Formula |
| :--- | :--- | :--- |
| **Total Revenue** | **$817.9K** | `SUM(total_price)` |
| **Total Orders** | **21,350** | `COUNT(DISTINCT order_id)` |
| **Total Pizzas Sold** | **49,574** | `SUM(quantity)` |
| **Average Order Value (AOV)** | **$38.31** | `Total Revenue / Total Orders` |
| **Average Pizzas Per Order** | **2.32** | `Total Pizzas Sold / Total Orders` |

---

## Tech Stack & Tools Used

* **Database Engine:** Microsoft SQL Server / SQL Server Management Studio (SSMS 20.0.70.0)
* **Spreadsheet Analysis:** Microsoft Excel 2024 (Pivot Tables, Custom Aliases, Dashboards)
* **Business Intelligence & Dashboards:** Power BI (2026.2.0) & Tableau (2026.2.0)
* **Documentation & Data Extraction:** Microsoft Word (BRD Document) & Python/Jupyter Notebook

---

## Database Schema & Data Structure

Primary Dataset: `pizza_sales.csv` imported into MS SQL Server.

| Field Name | Data Type | Description |
| :--- | :--- | :--- |
| `order_id` | INT | Unique identifier for each customer order transaction |
| `pizza_id` | VARCHAR/INT | Unique identifier for each specific item in an order |
| `pizza_name` | VARCHAR | Full descriptive name of the pizza ordered |
| `quantity` | INT | Number of pizzas ordered per transaction line item |
| `total_price` | DECIMAL(10,2) | Total price for the line item (`quantity` × unit price) |
| `order_date` | DATE | Date the order was placed |
| `order_time` | TIME | Timestamp of the transaction |
| `pizza_category` | VARCHAR | Category classification (`Classic`, `Supreme`, `Veggie`, `Chicken`) |
| `pizza_size` | VARCHAR | Size designation (`S`, `M`, `L`, `XL`, `XXL`) |

---

## Key Insights & Analytical Findings

### 1. Temporal & Order Trends
* **Busiest Days:** Orders peak on **Friday** (3,538 orders) and **Thursday** (3,239 orders). Sundays see the lowest order volume (2,624 orders).
* **Peak Hours:** Order volume surges twice daily—during lunch hours (**12:00 PM – 1:00 PM**) and evening rush hours (**5:00 PM – 7:00 PM**).

### 2. Category & Size Breakdown
* **Top Category by Revenue & Volume:** **Classic** category drives the highest sales volume (14,888 pizzas sold, ~26.91% of total revenue).
* **Dominant Size:** **Large (L)** size pizzas contribute the most significantly to overall sales revenue (~45.89%).

### 3. Product Performance Breakdown
* **Top Performers (by Volume/Revenue):** *The Classic Deluxe Pizza*, *The Barbecue Chicken Pizza*, and *The Hawaiian Pizza* lead overall quantity and revenue performance.
* **Lowest Performers:** *The Brie Carre Pizza* generated the lowest revenue ($11,588 / 490 sold), followed by *The Mediterranean Pizza* and *The Calabrese Pizza*.

---

## SQL Queries Reference

### Total Revenue

SELECT SUM(total_price) AS Total_Revenue 
FROM pizza_sales;


### Average Order Value

SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value 
FROM pizza_sales;


### Daily Trend for Total Orders

SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales
GROUP BY DATENAME(DW, order_date);


### Percentage of Sales by Category

SELECT 
    pizza_category, 
    CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue,
    CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category;


### Top 5 Best Sellers by Revenue

SELECT TOP 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue DESC;


---

## How to Replicate This Project

1. **Database Setup:** 
   * Open SQL Server Management Studio (SSMS).
   * Import `pizza_sales.csv` as a flat file into your target database.
   * Execute queries from `PIZZA SALES SQL QUERIES.docx` to verify metrics.
2. **Dashboard Connections:**
   * Connect MS Excel, Power BI, or Tableau to the SQL Server database.
   * Map size aliases (`S` -> Small, `M` -> Medium, `L` -> Large).
   * Build time-series, category distribution, and top/bottom product performance visual cards.
