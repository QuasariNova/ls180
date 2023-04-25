1. Write a SQL statement that will create the following table, `people`:

| name | age | occupation |
| :---: | :---: | :---: |
| Abby | 34 | biologist |
| Mu'nisah | 26 | NULL |
| Mirabelle |	40 | contractor |

```sql
CREATE TABLE people (
    name       varchar(15), -- Launch School used n = 255
    age        integer,
    occupation varchar(15) -- Launch School used n = 255
);
```
---

2. Write SQL statements to insert the data shown in #1 into the table.

```sql
INSERT INTO people VALUES ('Abby', 34, 'biologist');
INSERT INTO people VALUES ('Mu''nisah', 26); -- Since has 1 less column, needs to be a different statement
INSERT INTO people VALUES ('Mirabelle', 40, 'contractor');
```
---

3. Write 3 SQL queries that can be used to retrieve the second row of the table shown in #1 and #2.

```sql
SELECT * FROM people WHERE name LIKE '%nisah';
SELECT * FROM people WHERE occupation IS NULL;
SELECT * FROM people WHERE age BETWEEN 20 AND 30;
```
---

4. Write a SQL statement that will create a table named `birds` that can hold the following values:

| name | length | wingspan | family | extinct |
|:---:|:---:|:---:|:---:|:---:|
| Spotted Towhee | 21.6 | 26.7 | Emberizidae | f |
| American Robin | 25.5 | 36.0 | Turdidae | f |
| Greater Koa Finch | 19.0 | 24.0 | Fringillidae | t |
| Carolina Parakeet | 33.0 | 55.8 | Psittacidae | t |
| Common Kestrel | 35.5 | 73.5 | Falconidae | f |

```sql
CREATE TABLE birds (
    name     varchar(25),   --- launch school used 255
    length   decimal(3, 1), --- launch school used 4, 1
    wingspan decimal(3, 1), --- launch school used 4, 1
    family   varchar(25),   --- launch school used 255
    extinct  boolean
);
```
---

5. Using the table created in #4, write the SQL statements to insert the data as shown in the listing.

```sql
INSERT INTO birds
VALUES ('Spotted Towhee', 21.6, 26.7, 'Emberdizidae', false),
       ('American Robin', 25.5, 36, 'Turdidae', false),
       ('Greater Koa Finch', 19, 24, 'Fringillidae', true),
       ('Carolina Parakeet', 33, 55.8, 'Psittacidae', true),
       ('Common Kestrel', 35.5, 73.5, 'Falconidae', false);
```

6. Write a SQL statement that finds the names and families for all birds that are not extinct, in order from longest to shortest (based on the `length` column's value).

```sql
SELECT name, family
  FROM birds
 WHERE extinct = false
 ORDER BY length DESC;
```
---

7. Use SQL to determine the average, minimum, and maximum wingspan for the birds shown in the table.

```sql
SELECT round(avg(wingspan), 1), min(wingspan), max(wingspan)
  FROM birds;
```
---

8. Write a SQL statement to create the table shown below, `menu_items`:

| item | prep_time | ingredient_cost | sales | menu_price |
|:---:|:---:|:---:|:---:|:---:|
| omelette | 10 | 1.50 | 182 | 7.99 |
| tacos | 5 | 2.00 | 254 | 8.99 |
| oatmeal | 1 | 0.50 | 79 | 5.99 |

```sql
CREATE TABLE menu_items (
    item            varchar(15),    -- text
    prep_time       integer,
    ingredient_cost decimal(3, 2),  -- 4, 2
    sales           integer,
    menu_price      decimal(3, 2)   -- 4, 2
);
```
---

9. Write SQL statements to insert the data shown in #8 into the table.

```sql
INSERT INTO menu_items
VALUES ('omelette', 10, 1.5, 182, 7.99),
       ('tacos', 5, 2, 254, 8.99),
       ('oatmeal', 1, 0.5, 79, 5.99);
```

---

10. Using the table and data from #8 and #9, write a SQL query to determine which menu item is the most profitable based on the cost of its ingredients, returning the name of the item and its profit.

```sql
SELECT item, menu_price - ingredient_cost AS profit
  FROM menu_items
 ORDER BY profit DESC -- aliasing is cool
 LIMIT 1;
```
---

11. Write a SQL query to determine how profitable each item on the menu is based on the amount of time it takes to prepare one item. Assume that whoever is preparing the food is being paid $13 an hour. List the most profitable items first. Keep in mind that `prep_time` is represented in minutes and `ingredient_cost` and `menu_price` are both in dollars and cents:

```sql
SELECT item, menu_price, ingredient_cost, round(prep_time * 13.0 / 60, 2) AS labor, menu_price - ingredient_cost - round(prep_time * 13.0 / 60, 2) AS profit
  FROM menu_items;
