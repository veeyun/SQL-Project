Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

Data is cleaned by looking at countries and cities and ensuring that each country and city corresponds correctly with one another and that there are no errors (e.g. New York is not in Canada).

The sum of totaltransactionrevenue is counted across each city, country by using a window function. 

SQL Queries:

```SQL
SELECT DISTINCT country, city, SUM(totaltransactionrevenue)
OVER (
     PARTITION BY country, city) as revenueByCountryCity
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

```SQL
--by COUNTRY:
SELECT ROUND(avg(productquantity)), country
FROM all_sessions
GROUP BY country
HAVING avg(productquantity) IS NOT NULL
ORDER BY avg(productquantity) ASC;

--by CITY:
SELECT ROUND(avg(productquantity)), city
FROM all_sessions
GROUP BY city
HAVING avg(productquantity) IS NOT NULL
ORDER BY avg(productquantity) ASC;
```


Answer:






**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```SQL
SELECT DISTINCT v2productname, v2productcategory
FROM all_sessions;  --1571 rows for all distinct name and category

SELECT DISTINCT v2productname, v2productcategory
FROM all_sessions
WHERE v2productcategory != '(not set)'  --1322 rows for all category not including '(not set)'

SELECT DISTINCT v2productname, v2productcategory
FROM all_sessions
WHERE v2productcategory = '(not set)';  --249 rows for '(not set)'

SELECT DISTINCT v2productcategory  --74 distinct category
FROM all_sessions;


SELECT s.v2productcategory, s.country, s.city, COUNT (s.v2productcategory) AS Count_ProductCategory
FROM (
	SELECT DISTINCT *
	FROM sales_by_sku
	WHERE total_ordered > 0   -- assuming that total_ordered in the sales_by_sku is the total products ordered
) AS alt   -- subquery with FROM requires alias
LEFT JOIN all_sessions s
USING (productsku)
GROUP BY s.v2productcategory, s.country, s.city
ORDER BY Count_ProductCategory DESC, country, city;
```

Answer:
United States purchases a lot of YouTube, men's t-shirts and other apparel, electronics, accessories and bags, and office supplies. 
As opposed to the United States, countries, such as Hungary, India, Spain, Sweden, Thailand, Ukraine, purchase the least products from Youtube.
It appears that United States purchases more men apparel/t-shirts more than women's t-shirts. We also see that in Mountain View, there is a large number of purchases for Nest products, as well as Google brands. It could also be due to Google's HQ in Mountain View.
While Mountain View appears to have more purchases for Nest products, they have minimal/less purchases on children's drinkware and apparel. 
Germany and the United Kingdom also seem to purchase a lot of YouTube products. 
I believe that next steps, if time permits, I should clean the product category field as the spellings are not consistent. For example, USA purchases a lot of Nest products (86 count) in Mountain view, but when sorted, 
there is also Nest-USA and count appears to be signigicantly less (2).



**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries: 
```SQL
SELECT DISTINCT v2productname, country, city, SUM(totaltransactionrevenue) AS SUM_total_transaction_revenue
FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL
GROUP BY country, city, v2productname
ORDER BY SUM_total_transaction_revenue DESC;
```


Answer:

The top-selling product from:

- United States - Reusable Shopping Bag
- Israel - Nest Protect Smoke + CO Black Wired Alarm
- Australia - Nest Cam Indoor Security Camera
- Canada - Google Men's 3/4 Sleeve Raglan Henley Grey
- Switzerland - Youtube Men's 3/4 Sleeve Henley






**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```SQL
-- Impact of revenue generated from each city from descending order
SELECT DISTINCT country, city, SUM(totaltransactionrevenue) AS SUM_total_transaction_revenue
FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL
GROUP BY city, country
ORDER BY SUM(totaltransactionrevenue) DESC;

-- Impact of revenue generated from each country only from descending order
SELECT DISTINCT country, SUM(totaltransactionrevenue) AS SUM_total_transaction_revenue
FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL
GROUP BY country
ORDER BY SUM(totaltransactionrevenue) DESC;
```


Answer:

- United States is the country that had the most impact of revenue generated. San Francisco in the United States generated the most revenue as opposed to the other cities in the United States, such as Suunyvale, Atlanta, and Palo Alto.
- Other than the United States, Tel Aviv-Yafo, Israel, and Sydney in Australia also had a significant impact from the revenue generated.
- Canada and Switzerland, respectively, had the least signficant impact. 

| Country      | Sum of Total Transaction Revenue     |
| :---:          |          :---: |
| United States | 13222.16 |
|Israel |	602.00 |
|Australia |	358.00 |
|Canada |	82.16 |
|Switzerland |	16.99 |





