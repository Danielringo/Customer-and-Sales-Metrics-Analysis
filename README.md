

Customer Activity and Sales Metrics Analysis

Hello Everyone,
In this project, I utilized Google BigQuery as the primary tool for querying and analyzing the dataset, which is sourced from Kaggle and includes data from the user_tab and order_tab tables. Google BigQuery allowed me to efficiently handle large volumes of data and execute complex SQL queries to derive meaningful insights.

In this project, Google BigQuery was utilized as the primary data processing tool. BigQuery allowed for efficient querying and analysis of large datasets related to branch performance, customer activity, and transaction types. The SQL queries executed in BigQuery provided the foundational insights necessary for further analysis.

Google Big QueryAfter querying the data, the results were exported to CSV files. CSV files served as an intermediate step to convert the raw query outputs into a format that could be easily imported into visualization tools. This process ensured that the data was structured and ready for the next phase of the project.
CSV (Comma Seperated Value)Finally, Google Looker Studio was used to create visualizations that transformed the raw data into meaningful insights. Looker Studio allowed for the creation of various charts and graphs that made the data easier to interpret and share with stakeholders.
Goole Looker Studio

Analysis Overview
Step 1: Querying Data in Google BigQuery
The first step in this analysis involves querying the data stored in Google BigQuery. The SQL queries aim to extract valuable insights from the dataset related to branch performance, customer activity, and transaction types. Here is a brief explanation of each query used:
Branch-wise Sales Performance

SELECT 
  u.branch,
  SUM(o.amount) AS total_sales,
  COUNT(DISTINCT o.transactionid) AS num_transactions
FROM `Projet_SQL.order_tab` o
LEFT JOIN `Projet_SQL.user_tab` u ON o.customerid = u.customerid
GROUP BY u.branch
ORDER BY total_sales DESC;
Result:

This query calculates the total sales and the number of transactions for each branch. The goal is to identify which branches generate the most revenue and have the highest transaction volumes.

2. Active Customers Over Time
SELECT 
  u.branch,
  DATE(o.transaction_time) AS transaction_date,
  COUNT(DISTINCT o.customerid) AS active_customers
FROM `Projet_SQL.order_tab` o
LEFT JOIN `Projet_SQL.user_tab` u ON o.customerid = u.customerid
GROUP BY u.branch, transaction_date
ORDER BY transaction_date, u.branch;
Query Result:

The Total of Rows 1064 Rows Data 
By breaking down customer activity by branch and transaction date, I could identify peak periods and days with lower customer engagement. This insight helps in optimizing marketing strategies and resource allocation.

3. Transaction Types
SELECT 
  u.branch,
  o.transaction_type,
  SUM(o.amount) AS total_amount
FROM `Projet_SQL.order_tab` o
LEFT JOIN `Projet_SQL.user_tab` u ON o.customerid = u.customerid
GROUP BY u.branch, o.transaction_type
ORDER BY u.branch, total_amount DESC
LIMIT 5;
Query Result:

The analysis of different transaction types across branches provided insights into which transaction categories generate the most revenue. This can help in tailoring services to customer preferences.

4. Customer Segmentation
WITH customer_frequency AS (
  SELECT 
    customerid,
    COUNT(transactionid) AS purchase_count
  FROM `Projet_SQL.order_tab`
  GROUP BY customerid
)
SELECT 
  customerid,
  CASE
    WHEN purchase_count = 1 THEN 'One-time Customer'
    WHEN purchase_count BETWEEN 2 AND 5 THEN 'Frequent Customer'
    ELSE 'Loyal Customer'
  END AS customer_segment,
  purchase_count
FROM customer_frequency
ORDER BY purchase_count DESC;
Query Result:

The Total of Rows 10181 Rows Data
Segmenting customers based on their purchase frequency allowed us to identify key customer groups, such as one-time customers, frequent customers, and loyal customers. This segmentation is essential for targeted marketing and customer retention strategies.

5. Transaction Patterns
SELECT 
  user_tab.branch,
  CASE
    WHEN MOD(order_tab.transactionid, 2) = 0 THEN 'Even'
    ELSE 'Odd'
  END AS transaction_category,
  COUNT(DISTINCT user_tab.customerid) AS buyer_count
FROM 
  `Projet_SQL.order_tab` as order_tab
JOIN 
  `Projet_SQL.user_tab` as user_tab
  ON order_tab.customerid = user_tab.customerid
GROUP BY 
  user_tab.branch, transaction_category;
Query Result:

The even and odd transaction categorization offers a novel way to examine buyer behavior, which could be leveraged in unique promotional strategies.

Extracting Data to CSV
Extract From Query to CSVOnce the queries are executed in BigQuery, the next step is to export the results to CSV files. This allows the data to be easily imported into Google Looker Studio for visualization. Exporting to CSV ensures that the data is in a format compatible with visualization tools and can be processed efficiently.

Visualizing Data in Google Looker Studio
After the data is exported to CSV, it's time to create visualizations that make the insights more accessible and actionable. Here's how each query result was visualized:
Visualization in Google Looker StudioInsights
Branch Performance: The bar chart in Looker Studio revealed clear differences in sales performance across branches. Top-performing branches were highlighted, while underperforming ones became evident. This insight suggests that targeted strategies should be developed to improve sales in weaker branches.
Customer Engagement: The line chart tracking active customers over time showed varying engagement levels across branches. Some branches consistently attracted customers, while others experienced fluctuations. This insight highlights the need to investigate the factors contributing to consistent customer engagement in certain branches and replicate those practices in others.
Transaction Types: The stacked bar chart provided a breakdown of transaction types, revealing which types contributed most to overall revenue. This insight suggests that focusing marketing and operational efforts on the most successful transaction types could yield better results.
Customer Loyalty: The pie chart segmented customers based on their purchase frequency, showing the distribution between One-time, Frequent, and Loyal customers. This insight emphasizes the importance of maintaining loyal customers while identifying opportunities to convert one-time buyers into more frequent shoppers.
Transaction Patterns: The pie chart categorizing transactions as 'Even' or 'Odd' offered a unique perspective on transaction patterns. This insight could be used to develop targeted promotional strategies based on transaction behavior.

Conclusion
This project demonstrated the effective use of Google BigQuery for data analysis, CSV files for data transfer, and Google Looker Studio for creating insightful visualizations. The combination of these tools allowed for a thorough analysis of branch performance, customer activity, and transaction types.
The insights gained from this analysis provide a roadmap for improving branch performance, enhancing customer engagement, and optimizing transaction strategies. By continuously monitoring and adapting based on these insights, the business can position itself for sustained growth and success in a competitive market.

---

Connect with me in: Linkedin
You can Visit the Code in Here: Github
