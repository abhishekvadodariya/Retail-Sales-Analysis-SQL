# Retail Sales Analysis SQL 

## Project Overview

**Project Title**: Retail Sales Analysis  
**Database**: `retail_sales_analysis`

This repository showcases SQL skills for retail sales data analysis. It includes database setup, exploratory data analysis (EDA), and SQL queries to answer business questions.

**Project Goals:**

* **Database Management:** Demonstrate proficiency in setting up and populating a retail sales database.
* **Data Preprocessing:** Apply data cleaning techniques to ensure data integrity.
* **Data Understanding:** Perform EDA to gain insights into the underlying data structure.
* **SQL for Business Intelligence:** Use SQL to answer business questions and extract valuable information.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `retail_sales_analysis`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE retail_sales_analysis;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```

### 2. Data Exploration and Cleaning

* **Dataset Size Determination:** Calculate the total number of records within the `retail_sales` table.
* **Unique Customer Identification:** Determine the count of distinct customers present in the dataset.
* **Product Category Enumeration:** Identify and list all unique product categories available.
* **Null Value Management:** Identify records containing null values and remove them to ensure data integrity.

```sql
SELECT COUNT(*) FROM retail_sales;

SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE transaction_id IS NULL;

SELECT * FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantity IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;

DELETE FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantity IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve monthly sales**:
```sql
SELECT
    DATE_FORMAT(sale_date, '%Y-%m') AS sale_month,
    SUM(total_sale) AS monthly_sales
FROM
    retail_sales
GROUP BY
    sale_month
ORDER BY
    sale_month;
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT *
FROM retail_sales
WHERE
    category = 'Clothing'
    AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11'
    AND quantity >= 4;
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category**:
```sql
SELECT category, SUM(TOTAL_SALE) 
 FROM retail_sales 
 GROUP BY category;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category**:
```sql
SELECT ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = "Beauty";
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000;

SELECT COUNT(*) FROM retail_sales
WHERE total_sale > 1000;
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category**:
```sql
SELECT
    category,
    gender,
    COUNT(*) AS total_transactions
FROM
    retail_sales
GROUP BY
    category,
    gender
ORDER BY
    category, gender;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT
    sale_year,
    sale_month,
    average_sale
FROM
    (
        SELECT
            EXTRACT(YEAR FROM sale_date) AS sale_year,
            EXTRACT(MONTH FROM sale_date) AS sale_month,
            AVG(total_sale) AS average_sale,
            RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS sale_rank
        FROM
            retail_sales
        GROUP BY
            sale_year, sale_month
    ) AS ranked_sales
WHERE
    sale_rank = 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales**:
```sql
SELECT
    customer_id,
    SUM(total_sale) AS total_sales
FROM
    retail_sales
GROUP BY
    customer_id  
ORDER BY
    total_sales DESC 
LIMIT 5;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category**:
```sql
SELECT category,
COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift;
```

## Key Findings

* **Customer Profile Diversity:** The dataset reveals a range of customer age demographics and purchasing patterns across various product categories, including Clothing and Beauty.
* **Premium Transaction Identification:** Analysis highlighted transactions exceeding $1000, indicating high-value sales.
* **Temporal Sales Patterns:** Monthly sales analysis demonstrated fluctuations, revealing potential seasonal trends.
* **Customer and Product Performance:** Top-spending customers and best-selling product categories were identified, providing key business insights.

## Reports Generated

* **Comprehensive Sales Overview:** A report detailing total sales figures, customer demographic breakdowns, and product category performance summaries.
* **Temporal Trend Analysis:** Reports providing insights into sales variations across months, highlighting potential trends and shifts.
* **Customer Segmentation and Analysis:** Reports focusing on identifying top-spending customers and analyzing unique customer counts per product category.

## Conclusion

This project provides a practical introduction to SQL for data analytics, encompassing database creation, data preprocessing, exploratory analysis, and the development of business-focused SQL queries. The derived insights into sales trends, customer behavior, and product performance can inform strategic business decisions.
