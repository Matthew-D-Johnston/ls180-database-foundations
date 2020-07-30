##### LS181 Assessment: Database Foundations > Instructions for Assessment LS181

---

## Study Guide

### SQL

##### Explain the difference between INNER, LEFT OUTER, and RIGHT OUTER joins.

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