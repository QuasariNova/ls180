# Getting Started
## SQL Basics Tutorial
1. Write a query that returns all of the customer names from the `orders` table

```sql
SELECT customer_name FROM order;
```

```
  customer_name
-----------------
 Todd Perez
 Florence Jordan
 Robin Barnes
 Joyce Silva
 Joyce Silva
(5 rows)
```
---

2. Write a query that returns all of the orders that include a Chocolate Shake.

```sql
SELECT * FROM orders WHERE drink = 'Chocolate Shake';
```

```
 id |  customer_name  |         burger          | side  |      drink
----+-----------------+-------------------------+-------+-----------------
  2 | Florence Jordan | LS Cheeseburger         | Fries | Chocolate Shake
  4 | Joyce Silva     | LS Double Deluxe Burger | Fries | Chocolate Shake
(2 rows)
```
---

3. Write a query that returns the burger, side, and drink for the order with an id of 2.

```sql
SELECT burger, side, drink FROM orders WHERE id = 2;
```

```
     burger      | side  |      drink
-----------------+-------+-----------------
 LS Cheeseburger | Fries | Chocolate Shake
(1 row)
```
---

4. Write a query that returns the name of anyone who ordered Onion Rings.

```sql
SELECT customer_name FROM orders WHERE side = 'Onion Rings';
```

```
 customer_name
---------------
 Robin Barnes
 Joyce Silva
(2 rows)
```

# Your First Database: Schema
## Create and View Databases
1. From the Terminal, create a database called database_one.

```bash
createdb database_one
```
---

2. From the Terminal, connect via the psql console to the database that you created in the previous question.

```bash
psql -d database_one
```
---

3. From the psql console, create a database called database_two.

```SQL
CREATE DATABASE database_two;
```

```
CREATE DATABASE
```
---

4. From the psql console, connect to database_two.

```
\c database_two
```

```
You are now connected to database "database_two" as user "postgres".
database_two=#
```
---
5. Display all of the databases that currently exist.

```
\list
```

```
     Name     |  Owner   | Encoding |          Collate           |           Ctype            | ICU Locale | Locale Provider |   Access privileges
--------------+----------+----------+----------------------------+----------------------------+------------+-----------------+-----------------------
 database_one | postgres | UTF8     | English_United States.1252 | English_United States.1252 |            | libc            |
 database_two | postgres | UTF8     | English_United States.1252 | English_United States.1252 |            | libc            |
 postgres     | postgres | UTF8     | English_United States.1252 | English_United States.1252 |            | libc            |
 sql_book     | postgres | UTF8     | English_United States.1252 | English_United States.1252 |            | libc            |
 template0    | postgres | UTF8     | English_United States.1252 | English_United States.1252 |            | libc            | =c/postgres          +
              |          |          |                            |                            |            |                 | postgres=CTc/postgres
 template1    | postgres | UTF8     | English_United States.1252 | English_United States.1252 |            | libc            | =c/postgres          +
              |          |          |                            |                            |            |                 | postgres=CTc/postgres
(6 rows)
```
---

6. From the psql console, delete database_two.

```sql
\c database_one
DROP DATABASE database_two;
```

```
You are now connected to database "database_one" as user "postgres".
DROP DATABASE
```
---

7. From the Terminal, delete the database_one and ls_burger databases.

```bash
dropdb database_one
dropdb ls_burger
```

## Create and View Tables
1. From the Terminal, create a database called encyclopedia and connect to it via the psql console.

```bash
createdb encyclopedia
psql -d encyclopedia
```
---

2. Create a table called countries. It should have the following columns:

- An id column of type serial
- A name column of type varchar(50)
- A capital column of type varchar(50)
- A population column of type integer

The name column should have a UNIQUE constraint. The name and capital columns should both have NOT NULL constraints.

```sql
CREATE TABLE countries (
id serial UNIQUE NOT NULL,
name varchar(50) NOT NULL,
captial varchar(50) NOT NULL,
population integer
);
```
---

3. Create a table called famous_people. It should have the following columns:

- An id column that contains auto-incrementing values
- A name column. This should contain a string up to 100 characters in length
- An occupation column. This should contain a string up to 150 characters in length
- A date_of_birth column that should contain each person's date of birth in a string of up to 50 characters
- A deceased column that contains either true or false

The table should prevent NULL values being added to the name column. If the value of the deceased column is absent or unknown then false should be used.

```sql
CREATE TABLE famous_people (
id serial UNIQUE NOT NULL,
name varchar(100) NOT NULL,
occupation varchar(150),
date_of_birth varchar(50),
deceased boolean DEFAULT false
);
```
---

4. Create a table called animals that could contain the sample data below:

| Name | Binomial Name | Max Weight (kg) | Max Age (years) | Conservation Status |
|:---:|:---:|:---:|:---:|:---:|
| Lion | Pantera leo | 250 | 20 | VU |
| Killer Whale | Orcinus orca | 6,000 | 60 | DD |
| Golden Eagle | Aquila chrysaetos | 6.35 | 24 | LC |

- The database table should also contain an auto-incrementing id column.
- Each animal should always have a name and a binomial name.
- Names and binomial names vary in length but never exceed 100 characters.
- The max weight column should be able to hold data in the range 0.001kg to 40,000kg
- Conservation Status is denoted by a combination of two letters (CR, EN, VU, etc).

