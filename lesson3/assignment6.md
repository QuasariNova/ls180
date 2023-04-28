1. Import this file into an empty PostgreSQL database. Note: the file contains a lot of data and may take a while to run; your terminal should return to the command prompt once the import is complete.

<br>
<br>
<br>

```bash
psql -d some_database < ~/theater_full.sql
```
---

2. Write a query that determines how many different customers purchased tickets to at least one event.

<br>
<br>
<br>

```sql
SELECT count(id)
  FROM tickets;
```
---

3. Write a query that determines how many different customers purchased tickets to at least one event.

<br>
<br>
<br>

```sql
SELECT count(DISTINCT customer_id)
  FROM tickets;
```
---

4. Write a query that determines what percentage of the customers in the database have purchased a ticket to one or more of the events.

<br>
<br>
<br>

```sql
SELECT round( count(DISTINCT t.customer_id)
            / count(DISTINCT c.id)::decimal * 100,
            2)
       AS percent
  FROM customers AS c
  LEFT OUTER JOIN tickets AS t
    ON c.id = t.customer_id;
```
---

5. Write a query that returns the name of each event and how many tickets were sold for it, in order from most popular to least popular.

<br>
<br>
<br>

```sql
SELECT e.name, count(t.id) as popularity
  FROM events AS e
 LEFT OUTER JOIN tickets AS t
    ON t.event_id = e.id
 GROUP BY e.name
 ORDER BY popularity DESC;
```
---

6. Write a query that returns the user id, email address, and number of events for all customers that have purchased tickets to three events.

<br>
<br>
<br>

```sql
SELECT c.id, c.email, count(DISTINCT t.event_id)
  FROM customers as c
 INNER JOIN tickets as t
    ON t.customer_id = c.id
 GROUP BY c.id
HAVING count(DISTINCT t.event_id) >= 3
 ORDER BY c.id;
```

7. Write a query to print out a report of all tickets purchased by the customer with the email address 'gennaro.rath@mcdermott.co'. The report should include the event name and starts_at and the seat's section name, row, and seat number.

<br>
<br>
<br>

```sql
SELECT events.name AS event,
       events.starts_at,
       sections.name AS section,
       seats.row,
       seats.number AS seat
  FROM tickets
 INNER JOIN customers
    ON tickets.customer_id = customers.id
 INNER JOIN events
    ON tickets.event_id = events.id
 INNER JOIN seats
    ON tickets.seat_id = seats.id
 INNER JOIN sections
    ON seats.section_id = sections.id
 WHERE customers.email =  'gennaro.rath@mcdermott.co';
```
