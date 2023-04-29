1. This set of exercises will focus on an auction. Create a new database called `auction`. In this database there will be three tables, `bidders`, `items`, and `bids`.

After creating the database, set up the 3 tables using the following specifications:
### `bidders`

- `id` of type SERIAL: this should be a primary key
- `name` of type text: this should be `NOT NULL`

### `items`

- `id` of type SERIAL: this should be a primary key
- `name` of type text: this should be `NOT NULL`
- `initial_price` and `sales_price`: These two columns should both be of type numeric. Each column should be able to hold a positive number as high as 1000 dollars with 2 decimal points of precision.
- The `initial_price` represents the starting price of an item when it is first put up for auction. This column should never be `NULL`.
- The `sales_price` represents the final price at which the item was sold. This column may be `NULL`, as it is possible to have an item that was never sold off.

### `bids`

- `id` of type `SERIAL`: this should be a primary key
- `bidder_id`, `item_id`: These will be of type integer and should not be `NULL`. This table connects a bidder with an item and each row represents an individual bid. There should never be a row that has bidder_id or item_id unknown or `NULL`. Nor should there ever be a bid that references a nonexistent item or bidder. If the item or bidder associated with a bid is removed, that bid should also be removed from the database.
- Create your `bids` table so that both `bidder_id` and `item_id` together form a composite index for faster lookup.
- `amount` - The amount of money placed for each individual bid by a bidder. This column should be of the same type as `items.initial_price` and have the same constraints.

Finally, use the `\copy` meta-command to import the below files into your auction database. You'll have to create these files yourself before you can import them with `\copy`.

### bidders.csv

```csv
id, name
1,Alison Walker
2,James Quinn
3,Taylor Williams
4,Alexis Jones
5,Gwen Miller
6,Alan Parker
7,Sam Carter
```

### items.csv

```csv
id, name, initial_price, sales_price
1,Video Game, 39.99, 70.87
2,Outdoor Grill, 51.00, 83.25
3,Painting, 100.00, 250.00
4,Tent, 220.00, 300.00
5,Vase, 20.00, 42.00
6,Television, 550.00,
```

### bids.csv

```csv
id, bidder_id, item_id, amount
1,1, 1, 40.00
2,3, 1, 52.00
3,1, 1, 53.00
4,3, 1, 70.87
5,5, 2, 83.25
6,2, 3, 110.00
7,4, 3, 140.00
8,2, 3, 150.00
9,6, 3, 175.00
10,4, 3, 185.00
11,2, 3, 200.00
12,6, 3, 225.00
13,4, 3, 250.00
14,1, 4, 222.00
15,2, 4, 262.00
16,1, 4, 290.00
17,1, 4, 300.00
18,2, 5, 21.72
19,6, 5, 23.00
20,2, 5, 25.00
21,6, 5, 30.00
22,2, 5, 32.00
23,6, 5, 33.00
24,2, 5, 38.00
25,6, 5, 40.00
26,2, 5, 42.00
```

<br>
<br>
<br>

```sql
CREATE DATABASE auction;
\c auction

CREATE TABLE bidders (
       id serial PRIMARY KEY,
       name text NOT NULL
);

\copy bidders from '~/bidders.csv' WITH DELIMITER ',' CSV HEADER;

CREATE TABLE items (
       id serial PRIMARY KEY,
       name text NOT NULL,
       initial_price numeric(6, 2) NOT NULL
                     CHECK (initial_price BETWEEN 0.01 AND 1000.00),
       sales_price numeric(6,2)
                   CHECK (sales_price BETWEEN 0.01 AND 1000.00)
);

\copy items from '~/items.csv' WITH DELIMITER ',' CSV HEADER;

CREATE TABLE bids (
       id serial PRIMARY KEY,
       bidder_id integer NOT NULL
                 REFERENCES bidders (id)
                         ON DELETE CASCADE,
       item_id integer NOT NULL
               REFERENCES items (id)
                       ON DELETE CASCADE,
       amount numeric(6,2) NOT NULL
              CHECK (amount BETWEEN 0.01 AND 1000.00)
);

CREATE INDEX ON bids (bidder_id, item_id);

\copy bids from '~/bids.csv' WITH DELIMITER ',' CSV HEADER;
```
---

2. Write a SQL query that shows all items that have had bids put on them. Use the logical operator IN for this exercise, as well as a subquery.

Here is the expected output:

```
 Bid on Items
---------------
 Video Game
 Outdoor Grill
 Painting
 Tent
 Vase
(5 rows)
```

<br>
<br>
<br>

```sql
SELECT name as "Bid on Item"
  FROM items
 WHERE items.id IN (
       SELECT DISTINCT item_id -- DISTINCT isn't 100% necessary
         FROM bids
       );
```
---

3. Write a SQL query that shows all items that have not had bids put on them. Use the logical operator `NOT IN` for this exercise, as well as a subquery.

