##### LS181 Assessment: Database Foundations > Instructions for Assessment LS181

---

## Study Guide

### SQL

##### Explain the difference between INNER, LEFT OUTER, and RIGHT OUTER joins.

* INNER JOIN
  * An `INNER JOIN` returns a result set that contains the common elements of the tables, i.e. the intersection where they match on the joined condition. 
  * INNER JOINs are the most frequently used JOINs; in fact if you don't specify a join type and simply use the `JOIN` keyword, then PostgreSQL will assume you want an inner join.
  * simple definition: combines rows from two tables whenever the join condition is met.
* LEFT JOIN
  * A LEFT JOIN or a LEFT OUTER JOIN takes all the rows from one table, defined as the `LEFT` table, and joins it with a second table. 
  * The `JOIN` is based on the conditions supplied in the parentheses. 
  * A `LEFT JOIN` will always include the rows from the `LEFT` table, even if there are no matching rows in the table it is JOINed with.
  * Note that using either `LEFT JOIN` or `LEFT OUTER JOIN` does exactly the same thing, and the `OUTER` part is often omitted. Even so, it is still common to refer to this type of join as an 'outer' join in order to differentiate it from an 'inner' join.
  * simple definition: same as inner join, except rows from the first table are added to the join table, regardless of the evaluation of the join condition.
* RIGHT JOIN
  * A `RIGHT JOIN` is similar to a `LEFT JOIN` except that the roles between the two tables are reversed, and all the rows on the second table are included along with any matching rows from the first table.
  * simple definition: same as inner join, except rows from the second table are added to the join table, regardless of the evaluation of the join condition.
* FULL JOIN
  * A `FULL JOIN` or `FULL OUTER JOIN` is essentially a combination of `LEFT JOIN` and `RIGHT JOIN`. This type of join contains all of the rows from both of the tables. 
  * Where the join condition is met, the rows of the two tables are joined, just as in the previous examples we've seen.
  * For any rows on either side of the join where the join condition is not met, the columns for the _other_ table have `NULL` values for that row.
* CROSS JOIN
  * A `CROSS JOIN`, also known as a Cartesian JOIN, returns all rows from one table crossed with every row from the second table. In other words, the join table of a cross join contains every possible combination of rows from the tables that have been joined. 
  * Since it returns all combinations, a `CROSS JOIN` does not need to match rows using a join condition, therefore it does not have an `ON` clause.

##### Name and define the three sublanguages of SQL and be able to classify different statements by sublanguage.

* **Data Definition Language (DDL)**: used to define the structure, or schema, of a database and the tables and columns within it.
  * **controls**: relation structure and rules.
  * **SQL Constructs:** `CREATE`, `DROP`, `ALTER`.
  * The _data definition_ parts of SQL are what allow a user to create and modify the schema stored within a database. It includes **CREATE TABLE, ALTER TABLE, ADD COLUMN**, and several other statements for creating or modifying the structure of or rules that govern the data that is held within a database.
* **Data Manipulation Language (DML):** used to retrieve or modify data stored in a database. `SELECT` queries are part of DML.
  * DML statements can be categorized into four different types:
    1. `INSERT` statements: these add new data into a database table.
    2. `SELECT` statements: also referred to as Queries; these retrieve existing data from database tables.
    3. `UPDATE` statements: these update existing data in a database table.
    4. `DELETE` statements: these delete existing data from a database table.
  * **controls:** values stored within relations.
  * **SQL Constructs:** `SELECT`, `INSERT`, `UPDATE`, `DELETE`.
  * This part of the SQL language is what allows a user to retrieve or modify the data stored within a database. Some databases consider the retrieval and manipulation as two separate languages themselves, but PostgreSQL's documentation combines them and accordingly, we will as well.
* **Data Control Language (DCL):** used to determine what various users are allowed to do when interacting with a database.
  * **controls:** who can do what.
  * **SQL Constructs:** `GRANT`, `REVOKE`.
  * **DCL** is tasked with controlling the rights and access roles of the users interacting with a database or table. We aren't going to spend much time on this part of SQL in this course since most databases you'll be working with will utilize a database user that has complete control of and access to a database, its schema, and its data. You may work on a project at some point where this isn't the case, but what that usually looks like is that you have been granted read-only access and can only use `SELECT` statements.

##### Write SQL statements using INSERT, UPDATE, DELETE, CREATE/ALTER/DROP TABLE, ADD/ALTER/DROP COLUMN.

##### Understand how to use GROUP BY, ORDER BY, WHERE, and HAVING.

* `HAVING` conditions are very similar to `WHERE` conditions, only they are applied to the values that are used to create groups and not individual rows. This means that a column that is mentioned in a `HAVING` clause should almost always appear in a `GROUP BY` clause and/or an aggregate function in the same query. Both `GROUP BY` and aggregate functions perform grouping, and the `HAVING` clause is used to filter that aggregated/grouped data.

### PostgreSQL

##### Describe what a sequence is and what they are used for.

* A **sequence** is a special kind of relation that generates a series of numbers. A sequence will remember the last number it generated, so it will generate numbers in a predetermined sequence automatically.

* To create a sequence:

  ```sql
  CREATE SEQUENCE sequence_name;
  ```

##### Create an auto-incrementing column.

* Use datatype `serial`.

* It turns out that **serial** columns in PostgreSQL are actually a short-hand for a column definition that is much longer:

  ```sql
  -- This statement:
  CREATE TABLE colors (id serial, name text);
  
  -- is actually interpreted as if it were this one:
  CREATE SEQUENCE colors_id_seq;
  CREATE TABLE colors (
  		id integer NOT NULL DEFAULT nextval('colors_id_seq'),
  		name text
  );
  ```

