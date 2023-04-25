1. SQL consists of 3 different sublanguages. For example, one of these sublanguages is called the Data Control Language (DCL) component of SQL. It is used to control access to a database; it is responsible for defining the rights and roles granted to individual users. Common SQL DCL statements include:

```sql
GRANT
REVOKE
```

Name and define the remaining 2 sublanguages, and give at least 2 examples of each.

    1. Data Definition Language (DDL)
        - The DDL is used to define the schema for the database.
        - It is responsible for the structure of the data.
        - ```sql
          CREATE
          ALTER
          DROP
          ```
    2. Data Manipulation Language (DML)
        - The DML is used to manipulate the data within the database
        - It follows CRUD(create, read, update, and delete)
        - ```sql
          INSERT
          SELECT
          UPDATE
          DELETE
          ```

---

2. Does the following statement use the Data Definition Language (DDL) or the Data Manipulation Language (DML) component of SQL?

```sql
SELECT column_name FROM my_table;
```

This statement is used to read data, thus it is part of the Data Manipulation Language (DML).

---

3. Does the following statement use the DDL or DML component of SQL?

```sql
CREATE TABLE things (
  id serial PRIMARY KEY,
  item text NOT NULL UNIQUE,
  material text NOT NULL
);
```

This statement creates a table and defines its structure. As it is detailing how data will be structured it uses the Data Definition Language (DDL).

---

4. Does the following statement use the DDL or DML component of SQL?

```sql
ALTER TABLE things
DROP CONSTRAINT things_item_key;
```

The statement alters a tables structure by removing a constraint. As such it deals with how data is structured, thus it uses the Data Definition Language (DDL).

---

5. Does the following statement use the DDL or DML component of SQL?

```sql
INSERT INTO things VALUES (3, 'Scissors', 'Metal');
```

This statement will add a row inside the table things. Since this is adding data to a table, thus creating data, it uses the Data Manipulation Language (DML).

---

6. Does the following statement use the DDL or DML component of SQL?

```sql
UPDATE things
SET material = 'plastic'
WHERE item = 'Cup';
```

This statement updates values in a table. Since it is manipulating data by updating it, it uses the Data Manipulation Language (DML).

---

7. Does the following statement use the DDL, DML, or DCL component of SQL?

```sql
\d things
```

This does not use any of the SQL sublanguages. This is instead a meta command used by PostgreSQL in its command line interface.

---

8. Does the following statement use the DDL or DML component of SQL?

```sql
DELETE FROM things WHERE item = 'Cup';
```

This statement deletes rows from a table. As it is manipulating data by deleting the data, it is using Data Manipulation Language (DML).

---

9. Does the following statement use the DDL or DML component of SQL?

```sql
DROP DATABASE xyzzy;
```

This command permanently destroys a database. This uses the Data Definition Language (DDL) as it is removing the structure from the data, the data just gets deleted with the structure removal.

---

10. Does the following statement use the DDL or DML component of SQL?

```sql
CREATE SEQUENCE part_number_sequence;
```

This is Data Definition Language. This modifies the database by adding a sequence object. This command itself does not manipulate data.
