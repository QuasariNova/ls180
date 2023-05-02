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

The data definition sublanguage of SQL is responsible for allowing users to create and modify the schema stored inside a database. Statements that belong to the data definition language include CREATE TABLE, ALTER TABLE, ADD COLUMN, DROP TABLE, and many other statements related to the structure of or rules that govern the data within the database.

### Data Manipulation Language (DML)

The data manipulation sublanguage of SQL is responsible for allowing users to create, retrieve, update, or delete data within a database. The DML includes INSERT, SELECT, UPDATE, and DELETE.

Some database separate retrieval and manipulation statements into two separate languages, but PostgreSQL considers them combined.

### Data Control Language (DCL)
The data control sublanguage of SQL is responsible for allowing users to manipulate the rights and access roles for access to a database or table.

## Write SQL statements using INSERT, UPDATE, DELETE, CREATE/ALTER/DROP TABLE, ADD/ALTER/DROP COLUMN.
### INSERT ([Inserting Data into a Table](https://launchschool.com/books/sql/read/add_data))

`INSERT` is the Create part of CRUD. It allows you to add data to a Table.

```sql
INSERT INTO things
       (column1, column2,...)
VALUES (1, 'two', ...);
```

### UPDATE ([Updating Data](https://launchschool.com/books/sql/read/update_and_delete_data#updatingdata))

`UPDATE` is the Update part of CRUD. It allows you to change column(s) in a row of data.

```sql
UPDATE things
   SET column = 'value'
 WHERE condition;
```

Without using a `WHERE` condition, this command will update the column for every row.

### DELETE ([Deleting Data](https://launchschool.com/books/sql/read/update_and_delete_data#deletingdata))

`DELETE` is the delete part of CRUD. It allows you to remove rows from a table.

```sql
DELETE FROM things
 WHERE condition;
```

Without a `WHERE` condition, this command will delete every row in the table.

### CREATE TABLE ([Create and View Tables](https://launchschool.com/books/sql/read/create_table))

`CREATE TABLE` is used to create a table schema for data to be inserted in to.

```sql
CREATE TABLE table_name (
  column_name integer,
  column2_name text,
  CONSTRAINT constraint_name CHECK (check)
);
```

### ADD ([Alter a Table](https://launchschool.com/books/sql/read/alter_table)) ([Adding a Column](https://launchschool.com/books/sql/read/alter_table#addingacolumn)) ([Adding a Constraint](https://launchschool.com/books/sql/read/alter_table#addingaconstraint))

`ADD` can be used with an `ALTER TABLE` command to add a new column or constraint.

### ALTER ([Alter a Table](https://launchschool.com/books/sql/read/alter_table))

`ALTER` can allow you to alter a table, column, or other relation.

### DROP ([Alter a Table](https://launchschool.com/books/sql/read/alter_table)) ([Removing a Column](https://launchschool.com/books/sql/read/alter_table#removingacolumn)) ([Dropping Tables](https://launchschool.com/books/sql/read/alter_table#droppingtables))

`DROP` allows you to permanently delete schema along with its associated data.

```sql
DROP TABLE tablename;

DROP DATABASE dbname;

ALTER TABLE tablename
 DROP COLUMN columnname;

ALTER TABLE tablename
 DROP CONSTRAINT constraintname;

ALTER TABLE tablename
ALTER COLUMN columnname
 DROP NOT NULL;
```

## Understand how to use GROUP BY, ORDER BY, WHERE, and HAVING.
### GROUP BY [Group By](https://launchschool.com/books/sql/read/more_on_select#groupby)

Used in a `SELECT` query to form groups of rows based on columns which share a common value. You can only `SELECT` or `ORDER BY` columns that are aggregated via an aggregate function or are part of a `GROUP BY` clause when you use `GROUP BY`.

### ORDER BY [ORDER BY](https://launchschool.com/books/sql/read/select_queries#ordering)

Used in a `SELECT` query to dictate the ordering of the output rows. You can set it to order in `ASC` or `DESC` order and it will order rows based on where you place them in the clause.

```sql
SELECT full_name, enabled FROM users
ORDER BY enabled DESC, full_name ASC;
```

The above would order accounts by if they are enabled(true first) then by the full name of the account holder in alphabetical order.

### WHERE [Select Query Syntax](https://launchschool.com/books/sql/read/select_queries#selectquerysyntax)

`WHERE` clauses filter the results of a `SELECT` query. It will only return rows that fit the criteria of the `WHERE` condition.

```sql
SELECT full_name FROM users
WHERE enabled = true;
```

The above whould only have rows where the user account was enabled.

### HAVING [How PostgreSQL Executes Queries](https://launchschool.com/lessons/a1779fd2/assignments/f4b7a9dc)

