### Introduction
to get started you need a database sample called **sakila**, which can be downloaded from https://github.com/jOOQ/sakila. you may also need the following command to import sql files:
```bash
createdb sakila
psql -U $USER -d sakila -a -f postgres-sakila-schema.sql
psql -U $USER -d sakila -a -f postgres-sakila-insert-data.sql
```

### Chapter 1
- a **primary key** consisting of two or more columns is known as a **compound key**. page 6
- **natural key** vs **surrogate key**. page 6
- **foreign keys** One or more columns that can be used together to identify a single row in another table. page 8
- **SQL schema statements**, **SQL data statements**, **SQL transaction statements**, page 9
- **data dictionary**,is known collectively as metadata. page 9
- the **optimizer** & **optimizer hints** engine. page 10.

### Chapter 2
- listing databases, **mysql** `show databases`, **postgresql** `\list` or `\l`, page 19
- switching database, **mysql** `use <database name>`, postgresql `\connect <database name>` or `\c <database name>`
- in mysql db running a query like `SELECT now()` will show timing, to enable timing in postgresql use `\timing`, page 19
```psql
\timing
/* for more information use the following */
\?
```
- exiting mysql `quit;` or `exit;`, exiting postgresql `\q`, page 20
- list database character set, in mysql `SHOW CHARACTER SET`, it could be in postgresql using `SHOW SERVER_ENCODING;` or the following:
```sql
SELECT DISTINCT pg_catalog.pg_encoding_to_char(conforencoding) from pg_catalog.pg_conversion order by pg_encoding_to_char;
```
- for table description, mysql `desc <table name>`, postgresql `\d <table name>` , there is also table schema `\d+ <table name>` page 31
- to the result as xml, in mysql you could use `--xml` from command line when prompted to database as following:
```bash
mysql -u lrngsql -p --xml bank
```
or 
```sql
SELECT * FROM favorite_food FOR XML AUTO, ELEMENTS
```
  in postgresql you could use function as
```sql
select query_to_xml('select * from persona', true, false, '');
select table_to_xml('persona', true, false, '');
```
  page 37
-Column Value Violations


### Chapter 3

- Query Clauses

|Clause name| Purpose|
|--------------|----------|
|SELECT| Determines which columns to include in the query's result set|
|FROM| Identifies the tables from which to retrieve data and how the tables should be joined|
|WHERE| Filters out unwanted data|
|GROUP BY| Used to group rows together by common column values|
|HAVING| Filters out unwanted groups|
|ORDER BY| Sorts the rows of the final result set by one or more columns|

- to get the current version of db and other info
```psql
select version();
select session_user, current_user;
```

- types of table which used by the __FROM__ clause

| Type |
|------|
| Permanent tables (created using create `table` statement) |
| Derived tables (rows returned by a subquery and held in memory) |
| Temporary tables (volatile data held in memory) |
| Virtual tables (created using the `create view` statement) |

- example of (subquery-generated) tables
```sql
SELECT concat(cust.last_name, ', ', cust.first_name) full_name 
	FROM (
		SELECT first_name, last_name, email FROM customer WHERE first_name = 'JESSIE') cust;
```

- example of creating temporary table
```sql
CREATE TEMP TABLE actors_j (
  actor_id SMALLINT,
  first_name VARCHAR(45),
  last_name VARCHAR(45)
);
```

- example of creating a view
```sql
CREATE VIEW cust_vw AS SELECT customer_id, first_name, last_name, active FROM customer;
```

- example of creating a table link or `JOIN`
```sql
SELECT customer.first_name, customer.last_name, rental.rental_date::time
  FROM customer INNER JOIN rental 
    ON customer.customer_id = rental.customer_id;
```
- another example of casting
```sql
SELECT customer.first_name, customer.last_name, cast(rental.rental_date AS time)
  FROM customer INNER JOIN rental 
    ON customer.customer_id = rental.customer_id;
```


Exercise 3-1
```sql
SELECT actor_id AS "ID", first_name AS "First Name", last_name AS "Last Name" FROM actor ORDER BY last_name, first_name;
```
Exercise 3-2

```sql
SELECT actor_id AS "ID", first_name AS "First Name", last_name AS "Last Name" From actor WHERE last_name = 'WILLIAMS' OR last_name = 'DAVIS';

/* or it could be the following */

SELECT actor_id AS "ID", first_name AS "First Name", last_name AS "Last Name" From actor WHERE last_name IN ('WILLIAMS','DAVIS');

```

Exercise 3-3 
```sql
SELECT DISTINCT customer_id FROM rental WHERE rental_date::date = '2005-07-05';
SELECT DISTINCT customer_id FROM rental WHERE cast(rental_date AS date) = '2005-07-05';
```

