# PostgreSQL

1. **Creating a Table**

```sql
CREATE TABLE person (
	name varchar(50),
  age INTEGER,
  profile_url varchar(200)
)
```

1. **Inserting into a Table**

```sql
INSERT INTO
  cities (name, age, profile_url)
VALUES
  ('myName', 20, 'https://profile_url.com'),
  ('mySecondName', 30, 'https://profile_url2.com');
```

1. **Selecting from a Table**

```sql
SELECT * FROM cities
```

```sql
SELECT name , country FROM cities
```

1. **We can also transform data while fetching data from Table**

```sql
SELECT name , population / area AS population_density FROM cities
```

1. **Introduction to Filtering**

```sql
SELECT name FROM cities WHERE name = 'Mumbai'
```

1. **Filtering : BETWEEN**

```sql
SELECT name FROM cities WHERE area BETWEEN 2000 AND 4000
```

1. **Filtering : IN / NOT IN**

```sql
SELECT name FROM cities WHERE name IN ('Delhi' , 'Sanghai')
```

1. **Compound Filtering**

```sql
SELECT name FROM cities WHERE name IN ('Delhi' , 'Sanghai') AND area = 2000
```

1. **Updating Records in Table**

```sql
UPDATE cities SET population = 2000 WHERE name = 'Tokyo'
```

1. **Deleting Records from Table**

```sql
DELETE FROM cities WHERE name = 'Tokyo'
```

1. **Relations**
    1. **One To Many**
    2. **Many To One**
2. **Key**
    1. **Primary Key**
    2. ******Foreign Key******
3. **Primary Key**

```sql
CREATE TABLE users (
	id SERIAL PRIMARY KEY,
  name VARCHAR(50)
);
```

1. **Foreign Key**

```sql
CREATE TABLE photos (
	id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES users(id)
);
```

1. ******Foreign Key constraints around Insertion******
    1. ******If you are providing a value to foreign key they doesn’t exist it will give an ERROR!******
    2. ********************************************************************If you don’t want to associate a value to foreign key you can use NULL********************************************************************
2. **********************************************Foreign Key constraints around Deletion**********************************************
    1. ************************While deleting a record that is referenced in another table we can define some behaviors************************ 
    2. ****************************************These behaviors are****************************************
    
    | Name | Behavior |
    | --- | --- |
    | ON DELETE RESTRICT | Throw an Error |
    | ON DELETE CASCADE | Delete the reference too |
    | ON DELETE SET NULL | Set the reference to NULL |
3. ****Foreign Key constraints around Deletion****

```sql
CREATE TABLE photoes (
	id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE
);
```

1. **JOINS**

```sql
SELECT contents , username 
FROM comments
JOIN users on users.id = comments.user_id
```

1. **************Some gotchas on JOINS**************
    1. **********************If names of the columns collide we must provide a context**********************
    2. **************************There are multiple types of JOIN**************************
    3. **************************The order between FROM and JOIN sometimes makes a difference (When we don’t find a match it’s excluded)**************************
2. ********Different kind of JOINS********
    1. ************************************************************************************INNER JOIN (Include only when found in both table)************************************************************************************
    2. ******LEFT  JOIN (Include only when found in both table or in the left table)******
    3. ******RIGHT  JOIN (Include only when found in both table or in the right table)******
    4. ********FULL JOIN (Include when found in either of the table)********

```sql
SELECT url , username
FROM photos
LEFT JOIN users on users.id = photos.user_id
```

1. ****************************************Aggregation of Records : Grouping and Aggregation****************************************
2. ****************GROUP BY****************
    1. **************************************************************************It first find all the unique values of the GROUP BY column**************************************************************************
    2. ************************************After finding unique values, it fill all the rows in each group of unique values************************************
    3. ************************************************We can only select the grouped columns************************************************
    
    ```sql
    SELECT user_id
    FROM photos
    GROUP BY user_id
    ```
    
3. ******************************************************************GROUP BY alone is not so useful, therefore we use AGGREGATE functions to make is more useful******************************************************************
4. **************************************AGGREGATE Functions**************************************
    1. ******The columns that we can’t select in GROUP BY , we can apply an AGGREGATE functions to them and get useful data from them******
    2. ******************************Some of the aggregate functions are : MIN() , MAX() , COUNT() , AVG() , SUM()****************************** 
5. **********************************************************Combining GROUP BY and AGGREGATES**********************************************************

```sql
SELECT user_id , COUNT(id)
FROM photos
GROUP BY user_id
```

1. **************************The correct order : FROM → JOIN → WHERE → GROUP BY → HAVING**************************
2. **************HAVING************** 
    1. ********************************************************************************You will always use HAVING with GROUP BY********************************************************************************
    2. ******HAVING is used for filtering in groups******
    
    ```sql
    SELECT photo_id , COUNT(*)
    FROM COMMENTS 
    WHERE photo_id < 3
    GROUP BY photo_id
    HAVING COUNT(*) > 2;
    ```
    
3. ******SORTING******
    
    ```sql
    SELECT * 
    FROM products
    ORDER BY price DESC , weight ASC;
    ```
    
4. ************OFFSET************
    1. **************************************************************************OFFSET is used to skip first “n” number of values************************************************************************** 
    
    ```sql
    SELECT * 
    FROM products
    OFFSET 40;
    ```
    
5. **********LIMIT**********
    1. ****************************************LIMIT is used to get first “n” number of values****************************************
    
    ```sql
    SELECT * 
    FROM products
    ORDER BY price
    LIMIT 3
    ```
    
6. ************UNIONS************
    1. ****It is used to merge the result of two or more different queries into one****
    2. ************************The common item will be included only once , but if you want to include to separately use UNION ALL instead of UNION************************
    3. ************************While doing unions the names of columns and the type of each named columns must be same************************ 
    
    ```sql
    (	
      SELECT * 
    	FROM products
    	ORDER BY price
    	LIMIT 4
    )
    JOIN
    (
      SELECT * 
    	FROM products
    	ORDER BY price / weight 
    	LIMIT 4
    );
    ```
    
7. ********************SUB-QUERIES********************
    
    ```sql
    SELECT name , price 
    FROM products
    WHERE price > (
    	SELECT MAX(price)
      FROM products
      WHERE department = 'Toys'
    );
    ```
    
8. **********To learn more about sub-queries go to the sub-queries folder**********
9. ************************************Find unique values************************************
    
    ```sql
    SELECT DISTINCT departments 
    FROM products
    ```
    
10. **********************CASE in SQL**********************
    
    ```sql
    SELECT 
    	name, 
      price,
      CASE
      	WHEN price > 500 THEN 'costly'
        WHEN price > 300 THEN 'very costly'
        ELSE 'cheap'
      END
    FROM products
    ```