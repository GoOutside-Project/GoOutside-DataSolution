# SQL Queries on BigQuery for the Finance Leader
This file documents the key SQL queries used in BigQuery to generate insights for Sarah, the Finance Manager at GoOutside. These queries feed into Google Sheets to enable revenue tracking, cost analysis, and forecasting.


# Query 1: "first_query" - Daily Sales with Method Type
> Based on the `daily_sales` table, joined with the `methods` table to have the method type, instead of the method code. 
> Used to generate the **Daily_sales** worksheet (in Google Sheets).

```sql
SELECT 
    ds.Retailer_code,
    ds.Product_number, 
    m.Order_method_type, 
    ds.Date, 
    ds.Quantity, 
    ds.Unit_price, 
    ds.Unit_sale_price,
FROM `gooutside-project4.gooutside.daily_sales` AS ds
LEFT JOIN `gooutside.methods` AS m
ON ds.Order_method_code = m.Order_method_code
```

# Query 2: "products_query" - Products Sold
> Based on the `products` offered by our company, joined with the `daily_sales` and `methods` tables to retrieve all the details regarding the Products sold by the company. 
> Used to generate the **Products** worksheet (in Google Sheets).

```sql
SELECT 
    ds.Product_number, 
    ds.date, 
    m.Order_method_type, 
    p.Product_line, 
    p.Product_type, 
    p.Product, 
    p.Product_color, 
    p.Unit_cost, 
    p.Unit_price, 
    ds.quantity, 
FROM `gooutside-project4.gooutside.products` AS p
RIGHT JOIN `gooutside.daily_sales` AS ds
ON p.Product_number = ds.Product_number
LEFT JOIN `gooutside.methods` AS m
ON ds.Order_method_code = m.Order_method_code
```

# Query 3: "daily_sales-retailers2" - Retailers and Gross Margin Analysis
> Based on the `daily_sales` table, joined with the `retailers` and `products`, to calculate gross marign and analyze sales by retailer and region. 
> Used to generate the **Retailers** worksheet (in Google Sheets).

```sql
SELECT 
    ret.Type, 
    ret.Retailer_name, 
    ds.quantity, 
    ds.unit_sale_price, 
    ds.unit_sale_price-pr.unit_cost AS Gross_Margin, 
    pr.unit_cost AS Cost, 
    (ds.unit_sale_price-pr.unit_cost)/ds.unit_sale_price*100 AS Gross_Margin_Percentage, 
    ret.Country
FROM `gooutside.daily_sales`AS ds
LEFT JOIN `gooutside.retailers` AS ret using (retailer_code)
LEFT JOIN `gooutside.products`AS pr using (product_number)
WHERE ds.unit_sale_price>0;
```

All queries are executed within the BigQuery project: gooutside-project4.gooutside