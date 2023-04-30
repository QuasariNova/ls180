# SQL
## Identify the different types of JOINs and explain their differences. ([Types of Joins](https://launchschool.com/books/sql/read/joins#typesofjoins))

A JOIN is a clause in a SQL statement that links two tables together. They usually do this by using keys that define the relationships between the two tables.

### INNER JOIN

```sql
SELECT column
  FROM table
 INNER JOIN another_table
         ON table.a = another_table.b;
```

An `INNER JOIN` links two tables together and returns a result set that contains the common elements of the two tables. Basically, they take the JOIN condition and only return results that match that condition.

### LEFT OUTER JOIN

```sql
SELECT column
  FROM table
 OUTER LEFT JOIN another_table
         ON table.a = another_table.b;
```

A `LEFT OUTER JOIN` always include all the rows from the first table, even if there are no matching rows in the table it is being JOINed with. Like an `INNER JOIN` it still links the two tables together based on the JOIN condition given. Any row that does not have a matching row on the other table will have `NULL` to represent the data of the other table.

### RIGHT OUTER JOIN

```sql
SELECT column
  FROM table
 OUTER RIGHT JOIN another_table
          ON table.a = another_table.b;
```

A `RIGHT OUTER JOIN` is just like a `LEFT OUTER JOIN` except the second table is the one that will include all of its rows. Any row from the second table that is not matched with the first will have `NULL` represent the data of the first table.

### FULL OUTER JOIN

```sql
SELECT column
  FROM table
  FULL OUTER JOIN another_table
          ON table.a = another_table.b;
```

A `FULL OUTER JOIN` is a combination of both the `LEFT OUTER JOIN` and `RIGHT OUTER JOIN`. It returns all rows from both tables and where the join condition is met, the tables are joined. Anywhere the join condition is not met, the missing data is represented as `NULL` values.

### CROSS JOIN

A `CROSS JOIN` does not use a join condition, instead it joins every row of the first table with every row of the second. This is not used very often.

## Name and define the three sublanguages of SQL and be able to classify different statements by sublanguage. ([The SQL Language](https://launchschool.com/lessons/a1779fd2/assignments/7673d9a9))
### Data Definition Language (DDL)

### Data Manipulation Language (DML)

### Data Control Language (DCL)


## Write SQL statements using INSERT, UPDATE, DELETE, CREATE/ALTER/DROP TABLE, ADD/ALTER/DROP COLUMN.
### INSERT ([Inserting Data into a Table](https://launchschool.com/books/sql/read/add_data))

### UPDATE ([Updating Data](https://launchschool.com/books/sql/read/update_and_delete_data#updatingdata))

### DELETE ([Deleting Data](https://launchschool.com/books/sql/read/update_and_delete_data#deletingdata))

### CREATE TABLE ([Create and View Tables](https://launchschool.com/books/sql/read/create_table))

### ADD ([Alter a Table](https://launchschool.com/books/sql/read/alter_table)) ([Adding a Column](https://launchschool.com/books/sql/read/alter_table#addingacolumn)) ([Adding a Constraint](https://launchschool.com/books/sql/read/alter_table#addingaconstraint))

### ALTER ([Alter a Table](https://launchschool.com/books/sql/read/alter_table))

### DROP ([Alter a Table](https://launchschool.com/books/sql/read/alter_table)) ([Removing a Column](https://launchschool.com/books/sql/read/alter_table#removingacolumn)) ([Dropping Tables](https://launchschool.com/books/sql/read/alter_table#droppingtables))


## Understand how to use GROUP BY, ORDER BY, WHERE, and HAVING.
### GROUP BY

### ORDER BY

### WHERE

### HAVING


## Understand how to create and remove constraints, including CHECK constraints
### Constraints
#### Table Constraints

#### Column Constraints ([NOT NULL and Default Vlaues](https://launchschool.com/lessons/a1779fd2/assignments/c6a5a6cb))


## Be familiar with using subqueries

# PostgreSQL
## Describe what a sequence is and what they are used for.

## Create an auto-incrementing column.

## Define a default value for a column.

## Be able to describe what primary, foreign, natural, and surrogate keys are. ([Keys](https://launchschool.com/books/sql/read/table_relationships#keys)) ([Using Keys](https://launchschool.com/lessons/a1779fd2/assignments/00e428da))
### Primary Keys

A Primary Key is used to identify a row of data in a table. Primary keys use the `NOT NULL` and `UNIQUE` constraints to ensure that they can uniquely identify the row.

### Foreign Keys

Foreign keys allow us to associate a row from one table with another tables row. These column values are the same as the another tables Primary Key value for the target row.

Foreign Keys do not have to implement the `UNIQUE` or `NOT NULL` constraints by default, but depending on cardinality and modality would make it necessary one and/or the other.

If a row has a non `NULL` value for its Foreign Key column, the value must be a value contained within the Primary Keys column in the related table. This is because of Referential Integrity.

### Natural Keys

A natural key is a value that exists naturally in a dataset that can be used to uniquely identify each row of data from that dataset.

These are tricky to find as data is prone to change and therefore can't always be unique.

### Surrogate Keys

A surrogate key is a created value added for the purpose of identifying a row of data in a database.

## Create and remove CHECK constraints from a column.

## Create and remove foreign key constraints from a column.
### Creating a Foreign Key

### Removing a Foreign Key


# Database Diagrams

## Talk about the different levels of schema. ([Database Diagrams: Levels of Schema](https://launchschool.com/lessons/5ae760fa/assignments/2f3bc8f7))
### Conceptual Schema

A high-level design that focuses on entities and the relationships between them. Uses a high level of abstraction, interested in the bigger picture.

#### Example:
Phone Contact has a one to many relationship with a Phone Call

We don't care about the data inside the contact or phone call for this schema, just their concepts.

### Logical Schema

Somewhere in the middle of the Conceptual and Physical Schema. It is probably closer to physical schema, but it isn't concerned with implementation.

### Physical Schema

A low-level database specific diagram. It focuses on how we implement the relations in the database. This means it defines tables, columns, and keys and defines the relationships for the implementation.

## Define cardinality and modality. ([Database Diagrams: Cardinality and Modality](https://launchschool.com/lessons/5ae760fa/assignments/46053e3b))
### Cardinality

Cardinality deals with how many objects are on one side of a relationship. You can have a cardinality of either one or many.

### Modality

Modality deals with whether an object is required on one side of a relationship. You can have a modality of 0, which means that the object is not required, or you can have a modality of 1, which means at least 1 object needs to exist on this side of the relationship.

#### Examples:
A ticket for a seat at the movie theater would have a 0 for modality in its relationship with the seat. You do not need a ticket for a seat to exist.

A seat, on the otherhand, would have a modality of 1 in this relationship. This is because you can't sell a ticket in which you don't have a seat for.

## Be able to draw database diagrams using crow's foot notation.

- Crow foot = many cardinality
- line = one cardinality
- added circle = 0 modality
- added line = 1 modality