```sql
CREATE TABLE animals (
id serial UNIQUE NOT NULL,
name varchar(100) NOT NULL,
binomial_name varchar(100) NOT NULL,
max_weight_kg decimal(8, 3),
max_age_years integer,
conservation_status char(2)
);
```
---

5. List all of the tables in the encyclopedia database.

```
\dt
```

```
             List of relations
 Schema |     Name      | Type  |  Owner
--------+---------------+-------+----------
 public | animals       | table | postgres
 public | countries     | table | postgres
 public | famous_people | table | postgres
(3 rows)
```
---

6. Display the schema for the animals table.

```
\d animals
```

```
                                          Table "public.animals"
       Column        |          Type          | Collation | Nullable |               Default
---------------------+------------------------+-----------+----------+-------------------------------------
 id                  | integer                |           | not null | nextval('animals_id_seq'::regclass)
 name                | character varying(100) |           | not null |
 binomial_name       | character varying(100) |           | not null |
 max_weight_kg       | numeric(8,3)           |           |          |
 max_age_years       | integer                |           |          |
 conservation_status | character(2)           |           |          |
Indexes:
    "animals_id_key" UNIQUE CONSTRAINT, btree (id)
```
---

7. Create a database called ls_burger and connect to it.

```sql
CREATE DATABASE ls_burger;
\c ls_burger
```
---

8. Create a table in the ls_burger database called orders. The table should have the following columns:

- An id column, that should contain an auto-incrementing integer value.
- A customer_name column, that should contain a string of up to 100 characters
- A burger column, that should hold a string of up to 50 characters
- A side column, that should hold a string of up to 50 characters
- A drink column, that should hold a string of up to 50 characters
- An order_total column, that should hold a numeric value in dollars and cents. Assume that all orders will be less than $100.

The customer_name and order_total columns should always contain a value.

```sql
CREATE TABLE orders (
id serial,
customer_name varchar(100) NOT NULL,
burger varchar(50),
side varchar(50),
drink varchar(50),
order_total decimal(4, 2) NOT NULL
);
```

## Alter a Table
1. Make sure you are connected to the encyclopedia database. Rename the famous_people table to celebrities.

```sql
\c encyclopedia
ALTER TABLE famous_people
  RENAME TO celebrities;
```
---

2. Change the name of the name column in the celebrities table to first_name, and change its data type to varchar(80).

```sql
ALTER TABLE celebrities
  RENAME COLUMN name TO first_name;
ALTER TABLE celebrities
  ALTER COLUMN first_name TYPE varchar(80);
```
---

3. Create a new column in the celebrities table called last_name. It should be able to hold strings of lengths up to 100 characters. This column should always hold a value.

```sql
ALTER TABLE celebrities
  ADD COLUMN last_name varchar(100)
    NOT NULL;
```

4. Change the celebrities table so that the date_of_birth column uses a data type that holds an actual date value rather than a string. Also ensure that this column must hold a value.

```sql
ALTER TABLE celebrities
  ALTER COLUMN date_of_birth TYPE date
    USING date_of_birth::date;
ALTER TABLE celebrities
  ALTER COLUMN date_of_birth SET NOT NULL;
```
---
5. Change the max_weight_kg column in the animals table so that it can hold values in the range 0.0001kg to 200,000kg

```sql
ALTER TABLE animals
  ALTER COLUMN max_weight_kg TYPE decimal(10,4);
```
---

6. Change the animals table so that the binomial_name column cannot contain duplicate values.

```sql
ALTER TABLE animals
  ADD CONSTRAINT unique_binomial_name UNIQUE
  (binomial_name);
```
---

7. Connect to the ls_burger database. Add the following columns to the orders table:

- A column called customer_email; it should hold strings of up to 50 characters.
- A column called customer_loyalty_points that should hold integer values. If no value is specified for this column, then a value of 0 should be applied.

```sql
\c ls_burger
ALTER TABLE orders
  ADD COLUMN customer_email varchar(50),
  ADD COLUMN customer_loyalty_points integer DEFAULT;
```
---

8. Add three columns to the orders table called burger_cost, side_cost, and drink_cost to hold monetary values in dollars and cents (assume that all values will be less than $100). If no value is entered for these columns, a value of 0 dollars should be used.

```sql
ALTER TABLE orders
  ADD COLUMN burger_cost decimal(4, 2) DEFAULT 0,
  ADD COLUMN side_cost decimal(4, 2) DEFAULT 0,
  ADD COLUMN drink_cost decimal(4, 2) DEFAULT 0;
```
---

9. Remove the order_total column from the orders table.

```sql
ALTER TABLE orders
  DROP COLUMN order_total;
```

# Your First Database: DATA
## Add Data with INSERT
1. Make sure you are connected to the `encyclopedia` database. Add the following data to the `countries` table:

| Name | Capital | Population |
|:---:|:---:|:---:|
| France | Paris | 67,158,000 |

```sql
INSERT INTO countries (name, capital, population)
  VALUES ('France', 'Paris', 67158000);
```
---

2. Now add the following additional data to the `countries` table:

| Name | Capital | Population |
|:---:|:---:|:---:|
| USA | Washington D.C. | 325,365,189 |
| Germany | Berlin | 82,349,400 |
| Japan | Tokyo | 126,672,000 |

```sql
INSERT INTO countries (name, capital, population)
  VALUES ('USA', 'Washington D.C.', 325365189),
         ('Germany', 'Berlin', 82349400),
         ('Japan', 'Tokyo', 126672000);
```
---

3. Add an entry to the `celebrities` table for the singer and songwriter Bruce Springsteen, who was born on September 23rd 1949 and is still alive.

```sql
INSERT INTO celebrities (first_name, last_name, occupation, date_of_birth)
  VALUES ('Bruce', 'Springsteen', 'Singer/Songwriter', '1949-09-23');
```
---

4. Add an entry for the actress Scarlett Johansson, who was born on November 22nd 1984. Use the default value for the `deceased` column.

```sql
INSERT INTO celebrities (first_name, last_name, occupation, date_of_birth)
  VALUES ('Scarlett', 'Johansson', 'Actress', '1984-11-22');
```
---

5. Add the following two entries to the `celebrities` table with a single `INSERT` statement. For Frank Sinatra set `true` as the value for the deceased column. For Tom Cruise, don't set an explicit value for the deceased column, but use the default value.

| First Name | Last Name | Occupation | D.O.B. |
|:---:|:---:|:---:|:---:|
| Frank | Sinatra | Singer, Actor | December 12, 1915 |
| Tom | Cruise | Actor | July 03, 1962 |

```sql
INSERT INTO celebrities (first_name, last_name, occupation, date_of_birth, deceased)
  VALUES ('Frank', 'Sinatra', 'Singer, Actor', '1915-12-12', true),
         ('Tom', 'Cruise', 'Actor', '1962-07-03', DEFAULT);
```
---

6. Look at the schema of the celebrities table. What do you think will happen if we try to insert the following data?

| First Name | Last Name | Occupation | D.O.B. | Deceased |
|:---:|:---:|:---:|:---:|:---:|
| Madonna |  | Singer, Actress | '08/16/1958' | false |
| Prince |  | Singer, Songwriter, Musician, Actor | '06/07/1958' | true |

Since the `last_name` column has a `NOT NULL` constraint, if we do not set a value for the `last_name` column there will be an error thrown by PostgreSQL when we try to `INSERT` the data.

---

7. Update the `last_name` column of the `celebrities` table so that the data in the previous question can be entered, and then add the data to the table.

```sql
ALTER TABLE celebrities
  ALTER COLUMN last_name
    DROP NOT NULL;

INSERT INTO celebrities (first_name, occupation, date_of_birth, deceased)
  VALUES ('Madonna', 'Singer, Actress', '08/16/1958', false),
         ('Prince', 'Singer, Songwriter, Musician, Actor', '06/07/1958', true);
```

8. Check the schema of the `celebrities` table. What would happen if we specify a `NULL` value for `deceased` column, such as with the data below?

| First Name | Last Name | Occupation | D.O.B. | Deceased |
|:---:|:---:|:---:|:---:|:---:|
| Elvis | Presley | Singer, Musician, Actor | '01/08/1935' | NULL |

Since the `deceased` column does not have a `NOT NULL` constraint it should be set to `NULL`. Even though it set to have a `DEFAULT` constraint, explicitly setting something to `NULL` does not allow the `DEFAULT` constraint to work.

---

9. Check the schema of the animals table. What would happen if we tried to insert the following data to the table?

| Name | Binomial Name | Max Weight (kg) | Max Age (years) | Conservation Status |
|:---:|:---:|:---:|:---:|:---:|
| Dove | Columbidae Columbiformes | 2 | 15 | LC |
| Golden Eagle | Aquila Chrysaetos | 6.35 | 24 | LC |
| Peregrine Falcon | Falco Peregrinus | 1.5 | 15 | LC |
| Pigeon | Columbidae Columbiformes | 2 | 15 | LC |
| Kakapo | Strigops habroptila | 4 | 60 | CR |

The `binomial_name` column has a `UNIQUE` constraint set, so both the Pigeon and Dove rows will force this to raise an error. To fix this, we can remove the constraint.

```sql
ALTER TABLE animals
  DROP CONSTRAINT unique_binomial_name;
INSERT INTO animals (name, binomial_name, max_weight_kg, max_age_years, conservation_status)
  VALUES ('Dove', 'Columbidae Columbiformes', 2, 15, 'LC'),
         ('Golden Eagle', 'Aquila Chrysaetos', 6.35, 24, 'LC'),
         ('Peregrine Falcon', 'Falco Peregrinus', 1.5, 15, 'LC'),
         ('Pigeon', 'Columbidae Columbiformes', 2, 15, 'LC'),
         ('Kakapo', 'Strigops habroptila', 4, 60, 'CR');
```

10. Connect to the `ls_burger` database and examine the schema for the `orders` table.

Based on the table schema and following information, write and execute an INSERT statement to add the appropriate data to the orders table.

There are three customers -- James Bergman, Natasha O'Shea, Aaron Muller. James' email address is james1998@email.com. Natasha's email address is natasha@osheafamily.com. Aaron doesn't supply an email address.

