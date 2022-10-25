## Cleaning Data

Queries:
1. Examples of filtering out duplicates, NULLs and remove columns with no significance (e.g. contains no field data)

```SQL
SELECT totaltransactionrevenue, *
FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL
ORDER BY totaltransactionrevenue;
```

```SQL
SELECT DISTINCT v2productname, v2productcategory
FROM all_sessions
WHERE v2productcategory != '(not set)';
```

```SQL
ALTER TABLE all_sessions
DROP COLUMN productRefundAmount;
```

```SQL
ALTER TABLE all_sessions
DROP COLUMN itemQuantity;
```
```SQL
ALTER TABLE all_sessions
DROP COLUMN itemRevenue;
```

```SQL
ALTER TABLE all_sessions
DROP COLUMN searchKeyword;
```


2. Ensure accurate datatypes

- Example of computation (dividing by 1000000) for unit pricing
totaltransactionrevenue is divided by 1000000 to get the correct dollar amount
``` SQL
UPDATE all_sessions
SET totaltransactionrevenue = ROUND(totaltransactionrevenue/1000000, 2)
WHERE totaltransactionrevenue IS NOT NULL;
```

- City and country are cleaned by setting rows with out of range data to NULL
```SQL
UPDATE all_sessions
SET 
	city = NULL 
WHERE 
	city = '(not set)' OR
	city = 'not available in demo dataset'
```

```SQL
UPDATE all_sessions
SET 
	country = NULL 
WHERE 
	country = '(not set)'
```

3. Address missing values 

We can't have a totalTransactionRevenue WITHOUT any transactions. 
So, we need to check for totaltransactionrevenue as NULL and transactions as NOT NULL because if they both don't have any RETURNS, then there are no missing values.
Because there no return query, there is no missing value (both totaltransactionrevenue and transaction are NULL)

```SQL
SELECT totaltransactionrevenue, transactions, transactionrevenue, transactionid 
FROM all_sessions
WHERE totaltransactionrevenue IS NULL
	AND transactions IS NOT NULL;
```


Also looked missing values by seeing if other tables have information 
but couldn't find. 



4. Fix Typos 

One typo encountered during the exercise was the city New York being assigned to country Canada. 
Assumption was made that the city is correct and country is a typo. 

Thus the country for this particular row was updated to United States with the SQL query below:

```SQL
UPDATE all_sessions
SET country = 'United States'
WHERE country = 'Canada' AND city = 'New York';
```

If I had more time, I would also fix typos and clean the field "v2productcategory" in all_sessions table. 
That field consisted of data with wordings that were not consistent. For example, NEST products in USA appeared as one of the significant products ordered in the USA, Mountain View
but also the least significant product ordered in USA because the text is not consistent.

