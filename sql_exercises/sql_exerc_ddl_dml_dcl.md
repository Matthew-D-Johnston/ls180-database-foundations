

##### LS180 - SQL Fundamentals > DML, DDL, and DCL

---

### DML/DDL/DCL Part 1

SQL consists of 3 different sublanguages. For example, one of these sublanguages is called the Data Control Language (DCL) component of SQL. It is used to control access to a database; it is responsible for defining the rights and roles granted to individual users. Common SQL DCL statements include:

```sql
GRANT
REVOKE
```

##### My Response:

###### Data Definition Lanugage (DDL)

* DDL allows users to define the structure, or schema, of a database.

  ```sql
  CREATE
  ALTER
  ```

###### Data Manipulation Language (DML)

* DML allows users to interact with the database by inserting specified data, selecting them, and manipulating them in various ways.

  ```sql
  SELECT
  INSERT
  ```

##### LS Solution

The Data Definition Language (DDL) component of SQL is used to create, modify, and delete databases and tables; that is, the DDL is responsible for describing how data is structured. Common SQL DDL statements include:

```sql
CREATE
ALTER
DROP
```

The Data Manipulation Language (DML) component of SQL is used to create, read, update, and delete the actual data in a database. Common SQL DML statements include:

```sql
SELECT
INSERT
UPDATE
DELETE
```

---

### DML/DDL/DCL Part 2

Does the following statement use the Data Definition Language (DDL) or the Data Manipulation Language (DML) component of SQL?

```sql
SELECT column_name FROM my_table;
```

##### My Response:

The statement uses the DML component of SQL? Here we are selecting specific data from a table.

---

### DML/DDL/DCL Part 3

Does the following statement use the DDL or DML component of SQL?

```sql
CREATE TABLE things (
id serial PRIMARY KEY,
item text NOT NULL UNIQUE,
material text NOT NULL
);
```

##### My Response:

The statement uses the DDL component of SQL. Here we are creating a new table and defining its columns as well as their data types and their constraints.

---

### DML/DDL/DCL Part 4

Does the following statement use the DDL or DML component of SQL?

```sql
ALTER TABLE things
DROP CONSTRAINT things_item_key;
```

##### My Response:

The statement uses the DDL component of SQL. We are droping a table constraint, which has to do with the tables structure.

---

### DML/DDL/DCL Part 5

Does the following statement use the DDL or DML component of SQL?

```sql
INSERT INTO things VALUES (3, 'Scissors', 'Metal');
```

##### My Response:

The statement uses the DML component of SQL. Here we are adding a record to a table called `things`, which means we are dealing with specific data.

---

### DML/DDL/DCL Part 6

Does the following statement use the DDL or DML component of SQL?

```sql
UPDATE things
SET material = 'plastic'
WHERE items = 'Cup';
```

##### My Response:

The statement uses the DML component of SQL. Here we are changing/updating a specific bit of data.

---

### DML/DDL/DCL Part 7

Does the following statement use the DDL, DML, or DCL component of SQL?

```sql
\d things
```

##### My Response:

The statement uses neither the DDL, DML, nor the DCL component of SQL. It is a meta command of PostgreSQL.

##### LS Solution

None of these. You are mostly correct if you said DDL.

###### Discussion

This is a trick question. `\d` is a `psql` console command, not part of SQL. As such, it is not part of any SQL sublanguage. However, it does act something like a DDL statement -- it displays the schema of the named table.

---

### DML/DDL/DCL Part 8

Does the following statement use the DDL or DML component of SQL?

```sql
DELETE FROM things WHERE item = 'Cup';
```

##### My Response:

The statement uses the DML component of SQL. It deletes a row from the `things` table based on the condition that `item = 'Cup'`.

---

### DML/DDL/DCL Part 9

Does the following statement use the DDL or DML component of SQL?

```sql
DROP DATABASE xyzzy;
```

##### My Response:

The statement uses the DDL component of SQL. Here we are removing an entire database. 

##### LS Solution

DDL

###### Discussion

This one is a bit ambiguous. Depending on how you look at `DROP DATABASE`, you might think it is part of the DML, part of the DDL, or both.  

`DROP DATABASE` removes all data from the database, including any and all tables in the database. In this respect, it manipulates data, so some people think of it as part of DML. However, `DROP DATABASE` also deletes everything about the structure of the database and its tables. Furthermore, all variants of the `DROP` statement are generally treated as DDL. In these respects, `DROP DATABASE` manipulates how the data is structured, so it is considered part of the DDL sublanguage.  

For our purposes at Launch School, `DROP DATABASE` is DDL since its main purpose is to operate on data definitions; the deletion of data is merely a side-effect.

---

### DML/DDL/DCL Part 10

Does the following statement use the DDL or DML component of SQL?

```sql
CREATE SEQUENCE part_number_sequence;
```

##### My Response:

I don't know much about sequences at this point, but I think the statement uses the DDL component of SQL. We are creating some sequence, which I believe pertains to the structure of data and not the specific data itself.

##### LS Solution

DDL

###### Discussion

`CREATE SEQUENCE` statements modify the characteristics and attributes of a database by adding a sequence object to the database structure. It does not actually manipulate any data, but instead manipulates the data definitions. As such, `CREATE SEQUENCE` statements are part of the DDL sublanguage.  

It could also be argued that `CREATE SEQUENCE` is DML; the sequence object it creates is a bit of data that is used to keep track of a sequence of automatically generated values, so it can be thought of as being part of the data instead of a characteristic of the data. However, all `CREATE` statements (not just `CREATE SEQUENCE`) are generally thought of as DDL.

