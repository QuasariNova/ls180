1. What kind of programming language is SQL?

SQL is a "special purpose language." It was built to specifically interact with relational database instead of being of use for generalized purposes. SQL is also a declaritive language. It describes what must be done not how it is done.

---

2. What are the three sublanguages of SQL?

    1. Data Definition Language (DDL)
    2. Data Manipulation Language (DML)
    3. Data Control Language (DCL)

---

3. Write the following values as quoted string values that could be used in a SQL query.

```
canoe
a long road
weren't
"No way!"
```

```sql
'canoe'
'a long road'
'weren''t'
'"No way!"'
```

---

4. What operator is used to concatenate strings?

According to the [documentation](https://www.postgresql.org/docs/15/functions-string.html), the `||` operator will concatenate strings like so:

```sql
'Hello ' || 'World' /* 'Hello World'
```

---

5. What function returns a lowercased version of a string? Write a SQL statement using it.

According to the [documentation](https://www.postgresql.org/docs/15/functions-string.html), the `lower()` function will convert the string to lowercase.

```sql
SELECT lower('Oh My GAWSH!');
/*
    lower
--------------
 oh my gawsh!
(1 row)
*/
```
---

6. How does the `psql` console display true and false values?

It displays true as `t` and false as `f`.

---

7. The surface area of a sphere is calculated using the formula `A = 4π r2`, where `A` is the surface area and `r` is the radius of the sphere.

Use SQL to compute the surface area of a sphere with a radius of 26.3cm, truncated to return an integer.

```sql
SELECT trunc(4 * pi() * (26.3 ^ 2)) AS surface_area;
/*
 surface_area
--------------
         8692
(1 row)
*/
```