James orders a LS Chicken Burger, Fries and a Cola. Natasha has two orders -- an LS Cheeseburger with Fries but no drink, and an LS Double Deluxe Burger with Onion Rings and a Chocolate Shake. Aaron orders an LS Burger with no side or drink.

The item costs and loyalty points are listed below:
| Item | Cost ($) | Loyalty Points |
| :---: | :---: | :---: |
| LS Burger |	3.00 |	10 |
| LS Cheeseburger |	3.50 |	15 |
| LS Chicken Burger |	4.50 |	20 |
| LS Double Deluxe Burger |	6.00 |	30 |
| Fries |	0.99 |	3 |
| Onion Rings |	1.50 |	5 |
| Cola |	1.50 |	5 |
| Lemonade |	1.50 |	5 |
| Vanilla Shake |	2.00 |	7 |
| Chocolate Shake |	2.00 |	7 |
| Strawberry Shake |	2.00 |	7 |

```sql
\c ls_burger
\d orders
INSERT INTO orders (customer_name, customer_email, burger, burger_cost, side, side_cost, drink, drink_cost, customer_loyalty_points)
  VALUES ('James Bergman', 'james1998@email.com', 'LS Chicken Burger', 4.5, 'Fries', .99, 'Cola', 1.5, 28),
         ('Natash O''Shea', 'natasha@osheafamily.com', 'LS Cheeseburger', 3.5, 'Fries', .99, NULL, DEFAULT, 18),
         ('Natash O''Shea', 'natasha@osheafamily.com', 'LS Double Deluxe Burger', 6, 'Onion Rings', 1.5, 'Chocolate Shake', 2, 43),
         ('Aaron Muller', NULL, 'LS Burger', 2, NULL, DEFAULT, NULL, DEFAULT, 10);
```

## Select Queries
1. Make sure you are connected to the encyclopedia database. Write a query to retrieve the population of the USA.

```sql
\c encyclopedia
SELECT population
  FROM countries
  WHERE name = 'USA';
```
---

2. Write a query to return the population and the capital (with the columns in that order) of all the countries in the table.

```sql
SELECT population, capital
  FROM countries;
```
---

3. Write a query to return the names of all the countries ordered alphabetically.

```sql
SELECT name
  FROM countries
  ORDER BY name;
```
---

4. Write a query to return the names and the capitals of all the countries in order of population, from lowest to highest.

```sql
SELECT name, capital
  FROM countries
  ORDER BY population;
```
---

5. Write a query to return the same information as the previous query, but ordered from highest to lowest.

```sql
SELECT name, capital
  FROM countries
  ORDER BY population DESC;
```
---

6. Write a query on the animals table, using ORDER BY, that will return the following output:

```
       name       |      binomial_name       | max_weight_kg | max_age_years
------------------+--------------------------+---------------+---------------
 Peregrine Falcon | Falco Peregrinus         |        1.5000 |            15
 Pigeon           | Columbidae Columbiformes |        2.0000 |            15
 Dove             | Columbidae Columbiformes |        2.0000 |            15
 Golden Eagle     | Aquila Chrysaetos        |        6.3500 |            24
 Kakapo           | Strigops habroptila      |        4.0000 |            60
(5 rows)
```

Use only the columns that can be seen in the above output for ordering purposes.

```sql
SELECT name, binomial_name, max_weight_kg, max_age_years
  FROM animals
  ORDER BY max_age_years, max_weight_kg, name DESC;
```
---

7. Write a query that returns the names of all the countries with a population greater than 70 million.

```sql
SELECT name
  FROM countries
  WHERE population > 70000000;
```
---

8. Write a query that returns the names of all the countries with a population greater than 70 million but less than 200 million.

```sql
SELECT name
  FROM countries
  WHERE population > 70000000
    AND population < 200000000;
```
---

9. Write a query that will return the first name and last name of all entries in the celebrities table where the value of the deceased column is not true.

```sql
SELECT first_name, last_name
  FROM celebrities
  WHERE deceased <> 'true';
```

Apparently, `NULL` keeps Elvis from showing up even though his `deceased` value is `NULL` not `true`. So launch School says to use this:

```sql
SELECT first_name, last_name
  FROM celebrities
  WHERE deceased <> 'true'
    OR deceased IS NULL;
```
---

10. Write a query that will return the first and last names of all the celebrities who sing.

```sql
SELECT first_name, last_name
  FROM celebrities
  WHERE occupation LIKE '%Singer%';
```
---

11. Write a query that will return the first and last names of all the celebrities who act.

```sql
SELECT first_name, last_name
  FROM celebrities
  WHERE occupation LIKE '%Act%';
```
---

12. Write a query that will return the first and last names of all the celebrities who both sing and act.

```sql
SELECT first_name, last_name
  FROM celebrities
  WHERE occupation LIKE '%Singer%'
    AND (occupation LIKE '%Actress%'
      OR occupation LIKE '%Actor%');
```
---

13. Connect to the `ls_burger` database. Write a query that lists all of the burgers that have been ordered, from cheapest to most expensive, where the cost of the burger is less than $5.00.