Exercise 3-4 

```sql
SELECT c.email, r.return_date FROM customer c INNER JOIN rental r ON c.customer_id = r.customer_id WHERE CAST(r.rental_date AS date) = '2005-06-14' ORDER BY 2 DESC;

SELECT c.email, r.return_date FROM customer c INNER JOIN rental r ON c.customer_id = r.customer_id WHERE r.rental_date::date = '2005-06-14' ORDER BY 2 DESC;
```

### Chapter 4

- Inequality operator `<>` like `!=`

- converting timestamp to year using `DATE_PART`
```sql

SELECT c.email, DATE_PART('YEAR', r.rental_date) FROM customer c INNER JOIN rental r ON c.customer_id = r.customer_id WHERE DATE_PART('YEAR', r.rental_date) <> 2005;

```
- example of using `BETWEEN` operator
```sql
SELECT customer_id, rental_date FROM rental WHERE rental_date::date BETWEEN '2005-06-14' AND '2005-06-16';
```

- `BETWEEN` can be used in string ranges
```sql
SELECT last_name, first_name FROM customer WHERE last_name BETWEEN 'FA' AND 'FR';
```

- using regular expression to filter on `SELECT`
```sql
SELECT last_name, first_name FROM customer WHERE last_name ~ '^[QY]';
```

### Chapter 5

- Cartesian product example
```sql
SELECT c.first_name, c.last_name, a.address FROM customer c CROSS JOIN address a;
```

- example of using inner joins
```sql
SELECT c.first_name, c.last_name, a.address FROM customer c JOIN address a ON c.adderss_id = a.address_id;
/* same as */
SELECT c.first_name, c.last_name, a.address FROM customer c JOIN address a USING(address_id);
/* same as */
SELECT c.first_name, c.last_name, a.address FROM customer c INNER JOIN address a USING(address_id);

```

- joining three tables
```sql
SELECT c.first_name, c.last_name, ct.city FROM customer c
    INNER JOIN address a ON c.address_id = a.address_id 
    INNER JOIN city ct ON a.city_id = ct.city_id;
```

### Chapter 6

* page 106: difference between `UNION` & `UNION ALL`
* page 107: the following query
```sql
SELECT 'ACTR' typ, a.first_name, a.last_name FROM actor a UNION ALL
  SELECT 'ACTR' typ, a.first_name, a.last_name FROM actor a;

/* can be counted by the following query */
SELECT count(*) FROM (
  SELECT 'ACTR' typ, a.first_name, a.last_name FROM actor a UNION ALL
    SELECT 'ACTR' typ, a.first_name, a.last_name FROM actor a
) combo;
```
* page 111: sorting compound query
```sql
/* if you didn't specify column alias as in the following sorting could result an error*/
SELECT a.first_name fname, a.last_name lname FROM actor a WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
UNION ALL
SELECT c.first_name, c.last_name FROM customer c WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
ORDER BY lname, fname;

/* error arise from the following query */
SELECT a.first_name fname, a.last_name lname FROM actor a WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
UNION ALL
SELECT c.first_name, c.last_name FROM customer c WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%'
ORDER BY last_name, first_name;

```

### Chapter 7

* page 118: many function for string manipulation doesn't exist in postgresql so the following website is good for reference https://www.sqliz.com/postgresql-ref/quote_literal/



### Chapter 8

- page 149: ordering by a generated column could be by 2 ways
```sql
SELECT customer_id, count(*) FROM rental GROUP BY customer_id ORDER BY 2 DESC;
/* THIS IS THE SAME LIKE THE FOLLOWING QUERY */
SELECT customer_id, count(*) e FROM rental GROUP BY customer_id ORDER BY e DESC;
```
- page 150: usage of `HAVING`

```sql
/* FILTER OUT RESULT OF A GENERATED COLUMN */
SELECT customer_id, count(*) FROM rental GROUP BY customer_id HAVING count(*) >= 40;
```


- page 157: grouping via expressions

```sql
SELECT extract('YEAR' FROM rental_date), count(*) how_many FROM rental GROUP BY extract('YEAR' from rental_date);

/* or */

SELECT extract('YEAR' FROM rental_date) ee, count(*) how_many FROM rental GROUP BY ee;

```
- page 157: generating rollups
```sql
SELECT fa.actor_id, f.rating, count(*) FROM film_actor fa
  INNER JOIN film f ON fa.film_id = f.film_id
  GROUP BY ROLLUP (fa.actor_id, f.rating)
  ORDER BY 1,2;
```

### Chapter 9
