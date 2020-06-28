##### LS180 - SQL Fundamentals > Easy 1

---

### Create a Database

---

Let's start by creating a database. Create a new database called `animals`.  

The wrapper function for CREATE DATABASE, `createdb`.

```sql
matthewjohnston$ createdb animals
```

Or, we may also use an SQL statement:

```sql
CREATE DATABASE animals;
```

For this exercise, we need to create a new database. With PostgreSQL, the easiest way to do that is to use the Postgres wrapper function, `createdb`. But, if we want to stick with standard SQL, then we can use the `CREATE DATABASE` SQL command. Remember: to use `CREATE DATABASE` (and other SQL), you must use the `psql` console; to use `createdb` (and other wrapper functions), you must use the regular terminal console.

---

### Create a Table

---

Now that we have an `animals` database, we can lay the groundwork needed to add some data to it.  

Make a table called `birds`. It should have the following fields:

* id (a primary key)
* name (string with space for up to 25 characters)
* age (integer)
* species (a string with room for no more than 15 characters)

###### My Solution

```sql
CREATE TABLE birds (
id serial PRIMARY KEY,
name varchar(25),
age int,
species varchar(15)
);
```

###### LS Solution

```sql
CREATE TABLE birds(
  id serial PRIMARY KEY,
  name character varying(25),
  age integer,
  species character varying(15)
);
```

Let's start from the top. We want a primary key that auto-increments. For that last part, we'll use the pseudotype `serial`. It auto-increments `id` for us as we create new `bird` rows in the table. This column should be a primary key, which ensures that `id` is `NOT NULL` and `UNIQUE`. (There's some overlap here; `serial` also includes `NOT NULL`.)  

We also want `name` and `species` columns. Those will work as text. Here, we use `varchar`, `character varying`, or `text`, which are aliases, so all will work.  

The `age` column is last; we set it as type `integer`.

---

### Insert Data

---

For this exercise, we'll add some data to our `birds` table. Add five records to this database so that our data looks like:

```
 id |   name   | age | species
----+----------+-----+---------
  1 | Charlie  |   3 | Finch
  2 | Allie    |   5 | Owl
  3 | Jennifer |   3 | Magpie
  4 | Jamie    |   4 | Owl
  5 | Roy      |   8 | Crow
(5 rows)
```

###### My Solution

```sql
INSERT INTO birds (name, age, species)
VALUES ('Charlie', 3, 'Finch'),
('Allie', 5, 'Owl'),
('Jennifer', 3, 'Magpie'),
('Jamie', 4, 'Owl'),
('Roy', 8, 'Crow');
```

###### LS Solution

```sql
INSERT INTO birds (name, age, species) VALUES ('Charlie', 3, 'Finch');
INSERT INTO birds (name, age, species) VALUES ('Allie', 5, 'Owl');
INSERT INTO birds (name, age, species) VALUES ('Jennifer', 3, 'Magpie');
INSERT INTO birds (name, age, species) VALUES ('Jamie', 4, 'Owl');
INSERT INTO birds (name, age, species) VALUES ('Roy', 8, 'Crow');
```

To add data to our `birds` table, we need to use the `INSERT INTO` SQL statement. Its general form is:

```sql
INSERT INTO table_name [ (column_name [, ...]) ] VALUES (value [, ...])
```

We specify a table, the column names we wish to deal with and the values we wish to insert under each of these column. Order is important here; when listing the VALUES, make sure the order of those values corresponds to the order of the column names.  

To be clear, we're using PostgreSQL documentation syntax here. "(column_name) [, ...]" means that we can have one or more columns delimited by commas. The parentheses are required.

###### Further Exploration

There is a form of `INSERT INTO` that doesn't require the column names. How does that form of `INSERT INTO` work, and when would you use is?

###### Response

We can use `INSERT INTO` without column names so long as the `VALUES` that are input are specified in the order that the columns are organized in the table. Hence:

```sql
INSERT INTO birds
VALUES (DEFAULT, 'Charlie', 3, 'Finch'),
(DEFAULT, 'Allie', 5, 'Owl'),
(DEFAULT, 'Jennifer', 3, 'Magpie'),
(DEFAULT, 'Jamie', 4, 'Owl'),
(DEFAULT, 'Roy', 8, 'Crow');
```

---

### Select Data

---

Write an SQL statement to query all data that is currently in our birds table.