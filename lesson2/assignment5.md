1. Describe the difference between the `varchar` and `text` data types.

Both `varchar` and `text` are string types which allow a variable size. `varchar` has you set an upper limit to the number of characters that can be in the string, while `text` does not. `text` however is not part of the SQL standard though.

---

2. Describe the difference between the `integer`, `decimal`, and `real` data types.

`integer`s are whole numbers, `real` are floating point numbers, and `decimal` are non-floating point fractional values with a limited precision.

---

3. What is the largest value that can be stored in an `integer` column?

According to the [documentation](https://www.postgresql.org/docs/current/datatype-numeric.html), `integer`s can go up to `2,147,483,647`.

---

4. Describe the difference between the `timestamp` and `date` data types.

`timestamp` data has both a time and date component to it, while `date` only has the date part.

---

5. Can a time with a time zone be stored in a column of type `timestamp`?

No, but there is the `timestamp with time zone` type.
