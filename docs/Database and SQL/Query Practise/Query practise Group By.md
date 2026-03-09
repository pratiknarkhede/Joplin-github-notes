https://sqliteonline.com/ 

&nbsp;

&nbsp;

The `GROUP BY` clause is used to group rows that have the same values into summary rows.

```sql
SELECT country,COUNT(*) FROM Customer group by country;
```

&nbsp;

![fed300454602dfba3746ca51fe1619cb.png](../../_resources/fed300454602dfba3746ca51fe1619cb.png)

&nbsp;

* * *

## Aggregate Functions with GROUP BY

GROUP BY is usually used with aggregate functions like COUNT(), MAX(), MIN(), SUM(), AVG().

total here is billed amount  
<br/>

```sql
SELECT 
billingcity,
sum(total) as Bill_per_city,
avg(total) as AVG_BILL_PER_CITY,
max(total) as max_AVG_BILL_PER_CITY,
min(total) as min_AVG_BILL_PER_CITY,
count(*) as number_of_bills_issued_per_city
FROM Invoice
GROUP BY billingcity;
```

![b740b97d494520aa86d07b46037d2fba.png](../../_resources/b740b97d494520aa86d07b46037d2fba.png)

&nbsp;

&nbsp;

* * *

&nbsp;

Find composers having more than 10 tracks, column should have composer name and count of record in the output result

```sql
--find composers having more than 10 tracks
SELECT composer, count(*) 
FROM Track
group by composer having COUNT(*) > 10; 

```

![c1a7a6e8f005200e624e161e2ff13c42.png](../../_resources/c1a7a6e8f005200e624e161e2ff13c42.png)