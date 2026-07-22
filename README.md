# Blinkit-Marketing-Performance-SQL-Analysis
End-to-end SQL data analysis on Blinkit marketing campaigns performance, channel metrics, conversion rates, and ROAS.

# 🛒 Blinkit Marketing Performance - SQL Data Analysis

![SQL](https://img.shields.io/badge/Language-SQL-orange?style=for-the-badge&logo=mysql)
![Database](https://img.shields.io/badge/Database-MySQL-blue?style=for-the-badge&logo=mysql)
![Analytics](https://img.shields.io/badge/Use%20Case-Marketing%20Analytics-green?style=for-the-badge)

---

## 📌 Executive Summary
This project analyzes marketing campaign performance metrics for *Blinkit* to evaluate channel effectiveness, conversion efficiency, and return on ad spend (ROAS). Using MySQL, the dataset was queried to extract actionable business insights to help optimize ad budgets and channel strategies.

---

## 🧠 Key Data Concepts & Technical Skills Applied
* *Data Retrieval & Filtering:* SELECT, WHERE, IN, LIKE, LIMIT
* *Aggregation & Grouping:* SUM(), AVG(), COUNT(), ROUND(), GROUP BY, HAVING
* *Advanced Analytical Functions:* Window Functions (DENSE_RANK() OVER (PARTITION BY ...)), Subqueries
* *Database Objects & DDL/DML:* CREATE VIEW for modular reporting



---

## 💻 SQL Script Highlights

```sql

select * from blinkit_marketing_performance;

-- Q1: -- Display all unique channels available in the dataset.

select distinct(channel)
from blinkit_marketing_performance;

-- Q2: -- Fetch campaign_id, campaign_name, and spend where impressions are greater than 5000.

select campaign_id,campaign_name,spend,impressions
from blinkit_marketing_performance
where impressions > 5000;


-- Q3: -- Find the total revenue_generated for each target_audience.

select target_audience, round(sum(revenue_generated) )as revenue_generated
from blinkit_marketing_performance
group by target_audience;


-- Q4: -- Calculate the average conversions for each channel and sort by highest average first.

select channel, avg(conversions) as conversions
from blinkit_marketing_performance
group by channel
order by conversions desc
limit 1;

-- Q5: -- Retrieve top 5 campaigns based on conversions where channel is either 'App' or 'Email'.

SELECT campaign_name, SUM(conversions) AS total_conversions, channel
FROM blinkit_marketing_performance
WHERE channel IN ('App', 'Email')
GROUP BY campaign_name, channel
ORDER BY total_conversions DESC
LIMIT 5;

-- Q6: -- Count total campaigns for each channel having an average roas greater than 2.5.

select channel,  count(campaign_name) as campaigns
from blinkit_marketing_performance
group by channel
having  avg(roas) > 2.5;

-- Q7: -- Calculate Conversion Rate (conversions / clicks) as conversion_rate for each campaign.


select campaign_name, campaign_id,  round(conversions / clicks,2)  as conversion_rate
from blinkit_marketing_performance;


-- Q8: -- Create a view named top_performing_campaigns for rows where roas is greater than 3.0.

create view top_performing_campaigns
as
select campaign_id,campaign_name,roas
from blinkit_marketing_performance
where roas > 3.0;

select*from top_performing_campaigns;

-- Q9: -- Rank campaigns by spend within each channel using a window function.

select campaign_id,campaign_name,channel,spend  ,
dense_rank () over ( partition by  channel  order by spend desc)
from blinkit_marketing_performance;



-- Q10: -- Calculate the total spend and total revenue for each month from the date column.

select date_format(date,'%y-%m') as month, round(sum(spend)) as total_spend, round(sum(revenue_generated) )as total_revenue
from blinkit_marketing_performance
group by month 
order by month asc;
