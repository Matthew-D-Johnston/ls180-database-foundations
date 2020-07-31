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
* **Data Manipulation Language (DML):** used to retrieve or modify data stored in a database. `SELECT` queries are part of DML.
  * DML statements can be categorized into four different types:
    1. `INSERT` statements: these add new data into a database table.
    2. `SELECT` statements: also referred to as Queries; these retrieve existing data from database tables.
    3. `UPDATE` statements: these update existing data in a database table.
    4. `DELETE` statements: these delete existing data from a database table.
* **Data Control Language (DCL):** used to determine what various users are allowed to do when interacting with a database.

##### Write SQL statements using INSERT, UPDATE, DELETE, CREATE/ALTER/DROP TABLE, ADD/ALTER/DROP COLUMN.

##### Understand how to use GROUP BY, ORDER BY, WHERE, and HAVING.

### PostgreSQL

##### Describe what a sequence is and what they are used for.

##### Create an auto-incrementing column.

* Use datatype `serial`.

##### Define a default value for a column.

* Use the `DEFAULT` keyword followed by the default value.

##### Be able to describe what primary, foreign, natural, and surrogate keys are.

* **Primary Key:** 
  * A unique identifier for a row of data. 
  * Making a column a `PRIMARY KEY` is essentially equivalent to adding `NOT NULL` and `UNIQUE` constraints to that column.
* **Foreign Key:**
  * Allows us to associate a row in one table to a row in another table, which is done by setting a column in one table as a Foreign Key that references another table's Primary Key column.
  * Creating this relationship is done using the `REFERENCES` keyword.
  * With a `CREATE TABLE` statement we can specify a foreign key with the following command: `FOREIGN KEY (fk_column_name) REFERENCES target_table_name (pk_col_name);`.
* **Natural Key:**
* **Surrogate Key:** 

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

* 

### Database Diagrams

##### Define cardinality and modality

* **cardinality:**
* **modality:** 

##### Be able to draw database diagrams using crow's foot notation.