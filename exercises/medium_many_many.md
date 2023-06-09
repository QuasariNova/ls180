1. In this set of exercises, we will work with a billing database for a company that provides web hosting services to its customers. The database will contain information about its customers and the services each customer uses. Each customer can have any number of services, and every service can have any number of customers. Thus, there will be a many-to-many (M:M) relationship between the customers and the services. Some customers don't presently have any services, and not every service must be in use by any customers.

Initially, we need to create a `billing` database with a `customers` table and a `services` table. The `customers` table should include the following columns:

- `id` is a unique numeric customer id that auto-increments and serves as a primary key for this table.
- `name` is the customer's name. This value must be present in every record and may contain names of any length.
- `payment_token` is a required unique 8-character string that consists of solely uppercase alphabetic letters. It identifies each customer's payment information with the payment processor the company uses.

The `services` table should include the following columns:

- `id` is a unique numeric service id that auto-increments and serves as a primary key for this table.
- `description` is the service description. This value must be present and may contain any text.
- `price` is the annual service price. It must be present, must be greater than or equal to 0.00. The data type is numeric(10, 2).

Once you've created these tables, here is some data that you can enter into them (feel free to enter some data of your own as well):

```
-- Data for the customers table

id | name          | payment_token
--------------------------------
1  | Pat Johnson   | XHGOAHEQ
2  | Nancy Monreal | JKWQPJKL
3  | Lynn Blake    | KLZXWEEE
4  | Chen Ke-Hua   | KWETYCVX
5  | Scott Lakso   | UUEAPQPS
6  | Jim Pornot    | XKJEYAZA

-- Data for the services table

id | description         | price
---------------------------------
1  | Unix Hosting        | 5.95
2  | DNS                 | 4.95
3  | Whois Registration  | 1.95
4  | High Bandwidth      | 15.00
5  | Business Support    | 250.00
6  | Dedicated Hosting   | 50.00
7  | Bulk Email          | 250.00
8  | One-to-one Training | 999.00
```

Once you have entered the data into your tables, create a join table that associates customers with services and vice versa. The join table should have columns for both the services id and the customers id, as well as a primary key named id that auto-increments.

The customer id in this table should have the property that deleting the corresponding customer record from the customers table will automatically delete all rows from the join table that have that customer_id. Do not apply this same property to the service id.

Each combination of customer and service in the table should be unique. In other words, a customer shouldn't be listed as using a particular service more than once.

Enter some data in the join table that shows which services each customer uses as follows:

- Pat Johnson uses Unix Hosting, DNS, and Whois Registration
- Nancy Monreal doesn't have any active services
- Lynn Blake uses Unix Hosting, DNS, Whois Registration, High Bandwidth, and Business Support
- Chen Ke-Hua uses Unix Hosting and High Bandwidth
- Scott Lakso uses Unix Hosting, DNS, and Dedicated Hosting
- Jim Pornot uses Unix Hosting, Dedicated Hosting, and Bulk Email

<br>
<br>
<br>

```sql
CREATE DATABASE billing;
\c billing

CREATE TABLE customers (
       id serial PRIMARY KEY,
       name text NOT NULL,
       payment_token char(8) UNIQUE NOT NULL,
       CHECK (payment_token ~ '^[A-Z]{8}$')
);

INSERT INTO customers
       (name, payment_token)
VALUES ('Pat Johnson', 'XHGOAHEQ'),
       ('Nancy Monreal', 'JKWQPJKL'),
       ('Lynn Blake', 'KLZXWEEE'),
       ('Chen Ke-Hua', 'KWETYCVX'),
       ('Scott Lakso', 'UUEAPQPS'),
       ('Jim Pornot', 'XKJEYAZA');

CREATE TABLE services (
       id serial PRIMARY KEY,
       description text NOT NULL,
       price numeric(10,2) NOT NULL,
       CHECK (price >= 0.00)
);

INSERT INTO services
       (description, price)
VALUES ('Unix Hosting', 5.95),
       ('DNS', 4.95),
       ('Whois Registration', 1.95),
       ('High Bandwidth', 15.00),
       ('Business Support', 250.00),
       ('Dedicated Hosting', 50.00),
       ('Bulk Email', 250.00),
       ('One-to-one Training', 999.00);

CREATE TABLE customers_services (
       id serial PRIMARY KEY,
       service_id integer NOT NULL
         REFERENCES services (id),
       customer_id integer NOT NULL
         REFERENCES customers (id)
                 ON DELETE CASCADE,
       UNIQUE (service_id, customer_id)
);

INSERT INTO customers_services
       (customer_id, service_id)
VALUES (1, 1),
       (1, 2),
       (1, 3),
       (3, 1),
       (3, 2),
       (3, 3),
       (3, 4),
       (3, 5),
       (4, 1),
       (4, 4),
       (5, 1),
       (5, 2),
       (5, 6),
       (6, 1),
       (6, 6),
       (6, 7);
```
---

2. Write a query to retrieve the `customer` data for every customer who currently subscribes to at least one service.

<br>
<br>
<br>

```sql
SELECT DISTINCT customers.*
  FROM customers
 INNER JOIN customers_services AS cs
         ON cs.customer_id = customers.id;
```
---

3. Write a query to retrieve the `customer` data for every customer who does not currently subscribe to any services.

<br>
<br>
<br>

```sql
SELECT customers.*
  FROM customers
  LEFT OUTER JOIN customers_services
          ON customer_id = customers.id
 WHERE service_id IS NULL;
```

<br>
<br>
<br>

Further Exploration: Can you write a query that displays all customers with no services and all services that currently don't have any customers? The output should look like this:

