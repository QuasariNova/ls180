1. For this exercise set, we will be working with the DDL statements to create and modify tables in a database that tracks planets around stars other than our Sun. To get started, first create a postgresql database named "extrasolar", and then create two tables in the database as follows:

`stars` table

- `id`: a unique serial number that auto-increments and serves as a primary key for this table.
- `name`: the name of the star,e,g., "Alpha Centauri A". Allow room for 25 characters. Must be unique and non-null.
- `distance`: the distance in light years from Earth. Define this as a whole number (e.g., 1, 2, 3, etc) that must be non-null and greater than 0.
- `spectral_type`: the spectral type of the star: O, B, A, F, G, K, and M. Use a one character string.
- `companions`: how many companion stars does the star have? A whole number will do. Must be non-null and non-negative.

`planets` table

- `id`: a unique serial number that auto-increments and serves as a primary key for this table.
- `designation`: a single alphabetic character that uniquely identifies the planet in its star system ('a', 'b', 'c', etc.)
- `mass`: estimated mass in terms of Jupiter masses; use an integer for this value.

<br>
<br>

```sql
CREATE DATABASE extrasolar;
\c extrasolar

CREATE TABLE stars(
       id serial PRIMARY KEY,
       name varchar(25) UNIQUE NOT NULL,
       distance integer NOT NULL,
       spectral_type char(1),
       companions integer NOT NULL,
       CHECK (distance > 0),
       CHECK (companions >= 0)
);

CREATE TABLE planets (
       id serial PRIMARY KEY,
       designation char(1) UNIQUE,
       mass integer
);
```
---

2. You may have noticed that, when we created the `stars` and `planets` tables, we did not do anything to establish a relationship between the two tables. They are simply standalone tables that are related only by the fact that they both belong to the `extrasolar` database. However, there is no relationship between the rows of each table.

To correct this problem, add a `star_id` column to the `planets` table; this column will be used to relate each planet in the `planets` table to its home star in the `stars` table. Make sure the column is defined in such a way that it is required and must have a value that is present as an id in the stars table.

<br>
<br>

```sql
ALTER TABLE planets
  ADD COLUMN star_id integer NOT NULL
  REFERENCES stars (id);
```
---

3. Hmm... it turns out that 25 characters isn't enough room to store a star's complete name. For instance, the star "Epsilon Trianguli Australis A" requires 30 characters. Modify the column so that it allows star names as long as 50 characters.

<br>
<br>

```sql
ALTER TABLE stars
ALTER COLUMN name TYPE varchar(50);
```

<br>
<br>

Further Exploration:
Assume the `stars` table already contains one or more rows of data. What will happen when you try to run the above command? To test this, revert the modification and add a row or two of data:

```sql
ALTER TABLE stars
ALTER COLUMN name TYPE varchar(25);

INSERT INTO stars (name, distance, spectral_type, companions)
VALUES ('Alpha Centauri B', 4, 'K', 3);

ALTER TABLE stars
ALTER COLUMN name TYPE varchar(50);
```

<br>
<br>

Since we are increasing the lmit of characters in its data type, nothing different happens. The command is successful as nothing needs to be converted between one datatype and the other.

Now, if we decreased it, and the value stored in the column would be invalid, we would get an error. We could fix this by updating the data in the column to fit the new limit.

---

4. For many of the closest stars, we know the distance from Earth fairly accurately; for instance, Proxima Centauri is roughly 4.3 light years away. Our database, as currently defined, only allows integer distances, so the most accurate value we can enter is 4. Modify the `distance` column in the `stars` table so that it allows fractional light years to any degree of precision required.

<br>
<br>

```sql
ALTER TABLE stars
ALTER COLUMN distance TYPE numeric;
/* numeric instead of real, though I went with real first */
```

<br>
<br>

Further Exploration: Assume the stars table already contains one or more rows of data. What will happen when you try to run the above command? To test this, revert the modification and add a row or two of data:

```sql
ALTER TABLE stars
ALTER COLUMN distance TYPE integer;

INSERT INTO stars (name, distance, spectral_type, companions)
           VALUES ('Alpha Orionis', 643, 'M', 9);

ALTER TABLE stars
ALTER COLUMN distance TYPE numeric ;
```

<br>
<br>

In this case, again since we are converting from integer to a fractional value, it can automatically be convert, thus the command will go through.

However, if we were going the other way around, we would lose the fractional data of the data.

---

5. The `spectral_type` column in the `stars` table is currently defined as a one-character string that contains one of the following 7 values: `'O'`, `'B'`, `'A'`, `'F'`, `'G'`, `'K'`, and `'M'`. However, there is currently no enforcement on the values that may be entered. Add a constraint to the table stars that will enforce the requirement that a row must hold one of the 7 listed values above. Also, make sure that a value is required for this column.

<br>
<br>

```sql
ALTER TABLE stars
ALTER COLUMN spectral_type SET NOT NULL,
  ADD CHECK (spectral_type IN ('O', 'B', 'A', 'F', 'G', 'K', 'M'));
```

<br>
<br>

Further Exploration: Assume the `stars` table contains one or more rows that are missing a `spectral_type` value, or that have an illegal value. What will happen when you try to alter the table schema? How would you go about adjusting the data to work around this problem. To test this, revert the modification and add some data:

```sql
ALTER TABLE stars
DROP CONSTRAINT stars_spectral_type_check,
ALTER COLUMN spectral_type DROP NOT NULL;

INSERT INTO stars (name, distance, companions)
           VALUES ('Epsilon Eridani', 10.5, 0);

INSERT INTO stars (name, distance, spectral_type, companions)
           VALUES ('Lacaille 9352', 10.68, 'X', 0);

ALTER TABLE stars
ADD CHECK (spectral_type IN ('O', 'B', 'A', 'F', 'G', 'K', 'M')),
ALTER COLUMN spectral_type SET NOT NULL;
```

