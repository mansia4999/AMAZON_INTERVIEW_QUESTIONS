# AMAZON_INTERVIEW_QUESTIONS
1. Retrieve the total number of customers in the database.
 ```SQL
SELECT 
	COUNT(CUSTOMER_ID)AS TOTALCUTOMERS
FROM ORDERS;

SELECT 
	COUNT(CUSTOMER_ID)
FROM CUSTOMERS;

 ```
 2. Calculate the total number of sellers registered on Amazon.
```SQL
SELECT 
	COUNT(SELLER_ID)AS TOTALSELLERS
 FROM SELLERS;
```
3. List all unique product categories available.
```SQL
SELECT
      DISTINCT(CATEGORY)
FROM ORDERS;
```
4. Find the top 5 best-selling products by quantity sold.
```SQL
SELECT
      PRODUCT_ID,
     SUM(QUANTITY)AS TOTAL_QUANTITY_SOLD
FROM ORDERS
GROUP BY 1
ORDER BY SUM(QUANTITY) DESC
LIMIT 5;
```
5. Determine the total revenue generated from sales.
```SQL
SELECT
      SUM(SALE) AS TOTAL_REVENUE
FROM ORDERS;
```
6. List all customers who have made at least one return.
```SQL
SELECT 
      DISTINCT(ORDER_ID) AS CUSTOMERS
FROM RETURNS
WHERE RETURN_ID IS NOT NULL
```
7. Calculate the average price of products sold.
```SQL


SELECT 
      PRODUCT_NAME,
      AVG(PRICE)
FROM PRODUCTS
GROUP BY 1
ORDER BY 2
```
8. Identify the top 3 states with the highest total sales.
```SQL
SELECT 
	STATE,
	SUM(SALE)AS TOTAL_SALES
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC
LIMIT 3
```
9. Find the product category with the highest average sale price.
```SQL
SELECT 
      CATEGORY,
      AVG(SALE) AS AVG_SALEPRICE
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC
```	
10. List all orders with a sale amount greater than $100.
```SQL
SELECT 
       *
FROM ORDERS
WHERE SALE>100
```

11. Calculate the total number of returns processed.
```SQL
SELECT
	COUNT(RETURN_ID) AS TOTAL_RETURN_PROCESSED
FROM RETURNS
	
SELECT 
	COUNT(*)
FROM RETURNS
```
 12. Identify the top-selling seller based on total sales amount.
```SQL
SELECT
	SELLER_ID,
	SUM(SALE) AS TOTAL_SALE_AMOUNT
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC
```
 13. List the products with the highest quantity sold in each category.
```SQL
SELECT
      CATEGORY,
      PRODUCT_ID,
      SUM(QUANTITY) AS QUANTITY_SOLD
FROM ORDERS
GROUP BY 1,2
ORDER BY 3 DESC
```
14. Determine the average sale amount per order.
```SQL
SELECT 
	ORDER_ID,
        AVG(SALE) AS AVGERAGE_SALE
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC
```	
15. Find the top 5 customers who have spent the most money.
```SQL
SELECT 
       CUSTOMER_ID,
       SUM(SALE)AS TOTAL_MONEY
FROM ORDERS
GROUP BY 1
ORDER BY 2
LIMIT 5
```
 16. Calculate the total number of orders placed in each state.
```SQL
SELECT
	STATE,
	COUNT(ORDER_ID) AS TOTAL_NUM_ORDERS
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC
```
 17. Identify the product sub-category with the highest total sales.
```SQL
SELECT
       SUB_CATEGORY ,
       SUM(SALE)AS TOTAL_SALES
FROM ORDERS
GROUP BY 1
ORDER BY 2
LIMIT 1
```
18. List the orders with the highest total sale amount.
```SQL
SELECT
       ORDER_ID,
       SUM(SALE) AS TOTAL_SALE_AMOUNT
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1
```
19. Calculate the total sales revenue for each seller.
```SQL
SELECT
      SELLER_ID,
     SUM(SALE)AS TOTAL_SALES_REVENUE
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC
```
20. Find the top 3 states with the highest average sale per order.
```SQL	
SELECT
       ORDER_ID,
       STATE,
       AVG(SALE)AS AVERAGE_SALES
FROM ORDERS
GROUP BY 1,2
ORDER BY 3 DESC
LIMIT 3
```

21. Identify the product category with the highest total quantity
 sold.
```SQL
SELECT 
      CATEGORY,
      SUM(QUANTITY) AS TOTAL_QUANTITY
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1
```

 22. List the orders with the highest quantity of products
 purchased.

```SQL
SELECT 
      ORDER_ID,
     SUM(QUANTITY)AS QUANTITY_OF_PRODCUTS
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC 
LIMIT 1
```
23. Calculate the average sale amount for each product category
```SQL
SELECT
      CATEGORY AS PRODUCT_CATEGORY,
      AVG(SALE) AVERAGE_SALE_AMOUNT
FROM ORDERS
GROUP BY 1
```
 24. Find the top-selling seller based on the number of orders
 processed.
```SQL
SELECT 
      SELLER_ID,
      COUNT(ORDER_ID)AS COUNT_OF_ORDERS
FROM ORDERS
GROUP BY 1
ORDER BY  COUNT_OF_ORDERS DESC 
LIMIT 1
```

 25. Identify the customers who have made returns more than once
