1. Import this file into a new database.

<br>
<br>
<br>

```bash
psql -d database_name < ~/orders_products1.sql
```
---

2. Update the `orders` table so that referential integrity will be preserved for the data between `orders` and `products`.

<br>
<br>
<br>

```sql
ALTER TABLE orders
  ADD FOREIGN KEY (product_id)
      REFERENCES products (id);
```
---

3. Use psql to insert the data shown in the following table into the database:

| Quantity | Product |
|:---:|:---:|
| 10 | small bolt |
| 25 | small bolt |
| 15 | large bolt |

<br>
<br>
<br>

```sql
INSERT INTO products
       (name)
VALUES ('small bolt'),
       ('large bolt');

INSERT INTO orders
       (quantity, product_id)
VALUES (10, 1),
       (25, 1),
       (15, 2);
```
---

4. Write a SQL statement that returns a result like this:

```
 quantity |    name
----------+------------
       10 | small bolt
       25 | small bolt
       15 | large bolt
(3 rows)
```

<br>
<br>
<br>

```sql
SELECT orders.quantity, products.name
  FROM orders
 INNER JOIN products
    ON products.id = orders.product_id;
```
---

5. Can you insert a row into orders without a `product_id`? Write a SQL statement to prove your answer.

<br>
<br>
<br>

Yes, at the moment `product_id` does not have a `NOT NULL` constraint, so it can be `NULL`.

```sql
INSERT INTO orders
       (quantity)
VALUES (10);
```
---

6. Write a SQL statement that will prevent `NULL` values from being stored in orders.`product_id`. What happens if you execute that statement?

<br>
<br>
<br>

```sql
ALTER TABLE orders
ALTER COLUMN product_id
  SET NOT NULL;
```

Since we still have a `NULL` value for `product_id` in the row we inserted in question 5, this will fails with an error.

---

7. Make any changes needed to avoid the error message encountered in #6.

<br>
<br>
<br>

```sql
DELETE FROM orders
 WHERE product_id IS NULL;

ALTER TABLE orders
ALTER COLUMN product_id
  SET NOT NULL;
```
---

8. Create a new table called `reviews` to store the data shown below. This table should include a primary key and a reference to the `products` table.

| Product | Review |
|:---:|:---:|
| small bolt | a little small |
| small bolt | very round! |
| large bolt | could have been smaller |

<br>
<br>
<br>

```sql
CREATE TABLE reviews (
       id serial PRIMARY KEY,
       product_id integer NOT NULL,
       review text NOT NULL,
       FOREIGN KEY (product_id)
        REFERENCES products (id)
                ON DELETE CASCADE
);
```
---

9. Write SQL statements to insert the data shown in the table in #8.

<br>
<br>
<br>

```sql
INSERT INTO reviews
       (product_id, review)
VALUES (1, 'a little small'),
       (1, 'very round!'),
       (2, 'could have been smaller');
```
---

10. **True** or **false**: A foreign key constraint prevents `NULL` values from being stored in a column.

<br>
<br>
<br>

**FALSE**

Foreign Keys do not enforce `NOT NULL`, if you want to have a `NOT NULL` constraint you must explicitly add it yourself.