<br>
<br>

If you are missing values and try to add a `NOT NULL` constraint, this will fail. You can update the rows that have the missing data or you can delete the rows with the missing data. The same is true for invalid data and adding a `CHECK` constraint.

---

6. In the previous exercise, we added a `CHECK` constraint to the `stars` table to enforce that the value stored in the spectral_type column for each row is valid. In this exercise, we will use an alternate approach to the same problem.

PostgreSQL provides what is called an enumerated data type; that is, a data type that must have one of a finite set of values. For instance, a traffic light can be red, green, or yellow: these are enumerate values for the color of the currently lit signal light.

Modify the `stars` table to remove the `CHECK` constraint on the `spectral_type` column, and then modify the `spectral_type` column so it becomes an enumerated type that restricts it to one of the following 7 values: `'O'`, `'B'`, `'A'`, `'F'`, `'G'`, `'K'`, and `'M'`.

<br>
<br>

```sql
ALTER TABLE stars
 DROP CONSTRAINT stars_spectral_type_check;

CREATE TYPE spectral_type_enum
    AS ENUM ('O', 'B', 'A', 'F', 'G', 'K');

ALTER TABLE stars
ALTER COLUMN spectral_type TYPE spectral_type_enum
       USING spectral_type::spectral_type_enum;
```
---

7. We will measure Planetary masses in terms of the mass of Jupiter. As such, the current data type of `integer` is inappropriate; it is only really useful for planets as large as Jupiter or larger. These days, we know of many extrasolar planets that are smaller than Jupiter, so we need to be able to record fractional parts for the mass. Modify the `mass` column in the `planets` table so that it allows fractional masses to any degree of precision required. In addition, make sure the mass is required and positive.

While we're at it, also make the `designation` column required.

<br>
<br>

```sql
ALTER TABLE planets
ALTER COLUMN mass TYPE numeric,
ALTER COLUMN mass SET NOT NULL,
ALTER COLUMN designation SET NOT NULL,
  ADD CHECK (mass > 0);
```

8. Add a `semi_major_axis` column for the semi-major axis of each planet's orbit; the semi-major axis is the average distance from the planet's star as measured in astronomical units (1 AU is the average distance of the Earth from the Sun). Use a data type of `numeric`, and require that each planet have a value for the `semi_major_axis`.

<br>
<br>

```sql
ALTER TABLE planets
  ADD COLUMN semi_major_axis numeric NOT NULL;
```

<br>
<br>

Further Exploration: Assume the `planets` table already contains one or more rows of data. What will happen when you try to run the above command? What will you need to do differently to obtain the desired result? To test this, delete the `semi_major_axis` column and then add a row or two of data (note: your `stars` table will also need some data that corresponds to the `star_id` values):

```sql
ALTER TABLE planets
DROP COLUMN semi_major_axis;

DELETE FROM stars;
INSERT INTO stars (name, distance, spectral_type, companions)
           VALUES ('Alpha Centauri B', 4.37, 'K', 3);
INSERT INTO stars (name, distance, spectral_type, companions)
           VALUES ('Epsilon Eridani', 10.5, 'K', 0);

INSERT INTO planets (designation, mass, star_id)
           VALUES ('b', 0.0036, 4);
INSERT INTO planets (designation, mass, star_id)
           VALUES ('c', 0.1, 5);

ALTER TABLE planets
ADD COLUMN semi_major_axis numeric NOT NULL;
```

<br>
<br>

Because of the `NOT NULL` constraint and having set no `DEFAULT` for `semi_major_axis`, this command will not go through. We can either set a `DEFAULT`, or we can use `SET` to fill in this data as we create the column, thus satisifying our constraint.

Alternatively, you could not set the `NOT NULL` constraint and add it back after you have fixed the data.

---

9. Someday in the future, technology will allow us to start observing the moons of extrasolar planets. At that point, we're going to need a moons table in our `extrasolar` database. For this exercise, your task is to add that table to the database. The table should include the following data:

- `id`: a unique serial number that auto-increments and serves as a primary key for this table.
- `designation`: the designation of the moon. We will assume that moon designations will be numbers, with the first moon discovered for each planet being moon 1, the second moon being moon 2, etc. The designation is required.
- `semi_major_axis`: the average distance in kilometers from the planet when a moon is farthest from its corresponding planet. This field must be a number greater than 0, but is not required; it may take some time before we are able to measure moon-to-planet distances in extrasolar systems.
- `mass`: the mass of the moon measured in terms of Earth Moon masses. This field must be a numeric value greater than 0, but is not required.

Make sure you also specify any foreign keys necessary to tie each moon to other rows in the database.

<br>
<br>

```sql
CREATE TABLE moons (
       id serial PRIMARY KEY,
       designation integer NOT NULL CHECK (designation > 0),
       semi_major_axis numeric CHECK (semi_major_axis > 0),
       mass numeric CHECK (mass > 0),
       planet_id integer NOT NULL REFERENCES planets (id)
);
```
---

10. Delete the `extrasolar` database. Use the `psql` console -- don't use the terminal level commands.

Note: you may want to make a backup of your database before you drop it (also called a database dump). You can make a backup like this from the terminal:

```bash
pg_dump --inserts extrasolar > extrasolar.dump.sql
```

<br>
<br>

```sql
\c some_other_db
DROP DATABASE extrasolar;
```
