# SQL Project Retail Sales

## Project Overview

**Project Title**: Retail Sales

**Database**: `retail_db`

I’m working on a project highlighting SQL expertise by tackling retail data. The objective is to set up a sales database, explore the data through EDA, and clean it as needed. I’ll write SQL queries to analyze trends, answer specific business-related questions, and provide insights to improve decision-making. This project will help showcase my ability to work with complex datasets and extract meaningful insights using SQL.
## Objectives

1. **Build a retail sales database**: Create and populate the database with the provided sales data to establish a foundation for analysis.
2. **Clean the Data**: Identify and remove missing or null records to ensure data consistency and accuracy.
3. **Conduct Exploratory Data Analysis (EDA)**: Perform EDA using SQL to gain a basic understanding of the dataset, uncover patterns, and detect anomalies.
4. **Perform Business Analysis**: Utilize SQL queries to address key business questions, extract insights, and provide actionable recommendations based on sales 
                                  performance and trends.
## Project Structure

### 1. Database Setup

- **Database Setup**: The project kicks off by setting up a database named  `retail_db`.
- **Table Creation**: A table named `retail01` is designed to store essential sales data. The table structure includes columns that capture the key details of each transaction. (transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount).

```sql
CREATE DATABASE retail_db;

CREATE TABLE retail01
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(20),
    age INT,
    category VARCHAR(40),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Calculate the total number of entries available in the dataset to understand the dataset size.
- **Customer Count**: Identify the number of distinct customers by counting unique customer IDs to assess the customer base.
- **Category Count**: List all unique product categories to determine the range of products represented in the data 
- **Null Value Check**: Scan for any missing or null values in the dataset and remove incomplete records to ensure data quality.

```sql
SELECT COUNT(*) FROM retail01;
SELECT COUNT(DISTINCT customer_id) FROM retail01;
SELECT DISTINCT category FROM retail01;

SELECT * FROM retail01
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail01
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT * FROM retail01
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT * FROM retail01
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail01
GROUP BY 1
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail01
WHERE category = 'Beauty'
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail01
WHERE total_sale > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail01
GROUP 
    BY 
    category,
    gender
ORDER BY 1
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail01
GROUP BY 1, 2
) as t1
WHERE rank = 1
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail01
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail01
GROUP BY category
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
FROM retail01
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
```

## Findings

- **Customer Demographics**: The dataset encompasses a diverse range of customers spanning multiple age groups, with sales reflected across various categories 
                             like Clothing and Beauty.
- **High-Value Transactions**: Numerous transactions recorded total sales exceeding 1000, signifying high-end purchases.
- **Sales Trends**: Analyzing sales every month reveals fluctuations, assisting in recognizing peak shopping seasons.
- **Customer Insights**: The analysis highlights the highest-spending customers along with the most sought-after product categories.

## Reports

- **Sales Summary**: An in-depth report that provides an overview of total sales, demographic information about customers, and performance across different 
                     product categories.
- **Trend Analysis**: An in-depth report that provides an overview of total sales, demographic information about customers, and performance across different 
                      product categories.
- **Customer Insights**: Reports focusing on identifying the top customers and counting unique customers in each category.
