1. Import this file into a PostgreSQL database.

```bash
psql -d postgres < films2.sql
```
---

2. Modify all of the columns to be `NOT NULL`.

```sql
ALTER TABLE films
ALTER COLUMN title
  SET NOT NULL,
ALTER COLUMN "year"
  SET NOT NULL,
ALTER COLUMN genre
  SET NOT NULL,
ALTER COLUMN director
  SET NOT NULL,
ALTER COLUMN duration
  SET NOT NULL;
```
---

3. How does modifying a column to be `NOT NULL` affect how it is displayed by `\d films`?

The column labeled `Nullable` is set to `not null` for each row. In other versions, it would be the `Modifiers` column.

---

4. Add a constraint to the table `films` that ensures that all films have a unique title.

```sql
ALTER TABLE films
  ADD CONSTRAINT unique_title UNIQUE (title);
```
---

5. How is the constraint added in #4 displayed by \d films?

Under the table is a section labeled `Indexes`, it will show the `UNIQUE CONSTRAINT`.

---

6. Write a SQL statement to remove the constraint added in #4.

```sql
ALTER TABLE films
 DROP CONSTRAINT unique_title;
```
---

7. Add a constraint to `films` that requires all rows to have a value for `title` that is at least 1 character long.

```sql
ALTER TABLE films
  ADD CONSTRAINT non_empty_title CHECK (LENGTH(title) > 0);
```

8. What error is shown if the constraint created in #7 is violated? Write a SQL INSERT statement that demonstrates this.

```sql
INSERT INTO films
VALUES ('', 0, '', '', 0);

/*
ERROR:  new row for relation "films" violates check constraint "non_empty_title"
DETAIL:  Failing row contains (, 0, , , 0).
*/
```
---

9. How is the constraint added in #7 displayed by \d films?

It is displayed under "Check constraints".

---

10. Write a SQL statement to remove the constraint added in #7.

```sql
ALTER TABLE films
 DROP CONSTRAINT non_empty_title;
```
---

11. Add a constraint to the table `films` that ensures that all films have a year between 1900 and 2100.

```sql
ALTER TABLE films
  ADD CONSTRAINT recent_year CHECK (year BETWEEN 1900 AND 2100);
```
---

12. How is the constraint added in #11 displayed by `\d films`?

It appears under "Check constraints".

---

13. Add a constraint to `films` that requires all rows to have a value for director that is at least 3 characters long and contains at least one space character ( ).

```sql
ALTER TABLE films
  ADD CONSTRAINT director_name_is_full
           CHECK (length(director) >= 3 AND director ~ ' ');
/* Launch School used the position function instead of the `~` operator */
```
---

14. How does the constraint created in #13 appear in `\d films`?

It appears under the "Check constraints" heading.
---

15. Write an UPDATE statement that attempts to change the director for "Die Hard" to "Johnny". Show the error that occurs when this statement is executed.

```sql
UPDATE films
   SET director = 'Johnny'
 WHERE title = 'Die Hard';

/*
ERROR:  new row for relation "films" violates check constraint "director_name_is_full"
DETAIL:  Failing row contains (Die Hard, 1988, action, Johnny, 132).
*/
```
---

16. List three ways to use the schema to restrict what values can be stored in a column.

    1. Forcing all values in a column to be unique(UNIQUE)
    2. Forcing all values in a column to exist(NOT NULL)
    3. Forcing all values in a column to fit an arbitrary check(CHECK)
    4. Data type
    5. Default value

---

17. Is it possible to define a default value for a column that will be considered invalid by a constraint? Create a table that tests this.

Yes, you can define a default value for a column that is invalid by a constraint. If you were to try to use that default value however, the constraint won't let the row be added/updated.

```sql
CREATE TABLE failure (
       id serial,
       name varchar(25) NOT NULL DEFAULT NULL
);
-- CREATE TABLE
INSERT INTO failure (name)
VALUES (DEFAULT);
/*
ERROR:  null value in column "name" of relation "failure" violates not-null constraint
DETAIL:  Failing row contains (1, null).
*/
```
---

18. How can you see a list of all of the constraints on a table?

`\d tablename`
