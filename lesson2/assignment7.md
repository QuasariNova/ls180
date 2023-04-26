1. Load this file into your database.

    1. What does the file do?

        It creates a table named films and populates it with three rows. It contains the sql statements to do this.

    2. What is the output of the command? What does each line in this output mean?

        ```sql
        NOTICE:  table "films" does not exist, skipping
        DROP TABLE
        CREATE TABLE
        INSERT 0 1
        INSERT 0 1
        INSERT 0 1
        ```

        - The Notice tells us that it did not delete a table named `films` because it did not exist.
        - `DROP TABLE` Indicates that it dropped a table, but the notice above shows it didn't
        - `CREATE TABLE` indicates it created the `films` table.
        - The three `INSERT` indicate that it added 1 row each.

    3. Open up the file and look at its contents. What does the first line do?

        The first line tries to delete the `films` table if it exists.
---

2. Write a SQL statement that returns all rows in the films table.

```sql
SELECT * FROM films;
```
---

3. Write a SQL statement that returns all rows in the `films` table with a title shorter than 12 letters.

```sql
SELECT *
  FROM films
 WHERE length(title) < 12;
```
---

4. Write the SQL statements needed to add two additional columns to the `films` table: `director`, which will hold a director's full name, and `duration`, which will hold the length of the film in minutes.

```sql
ALTER TABLE films
  ADD COLUMN director varchar(50),
  ADD COLUMN duration integer;
```
---

5. Write SQL statements to update the existing rows in the database with values for the new columns:

| title | director | duration |
|---|---|---:|
| Die Hard | John McTiernan | 132 |
| Casablanca | Michael Curtiz | 102 |
| The Conversation | Francis Ford Coppola | 113 |

```sql
UPDATE films
   SET director = 'John McTiernan',
       duration = 132
 WHERE title = 'Die Hard';

UPDATE films
   SET director = 'Michael Curtiz',
       duration = 102
 WHERE title = 'Casablanca';

UPDATE films
   SET director = 'Francis Ford Coppola',
       duration = 113
 WHERE title = 'The Conversation';
```
---

6. Write SQL statements to insert the following data into the `films` table:

| title | year | genre | director | duration |
|---|---|---|---|---:|
| 1984 | 1956 | scifi | Michael Anderson | 90 |
| Tinker Tailor Soldier Spy | 2011 | espionage | Tomas Alfredson | 127 |
| The Birdcage | 1996 | comedy | Mike Nichols | 118 |

```sql
INSERT INTO films
VALUES ('1984', 1956, 'scifi', 'Michael Anderson', 90),
       ('Tinker Tailor Soldier Spy', 2011, 'espionage', 'Tomas Alfredson', 127),
       ('The Birdcage', 1996, 'comedy', 'Mike Nichols', 118);
```
---

7. Write a SQL statement that will return the title and age in years of each movie, with newest movies listed first.

```sql
SELECT title, date_part('year', now()) - "year" AS age
  FROM films
 ORDER BY age;
```
---

8. Write a SQL statement that will return the title and duration of each movie longer than two hours, with the longest movies first.

```sql
SELECT title, duration
  FROM films
 WHERE duration > 120
 ORDER BY duration DESC;
```
---

9. Write a SQL statement that returns the title of the longest film.

```sql
SELECT title
  FROM films
 ORDER BY duration DESC
 LIMIT 1;
```
