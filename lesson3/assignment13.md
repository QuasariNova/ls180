1. mport this file into a database using psql.

<br>
<br>
<br>

```bash
psql -d database_name < ~/films7.sql
```
---

2. Write the SQL statement needed to create a join table that will allow a film to have multiple directors, and directors to have multiple films. Include an `id` column in this table, and add foreign key constraints to the other columns.

<br>
<br>
<br>

```sql
/* multiple words = alphabetical order */
CREATE TABLE directors_films (
       id serial PRIMARY KEY,
       director_id integer NOT NULL
       REFERENCES directors (id)
               ON DELETE CASCADE,
       film_id integer NOT NULL
       REFERENCES films (id)
               ON DELETE CASCADE,
       UNIQUE (director_id, film_id)
);
```
---

3. Write the SQL statements needed to insert data into the new join table to represent the existing one-to-many relationships.

<br>
<br>
<br>

```sql
INSERT INTO directors_films
       (director_id, film_id)
VALUES (1, 1),
       (2, 2),
       (3, 3),
       (4, 4),
       (5, 5),
       (6, 6),
       (3, 7),
       (7, 8),
       (8, 9),
       (4, 10);

/* or */
INSERT INTO directors_films
       (director_id, film_id) (
         SELECT director_id, id
           FROM films
       );
```
---

4. Write a SQL statement to remove any unneeded columns from `films`.

<br>
<br>
<br>

```sql
ALTER TABLE films
 DROP COLUMN director_id;
```
---

5. Write a SQL statement that will return the following result:

```
           title           |         name
---------------------------+----------------------
 12 Angry Men              | Sidney Lumet
 1984                      | Michael Anderson
 Casablanca                | Michael Curtiz
 Die Hard                  | John McTiernan
 Let the Right One In      | Michael Anderson
 The Birdcage              | Mike Nichols
 The Conversation          | Francis Ford Coppola
 The Godfather             | Francis Ford Coppola
 Tinker Tailor Soldier Spy | Tomas Alfredson
 Wayne's World             | Penelope Spheeris
(10 rows)
```

<br>
<br>
<br>

```sql
SELECT films.title, directors.name
  FROM films
       INNER JOIN directors_films AS df
               ON df.film_id = films.id
       INNER JOIN directors
               ON df.director_id = directors.id
 ORDER BY films.title;
```
---

6. Write SQL statements to insert data for the following films into the database:

| Film | Year | Genre | Duration | Directors |
|:---:|:---:|:---:|:---:|:---:|
| Fargo | 1996 | comedy | 98 | Joel Coen |
| No Country for Old Men | 2007 | western | 122 | Joel Coen, Ethan Coen |
| Sin City | 2005 | crime | 124 | Frank Miller, Robert Rodriguez |
| Spy Kids | 2001 | scifi | 88 | Robert Rodriguez |

<br>
<br>
<br>

```sql
INSERT INTO films
       (title, year, genre, duration)
VALUES ('Fargo', 1996, 'comedy', 98),
       ('No Country for Old Men', 2007, 'western', 122),
       ('Sin City', 2005, 'crime', 124),
       ('Spy Kids', 2001, 'scifi', 88);

INSERT INTO directors
       (name)
VALUES ('Joel Coen'),
       ('Ethan Coen'),
       ('Frank Miller'),
       ('Robert Rodriguez');

INSERT INTO directors_films
       (film_id, director_id)
VALUES (11, 9),
       (12, 9),
       (12, 10),
       (13, 11),
       (13, 12),
       (14, 12);
```
---

7. Write a SQL statement that determines how many films each director in the database has directed. Sort the results by number of films (greatest first) and then name (in alphabetical order).

<br>
<br>
<br>

```sql
SELECT directors.name, count(directors_films.film_id) AS films
  FROM directors
 INNER JOIN directors_films
         ON directors_films.director_id = directors.id
 GROUP BY directors.id
 ORDER BY films DESC,
          directors.name;
```