```sql
\c ls_burger
SELECT burger
  FROM orders
  WHERE burger_cost < 5
  ORDER BY burger_cost;
```
---

14. Write a query to return the customer name and email address and loyalty points from any order worth 20 or more loyalty points. List the results from the highest number of points to the lowest.

```sql
SELECT customer_name, customer_email, customer_loyalty_points
  FROM orders
  WHERE customer_loyalty_points >= 20
  ORDER BY customer_loyalty_points DESC;
```
---

15. Write a query that returns all the burgers ordered by Natasha O'Shea.

```sql
SELECT burger
  FROM orders
  WHERE customer_name = 'Natasha O''Shea';
```
---

16. Write a query that returns the customer name from any order which does not include a drink item.

```sql
SELECT customer_name
  FROM orders
  WHERE drink IS NULL;
```
---
17. Write a query that returns the three meal items for any order which does not include fries.

```sql
SELECT burger, side, drink
  FROM orders
  WHERE side <> 'Fries' OR side IS NULL;
```
---

18. Write a query that returns the three meal items for any order that includes both a side and a drink.

```sql
SELECT burger, side, drink
  FROM orders
  WHERE side IS NOT NULL
    AND drink IS NOT NULL;
```
---

## More on Select
1. Make sure you are connected to the `encyclopedia` database. Write a query to retrieve the first row of data from the `countries` table.

```sql
SELECT *
  FROM countries
  LIMIT 1;
```
---

2. Write a query to retrieve the name of the country with the largest population.

```sql
SELECT name
  FROM countries
  ORDER BY population DESC
  LIMIT 1;
```
---

3. Write a query to retrieve the name of the country with the second largest population.

```sql
SELECT name
  FROM countries
  ORDER BY population DESC
  LIMIT 1 OFFSET 1;
```
---

4. Write a query to retrieve all of the unique values from the binomial_name column of the `animals` table.

```sql
SELECT DISTINCT binomial_name
  FROM animals;
```
---

5. Write a query to return the longest binomial name from the `animals` table.

```sql
SELECT binomial_name
  FROM animals
  ORDER BY length(binomial_name) DESC
  LIMIT 1;
```
---

6. Write a query to return the first name of any celebrity born in 1958.

```sql
SELECT first_name
  FROM celebrities
  WHERE date_part('year', date_of_birth) = 1958;
```
---

7. Write a query to return the highest maximum age from the `animals` table.

```sql
SELECT max(max_age_years)
  FROM animals;
```
---

8. Write a query to return the average maximum weight from the animals table.

```sql
SELECT avg(max_weight_kg)
  FROM animals;
```
---

9. Write a query to return the number of rows in the `countries` table.

```sql
SELECT count(ID)
  FROM countries;
```
---

10. Write a query to return the total population of all the countries in the `countries` table.

```sql
SELECT sum(population)
  FROM countries;
```
---

11. Write a query to return each unique conservation status code alongside the number of animals that have that code.

```sql
SELECT conservation_status, count(id)
  FROM animals
  GROUP BY conservation_status;
```
---

12. Connect to the `ls_burger` database. Write a query that returns the average burger cost for all orders that include fries.

```sql
SELECT avg(burger_cost)
  FROM orders
  WHERE side = 'Fries';
```
---

13. Write a query that returns the cost of the cheapest side ordered.

```sql
SELECT min(side_cost)
  FROM orders
  WHERE side IS NOT NULL;
```
---

14. Write a query that returns the number of orders that include Fries and the number of orders that include Onion Rings.

```sql
SELECT side, count(id)
  FROM orders
  WHERE side IS NOT NULL
  GROUP BY side;
```

## Update Data in a Table
1. Make sure you are connected to the encyclopedia database. Add a column to the `animals` table called `class` to hold strings of up to 100 characters.

Update all the rows in the table so that this column holds the value `Aves`.

```sql
ALTER TABLE animals
  ADD COLUMN class varchar(100);
UPDATE animals
  SET class = 'Aves';
```
---

2. Add two more columns to the `animals` table called `phylum` and `kingdom`. Both should hold strings of up to 100 characters.

Update all the rows in the table so that `phylum` holds the value `Chordata` and `kingdom` holds `Animalia` for all the rows in the table.

```sql
ALTER TABLE animals
  ADD COLUMN phylum varchar(100),
  ADD COLUMN kingdom varchar(100);
UPDATE animals
  SET phylum = 'Chordata',
      kingdom = 'Animalia';
```
---

3. Add a column to the `countries` table called `continent` to hold strings of up to 50 characters.

Update all the rows in the table so France and Germany have a value of `Europe` for this column, Japan has a value of `Asia` and the USA has a value of `North America`.

```sql
ALTER TABLE countries
  ADD COLUMN continent varchar(50);
UPDATE countries
  SET continent = 'Europe'
  WHERE name = 'France' OR
    name = 'Germany';
UPDATE countries
  SET continent = 'Asia'
  WHERE name = 'Japan';
UPDATE countries
  set continent = 'North America'
  WHERE name = 'USA';
```
---

4. In the `celebrities` table, update the Elvis row so that the value in the `deceased` column is true. Then change the column so that it no longer allows NULL values.

