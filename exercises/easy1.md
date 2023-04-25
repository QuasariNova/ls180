1. Let's start by creating a database. Create a new database called `animals`.

```sql
CREATE DATABASE animals;
```
---

2.

Now that we have an `animals` database, we can lay the groundwork needed to add some data to it.

Make a table called `birds`. It should have the following fields:

    - id (a primary key)
    - name (string with space for up to 25 characters)
    - age (integer)
    - species (a string with room for no more than 15 characters)

```sql
CREATE TABLE birds (
  id serial PRIMARY KEY,
  name varchar(25),
  age integer,
  species varchar(15)
);
```
---

3. For this exercise, we'll add some data to our birds table. Add five records to this database so that our data looks like:

```
 id |   name   | age | species
----+----------+-----+---------
  1 | Charlie  |   3 | Finch
  2 | Allie    |   5 | Owl
  3 | Jennifer |   3 | Magpie
  4 | Jamie    |   4 | Owl
  5 | Roy      |   8 | Crow
(5 rows)
```

```sql
INSERT INTO birds
  (name, age, species)
  VALUES
    ('Charlie', 3, 'Finch'),
    ('Allie', 5, 'Owl'),
    ('Jennifer', 3, 'Magpie'),
    ('Jamie', 4, 'Owl'),
    ('Roy', 8, 'Crow');
```

### Further Exploration:

There is a form of `INSERT INTO` that doesn't require the column names. How does that form of `INSERT INTO` work, and when would you use it?

If no columns are specified, `INSERT INTO` will use all columns in their default order. If you have a table where you have no `serial` column and want to insert data that fills the table you might use a `INSERT INTO` without column names.

---

4. Write an SQL statement to query all data that is currently in our `birds` table.

```sql
SELECT * FROM birds;
```
---

5. In this exercise, let's practice filtering the data we want to query. Using a `WHERE` clause, `SELECT` records for birds under the age of 5.

```sql
SELECT * FROM birds
  WHERE age < 5;
```
---

6. It seems there was a mistake when we were inserting data in the birds table. All the crows are actually ravens. Update the birds table so that the rows with a species of 'Crow' now read 'Raven'.

```sql
UPDATE birds
  SET species = 'Raven'
  WHERE species = 'Crow';
```

### Further Exploration:
Oops. Jamie isn't an Owl - he's a Hawk. Correct his information.

```sql
UPDATE birds
  SET species = 'Hawk'
  WHERE name = 'Jamie';
```
---

7. Write an SQL statement that deletes the record that describes a 3 year-old finch.

```sql
DELETE FROM birds
  WHERE species = 'Finch'
    AND age = 3;
```
---

8. When we initially created our `birds` table, we forgot to take into account a key factor: preventing certain values from being entered into the database. For instance, we would never find a bird that is negative n years old. We could have included a constraint to ensure that bad data isn't entered for a database record, but we forgot to do so.

For this exercise, write some code that ensures that only birds with a positive age may be added to the database. Then write and execute an SQL statement to check that this new constraint works correctly.

```sql
ALTER TABLE birds
  ADD CONSTRAINT positive_age
    CHECK (age >= 0);

INSERT INTO birds (age)
  VALUES (-1);
/* ERROR:  new row for relation "birds" violates check constraint "positive_age"
   DETAIL:  Failing row contains (6, null, -1, null). */
```

Launch School uses `age > 0`, which is more correct for positive age. But doesn't make sense if bird age is between newborn and 1 year. But if you are looking for positive numbers, 0 is not positive.

---

9. It seems we are done with our `birds` table, and no longer have a need for it in our `animals` database. Write an SQL statement that will drop the `birds` table and all its data from the animals database.

```sql
DROP TABLE birds;
```
---

10. Let's finish things up by dropping the database `animals`. With no tables in it, we don't have much use for it any more. Write a SQL statement or PostgreSQL command to drop the `animals` database.

```sql
DROP DATABASE animals;
```