`HAVING` conditions are similar to `WHERE` conditions, but instead of applying to individual rows they are instead applied to values used to create groups. The conditions are usually set to check an aggregate functions output or a column appearing in the `GROUP BY` clause.

You can't have a `HAVING` condition without a `GROUP BY` clause.

```sql
SELECT role, count(id) FROM users
GROUP BY role
HAVING count(id) >= 5;
```

This would show the rows where at least 5 users held a specific role.

## Understand how to create and remove constraints, including CHECK constraints
### Constraints ([Adding a constraint])(https://launchschool.com/books/sql/read/alter_table#addingaconstraint) ([NOT NULL and Default Values](https://launchschool.com/lessons/a1779fd2/assignments/c6a5a6cb))
- NOT NULL
    - Creation

    ```sql
    CREATE TABLE things (
      id integer NOT NULL
    );

    /* or */
    ALTER TABLE things
    ALTER COLUMN id
      SET NOT NULL;
    ```

    - Removal

    ```sql
    ALTER TABLE things
    ALTER COLUMN id
     DROP NOT NULL;
    ```

- UNIQUE
    - Creation

    ```sql
    CREATE TABLE things (
      id integer UNIQUE
    );

    /* or */
    ALTER TABLE things
      ADD UNIQUE (id);
    ```

    More than one column can be added to a `UNIQUE` constraint, making combinations of those columns must be unique.

    - Removal

    ```sql
    ALTER TABLE things
     DROP CONSTRAINT things_id_key;
    ```

- CHECK
    - Creation

    ```sql
    CREATE TABLE things (
      name text CHECK(length(name) > 0)
    );

    /* or */

    ALTER TABLE things
      ADD CHECK (length(name) > 0);
    ```

    - Removal

    ```sql
    ALTER TABLE things
      DROP CONSTRAINT things_name_check;
    ```

- FOREIGN KEY
      - Creation

      ```sql
      CREATE TABLE things (
        material_id integer REFERENCES materials(id)
      );

      /* or */

      ALTER TABLE things
        ADD FOREIGN KEY (material_id)
        REFERENCES materials(id);
      ```

      - Removal

      ```sql
      ALTER TABLE things
        DROP CONSTRAINT things_material_id_fkey;
      ```

## Be familiar with using subqueries ([Book: Subqueries](https://launchschool.com/books/sql/read/joins#subqueries)) ([Subqueries](https://launchschool.com/lessons/e752508c/assignments/2009d549))

TODO Subqueries

# PostgreSQL
## Describe what a sequence is and what they are used for. ([Using Keys](https://launchschool.com/lessons/a1779fd2/assignments/00e428da))

A sequence is a special kind of relation that is able to generate a series of numbers on demand by remembering the last number generated and generating a new number based on that.

They are typically used as surrogate keys for rows as a primary key.

## Create an auto-incrementing column.

```sql
CREATE TABLE things (id serial, name text);
```

Is the same as:

```sql
CREATE SEQUENCE things_id_seq;
CREATE TABLE things (
       id integer NOT NULL DEFAULT nextval('colors_id_seq'),
       name text
);
```

Also you can add it to a column like so:

```sql
ALTER TABLE things
  ADD COLUMN id series;
```

## Define a default value for a column.

```sql
CREATE TABLE accounts (
  id serial PRIMARY KEY,
  name text,
  amount numeric DEFAULT 0.0
);

/* or */

ALTER TABLE accounts
ALTER COLUMN amount
  SET DEFAULT 0.0;
```

### Remove default from a column
```sql
ALTER TABLE accounts
ALTER COLUMN amount
 DROP DEFAULT;
```

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
### Create CHECK constraints on a column
```sql
CREATE TABLE accounts (
  id serial PRIMARY KEY,
  amount numeric CHECK (amount >= 0)
);

/* or */

CREATE TABLE accounts (
  id serial PRIMARY KEY,
  amount numeric,
  CHECK (amount >= 0)
);

/* or */

ALTER TABLE accounts
  ADD CHECK (amount >= 0);
```

### Remove CHECK constraints from a column

```sql
ALTER TABLE accounts
  DROP CONSTRAINT accounts_amount_check;
```

## Create and remove foreign key constraints from a column.
### Creating a Foreign Key
```sql
CREATE TABLE things (
  id serial PRIMARY KEY,
  name text,
  material_id integer REFERENCES materials (id) ON DELETE CASCADE
);

/* or */

CREATE TABLE things (
  id serial PRIMARY KEY,
  name text,
  material_id integer,
  FOREIGN KEY (material_id) REFERENCES materials(id)
);

/* or */

ALTER TABLE things
  ADD FOREIGN KEY (material_id) REFERENCES materials(id);
```

### Removing a Foreign Key

```sql
ALTER TABLE things
  DROP CONSTRAINT things_material_id_fkey;
```

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