##### Define a default value for a column.

* Use the `DEFAULT` keyword followed by the default value.

* When first creating a table, the `DEFAULT` keyword is placed after the column's datatype.

* When adding a default constraint after a column has been created, use the following:

  ```sql
  ALTER TABLE table_name
  ALTER COLUMN column_name
  SET DEFAULT default_value;
  ```

##### Be able to describe what primary, foreign, natural, and surrogate keys are.

* **Primary Key:** 
  * A unique identifier for a row of data. 
  
  * Making a column a `PRIMARY KEY` is essentially equivalent to adding `NOT NULL` and `UNIQUE` constraints to that column.
  
  * By specifying `PRIMARY KEY` (in a similar way to how we would specify `NOT NULL`), PostgreSQL will create an index on that column that enforces it holding unique values in addition to preventing the column from holding NULL values. For the most part, this code:
  
    ```sql
    CREATE TABLE more_colors (id int PRIMARY KEY, name text);
    ```
  
  *  is the same as:
  
    ```sql
    CREATE TABLE more_colors (id int NOT NULL UNIQUE, name text);
    ```
  
  * The difference between the two is documenting your intention as a database designer. By using `PRIMARY KEY`, the fact that a certain column can be relied upon as a way to identify specific rows is baked into the table's schema.

* **Foreign Key:**
  
  * Allows us to associate a row in one table to a row in another table, which is done by setting a column in one table as a Foreign Key that references another table's Primary Key column.
  
  * Creating this relationship is done using the `REFERENCES` keyword.
  
  * With a `CREATE TABLE` statement we can specify a foreign key with the following command: `FOREIGN KEY (fk_column_name) REFERENCES target_table_name (pk_col_name);`.
  
  * Note that foreign keys don't receive the `NOT NULL` constraint automatically.
  
  * In database parlance, a _foreign key_ can refer to two different, but related, things:
  
    * A column that represents a relationship between two rows by pointing to a specific row in another table using its _primary key_. A complete name for these columns is _foreign key column_. To create a foreign key _column_, just create a column of the same type as the primary key column it will point to. If the primary key column uses the `integer` data type then the foreign key column must also be of type `integer`.
  
    * A constraint that enforces certain rules about what values are permitted in these foreign key relationships. A complete name for this type of constraint is _foreign key constraint_. To create a foreign key _constraint_, there are two syntaxes that can be used. The first is to add a `REFERENCES` clause to the description of a column in a `CREATE TABLE` statement:
  
      ```sql
      CREATE TABLE orders (
      	id serial PRIMARY KEY,
      	product_id integer REFERENCES products (id),-- this line creates the constraint
      	quantity integer NOT NULL
      );
      ```
  
      The second way is to add the foreign key constraint separately, just as you would any other constraint (note the use of `FOREIGN KEY` instead of `CHECK`):
  
      ```sql
      ALTER TABLE orders 
      	ADD CONSTRAINT orders_product_id_fkey 
      	FOREIGN KEY (product_id) REFERENCES products(id);
      ```
  
* **Natural Key:**

  * A **natural key** is an existing value in a dataset that can be used to uniquely identify each row of data in that dataset. On the surface there appear to be a lot of values that _might_ be satisfactory for this use: a person's phone number, email address, social security number, or a product number.
  * However, in reality most values that _seem_ like they are good candidates for natural keys turn out to not be. A phone number and email address can change hands. A social security number shoudn't change but only some people have them. And products often go through multiple revisions while retaining the same product number.
  * There are a variety of solutions to these problems, including using more than one existing value together as a **composite key**. But instead of solving the problems associated with natural keys, this will often just defer the problem until later without actually addressing it.
  * Luckily for database users everywhere, there is another option--surrogate keys.

* **Surrogate Key:**

  * A **surrogate key** is a value that is created solely for the purpose of identifying a row of data in a database table. Because it is created specifically for that purpose, it can avoid many of the problems associated with natural keys.
  * Perhaps the most common surrogate key in use today is an auto-incrementing integer. This is a value that is added to each row in a table as it is created. With each row, this value increases in order to remain unique in each row.

##### Create and remove CHECK constraints from a column.

* Create a `CHECK` constraint:

  * ```sql
    ALTER TABLE table_name
    ADD CHECK (column_name check_condition);
    
    --e.g.
    
    ALTER TABLE birds
    ADD CHECK (age > 0);
    ```

  * or,

  * ```sql
    ALTER TABLE table_name
    ADD CONSTRAINT constraint_name
    CHECK (column_name constraint_condition);
    
    --e.g.
    
    ALTER TABLE birds
    ADD CONSTRAINT check_age
    CHECK (age > 0);
    ```

* Drop a  `CHECK` constraint:

  * ```sql
    ALTER TABLE table_name
    DROP CONSTRAINT check_constraint_name;
    
    --e.g.
    
    ALTER TABLE birds
    DROP CONSTRAINT birds_age_check;
    ```

##### Create and remove foreign key constraints from a column.

* To add a Foreign Key constraint to an existing table and column:

  ```sql
  ALTER TABLE table_name
  ADD FOREIGN KEY (fk_column_name) REFERENCES other_table (other_table_pk);
  ```

### Database Diagrams

##### Define cardinality and modality

* **cardinality:**
  * The number of objects on each side of the relationship (1:1, 1:M, M:M).
* **modality:** 
  * Indicates whether the relationship is required (1) or optional (0).

##### Be able to draw database diagrams using crow's foot notation.