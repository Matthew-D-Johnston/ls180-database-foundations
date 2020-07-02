##### LS180 Database Foundations > Schema, Data, and SQL

---

### The SQL Language

---

**SQL** is a language used to manipulate the structure and values of datasets stored in a relational database. It is described as a **special purpose language** because it is typically used only for a very specific purpose: interacting with relational databases. This can be contrasted with a general-purpose programming language, such as Ruby, JavaScript, C, or many others, that can and are used for a variety of uses from application creation to scripting and embedding within other languages and runtimes.  

Unlike most of the programming languages you may have worked with, SQL is predominantly a **declarative language**. This means it describes _what_ needs to be done, but does not detail _how_ to accomplish this objective. In practice, this means that the same query might be executed differently on an identical dataset based on a variety of conditions. The SQL server for the most part abstracts these details away from the user, although there are ways to see how a specific query will be executed by the system. This is most commonly utilized when the performance of a query does not meet an application's requirements. For the most part, the SQL database engine selects the most efficient way to execute a query, but in some circumstances a few hints from a human user can improve performance dramatically.  

SQL is really three languages in one, containing smaller sub-languages for data definition, data manipulation, and data control. These are often referred to using acronyms:  

| sub-language                          | controls                       | SQL Constructs                         |
| :------------------------------------ | :----------------------------- | :------------------------------------- |
| **DDL** or data definition language   | relation structure and rules   | `CREATE`, `DROP`, `ALTER`              |
| **DML** or data manipulation language | values stored within relations | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **DCL** or data control language      | who can do what                | `GRANT`                                |

###### DDL: Data Definition Language

* Allows a user to create and modify the schema stored within a database.
* Includes **CREATE TABLE, ALTER TABLE, ADD COLUMN**, and several other statements for creating or modifying the structure or rules that govern the data that is held within a database.

###### DML: Data Manipulation Language

* Allows a user to retrieve or modify the data stored within a database.
* Some databases consider the retrieval and manipulation as two separate languages, but PostgreSQL's documentation combines them.
* Includes **SELECT, INSERT, UPDATE,** and **DELETE**.

###### DCL: Data Control Language

* Tasked with controlling the rights and access roles of the users interacting with a database or table.
* Some users have complete control and access to a database, its schema, and its data. Others may be granted read-only access and can only use SELECT statements.

###### Syntax

* SQL code is made up of **statements**. A SQL statement is terminated by a semicolon.

###### Practice Problems

1. What kind of programming language is SQL?

   Response: SQL is both a special purpose language and a declarative language. It is a special purpose language because it is typically used for a very specific purpose: interacting with relational databases. It is a declarative language because it describes _what_ needs to be done, but does not detail _how_ to accomplish this objective.

2. What are the three sublanguages of SQL?

   Response: The three sublanguages of SQL are: 1) Data Definition Language (DDL); 2) Data Manipulation Language (DML); and 3) Data Control Language (DCL).

3. Write the following values as quoted string values that could be used in a SQL query.

   ```
   canoe
   a long road
   weren't
   "No way!"
   ```

   Response:

   ```
   'canoe'
   'a long road'
   'weren''t'
   '"No way!"'
   ```

4. What operator is used to concatenate strings?

   Response: the double pipe operator, `||`.

5. What function returns a lowercased version of a string? Write a SQL statement using it.

   Response: `lower()`

   ```sql
   SELECT lower('Matt');
    lower
   -------
    matt
   (1 row)
   ```

6. How does the `psql` console display true and false values?

   Response: `t` for true and `f` for false.

7. The surface area of a sphere is calculated using the formula `A = 4π r2`, where `A` is the surface area and `r` is the radius of the sphere.  

   Use SQL to compute the surface area of a sphere with a radius of 26.3cm, truncated to return an integer.  

   Response:

   ```sql
   SELECT round(4*pi()*power(26.3, 2));
   ```

   or, LS solution:

   ```sql
   SELECT trunc(4 * pi() * 26.3 ^ 2);
   ```

---

### SQL Style Guide

---

