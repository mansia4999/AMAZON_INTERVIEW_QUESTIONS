# AMAZON_INTERVIEW_QUESTIONS
1. Retrieve the total number of customers in the database.
 ```sql– paste your code here
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
