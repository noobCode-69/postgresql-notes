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
CREATE TABLE photoes (
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