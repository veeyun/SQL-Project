What issues will you address by cleaning the data?

1. Remove unwanted/irrelevant/duplicate observations
2. Ensure accurate datatypes 
3. Address missing values (can also perform linear rgression or replace with median value?)
4. Fix typos 


Queries:
Below, provide the SQL queries you used to clean your data.


2. Ensure accurate datatypes

- totaltransactionrevenue is divided by 1000000 to get the correct dollar amount
```
UPDATE all_sessions
SET totaltransactionrevenue = ROUND(totaltransactionrevenue/1000000, 2)
WHERE totaltransactionrevenue IS NOT NULL;
```

- City and country are cleaned by setting rows with out of range data to NULL
```
UPDATE all_sessions
SET 
	city = NULL 
WHERE 
	city = '(not set)' OR
	city = 'not available in demo dataset'
```

```
UPDATE all_sessions
SET 
	country = NULL 
WHERE 
	country = '(not set)'
```

4. Fix Typos 

One typo encountered during the exercise was the city New York being assigned to country Canada. 
Assumption was made that the city is correct and country is a typo. 

Thus the country for this particular row was updated to United States with the SQL query below:

```
UPDATE all_sessions
SET country = 'United States'
WHERE country = 'Canada' AND city = 'New York';
```