```sql
UPDATE celebrities
  SET deceased = 'true'
  WHERE first_name = 'Elvis';
ALTER TABLE celebrities
  ALTER COLUMN deceased
    SET NOT NULL;
```
---

5. Remove Tom Cruise from the `celebrities` table.

```sql
DELETE FROM celebrities
  WHERE first_name = 'Tom'
    AND last_name = 'Cruise';
```
---

6. Change the name of the celebrities table to singers, and remove anyone who isn't a singer.

```sql
ALTER TABLE celebrities
  RENAME TO singers;
DELETE FROM singers
  WHERE occupation NOT LIKE '%Singer%';
```
---

7. Remove all the rows from the `countries` table.

```sql
DELETE FROM countries;
```
---

8. Connect to the `ls_burger` database. Change the drink on James Bergman's order from a Cola to a Lemonade.

```sql
UPDATE orders
  SET drink='Lemonade'
  WHERE id=1;
```
---

9. Add Fries to Aaron Muller's order. Make sure to add the cost ($0.99) to the appropriate field and add 3 loyalty points to the current total.

```sql
UPDATE orders
  SET side = 'Fries',
    side_cost = .99,
    customer_loyalty_points = 13
  WHERE id = 4;
```
---

10. The cost of Fries has increased to $1.20. Update the data in the table to reflect this.

```sql
UPDATE orders
  SET side_cost = 1.2
  WHERE side = 'Fries';
```

# Working With Multiple Tables
## Create Multiple Tables
1. Make sure you are connected to the `encyclopedia` database. We want to hold the continent data in a separate table from the country data.

    1. Create a continents table with an auto-incrementing id column (set as the Primary Key), and a continent_name column which can hold the same data as the continent column from the countries table.

    ```sql
    CREATE TABLE continents (
      id serial,
      continent_name varchar(50),
      PRIMARY KEY (id)
    );
    ```

    2. Remove the `continent` column from the `countries` table.

    ```sql
    ALTER TABLE countries
      DROP COLUMN continent;
    ```

    3. Add a `continent_id` column to the `countries` table of type integer.

    ```sql
    ALTER TABLE countries
      ADD COLUMN continent_id integer NOT NULL;
    ```

    4. Add a Foreign Key constraint to the `continent_id` column which references the `id` field of the `continents` table.

    ```sql
    ALTER TABLE countries
      ADD FOREIGN KEY (continent_id)
      REFERENCES continents (id);
    ```
---

2. Write statements to add data to the countries and continents tables so that the data below is correctly represented across the two tables. Add both the countries and the continents to their respective tables in alphabetical order.

| Name | Capital | Population | Continent |
| :---: | :---: | :---: | :---: |
| France | Paris | 67,158,000 |	Europe |
| USA |	Washington D.C. |	325,365,189 |	North America |
| Germany |	Berlin | 82,349,400 |	Europe |
| Japan |	Tokyo |	126,672,000 |	Asia |
| Egypt |	Cairo |	96,308,900 | Africa |
| Brazil | Brasilia |	208,385,000 |	South America |

```sql
INSERT INTO continents
  (continent_name)
  VALUES
    ('Europe'),
    ('North America'),
    ('Asia'),
    ('Africa'),
    ('South America');

INSERT INTO countries
  (continent_id, name, capital, population)
  VALUES
    (1, 'France', 'Paris', 67258000),
    (2, 'USA', 'Washington D.C.', 325365189),
    (1, 'Germany', 'Berlin', 82349400),
    (3, 'Japan', 'Tokyo', 126672000),
    (4, 'Egypt', 'Cairo', 96308900),
    (5, 'Brazil', 'Brasilia', 208385000);
```
---
3. Examine the data below:

| Album Name | Released | Genre | Label | Singer Name |
|:---:|:---:|:---:|:---:|:---:|
| Born to Run | August 25, 1975 | Rock and roll | Columbia | Bruce Springsteen |
| Purple Rain | June 25, 1984 | Pop, R&B, Rock | Warner Bros | Prince |
| Born in the USA | June 4, 1984 | Rock and roll, pop | Columbia | Bruce Springsteen |
| Madonna | July 27, 1983 | Dance-pop, post-disco | Warner Bros | Madonna |
| True Blue | June 30, 1986 | Dance-pop, Pop | Warner Bros | Madonna |
| Elvis | October 19, 1956 | Rock and roll, Rhythm and Blues | RCA Victor | Elvis Presley |
| Sign o' the Times | March 30, 1987 | Pop, R&B, Rock, Funk | Paisley Park, Warner Bros | Prince |
| G.I. Blues | October 1, 1960 | Rock and roll, Pop | RCA Victor | Elvis Presley |

We want to create an `albums` table to hold all the above data except the singer name, and create a reference from the `albums` table to the `singers` table to link each album to the correct singer. Write the necessary SQL statements to do this and to populate the table with data. Assume Album Name, Genre, and Label can hold strings up to 100 characters. Include an auto-incrementing id column in the albums table.