```SQL
SELECT
	ORDER_ID,
	COUNT(RETURN_ID)
FROM RETURNS
GROUP BY 1
HAVING COUNT(RETURN_ID)  >100

	
SELECT ORDER_ID, COUNT(*) AS return_count FROM returns GROUP BY ORDER_ID HAVING COUNT(*) > 1;
```
###INTERMEDIATE  INTERVIEW QUESTIONS
```SQL
1. Find out the top 5 customers who made the highest profits.
```SQL	
SELECT
       CUSTOMER_ID,
       SUM(SALE-PRICE_PER_UNIT) AS PROFIT
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```		
 2. Find out the average quantity ordered per category.
```SQL
SELECT
       CATEGORY,
       AVG(QUANTITY) AS AVERAGE_QUANTITY
FROM ORDERS
GROUP BY 1 
```
3. Identify the top 5 products that have generated the highest revenue.
```SQL	 
SELECT
       PRODUCT_ID,
       SUM(SALE) AS TOTAL_REVENUE
 FROM ORDERS
 GROUP BY 1
 ORDER BY 2
 LIMIT 5 
```	 	 
4.Determine the top 5 products whose revenue has decreased compared to the previous year.
```SQL
WITH CURRENT_YEAR
AS	 
	(
		 SELECT
				PRODUCT_ID,
				SUM(SALE) AS CURRENT_YEAR_REVENUE,
				TO_CHAR(ORDER_DATE ,'YYYY') AS CURRENT_YEAR
		FROM ORDERS
		GROUP BY 1,3
		ORDER BY 2,3
		LIMIT 5 
	 ),
PREVIOUS_YEAR
	 AS
(

	 SELECT
	 		PRODUCT_ID,
	EXTRACT(YEAR FROM ORDER_DATE)-1 AS PREVIOUS_YEAR,
	 -- TO_CHAR(ORDER_DATE,'YYYY')- INTERVAL 1 AS PREVIOUS_YEAR,
	 		SUM(SALE)AS PREVIOUS_YEAR_REVENUE
	 		
 	FROM ORDERS
	GROUP BY 1,2
 	ORDER BY 2,3 DESC
 	LIMIT 5
)

SELECT 
		C.PRODUCT_ID,
		C.CURRENT_YEAR_REVENUE,
		C. CURRENT_YEAR,
	 	P.PRODUCT_ID,
		P.PREVIOUS_YEAR_REVENUE,
		P.PREVIOUS_YEAR,
	 	(C.CURRENT_YEAR_REVENUE-P.PREVIOUS_YEAR_REVENUE) AS REVENUE_DIFFERENCE
FROM CURRENT_YEAR AS C
JOIN PREVIOUS_YEAR AS P
ON C.PRODUCT_ID = P.PRODUCT_ID
WHERE 	(C.CURRENT_YEAR_REVENUE-P.PREVIOUS_YEAR_REVENUE)<0
-- GROUP BY REVENUE_DIFFERENCE 
LIMIT 5;		
```

5. Identify the highest profitable sub-category.
```SQL
SELECT
      SUB_CATEGORY,
      SUM(SALE-PRICE_PER_UNIT)AS TOTAL_PROFIT
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;
```
6. Find out the states with the highest total orders.
```SQL
SELECT
       STATE,
       COUNT(ORDER_ID)AS TOTAL_ORDERS
FROM ORDERS
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;
```
7. Determine the month with the highest number of orders.
```SQL
SELECT	
     TO_CHAR(ORDER_DATE,'MONTH')AS MONTH,
     COUNT(ORDER_ID)AS NUM_OF_ORDERS
FROM ORDERS
GROUP BY 1
ORDER BY 2
LIMIT 1;
```

 8. Calculate the profit margin percentage for each sale (Profit divided by Sales).
```SQL
PROFIT = SALE -COGS* QUANTITY
	SELECT * FROM ORDERS

	SELECT * FROM PRODUCTS

SELECT
		O.SALE,
		 O.PRODUCT_ID,
	 	P.PRODUCT_NAME,
		SUM(O.SALE-(P.COGS * O.QUANTITY)) AS PROFIT,
	 	SUM(O.SALE-(P.COGS * O.QUANTITY))*100 AS PROFIT_PERCENTAGE
	 	
FROM ORDERS AS O 
JOIN PRODUCTS AS P
ON O.PRODUCT_ID = P.PRODUCT_ID
GROUP BY 1,2,3
ORDER BY 4
```
 9. Calculate the percentage contribution of each sub-category

```SQL
WITH total_sales AS (
    SELECT SUM(SALE) AS total_revenue
    FROM ORDERS
)
SELECT sub_category, 
       SUM(SALE) AS sub_category_revenue, 
       (SUM(SALE) / (SELECT total_revenue FROM total_sales)) * 100 AS percentage_contribution
FROM ORDERS
GROUP BY sub_category;
```
10.Identify top 2 category that has received maximum returns and their return %
```SQL
SELECT * FROM ORDERS;
SELECT * FROM RETURNS;

SELECT 
		CATEGORY,
		-- COUNT(ORDER_ID) AS TOTAL_ORDERS,
		COUNT(RETURN_ID) AS  TOTAL_RETURNS,
		(COUNT(RETURN_ID)/COUNT(ORDER_ID))*100 AS RETIRN_PERCENTAGE
FROM ORDERS AS O 
JOIN RETURNS AS R
ON O.ORDER_ID = R.ORDER_ID
GROUP BY 1
HAVING COUNT(RETURN_ID)
LIMIT 2 ;
```
