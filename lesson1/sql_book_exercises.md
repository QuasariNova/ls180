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
