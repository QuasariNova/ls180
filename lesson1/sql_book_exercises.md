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
