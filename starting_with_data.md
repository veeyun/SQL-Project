
Question 1: Find the total number of unique visitors by searching sites.

SQL Queries:

```SQL
SELECT DISTINCT COUNT(fullvisitorid), channelgrouping
FROM all_sessions
GROUP BY channelgrouping
HAVING COUNT(fullvisitorid) IS NOT NULL
```

Answer: There 8653 unique visitors who searched on the web.




Question 2: Where are people coming to this website from outside of the United States?

SQL Queries:
```SQL
SELECT country
FROM all_sessions
WHERE country != 'United States'
GROUP BY country
order by country;
```

Answer: There are about 134 countries, including Algeria, Argentina, Armenia, Australia...





Question 3: What is the average number of page views for people in each country?

SQL Queries:
```SQL
SELECT ROUND(AVG(pageviews)), country, city
FROM all_sessions
GROUP BY country, city
HAVING AVG(pageviews) IS NOT NULL
ORDER BY AVG(pageviews) DESC;

```

Answer: 19 San Marino, 18 Ireland, 16 United States, 14 France, 13 Japan... 
