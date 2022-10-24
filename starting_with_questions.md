Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

Data is cleaned by looking at countries and cities and ensuring that each country and city corresponds correctly with one another and that there are no errors (e.g. New York is not in Canada).

The sum of totaltransactionrevenue is counted across each city, country by using a window function. 

SQL Queries:

```
SELECT DISTINCT country, city, SUM(totaltransactionrevenue)
OVER (PARTITION BY country, city) as revenueByCountryCity
FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL and city IS NOT NULL
ORDER BY revenueByCountryCity DESC; 
```


Answer:

| Country      | City | Total Transaction Revenue     |
| :---        |    :----:   |          ---: |
| United States |	San Francisco |	1564.32 |
| United States |	Sunnyvale |	992.23 |
| United States |	Atlanta |	854.44 |
| United States |	Palo Alto |	608.00 |
| Israel |	Tel Aviv-Yafo |	602.00 |
| United States |	New York| 	598.35 |
| United States |	Mountain View |	483.36 |
| United States |	Los Angeles |	479.48 |
| United States |	Chicago |	449.52 |
| Australia |	Sydney | 358.00 |
| United States |	Seattle |	358.00 |
| United States |	San Jose |	262.38 |
| United States |	Austin | 157.78 |
| United States |	Nashville |	157.00 |
| United States |	San Bruno |	103.77 |
| Canada | Toronto |	82.16 |
| United States |	Houston |	38.98 |
| United States |	Columbus |	21.99 |
| Switzerland |	Zurich | 16.99 |


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