Key tips from [SQL Style Guide](https://www.sqlstyle.guide/) by Simon Holy well.



#### General

---

##### Do

* Use consistent and descriptive identifiers and names.

* Include comments in SQL code where necessary. Use the C style opening /* and closing */ where possible otherwise precede comments with -- and finish them with a new line.

  ```sql
  SELECT file_hash  -- stored ssdeep hash
    FROM file_system
   WHERE file_name = '.vimrc';
  ```

  ```sql
  /* Updating the file record after writing to the file */
  UPDATE file_system
     SET file_modified_date = '1980-02-22 13:19:01.00000',
         file_size = 209732
   WHERE file_name = '.vimrc';
  ```

##### Avoid

* Descriptive prefixes or Hungarian notation such as `sp_` or `tbl`.
* Plurals--use the more natural collective term where possible instead. For example `staff` instead of `employees` or `people` instead of `individuals`.

---

#### Naming Conventions

---

##### General

* Keep the length to a maximum of 30 bytes--in practice this is 30 characters unless you are using a multi-byte character set.
* Names must begin with a letter and may not end with an underscore.
* Only use letters, numbers and underscroes in names.
* Avoid abbreviations and if you have to use them make sure they are commonly understood.

##### Tables

* Use a collective name or, less ideally, a plural form. For example (in order of preference) `staff` and `employees`.
* Never give a table the same name as one of its columns and vice versa.
* Avoid, where possible, concatenating two table names together to create the name of a relationship table. Rather than `cars_mechanics` prefer `services`.

##### Columns

* Always use the singular name.
* Always use lowercase except where it may make sense not to, such as proper nouns.

##### Aliasing or correlations

* Should relate in some way to the object or expression they are aliasing.

* As a rule of thumb the correlation name should be the first letter of each word in the object's name.

* If there is already a correlation with the same name then append a number.

* Always include the `AS` keyword--makes it easier to read as it is explicit.

* For computed data (`SUM()` or `AVG()`) use the name you would give it were it a column defined in the schema.

  ```sql
  SELECT first_name AS fn
    FROM staff AS s1
    JOIN students AS s2
      ON s2.mentor_id = s1.staff_num;
  ```

  ```sql
  SELECT SUM(s.monitor_tally) AS monitor_total
    FROM staff AS s;
  ```

##### Stored procedures

* The name must contain a verb.

##### Uniform suffixes

The following suffixes have a universal meaning ensuring the columns can be read and understood easily from SQL code. Use the correct suffix where appropriate.

* `_id`--a unique identifier such as a column that is a primary key.
* `_status`--flag value or some other status of any type such as `publication_status`.
* `_total`--the total or sum of a collection of values.
* `_num`--denotes the field contains any kind of number.
* `_name`--signifies a name such as `first_name`.
* `_seq`--contains a contiguous sequence of values.
* `_date`--denotes a column that contains the date of something.
* `_tally`--a count.
* `_size`--the size of something such as a file size or clothing.
* `_addr`--an address for the record could be physical or intangible such as `ip_addr`.

---

#### Query Syntax

---

##### Reserved words

* It is best to avoid the abbreviated keywords and use the full length ones where available (prefer `ABSOLUTE` to `ABS`).

##### White space

To make the code easier to read it is important that the correct complement of spacing is used. Do not crowd code or remove natural language spaces.

###### Spaces

* Spaces should be used to line up the code so that the root keywords all end on the same character boundary. This forms a river down the middle making it easy for the readers eye to scan over the code and separate the keywords from the implementation detail. Rivers are bad in typography, but helpful here.

  ```sql
  (SELECT f.species_name,
          AVG(f.height) AS average_height, AVG(f.diameter) AS average_diameter
     FROM flora AS f
    WHERE f.species_name = 'Banksia'
       OR f.species_name = 'Sheoak'
       OR f.species_name = 'Wattle'
    GROUP BY f.species_name, f.observation_date)
  
    UNION ALL
  
  (SELECT b.species_name,
          AVG(b.height) AS average_height, AVG(b.diameter) AS average_diameter
     FROM botanic_garden_flora AS b
    WHERE b.species_name = 'Banksia'
       OR b.species_name = 'Sheoak'
       OR b.species_name = 'Wattle'
    GROUP BY b.species_name, b.observation_date);
  ```

* Notice that `SELECT`, `FROM`, etc. are all right aligned while the actual column names and implementation-specific details are left aligned.  

* Although not exhaustive, always include spaces:

  * before and after equals (=)
  * after commas (,)
  * surrounding apostrophes (') where not within parentheses or with a trailing comma or semicolon.

  ```sql
  SELECT a.title, a.release_date, a.recording_date
    FROM albums AS a
   WHERE a.title = 'Charcoal Lane'
      OR a.title = 'The New Danger';
  ```

###### Line spacing

* Always include newlines/vertical space:

  * before `AND` or `OR`
  * after semicolons to separate queries for easier reading
  * after each keyword definition
  * after a comma when separating multiple columns into logical groups
  * to separate code into related sections, which helps to ease the readability of large chunks of code.

* Keeping all the keywords aligned to the righthand side and the values left aligned creates a uniform gap down the middle of the query. It also makes it much easier to quickly scan over the query definition.

  ```sql
  INSERT INTO albums (title, release_date, recording_date)
  VALUES ('Charcoal Lane', '1990-01-01 01:01:01.00000', '1990-01-01 01:01:01.00000'),
         ('The New Danger', '2008-01-01 01:01:01.00000', '1990-01-01 01:01:01.00000');
  ```

  ```sql
  UPDATE albums
     SET release_date = '1990-01-01 01:01:01.00000'
   WHERE title = 'The New Danger';
  ```

  ```sql
  SELECT a.title,
         a.release_date, a.recording_date, a.production_date -- grouped dates together
    FROM albums AS a
   WHERE a.title = 'Charcoal Lane'
      OR a.title = 'The New Danger';
  ```

##### Identation

To ensure that SQL is readable it is important that standards of indentation are followed.

###### Joins

* Joins should be indented to the other side of the river and grouped with a new line where necessary.

  ```sql
  SELECT r.last_name
    FROM riders AS r
         INNER JOIN bikes AS b
         ON r.bike_vin_num = b.vin_num
            AND b.engine_tally > 2
  
         INNER JOIN crew AS c
         ON r.crew_chief_last_name = c.last_name
            AND c.chief = 'Y';
  ```

###### Subqueries

* Subqueries should also be aligned to the right side of the river and then laid out using the same style as any other query. Sometimes it will make sense to have the closing parenthesis on a new line at the same character position as its opening partner--this is especially true where you have nested subqueries.

  ```sql
  SELECT r.last_name,
         (SELECT MAX(YEAR(championship_date))
            FROM champions AS c
           WHERE c.last_name = r.last_name
             AND c.confirmed = 'Y') AS last_championship_year
    FROM riders AS r
   WHERE r.last_name IN
         (SELECT c.last_name
            FROM champions AS c
           WHERE YEAR(championship_date) > '2008'
             AND c.confirmed = 'Y');
  ```

##### Preferred formalisms

* Make use of `BETWEEN` where possible instead of combining multiple statements with `AND`.

* Similarly use `IN()` instead of multiple `OR` clauses.

* Where a value needs to be interpreted before leaving the database use the `CASE` expression. `CASE` statements can be nested to form more complex logical structures.

* Avoid the use of `UNION` clauses and temporary tables where possible. If the schema can be optimised to remove the reliance on these features then it most likely should be.

  ```sql
  SELECT CASE postcode
         WHEN 'BN1' THEN 'Brighton'
         WHEN 'EH1' THEN 'Edinburgh'
         END AS city
    FROM office_locations
   WHERE country = 'United Kingdom'
     AND opening_time BETWEEN 8 AND 9
     AND postcode IN ('EH1', 'BN1', 'NN1', 'KW1');
  ```

---

#### Create syntax

---

When declaring schema information it is also important to maintain human-readable code. To facilitate this ensure that the column definitions are ordered and grouped together where it makes sense to do so.  

Indent column definitions by four (4) spaces within the `CREATE` definition.

##### Choosing data types

* Only use `REAL` or `FLOAT` types where it is strictly necessary for floating point mathematics otherwise prefer `NUMERIC` and `DECIMAL` at all times. Floating point rounding errors are a nuisance!

##### Specifying defalut values

* The default value must be the same type as the column--if a column is declared a `DECIMAL` do not provide an `INTEGER` default value.
* Default values must follow the data type declaration and come before any `NOT NULL` statement.

##### Constraints and keys

###### Choosing keys

* Deciding the column(s) that will form the keys in the definition should be a carefully considered activity as it will effect performance and data integrity.
  1. The key should be unique to some degree.
  2. Consistency in terms of data type for the value across the schema and a lower likelihood of this changing in the future.
  3. Can the value be validated against a standard format (such as one published by ISO)? Encouraging conformity to point 2.
  4. Keeping the key as simple as possible whilst not being scared to use compound keys where necessary.

###### Defining constraints

* Once the keys are decided it is possible to define them in the system using constraints along with field value validation.

  **General**

  * Tables must have at least one key to be complete and useful.
  * Constraints should be given a custom name excepting `UNIQUE`, `PRIMARY KEY`, and `FOREIGN KEY` where the database vendor will generally supply sufficiently intelligible names automatically.

  **Layout and order**

  * Specify the primary key first right after the `CREATE TABLE` statement.
  * Constraints should be defined directly beneath the column they correspond to. Indent the constraint so that it aligns to the right of the column name.
  * If it is a multi-column constraint then consider putting it as close to both column definitions as possible and where this is difficult as a last resort include them at the end of the `CREATE TABLE` definition.
  * If it is a table-level constraint that applies to the entire table then it should also appear at the end.
  * Use alphabetical order where `ON DELETE` comes before `ON UPDATE`.
  * If it makes sense to do so, align each aspect of the query on the same character position. For example, all `NOT NULL` definitions could start at the same character position. This is not hard and fast, but it certainly makes the code much easier to scan and read.

  **Validation**

  * Use `LIKE` and `SIMILAR TO` constraints to ensure the integrity of strings where the format is known.
  * Where the ultimate range of a numerical value is known it must be written as a range `CHECK()` to prevent incorrect values entering the database or the silent truncation of data too large to fit the column definition. In the least it should check that the value is greater than 0 in most cases.
  * `CHECK()` constraints should be kept in separate clauses to ease debugging.

  **Example**

  ```sql
  CREATE TABLE staff (
      PRIMARY KEY (staff_num),
      staff_num      INT(5)       NOT NULL,
      first_name     VARCHAR(100) NOT NULL,
      pens_in_drawer INT(2)       NOT NULL,
                     CONSTRAINT pens_in_drawer_range
                     CHECK(pens_in_drawer >= 1 AND pens_in_drawer < 100)
  );
  ```

##### Designs to Avoid

* Object-oriented design principles do not effectively translate to relational database designs--avoid this pitfall.
* Placing the value in one column and the units in another column. The column should make the units self-evident to prevent the requirement to combine columns again later in the application. Use `CHECK()` to ensure valid data is inserted into the column.

---

### PostgreSQL Data Types

---