```
 id |     name      | payment_token | id |     description     | price
----+---------------+---------------+----+---------------------+--------
  2 | Nancy Monreal | JKWQPJKL      |    |                     |
    |               |               |  8 | One-to-one Training | 999.00
(2 rows)
```

<br>
<br>
<br>

```sql
SELECT customers.*, services.*
  FROM customers
  LEFT OUTER JOIN customers_services
          ON customer_id = customers.id
  FULL OUTER JOIN services
          ON service_id = services.id
 WHERE customers_services.id IS NULL;
```
---

4. Using RIGHT OUTER JOIN, write a query to display a list of all services that are not currently in use. Your output should look like this:

```
 description
-------------
 One-to-one Training
(1 row)
```

<br>
<br>
<br>

```sql
SELECT description
  FROM customers_services
 RIGHT OUTER JOIN services
          ON services.id = service_id
 WHERE service_id IS NULL;
```
---

5. Write a query to display a list of all customer names together with a comma-separated list of the services they use. Your output should look like this:

```
     name      |                                services
---------------+-------------------------------------------------------------------------
 Pat Johnson   | Unix Hosting, DNS, Whois Registration
 Nancy Monreal |
 Lynn Blake    | DNS, Whois Registration, High Bandwidth, Business Support, Unix Hosting
 Chen Ke-Hua   | High Bandwidth, Unix Hosting
 Scott Lakso   | DNS, Dedicated Hosting, Unix Hosting
 Jim Pornot    | Unix Hosting, Dedicated Hosting, Bulk Email
(6 rows)
```

<br>
<br>
<br>

```sql
SELECT name, string_agg(description, ', ')
  FROM customers
  LEFT JOIN customers_services
         ON customer_id = customers.id
  LEFT JOIN services
         ON services.id = service_id
 GROUP BY customers.id;
```

<br>
<br>
<br>

Futher Exploration: Can you modify the above command so the output looks like this?

```
     name      |    description
---------------+--------------------
 Chen Ke-Hua   | High Bandwidth
               | Unix Hosting
 Jim Pornot    | Dedicated Hosting
               | Unix Hosting
               | Bulk Email
 Lynn Blake    | Whois Registration
               | High Bandwidth
               | Business Support
               | DNS
               | Unix Hosting
 Nancy Monreal |
 Pat Johnson   | Whois Registration
               | DNS
               | Unix Hosting
 Scott Lakso   | DNS
               | Dedicated Hosting
               | Unix Hosting
(17 rows)
```

This won't be easy! Hint: you will need to use the window lag function together with a CASE condition in your SELECT. To get you started, try this command:

```sql
SELECT customers.name,
       lag(customers.name)
         OVER (ORDER BY customers.name)
         AS previous,
       services.description
FROM customers
LEFT OUTER JOIN customers_services
             ON customer_id = customers.id
LEFT OUTER JOIN services
             ON services.id = service_id;
```

<br>
<br>
<br>

```sql
SELECT
       CASE WHEN lag(name)
                 OVER (ORDER BY name) = name THEN NULL
            ELSE name
       END AS name,
       description
  FROM customers
  LEFT OUTER JOIN customers_services
          ON customer_id = customers.id
  LEFT OUTER JOIN services
          ON services.id = service_id;
```
---

6. Write a query that displays the description for every service that is subscribed to by at least 3 customers. Include the customer count for each description in the report. The report should look like this:

```
 description  | count
--------------+-------
 DNS          |     3
 Unix Hosting |     5
(2 rows)
```

<br>
<br>
<br>

```sql
SELECT description, count(service_id)
  FROM services
  LEFT OUTER JOIN customers_services
          ON services.id = service_id
 GROUP BY description
HAVING count(service_id) >= 3;
```
---

7. Assuming that everybody in our database has a bill coming due, and that all of them will pay on time, write a query to compute the total gross income we expect to receive.

<br>
<br>
<br>

```sql
SELECT sum(price) AS gross
  FROM services
 INNER JOIN customers_services
         ON services.id = service_id;
```
---

8. A new customer, 'John Doe', has signed up with our company. His payment token is 'EYODHLCN'. Initially, he has signed up for UNIX hosting, DNS, and Whois Registration. Create any SQL statement(s) needed to add this record to the database.

```sql
INSERT INTO customers
       (name, payment_token)
VALUES ('John Doe', 'EYODHLCN'); -- ID 7

INSERT INTO customers_services
       (customer_id, service_id)
VALUES (7, 1),
       (7, 2),
       (7, 3);
```
---

9. The company president is looking to increase revenue. As a prelude to his decision making, he asks for two numbers: the amount of expected income from "big ticket" services (those services that cost more than $100) and the maximum income the company could achieve if it managed to convince all of its customers to select all of the company's big ticket items.

For simplicity, your solution should involve two separate SQL queries: one that reports the current expected income level, and one that reports the hypothetical maximum. The outputs should look like this:

```
 sum
--------
 500.00
(1 row)

   sum
---------
 10493.00
(1 row)
```

<br>
<br>
<br>

```sql
SELECT sum(price)
  FROM services
 INNER JOIN customers_services
         ON services.id = service_id
 WHERE price > 100;

SELECT sum(price)
  FROM services
 CROSS JOIN customers
 WHERE price > 100;
```
---

10. Write the necessary SQL statements to delete the "Bulk Email" service and customer "Chen Ke-Hua" from the database.

<br>
<br>
<br>

```sql
DELETE FROM customers
 WHERE name = 'Chen Ke-Hua';

DELETE FROM customers_services
 WHERE service_id = 7;

DELETE FROM services
 WHERE description = 'Bulk Email';
```
