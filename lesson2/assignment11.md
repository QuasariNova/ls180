1. Write a SQL statement that makes a new sequence called "counter".

```sql
CREATE SEQUENCE counter;
```
---

2. Write a SQL statement to retrieve the next value from the sequence created in #1.

```sql
SELECT nextval('counter');
```
---

3. Write a SQL statement that removes a sequence called "counter".

```sql
DROP SEQUENCE counter;
```
---

4. Is it possible to create a sequence that returns only even numbers?

Yes
```sql
CREATE SEQUENCE evens INCREMENT 2 START WITH 2;
```
---

5. What will the name of the sequence created by the following SQL statement be?

```sql
CREATE TABLE regions (id serial PRIMARY KEY, name text, area integer);
```

`regions_id_seq`

---

6. Write a SQL statement to add an auto-incrementing integer primary key column to the films table.

```sql
ALTER TABLE films
  ADD COLUMN id serial PRIMARY KEY;
```
---

7. What error do you receive if you attempt to update a row to have a value for `id` that is used by another row?

```
ERROR:  duplicate key value violates unique constraint "films_pkey"
DETAIL:  Key (id)=(1) already exists.
```
---

8. What error do you receive if you attempt to add another primary key column to the `films` table?

```
ERROR:  multiple primary keys for table "films" are not allowed
```
---

9. Write a SQL statement that modifies the table films to remove its primary key while preserving the id column and the values it contains.

```sql
ALTER TABLE films
 DROP CONSTRAINT films_pkey;
```