```sql
ALTER TABLE singers /* Was already UNIQUE for me */
  ADD PRIMARY KEY (id);

CREATE TABLE albums (
  id serial PRIMARY KEY,
  name varchar(100) NOT NULL,
  release_date date,
  genre varchar(100) NOT NULL,
  label varchar(100) NOT NULL,
  singer_id integer NOT NULL,
  FOREIGN KEY (singer_id)
    REFERENCES singers(id)
    ON DELETE CASCADE
);

INSERT INTO albums
  (name, release_date, genre, label, singer_id)
  VALUES
    ('Born to Run', '1975-08-25', 'Rock and roll', 'Columbia', 1),
    ('Purple Rain', '1984-06-25', 'Pop, R&B, Rock', 'Warner Bros', 6),
    ('Born in the USA', '1984-06-04', 'Rock and roll, Pop', 'Columbia', 1),
    ('Madonna', '1983-07-27', 'Dance-pop, Post-disco', 'Warner Bros', 5),
    ('True Blue', '1986-06-30', 'Dance-pop, Pop', 'Warner Bros', 5),
    ('Elvis', '1956-10-19', 'Rock and roll, R&B', 'RCA Victor', 7),
    ('Sign o'' the Times', '1987-03-30', 'Pop, R&B, Rock and roll, Funk', 'Paisley Park, Warner Bros', 6),
    ('G.I. Blues', '1960-10-1', 'Rock and roll, Pop', 'RCA Victor', 7);
```
---

4. Connect to the `ls_burger` database. If you run a simple SELECT query to retrieve all the data from the orders table, you will see it is very unnormalised. We have repetition of item names and costs and of customer data.

We want to break this table up into multiple tables. First of all create a `customers` table to hold the customer name data and an `email_addresses` table to hold the customer email data. Create a one-to-one relationship between them, ensuring that if a customer record is deleted so is the equivalent email address record. Populate the tables with the appropriate data from the current orders table.

```sql
CREATE TABLE customers (
  id serial PRIMARY KEY,
  name varchar(100)
);

INSERT INTO customers
  (name)
  VALUES
    ('James Bergman'),
    ('Natasha O''Shea'),
    ('Aaron Muller');

CREATE TABLE email_addresses (
  id serial PRIMARY KEY,
  customer_id integer NOT NULL,
  email varchar(50),
  FOREIGN KEY (customer_id)
    REFERENCES customers (id)
    ON DELETE CASCADE
);

INSERT INTO email_addresses
  (customer_id, email)
  VALUES
    (2, 'natasha@osheafamily.com'),
    (1, 'james1998@email.com');
```
---

5. We want to make our ordering system more flexible, so that customers can order any combination of burgers, sides and drinks. The first step towards doing this is to put all our product data into a separate table called `products`. Create a table and populate it with the following data:

| Product Name | Product Cost | Product Type | Product Loyalty Points |
|:---:|:---:|:---:|:---:|
| LS Burger | 3.00 | Burger | 10 |
| LS Cheeseburger | 3.50 | Burger | 15 |
| LS Chicken Burger | 4.50 | Burger | 20 |
| LS Double Deluxe Burger | 6.00 | Burger | 30 |
| Fries | 1.20 | Side | 3 |
| Onion Rings | 1.50 | Side | 5 |
| Cola | 1.50 | Drink | 5 |
| Lemonade | 1.50 | Drink | 5 |
| Vanilla Shake | 2.00 | Drink | 7 |
| Chocolate Shake | 2.00 | Drink | 7 |
| Strawberry Shake | 2.00 | Drink | 7 |

The table should also have an auto-incrementing id column which acts as its PRIMARY KEY. The product_type column should hold strings of up to 20 characters. Other than that, the column types should be the same as their equivalent columns from the orders table.

```sql
CREATE TABLE products (
  id serial PRIMARY KEY,
  product_name varchar(50),
  product_cost decimal(4, 2) DEFAULT 0,
  product_type varchar(20),
  product_loyalty_points integer
);

INSERT INTO products
  (product_name, product_cost, product_type, product_loyalty_points)
  VALUES
    ('LS Burger', 3, 'Burger', 10),
    ('LS Cheeseburger', 3.5, 'Burger', 15),
    ('LS Chicken Burger', 4.5, 'Burger', 20),
    ('LS Double Deluxe Burger', 6, 'Burger', 30),
    ('Fries', 1.2, 'Side', 3),
    ('Onion Rings', 1.5, 'Side', 5),
    ('Cola', 1.5, 'Drink', 5),
    ('Lemonade', 1.5, 'Drink', 5),
    ('Vanilla Shake', 2, 'Drink', 7),
    ('Chocolate Shake', 2, 'Drink', 7),
    ('Strawbery Shake', 2, 'Drink', 7);
```
---

6. To associate customers with products, we need to do two more things:

    1. Alter or replace the `orders` table so that we can associate a customer with one or more orders (we also want to record an order status in this table).
    2. Create an `order_items` table so that an order can have one or more products associated with it.

Based on the order descriptions below, amend and create the tables as necessary and populate them with the appropriate data.

James has one order, consisting of a Chicken Burger, Fries, Onion Rings, and a Lemonade. It has a status of 'In Progress'.

Natasha has two orders. The first consists of a Cheeseburger, Fries, and a Cola, and has a status of 'Placed'; the second consists of a Double Deluxe Burger, a Cheeseburger, two sets of Fries, Onion Rings, a Chocolate Shake and a Vanilla Shake, and has a status of 'Complete'.

