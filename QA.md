What are your risk areas? Identify and describe them.

* The tables contained similar data, which made it more difficult to choose which data to use for our analysis.


* Column names were not always clear in multiple tables so there were assumptions, which could have led to the inaccuracy of results (e.g. product_ordered has no transaction)


* There were a lot of missing values and duplicate values which could risk to result in a totaltransactionrevenue being too low/high.


* Making assumptions when cleaning and updating the tables could risk to have the data be skewed (e.g. country and city mismatch)


* Updating fields could risk to losing important data of the existing tables


* Due to the redundency of the fields in multiple tables, not many tables were used in the queries, which could risk to a very skewed result (e.g. query based on a low sample size (81) for one year of transactions)

* Unit prices do not make sense, so as given in the hint, we need to divide by 1,000,000 and round decimal by monetary value


QA Process:
Describe your QA process and include the SQL queries used to execute it.

* Rather than using the all_sessions table (which had limited data on totaltransactionrevenue or products ordered), I used the table (sales_by_sku) with more data points by product SKU.

``` SQL
SELECT COUNT(*)
FROM (
    SELECT DISTINCT * 
    FROM table_name
    WHERE column_name IS NOT NULL
)
```
Where table_name is sales_by_sku

Where column_name is total_ordered 


* Updating fields (e.g. text) risks to lose important data in existing table so we can check with CASE WHEN. 


```SQL
SELECT column_name1, column_name2
CASE 
    WHEN column_name1 = 'country_name' AND column_name2 = 'city_name'
    ELSE column_name
END AS column_name
FROM table_name;
```

We can select and check columns first and can set them as NULL or 'insufficient data' if data does not make sense.
```SQL
UPDATE table_name
SET 
	column_name1 = NULL 
WHERE 
	column_name1 = '(not set)' OR
	column_name1 = 'not available in demo dataset'
```

Where column_name1 is city, column_name2 is country

Where table_name is all_sessions

* Addressing missing and duplicate data

First, always check raw table to identify the number of rows.

```SQL
SELECT * 
FROM table_name;
```
Then, check data with DISTINCT if table has duplicates. We can compare the number of rows from raw data table and then see the difference to that of DISTINCT

```SQL 
SELECT DISTINCT *
FROM table_name;
```

Missing values was difficult to address for the tables provided from this database. 
In this case, the best approach was to not remove, fill or update the data because we were unsure of the meaning for the fields in the multiple tables.
If missing values were filled with only assumption, results would be greatly skewed.
If we were to replace missing values, we can use the CASE WHEN statement:


```SQL
SELECT CASE
    WHEN column_name IS NULL THEN condition_1; 
    ELSE column_name
END as column_name
FROM table_name
```

* Make sense of the unit price values by dividing value by 1000000, and then ROUND to nearest.
Also need to include a WHERE statement to filter out the empty records in the field. 

```SQL
UPDATE all_sessions
SET column_name_1 = ROUND(column_name_1/1000000, 2)
WHERE column_name_1 IS NOT NULL;
```


