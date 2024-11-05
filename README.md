SELECT TOP (1000) [CustomerID]
      ,[CustomerName]
      ,[Region]
      ,[SubscriptionType]
      ,[SubscriptionStart]
      ,[SubscriptionEnd]
      ,[Canceled]
      ,[Revenue]
      ,[SubcriptionDuration]
  FROM [lita_capstoe_project].[dbo].[LITA_CustomerData]


   Select  region, count(distinct Customerid) as total_customers 
from LITA_CustomerData
Group by region;



2. Select top 1 subscriptiontype, count(distinct customerid) as total_customers
From LITA_CustomerData
Group by subscriptiontype 
Order by total_customers desc;


3. SELECT customerid
FROM LITA_CustomerData
WHERE subscriptionend <= DATEADD(month, 6, subscriptionstart);


4. Select avg(datediff(day, subscriptionstart, subscriptionend)) as avg_subscription_duration
From LITA_CustomerData

-----5. showig me shege
 SELECT customerid, COUNT(*) AS count_per_customer
FROM LITA_CustomerData
GROUP BY customerid
ORDER BY count_per_customer DESC;

SELECT DISTINCT subscriptiontype
FROM LITA_CustomerData;

SELECT customerid, subscriptiontype, COUNT(*) AS duplicate_count
FROM LITA_CustomerData
GROUP BY customerid, subscriptiontype
HAVING COUNT(*) > 1;

WITH CTE AS (
    SELECT customerid, 
           subscriptiontype, 
           subscriptionstart, 
           subscriptionend,
           ROW_NUMBER() OVER(PARTITION BY customerid, subscriptiontype, subscriptionstart, subscriptionend ORDER BY (SELECT NULL)) AS row_num
    FROM LITA_CustomerData
)
SELECT *
FROM CTE
WHERE row_num = 1;  -- This keeps only the first instance of each duplicate group

SELECT MONTH(subscriptionstart) AS start_month,
       COUNT(DISTINCT customerid) AS active_customers
FROM LITA_CustomerData
GROUP BY MONTH(subscriptionstart);
------
5. SELECT customerid, 
       subscriptiontype, 
       DATEDIFF(day, subscriptionstart, subscriptionend) AS subscription_duration
FROM LITA_CustomerData
WHERE DATEDIFF(day, subscriptionstart, subscriptionend) > 365;

6. Select subscriptiontype,
Sum(revenue) as total_revenue 
From LITA_CustomerData
GROUP BY subscriptiontype;

7. SELECT TOP 3 region,
       COUNT(CASE WHEN Canceled = 'true' THEN 1 END) AS cancelled_count
FROM LITA_CustomerData
GROUP BY region
ORDER BY cancelled_count DESC;

8.SELECT region,
       COUNT(CASE WHEN Canceled = 'true' THEN 1 END) AS cancelled_count,
       COUNT(CASE WHEN Canceled = 'false' THEN 1 END) AS active_count
FROM LITA_CustomerData
GROUP BY region
ORDER BY cancelled_count DESC;
