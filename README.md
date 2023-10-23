# Parch-and-Posey-Query-Analysis-
Parch and Posey is a fictional paper-selling company based in the United State The company sells three types of paper namely ‘Standard Paper’, “Gloss paper’ and ‘Poster Paper’ The steps carried out in this project include      -- Extraction of relevant data from Parch and Posey database    --Analysis of the extracted  data using MS Excel tool

--SQL Query Used for the analysis are as follows--


--revenue by year

SELECT DATE_PART('YEAR', occurred_at), 
  SUM(standard_amt_usd) standard_sales,
  SUM(gloss_amt_usd) gloss_sales, 
  SUM(poster_amt_usd) poster_sales, 
  SUM(total_amt_usd) total_sales
FROM orders
GROUP BY 1
ORDER BY 1

--total revenue by month

SELECT DATE_TRUNC('month', occurred_at), 
  SUM(standard_amt_usd) standard_sales,
  SUM(gloss_amt_usd) gloss_sales, 
  SUM(poster_amt_usd) poster_sales, 
  SUM(total_amt_usd) total_sales
FROM orders
GROUP BY 1
ORDER BY 1

--top 10 customers by revenue
SELECT a.id id, a.name company_name, 
  COUNT(total) order_count, SUM(standard_qty) standard_qty_ord, 
  SUM(gloss_qty) gloss_qty_ord, SUM(poster_qty) poster_qty_ord,
  SUM(standard_amt_usd) standard_sales_usd,
  SUM(gloss_amt_usd) gloss_sales_usd, 
  SUM(poster_amt_usd) poster_sales_usd, 
  SUM(total_amt_usd) total_sales_usd
FROM orders o
JOIN accounts a
ON o.account_id = a.id
GROUP BY 1,2
ORDER BY order_count DESC, total_sales_usd DESC



--top 10 customers by counts of orders--
SELECT a.id id, a.name company_name, 
  COUNT(total) order_count, SUM(standard_qty) standard_qty_ord, 
  SUM(gloss_qty) gloss_qty_ord, SUM(poster_qty) poster_qty_ord
FROM orders o
JOIN accounts a
ON o.account_id = a.id
GROUP BY 1,2
ORDER BY order_count DESC
LIMIT 10


-- Top 10 Sales Rep performance based on Num of order, num of customers, total revenue--

SELECT s.id sales_rep_id, s.name sales_rep_name, COUNT(o.id) num_of_ord, SUM(total_amt_usd) Total_revenue
FROM orders o
JOIN accounts a
ON o.account_id = a.id
JOIN sales_reps s
ON a.sales_rep_id = s.id
GROUP BY 1,2
ORDER BY Total_revenue DESC
LIMIT 10

--Region with highest sales revenue--
SELECT r.id region_id, r.name region_name, SUM(o.total) total_qty_ord, SUM(o.total_amt_usd) total_revenue
FROM orders o
JOIN accounts a
ON o.account_id = a.id
JOIN sales_reps s
ON a.sales_rep_id = s.id
JOIN region r
ON s.region_id = r.id
GROUP BY 1,2
ORDER BY 4 DESC


--Customer segmentation--
SELECT a.name company_name, SUM(total_amt_usd) total_sales,
   CASE
   WHEN SUM(total_amt_usd) <= 14138.11 THEN 'Low_Value_Customer'
    WHEN SUM(total_amt_usd) <= 66118.61 THEN 'Medium_Value_Customer'
     WHEN SUM(total_amt_usd) <= 276366.11 THEN 'High_Value_Customer'
     ELSE 'Very High_Value_Customer'
     END AS customer_segment
FROM accounts a
JOIN orders o
ON a.id = o.account_id
GROUP BY 1
ORDER BY 2 DESC
     

--customer segmentation and region--

SELECT a.name company_name, r.name region_name, SUM(total_amt_usd) total_sales,
   CASE
   WHEN SUM(total_amt_usd) <= 14138.11 THEN 'Low_Value_Customer'
    WHEN SUM(total_amt_usd) <= 66118.61 THEN 'Medium_Value_Customer'
     WHEN SUM(total_amt_usd) <= 276366.11 THEN 'High_Value_Customer'
     ELSE 'Very High_Value_Customer'
     END AS customer_segment
FROM orders o
JOIN accounts a
ON a.id = o.account_id
JOIN sales_reps s
ON a.sales_rep_id = s.id
JOIN region r
ON s.region_id =r.id
GROUP BY 1,2
ORDER BY 3 DESC

-- Total Num of acct--
SELECT id, name
FROM accounts

--web events channel--
SELECT channel, COUNT (channel) channel_count
FROM web_events
GROUP BY 1
ORDER BY 2 DESC



--Number of sales reps--
SELECT id sales_rep_id, name sales_rep_name
FROM sales_reps



-- years with the lowest revenue--

SELECT  DATE_PART('YEAR', occurred_at), COUNT(total) count_of_orders, SUM(total_amt_usd) total_sales 
FROM orders
GROUP BY 1
ORDER BY 2,3 


















     

