Here is the expected output:

```sql
SELECT name as "Not Bid On"
  FROM items
 WHERE items.id NOT IN (
       SELECT DISTINCT item_id
         FROM bids
       );
```
---

4. Write a `SELECT` query that returns a list of names of everyone who has bid in the auction. While it is possible (and perhaps easier) to do this with a `JOIN` clause, we're going to do things differently: use a subquery with the `EXISTS` clause instead. Here is the expected output:

```
      name
-----------------
 Alison Walker
 James Quinn
 Taylor Williams
 Alexis Jones
 Gwen Miller
 Alan Parker
(6 rows)
```

<br>
<br>
<br>

```sql
SELECT name
  FROM bidders
 WHERE EXISTS (
       SELECT 1
         FROM bids
         WHERE bidders.id = bidder_id
       );
```

<br>
<br>
<br>

Further Exploration: More often than not, we can get an equivalent result by using a `JOIN` clause, instead of a subquery. Can you figure out a `SELECT` query that uses a `JOIN` clause that returns the same output as our solution above?

<br>
<br>
<br>

```sql
SELECT name
  FROM bidders
 INNER JOIN bids
         ON bidder_id = bidders.id
 GROUP BY bidders.id
 ORDER BY bidders.id;
```
---

5. For this exercise, we'll make a slight departure from how we've been using subqueries. We have so far used subqueries to filter our results using a `WHERE` clause. In this exercise, we will build that filtering into the table that we will query. Write an SQL query that finds the largest number of bids from an individual bidder.

For this exercise, you must use a subquery to generate a result table (or transient table), and then query that table for the largest number of bids.

Your output should look like this:

```
  max
------
    9
(1 row)
```

<br>
<br>
<br>

```sql
SELECT max(bid_totals.count)
  FROM (SELECT count(bids.id)
          FROM bids
         GROUP BY bidder_id
       ) AS bid_totals;
```
---

6. For this exercise, use a scalar subquery to determine the number of bids on each item. The entire query should return a table that has the `name` of each item along with the number of bids on an item.

Here is the expected output:

```
    name      | count
--------------+-------
Video Game    |     4
Outdoor Grill |     1
Painting      |     8
Tent          |     4
Vase          |     9
Television    |     0
(6 rows)
```

<br>
<br>
<br>

```sql
SELECT name,
       (SELECT count(bids.id)
          FROM bids
         WHERE item_id = items.id
       )
  FROM items;
```

<br>
<br>
<br>

Further Exploration: If we wanted to get an equivalent result, without using a subquery, then we would have to use a `LEFT OUTER JOIN`. Can you come up with the equivalent query that uses `LEFT OUTER JOIN`?

<br>
<br>
<br>

```sql
SELECT name, count(item_id)
  FROM items
  LEFT OUTER JOIN bids
          ON items.id = item_id
 GROUP BY items.id
 ORDER BY items.id;
```
---

7. We want to check that a given item is in our database. There is one problem though: we have all of the data for the item, but we don't know the id number. Write an SQL query that will display the id for the item that matches all of the data that we know, but does not use the AND keyword. Here is the data we know:

`'Painting', 100.00, 250.00`

<br>
<br>
<br>

```sql
SELECT id
  FROM items
 WHERE ROW('Painting', 100.00, 250.00) =
       ROW(name, initial_price, sales_price);
/* ROW Doesn't technically need to be written */
```
---

8. For this exercise, let's explore the `EXPLAIN` PostgreSQL statement. It's a very useful SQL statement that lets us analyze the efficiency of our SQL statements. More specifically, use `EXPLAIN` to check the efficiency of the query statement we used in the exercise on `EXISTS`:

```sql
SELECT name FROM bidders
WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);
```

First use just `EXPLAIN`, then include the `ANALYZE` option as well. For your answer, list any SQL statements you used, along with the output you get back, and your thoughts on what is happening in both cases.

<br>
<br>
<br>