Aaron has one order, consisting of an LS Burger and Fries. It has a status of 'Placed'.

Assume that the `order_status` field of the `orders` table can hold strings of up to 20 characters.

```sql
DROP TABLE orders;

CREATE TABLE orders(
  id serial PRIMARY KEY,
  customer_id integer NOT NULL,
  order_status varchar(20),
  FOREIGN KEY (customer_id)
    REFERENCES customers (id)
    ON DELETE CASCADE
);

CREATE TABLE order_items (
  id serial PRIMARY KEY,
  order_id integer NOT NULL,
  product_id integer NOT NULL,
  FOREIGN KEY (order_id)
    REFERENCES orders (id)
    ON DELETE CASCADE,
  FOREIGN KEY (product_id)
    REFERENCES products(id)
    ON DELETE CASCADE
);

INSERT INTO orders
  (customer_id, order_status)
  VALUES
    (1, 'In Progress'),
    (2, 'Placed'),
    (2, 'Complete'),
    (3, 'Placed');

INSERT INTO order_items
  (order_id, product_id)
  VALUES
    (1, 3),
    (1, 5),
    (1, 6),
    (1, 8),
    (2, 2),
    (2, 5),
    (2, 7),
    (3, 4),
    (3, 2),
    (3, 5),
    (3, 5),
    (3, 6),
    (3, 10),
    (3, 9),
    (4, 1),
    (4, 5);
```

## SQL Joins

1. Connect to the `encyclopedia` database. Write a query to return all of the country names along with their appropriate continent names.

```sql
SELECT countries.name, continents.continent_name
  FROM countries
  INNER JOIN continents
    ON continents.id = countries.continent_id;
```
---

2. Write a query to return all of the names and capitals of the European countries.

```sql
SELECT countries.name, countries.capital
  FROM countries
  JOIN continents
    ON continents.id = countries.continent_id
  WHERE continents.continent_name = 'Europe';
```
---

3. Write a query to return the first name of any singer who had an album released under the Warner Bros label.

```sql
SELECT DISTINCT singers.first_name
  FROM singers
  JOIN albums
    ON albums.singer_id = singers.id
  WHERE albums.label LIKE '%Warner Bros%';
```
---

4. Write a query to return the first name and last name of any singer who released an album in the 80s and who is still living, along with the names of the album that was released and the release date. Order the results by the singer's age (youngest first).

```sql
SELECT singers.first_name, singers.last_name, a.name, a.release_date
  FROM singers
  JOIN albums a
    ON a.singer_id = singers.id
  WHERE date_part('year', a.release_date) >= 1980
    AND date_part('year', a.release_date) < 1990
    AND singers.deceased = 'false'
  ORDER BY singers.date_of_birth DESC;
```
---

5. Write a query to return the first name and last name of any singer without an associated album entry.

```sql
SELECT singers.first_name, singers.last_name, albums.id
  FROM singers
  LEFT JOIN albums
    ON singers.id = albums.singer_id
  WHERE albums.id IS NULL;
```
---

6. Rewrite the query for the last question as a sub-query.

```sql
SELECT first_name, last_name
  FROM singers
  WHERE id NOT IN (
    SELECT singer_id FROM albums
  );
```
---

7. Connect to the `ls_burger` database. Return a list of all orders and their associated product items.

```sql
SELECT orders.*, products.*
  FROM orders
  JOIN order_items
    ON order_items.order_id = orders.id
  JOIN products
    ON products.id = order_items.product_id;
```
---

8. Return the id of any order that includes Fries. Use table aliasing in your query.

```sql
SELECT itm.order_id
  FROM order_items itm
  JOIN products p
    ON p.id = itm.product_id
  WHERE p.product_name = 'Fries';
```

Launch School adds the `orders` table to this. Why? The `order_id` column from the `order_items` table is the same as the `id` from the `orders` table. Its superfluous to join it if we only care about the id. Yeah the next question wants us to build on it, but meh!

---

9. Build on the query from the previous question to return the name of any customer who ordered fries. Return this in a column called 'Customers who like Fries'. Don't repeat the same customer name more than once in the results.

```sql
SELECT DISTINCT c.name AS "Customers Who like Fries"
  FROM customers c
  JOIN orders o
    ON o.customer_id = c.id
  JOIN order_items i
    ON o.id = i.order_id
  JOIN products p
    ON i.product_id = p.id
  WHERE p.product_name = 'Fries';
```
---

10. Write a query to return the total cost of Natasha O'Shea's orders.

```sql
SELECT sum(p.product_cost)
  FROM orders o
  JOIN customers c
    ON o.customer_id = c.id
  JOIN order_items oi
    ON o.id = oi.order_id
  JOIN products p
    ON oi.product_id = p.id
  WHERE c.name = 'Natasha O''Shea';
```
---

11. Write a query to return the name of every product included in an order alongside the number of times it has been ordered. Sort the results by product name, ascending.

```sql
SELECT p.product_name, count(oi.id) AS "Quantity"
  FROM order_items oi
  JOIN products p
    ON oi.product_id = p.id
  GROUP BY p.id
  ORDER BY p.product_name;
```