```sql
EXPLAIN SELECT name
          FROM bidders
         WHERE EXISTS
               (SELECT 1
                  FROM bids
                 WHERE bids.bidder_id = bidders.id
               );
/*
                                QUERY PLAN
--------------------------------------------------------------------------
 Hash Join  (cost=33.38..66.47 rows=635 width=32)
   Hash Cond: (bidders.id = bids.bidder_id)
   ->  Seq Scan on bidders  (cost=0.00..22.70 rows=1270 width=36)
   ->  Hash  (cost=30.88..30.88 rows=200 width=4)
         ->  HashAggregate  (cost=28.88..30.88 rows=200 width=4)
               Group Key: bids.bidder_id
               ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4)
(7 rows)
*/

EXPLAIN ANALYZE SELECT name
                  FROM bidders
                 WHERE EXISTS
                       (SELECT 1
                          FROM bids
                         WHERE bids.bidder_id = bidders.id
                       );

/*
                                                     QUERY PLAN
---------------------------------------------------------------------------------------------------------------------
 Hash Join  (cost=33.38..66.47 rows=635 width=32) (actual time=0.073..0.078 rows=6 loops=1)
   Hash Cond: (bidders.id = bids.bidder_id)
   ->  Seq Scan on bidders  (cost=0.00..22.70 rows=1270 width=36) (actual time=0.027..0.029 rows=7 loops=1)
   ->  Hash  (cost=30.88..30.88 rows=200 width=4) (actual time=0.036..0.037 rows=6 loops=1)
         Buckets: 1024  Batches: 1  Memory Usage: 9kB
         ->  HashAggregate  (cost=28.88..30.88 rows=200 width=4) (actual time=0.031..0.034 rows=6 loops=1)
               Group Key: bids.bidder_id
               Batches: 1  Memory Usage: 40kB
               ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4) (actual time=0.014..0.018 rows=26 loops=1)
 Planning Time: 0.243 ms
 Execution Time: 0.146 ms
(11 rows)
*/
```

The first statement with just `EXPLAIN`, plans the query and outputs the statistics for each step in its query plan. In our case, the top node is a `Hash Join` and it has a startup cost of `33.8` and a total cost of `66.47`. It lists the estimated rows at `635` and the width in bytes of each row as `32`.

Each node another node is performed as part of the parent node.

When we use the `ANALYZE` statement, the query is ran and timed giving us the actual times involved with the query. In this case, the time took about `.146` ms to execute, while planning took `.243` ms.

---

9. In this exercise, we'll use EXPLAIN ANALYZE to compare the efficiency of two SQL statements. These two statements are actually from the "Query From a Virtual Table" exercise in this set. In that exercise, we stated that our subquery-based solution:

```sql
SELECT MAX(bid_counts.count) FROM
  (SELECT COUNT(bidder_id) FROM bids GROUP BY bidder_id) AS bid_counts;
```

was actually faster than the simpler equivalent without subqueries:

```sql
SELECT COUNT(bidder_id) AS max_bid FROM bids
  GROUP BY bidder_id
  ORDER BY max_bid DESC
  LIMIT 1;
```

In this exercise, we will demonstrate this fact.

Run `EXPLAIN ANALYZE` on the two statements above. Compare the planning time, execution time, and the total time required to run these two statements. Also compare the total "costs". Which statement is more efficient and why?

```sql
EXPLAIN ANALYZE SELECT MAX(bid_counts.count)
                  FROM (SELECT COUNT(bidder_id)
                         FROM bids
                        GROUP BY bidder_id
                       ) AS bid_counts;
/*                                                   QUERY PLAN
---------------------------------------------------------------------------------------------------------------
 Aggregate  (cost=37.15..37.16 rows=1 width=8) (actual time=0.050..0.051 rows=1 loops=1)
   ->  HashAggregate  (cost=32.65..34.65 rows=200 width=12) (actual time=0.046..0.049 rows=6 loops=1)
         Group Key: bids.bidder_id
         Batches: 1  Memory Usage: 40kB
         ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4) (actual time=0.026..0.028 rows=26 loops=1)
 Planning Time: 0.185 ms
 Execution Time: 0.102 ms
(7 rows) */
```

The subquery took `.287` ms to execute.

```sql
EXPLAIN ANALYZE SELECT COUNT(bidder_id) AS max_bid
                  FROM bids
                 GROUP BY bidder_id
                 ORDER BY max_bid DESC
                 LIMIT 1;
/*
                                                     QUERY PLAN
---------------------------------------------------------------------------------------------------------------------
 Limit  (cost=35.65..35.65 rows=1 width=12) (actual time=0.140..0.141 rows=1 loops=1)
   ->  Sort  (cost=35.65..36.15 rows=200 width=12) (actual time=0.139..0.139 rows=1 loops=1)
         Sort Key: (count(bidder_id)) DESC
         Sort Method: top-N heapsort  Memory: 25kB
         ->  HashAggregate  (cost=32.65..34.65 rows=200 width=12) (actual time=0.037..0.039 rows=6 loops=1)
               Group Key: bidder_id
               Batches: 1  Memory Usage: 40kB
               ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4) (actual time=0.019..0.021 rows=26 loops=1)
 Planning Time: 0.222 ms
 Execution Time: 0.215 ms
(10 rows)
*/
```

While the one that didn't use a subquery eneded up taking `.437` ms to perform. The added complexity of the second query dragged down both the planning time and execution time.

Both their inner most nodes are `Seq Scan on bids`, and have the same estimated cost. It's parent, `HashAggregate` is the same on both queries as well, having the same estimated cost. The subquery then diverges with only one other node present as the parent to the `HashAggregate`, while the other one still has to `Sort` and `Limit`, these obviously took more time than Aggregating. `HashAggregate` and `Sort` are our two most costly operations, while `Aggregate` that replaces the `Sort` on the subquery is not.
