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

* While PostgreSQL (and most database systems) support a large number of data types, we will focus on the following types in this course:

| Data Type                   | Type      | Value                             | Example Values          |
| :-------------------------- | :-------- | :-------------------------------- | :---------------------- |
| `varchar(length)`           | character | up to `length` characters of text | `canoe`                 |
| `text`                      | character | unlimited length of text          | `a long string of text` |
| `integer`                   | numeric   | whole numbers                     | `42`, `-1423290`        |
| `real`                      | numeric   | floating-point numbers            | `24.563`, `-14924.3515` |
| `decimal(precision, scale)` | numeric   | arbitrary precision numbers       | `123.45`, `-567.89`     |
| `timestamp`                 | date/time | date and time                     | `1999-01-08 04:05:06`   |
| `date`                      | date/time | only a date                       | `1999-01-08`            |
| `boolean`                   | boolean   | true or false                     | `true`, `false`         |

###### Practice Problems

1. Describe the difference between the `varchar` and `text` data types.

   ###### My Response:

   Both data types are considered "character" types. But `varchar` allows the user to define a length limit for the character string. `text`, on the other hand, is used for unlimited text lengths (or at least allows for very long character strings).

   ###### LS:

   Both store textual data. `varchar(n)` stores up to `n` characters. Text columns can store an unlimited number of characters.

2. Describe the difference between the `integer`, `decimal`, and `real` data types.

   ###### My Response:

   All three are considered "numeric" types and are thus used for storing numerical data. `integer`, however, is only used for storing whole numbers. Both `decimal` and `real` are used for storing numbers with decimal values. However, `decimal` is specifically used for storing "arbitrary precision numbers", whereas `real` is used for storing "floating-point numbers".

   ###### LS:

   `integer` values are non-fractional numbers. `real` are floating-point numbers that can include fractional values. `decimal` values can contain non-floating point fractional values with a limited precision.

3. What is the largest value that can be stored in an `integer` column?

   ###### My Response:

   2,147,483,647

   ###### LS:

   2147483647

4. Describe the difference between the `timestamp` and `date` data types.

   ###### My Response:

   Both are considered "date/time" types. `timestamp` will store both the date and the time, while `date` stores just the date.

   ###### LS:

   `timestamp` includes a date and a time of day, while `date` only includes the date (and no time).

5. Can a time with a time zone be stored in a column of type `timestamp`?

   ###### My Response:

   I believe so.

   ###### LS:

   No. But there is a `timestamp with time zone` (or `timestamptz`) data type that will store a timestamp with a timezone.

---

### Working with a Single Table

---

1. Write a SQL statement that will create the following table, **people**:

   | name      | age  | occupation |
   | :-------- | :--- | :--------- |
   | Abby      | 34   | biologist  |
   | Mu'nisah  | 26   | NULL       |
   | Mirabelle | 40   | contractor |

   ###### My Response:

   ```sql
   CREATE TABLE people (
   	name varchar(20),
   	age integer,
   	occupation varchar(30)
   );
   ```

   ###### LS Response:

   ```sql
   CREATE TABLE people (name varchar(255), age integer, occupation varchar(255));
   ```

2. Write SQL statements to insert the data shown in #1 into the table.

   ###### My Response:

   ```sql
   INSERT INTO people
   VALUES ('Abby', 34, 'biologist'),
   			 ('Mu''nisah', 26),
   			 ('Mirabelle', 40, 'contractor');
   ```

   ###### LS Response:

   ```sql
   INSERT INTO people (name, age, occupation) VALUES ('Abby', 34, 'biologist');
   INSERT INTO people (name, age) VALUES ('Mu''nisah', 26);
   INSERT INTO people (name, age, occupation) VALUES ('Mirabelle', 40, 'contractor');
   ```

3. Write 3 SQL queries that can be used to retrieve the second row of the table shown in #1 and #2.

   ###### My Response:

   ```sql
   /* First query */
   SELECT * FROM people
   LIMIT 1 OFFSET 1;
   
   /* Second query */
   SELECT * FROM people
   WHERE name = 'Mu''nisah';
   
   /*Third query */
   SELECT * FROM people
   WHERE age = 26;
   ```

   ###### LS Response:

   ```sql
   SELECT * FROM people WHERE name = 'Mu''nisah';
   SELECT * FROM people WHERE age = 26;
   SELECT * FROM people WHERE occupation IS NULL;
   ```

4. Write a SQL statement that will create a table named birds that can hold the following values:

   | name              | length | wingspan | family       | extinct |
   | :---------------- | :----- | :------- | :----------- | :------ |
   | Spotted Towhee    | 21.6   | 26.7     | Emberizidae  | f       |
   | American Robin    | 25.5   | 36.0     | Turdidae     | f       |
   | Greater Koa Finch | 19.0   | 24.0     | Fringillidae | t       |
   | Carolina Parakeet | 33.0   | 55.8     | Psittacidae  | t       |
   | Common Kestrel    | 35.5   | 73.5     | Falconidae   | f       |

   ###### My Response:

   ```sql
   CREATE TABLE birds (
   name varchar(255),
   length decimal(3,1),
   wingspan decimal(3,1),
   family varchar(255),
   extinct boolean
   );
   ```

   ###### LS Response:

   ```sql
   CREATE TABLE birds (
   name character varying(255),
   length numeric(4,1),
   wingspan numeric(4,1),
   family text,
   extinct boolean
   );
   ```

5. Using the table created in #4, write the SQL statements to insert the data as shown in the listing.

   ###### My Response:

   ```sql
   INSERT INTO birds
   VALUES ('Spotted Towhee', 21.6, 26.7, 'Emberizidae', false),
   ('American Robin', 25.5, 36.0, 'Turdidae', false),
   ('Greater Koa Finch', 19.0, 24.0, 'Fringillidae', true),
   ('Carolina Parakeet', 33.0, 55.8, 'Psittacidae', true),
   ('Common Kestrel', 35.5, 73.5, 'Falconidae', true);
   ```

   ###### LS Response:

   ```sql
   INSERT INTO birds VALUES ('Spotted Towhee', 21.6, 26.7, 'Emberizidae', false);
   INSERT INTO birds VALUES ('American Robin', 25.5, 36.0, 'Turdidae', false);
   INSERT INTO birds VALUES ('Greater Koa Finch', 19.0, 24.0, 'Fringillidae', true);
   INSERT INTO birds VALUES ('Carolina Parakeet', 33.0, 55.8, 'Psittacidae', true);
   INSERT INTO birds VALUES ('Common Kestrel', 35.5, 73.5, 'Falconidae', false);
   ```

6. Write a SQL statement that finds the names and families for all birds that are not extinct, in order from longest to shortest (based on the `length` column's value).

   ###### My Response:

   ```sql
   SELECT name, family FROM birds
   WHERE extinct = false
   ORDER BY length DESC;
   ```

   ###### LS Response:

   ```sql
   SELECT name, family FROM birds WHERE extinct=false ORDER BY length DESC;
   ```

7. Use SQL to determine the average, minimum, and maximum wingspan for the birds shown in the table.

   ###### My Response:

   ```sql
   SELECT avg(wingspan) FROM birds;
   SELECT min(wingspan) FROM birds;
   SELECT max(wingspan) FROM birds;
   
   -- or
   
   SELECT round(avg(wingspan)), min(wingspan), max(wingspan) FROM birds;
   ```

   ###### LS Response:

   ```sql
   SELECT round(avg(wingspan), 1), min(wingspan), max(wingspan) FROM birds;
   ```

8. Write a SQL statement to create the table shown below, **menu_items**:

   | item     | prep_time | ingredient_cost | sales | menu_price |
   | :------- | :-------- | :-------------- | :---- | :--------- |
   | omelette | 10        | 1.50            | 182   | 7.99       |
   | tacos    | 5         | 2.00            | 254   | 8.99       |
   | oatmeal  | 1         | 0.50            | 79    | 5.99       |

   ###### My Response:

   ```sql
   CREATE TABLE menu_items (
   	item varchar(255),
   	prep_time integer,
   	ingredient_cost decimal(4,2),
   	sales integer,
   	menu_price decimal(4,2)
   );
   ```

   ###### LS Response:

   ```sql
   CREATE TABLE menu_items (
   	item text,
   	prep_time integer,
   	ingredient_cost numeric(4,2),
   	sales integer,
   	menu_price numeric(4,2)
   );
   ```

9. Write SQL statements to insert the data shown in #8 into the table.

   ###### My Response:

   ```sql
   INSERT INTO menu_items
   VALUES ('omelette', 10, 1.50, 182, 7.99),
   ('tacos', 5, 2.00, 254, 8.99),
   ('oatmeal', 1, 0.50, 79, 5.99);
   ```

   ###### LS Response:

   ```sql
   INSERT INTO menu_items VALUES ('omelette', 10, 1.50, 182, 7.99);
   INSERT INTO menu_items VALUES ('tacos', 5, 2.00, 254, 8.99);
   INSERT INTO menu_items VALUES ('oatmeal', 1, 0.50, 79, 5.99);
   ```

10. Using the table and data from #8 and #9, write a SQL query to determine which menu item is the most profitable based on the cost of its ingredients, returning the name of the item and its profit.

    ###### My Response:

    ```sql
    SELECT item, max((menu_price - ingredient_cost) * sales) AS profit 
    FROM menu_items
    GROUP BY item
    LIMIT 1;
    ```

    ###### LS Response:

    ```sql
    SELECT item, menu_price - ingredient_cost AS profit FROM menu_items ORDER BY profit DESC LIMIT 1;
    ```

    

11. Write a SQL query to determine how profitable each item on the menu is based on the amount of time it takes to prepare. Assume that whoever is preparing the food is being paid $13 an hour. List the most profitable items first. Keep in mind that `prep_time` is represented in minutes and `ingredient_cost` and `menu_price` are in dollars and cents).

    ###### My Response:

    ```sql
    SELECT item, round(menu_price - (ingredient_cost + (prep_time * (13.0/60))), 2)
    		AS profit
    	FROM menu_items
     ORDER BY profit DESC;
    ```

    ###### LS Response:

    ```sql
    SELECT item, menu_price, ingredient_cost,
    			 round(prep_time/60.0 * 13.0, 2) AS labor,
    			 menu_price - ingredient_cost - round(prep_time/60.0 * 13.0, 2) AS profit
    	FROM menu_items
     ORDER BY profit DESC;
    ```

---

### Loading Database Dumps

---

* There are two ways to load SQL files into a PostgreSQL database.

#### psql

* The first option is to pipe the SQL file into the `psql` program, using redirection on the command line to stream the SQL file into `psql`'s standard input:

  ```
  $ psql -d my_database < file_to_imprt.sql
  ```

* This will execute the SQL statements within `file_to_import.sql` within the `my_database` database.

* If you already have a running `psql` session, though, you can import a SQL file using the `\i` meta command:

  ```sql
  my_database=# \i ~/some/files/file_to_import.sql
  ```

* This will have the same effect as the first command, but does not require exiting a running `psql` session.

#### Practice Problems

1. Load [this file](https://raw.githubusercontent.com/launchschool/sql_course_data/master/sql-and-relational-databases/schema-data-and-sql/loading-database-dumps/films1.sql) into your database.

   ```sql
   sql-course=# \i films1.sql
   ```

   1. What does the file do?

      The file `DROP`s the `public.films` table if it exists. It then `CREATE`s a `films` table with three columns: 1) `title` of varying length up to 255 characters; 2) `"year"` of type integer; and 3) `genre` of varying length up to 100 characters. It then inserts three separate rows of data.

   2. What is the output of the command? What does each line in this output mean?

      Upon executing the load command, the output that is returned is:

      ```
      psql:films1.sql:1: NOTICE:  table "films" does not exist, skipping
      DROP TABLE
      CREATE TABLE
      INSERT 0 1
      INSERT 0 1
      INSERT 0 1
      ```

   3. Open up the file and look at its contents. What does the first line do?

      Taking a look at the file's contents, we see that the first line uses the `DROP TABLE` command to delete the `public.films` table, but only if it exists.

2. Write a SQL statement that returns all rows in the **films** table.

   ###### My Response:

   ```sql
   SELECT * FROM films;
   ```

3. Write a SQL statement that returns all rows in the **films** table with a title shorter than 12 letters.

   ```sql
   SELECT * FROM films
   WHERE length(title) < 12;
   ```

4. Write the SQL statements needed to add two additional columns to the **films** table: `director`, which will hold a director's full name, and `duration`, which will hold the length of the film in minutes.

   ###### My Response:

   ```sql
   ALTER TABLE films
   ADD COLUMN director varchar(255)
   ADD COLUMN duration integer;
   ```

   ###### LS Response:

   ```sql
   ALTER TABLE films ADD COLUMN director varchar(255);
   ALTER TABLE films ADD COLUMN duration integer;
   ```

5. Write SQL statements to update the existing rows in the database with values for the new columns:

   | title            | director             | duration |
   | :--------------- | :------------------- | -------: |
   | Die Hard         | John McTiernan       |      132 |
   | Casablanca       | Michael Curtiz       |      102 |
   | The Conversation | Francis Ford Coppola |      113 |

   ```sql
   UPDATE films
   	 SET director = 'John McTiernan',
   	 		 duration = 132
    WHERE title = 'Die Hard';
   
   UPDATE films
   	 SET director = 'Michael Curtiz',
   	 		 duration = 102
    WHERE title = 'Casablanca';
   
   UPDATE films
   	 SET director = 'Francis Ford Coppola',
   	 		 duration = 113
    WHERE title = 'The Conversation';
   ```

6. Write SQL statements to insert the following data into the **films** table:

   ###### My Response:

   ```sql
   INSERT INTO films
   VALUES ('1984', 1956, 'scifi', 'Michael Anderson', 90),
   ('Tinker Tailor Soldier Spy', 2011, 'espionage', 'Tomas Alfredson', 127),
   ('The Birdcage', 1996, 'comedy', 'Mike Nichols', 118);
   ```

7. Write a SQL statement that will return the title and age in years of each movie, with newest movies listed first:

   ###### My Response:

   ```sql
   SELECT title, 2020 - year AS age FROM films
    ORDER BY age ASC;
   ```

   ###### LS Response:

   ```sql
   SELECT title, extract("year" from current_date) - "year" AS age
   	FROM films
    ORDER BY age ASC;
   ```

8. Write a SQL statement that will return the title and duration of each movie longer than two hours, with the longest movies first.

   ###### My Response:

   ```sql
   SELECT title, duration
   	FROM films
    WHERE duration > 120
    ORDER BY duration DESC;
   ```

9. Write a SQL statement that returns the title of the longest film.

   ###### My Response:

   ```sql
   SELECT title AS longest_title
   	FROM films
    ORDER BY duration DESC
    LIMIT 1;
   ```

---

### More Single Table Queries

---

1. Create a new database called `residents` using the PostgreSQL command line tools.

   ###### My Response:

   ```
   $ createdb residents
   ```

2. Load [this file](https://raw.githubusercontent.com/launchschool/sql_course_data/master/sql-and-relational-databases/schema-data-and-sql/more-single-table-queries/residents_with_data.sql) into the database created in #1.

   ```
   psql -d residents < residents_with_data.sql
   ```

3. Write a SQL query to list the ten states with the most rows in the `people` table in descending order.

   ###### My Response:

   ```sql
   SELECT state, count(state)
   	FROM people
    GROUP BY state
    ORDER BY count(state) DESC
    LIMIT 10;
   ```

   ###### LS Response:

   ```sql
   SELECT state, COUNT(id) FROM people GROUP BY state ORDER BY count DESC LIMIT 10;
   ```

4. Write a SQL query that lists each domain used in an email address in the `people` table and how many people in the database have an email address containing that domain. Domains should be listed with the most popular first.

   ###### My Response:

   ```sql
   SELECT substring(email, '@(.{0,})') AS domain, count(id) 
   	FROM people
    GROUP BY domain
    ORDER BY count DESC;
   ```

   ###### LS Response:

   ```sql
   SELECT substr(email, strpos(email, '@') + 1) as domain, COUNT(id) FROM people GROUP BY domain ORDER BY count DESC;
   ```

5. Write a SQL statement that will delete the person with ID `3399` from the `people` table.

   ###### My Response:

   ```sql
   DELETE FROM people
    WHERE id = 3399;
   ```

   ###### LS Response:

   ```sql
   DELETE FROM people WHERE id = 3399;
   ```

6. Write a SQL statement that will delete all users that are located in the state of California (`CA`).

   ###### My Response:

   ```sql
   DELETE FROM people WHERE state = 'CA';
   ```

   ###### LS Response:

   ```sql
   DELETE FROM people WHERE state = 'CA';
   ```

7. Write a SQL statement that will update the `given_name` values to be all uppercase for all users with an email address that contains `teleworm.us`.

   ###### My Response:

   ```sql
   UPDATE people SET given_name = upper(given_name)
   WHERE email LIKE '%teleworm.us';
   ```

   ###### LS Response:

   ```sql
   UPDATE people SET given_name = UPPER(given_name) WHERE email LIKE '%teleworm.us';
   ```

8. Write a SQL statement that will delete all rows from the `people` table.

   ###### My Response:

   ```sql
   DELETE FROM people;
   ```

---

### NOT NULL and Default Values

---

* The design of relational databases, and truthfully their power, comes from the work involved in defining a common set of attributes (the values of which are stored in columns). It follows that the more specific and exact the design of the schema is, the "neater" and more consistent the data will be. Relational databases allow a schema designer to build a variety of rules about the data into the system. These rules allow users of the system to make certain assumptions about the data that lead to simpler solutions.
* The effect of using any operator on a NULL value returns NULL. This makes it impossible to compare a NULL value with any other value, which is normally how a set of values would be sorted.

#### Practice Problems

1. What is the result of using an operator on a NULL value?

   ###### My Response:

   NULL values are unknown values, and the result of an operator on a NULL value is also unknown and often unexpected.

   ###### LS Response:

   The resulting value will also be NULL, which signifies an unknown value.

2. Set the default value of column `department` to "unassigned". Then set the value of the `department` column to "unassigned" for any rows where it has a NULL value. Finally, add a NOT NULL constraint to the `department` column.

   ###### My Response:

   ```sql
   ALTER TABLE employees
   ALTER COLUMN department
   	SET DEFAULT 'unassigned';
   	
   UPDATE employees
   	 SET department = 'unassigned'
    WHERE department IS NULL;
   
   ALTER TABLE employees
   ALTER COLUMN department
   	SET NOT NULL;
   ```

3. Write the SQL statement to create a table called **temperatures** to hold the following data:

   ```
       date    | low | high
   ------------+-----+------
    2016-03-01 | 34  | 43
    2016-03-02 | 32  | 44
    2016-03-03 | 31  | 47
    2016-03-04 | 33  | 42
    2016-03-05 | 39  | 46
    2016-03-06 | 32  | 43
    2016-03-07 | 29  | 32
    2016-03-08 | 23  | 31
    2016-03-09 | 17  | 28
   ```

   Keep in mind that all rows in the table should always contain all three values.

   ###### My Response:

   ```sql
   CREATE TABLE temperatures (
   	"date" date NOT NULL,
   	low integer NOT NULL,
   	high integer NOT NULL
   );
   ```

4. Write the SQL statements needed to insert the data shown in #3 into the **temperatures** table.

   ###### My Response:

   ```sql
   INSERT INTO temperatures
   VALUES ('2016-03-01', 34, 43),
   ('2016-03-02', 32, 44),
   ('2016-03-03', 31, 47),
   ('2016-03-04', 33, 42),
   ('2016-03-05', 39, 46),
   ('2016-03-06', 32, 43),
   ('2016-03-07', 29, 32),
   ('2016-03-08', 23, 31),
   ('2016-03-09', 17, 28);
   ```

5. Write a SQL statement to determine the average (mean) temperature -- divide the sum of the high and low temperatures by two) for each day from March 2, 2016 through March 8, 2016. Make sure to round each average value to one decimal place.

   ###### My Response:

   ```sql
   SELECT date, round((low + high)::numeric/2, 1) AS average FROM temperatures
   WHERE date >= '2016-03-02' AND date <= '2016-03-08';
   ```

   ###### LS Response:

   ```sql
   SELECT date, ROUND((high + low) / 2.0, 1) as average
     FROM temperatures
    WHERE date BETWEEN '2016-03-02' AND '2016-03-08';
   ```

6. Write a SQL statement to add a new column, `rainfall`, to the **temperatures** table. It should store millimetres of rain as integers and have a default value of `0`.

   ###### My Response:

   ```sql
   ALTER TABLE temperatures
   	ADD COLUMN rainfall integer DEFAULT 0;
   ```

7. Each day, it rains one millimeter for every degree the average temperature goes above 35. Write a SQL statement to update the data in the table **temperatures** to reflect this.

   ###### My Response:

   ```sql
   UPDATE temperatures
   	 SET rainfall = ((high + low)/2) - 35
    WHERE (high + low)/2 > 35;
   ```

   ###### LS Response:

   ```sql
   UPDATE temperatures
    	 SET rainfall = (high + low) / 2 - 35
    WHERE (high + low) / 2 > 35;
   ```

8. A decision has been made to store rainfall data in inches. Write the SQL statements required to modify the `rainfall` column to reflect these new requirements.

   ###### My Response:

   ```sql
   UPDATE temperatures
    	 SET rainfall = (rainfall/25.4)::decimal(6,3);
   ```

   This response doesn't work because `rainfall` has a data type of integer. Thus, all conversions convert to `0`.

   ###### LS Response:

   ```sql
   ALTER TABLE temperatures ALTER COLUMN rainfall TYPE numeric(6,3);
   UPDATE temperatures SET rainfall = rainfall * 0.039;
   ```

9. Write a SQL statement that renames the **temperatures** table to **weather**.

   ###### My Response:

   ```sql
   ALTER TABLE temperatures RENAME TO weather;
   ```

10. What `psql` meta command shows the structure of a table? Use it to describe the structure of **weather**.

    ###### My Response:

    ```
    \d weather
    ```

11. What PostgreSQL program can be used to create a SQL file that contains the SQL commands needed to recreate the current structure and data of the **weather** table?

    ###### My Response:

    Sadly, I do not know.

    ###### LS Response:

    ```
    $ pg_dump -d sql-course -t weather --inserts > dump.sql
    ```

    The resulting `dump.sql` file will contain:

    ```sql
    --
    -- PostgreSQL database dump
    --
    
    SET statement_timeout = 0;
    SET lock_timeout = 0;
    SET client_encoding = 'UTF8';
    SET standard_conforming_strings = on;
    SET check_function_bodies = false;
    SET client_min_messages = warning;
    
    SET search_path = public, pg_catalog;
    
    SET default_tablespace = '';
    
    SET default_with_oids = false;
    
    --
    -- Name: weather; Type: TABLE; Schema: public; Owner: instructor; Tablespace:
    --
    
    CREATE TABLE weather (
        date date NOT NULL,
        low integer NOT NULL,
        high integer NOT NULL,
        rainfall numeric(6,3) DEFAULT 0
    );
    
    ALTER TABLE weather OWNER TO instructor;
    
    --
    -- Data for Name: weather; Type: TABLE DATA; Schema: public; Owner: instructor
    --
    
    INSERT INTO weather VALUES ('2016-03-07', 29, 32, 0.000);
    INSERT INTO weather VALUES ('2016-03-08', 23, 31, 0.000);
    INSERT INTO weather VALUES ('2016-03-09', 17, 28, 0.000);
    INSERT INTO weather VALUES ('2016-03-01', 34, 43, 0.117);
    INSERT INTO weather VALUES ('2016-03-02', 32, 44, 0.117);
    INSERT INTO weather VALUES ('2016-03-03', 31, 47, 0.156);
    INSERT INTO weather VALUES ('2016-03-04', 33, 42, 0.078);
    INSERT INTO weather VALUES ('2016-03-05', 39, 46, 0.273);
    INSERT INTO weather VALUES ('2016-03-06', 32, 43, 0.078);
    
    --
    -- PostgreSQL database dump complete
    --
    ```

    If you leave off the `--inserts` argument to `pg_dump`, the resulting SQL statements will still restore the table and its data. The format will just be slightly different, using a `COPY FROM stdin` statement instead of multiple `INSERT` statements:

    ```sql
    COPY weather (date, low, high, rainfall) FROM stdin;
    2016-03-07	29	32	0.000
    2016-03-08	23	31	0.000
    2016-03-09	17	28	0.000
    2016-03-01	34	43	0.117
    2016-03-02	32	44	0.117
    2016-03-03	31	47	0.156
    2016-03-04	33	42	0.078
    2016-03-05	39	46	0.273
    2016-03-06	32	43	0.078
    \.
    ```

    `COPY FROM` is the default for a reason (it is more efficient on large data sets), but using `INSERTS` is also valid and creates SQL statements that are typically the same as those that a user of the database might write themselves when adding data to the table manually.

---

### More Constraints

---

1. Import [this file](https://raw.githubusercontent.com/launchschool/sql_course_data/master/sql-and-relational-databases/schema-data-and-sql/more-constraints/films2.sql) into a PostgreSQL database.

   ###### My Response:

   ```
   $ psql -d sql-course < films2.sql
   ```

2. Modify all of the columns to be `NOT NULL`.

   ###### My Response:

   ```sql
   ALTER TABLE films
   ALTER COLUMN
   title, year, genre, director, duration
   SET NOT NULL;
   ```

   Doesn't seem to work with multiple columns.

   ###### LS Response:

   ```sql
   ALTER TABLE films ALTER COLUMN title SET NOT NULL;
   ALTER TABLE films ALTER COLUMN year SET NOT NULL;
   ALTER TABLE films ALTER COLUMN genre SET NOT NULL;
   ALTER TABLE films ALTER COLUMN director SET NOT NULL;
   ALTER TABLE films ALTER COLUMN duration SET NOT NULL;
   ```

3. How does modifying a column to be `NOT NULL` affect how it is displayed by `\d films`?

   ###### My Response:

   When invoking the `\d` command on the `films` table the table's schema is displayed on the screen, and under the `Nullable` column `not null` should be displayed for each of the `films` table's columns that we specified to be `NOT NULL`.

   ```
   sql-course=# \d films
                           Table "public.films"
     Column  |          Type          | Collation | Nullable | Default 
   ----------+------------------------+-----------+----------+---------
    title    | character varying(255) |           | not null | 
    year     | integer                |           | not null | 
    genre    | character varying(100) |           | not null | 
    director | character varying(255) |           | not null | 
    duration | integer                |           | not null | 
   ```

   ###### LS Response:

   `not null` will be included in that column's `Modifiers` column:

   ```
   sql-course=# \d films
                Table "public.films"
     Column  |          Type          | Modifiers
   ----------+------------------------+-----------
    title    | character varying(255) | not null
    year     | integer                | not null
    genre    | character varying(100) | not null
    director | character varying(255) | not null
    duration | integer 
   ```

   Obviously, LS has a slightly different interface than what I am working with on my machine.

4. Add a constraint to the table **films** that ensures that all films have a unique title.

   ###### My Response:

   ```sql
   ALTER TABLE films
   ALTER COLUMN title
     SET UNIQUE;
   ```

   ###### LS Response:

   ```sql
   ALTER TABLE films ADD CONSTRAINT title_unique UNIQUE (title);
   ```

5. How is the constraint added in #4 displayed by `\d films`?

   ###### My Response:

   It is displayed at the bottom of the schema table in a short statement:

   ```
   "title_unique" UNIQUE CONSTRAINT, btree (title)
   ```

   ###### LS Response:

   It appears as an index:

   ```
   Indexes:
       "title_unique" UNIQUE CONSTRAINT, btree (title)
   ```

6. Write a SQL statement to remove the constraint added in #4.

   ###### My Response:

   ```sql
   ALTER TABLE films DROP CONSTRAINT title_unique;
   ```

   ###### LS Response:

   Same.

7. Add a constraint to **films** that requires all rows to have a value for `title` that is at least 1 character long.

   ###### My Response:

   ```sql
   ALTER TABLE films ADD CHECK length(title) >= 1;
   ```

   ###### LS Response:

   ```sql
   ALTER TABLE films ADD CONSTRAINT title_length CHECK (length(title) >= 1);
   ```

8. What error is shown if the constraint created in #7 is violated? Write a SQL `INSERT` statement that demonstrates this.

   ###### My Response:

   An error indicating that the check constraint `"title_length"` has been violated:

   ```
   sql-course=# INSERT INTO films VALUES ('', 1999, 'comedy', 'Wes Anderson', 115);
   ERROR:  new row for relation "films" violates check constraint "title_length"
   DETAIL:  Failing row contains (, 1999, comedy, Wes Anderson, 115).
   ```

   Here is the `INSERT` statement that I wrote

   ```sql
   INSERT INTO films VALUES ('', 1999, 'comedy', 'Wes Anderson', 115);
   ```

   ###### LS Response:

   ```sql
   sql-course=# INSERT INTO films VALUES ('', 1901, 'action', 'JJ Abrams', 126);
   ERROR:  new row for relation "films" violates check constraint "title_length"
   DETAIL:  Failing row contains (, 1901, action, JJ Abrams, 126).
   ```

9. How is the constraint added in #7 displayed by `\d films`?

   ###### My Response:

   As a check constraint:

   ```
   Check constraints:
       "title_length" CHECK (length(title::text) >= 1)
   ```

   ###### LS Response:

   It appears as a "check constraint":

   ```
   Check constraints:
       "title_length" CHECK (length(title::text) >= 1)
   ```

10. Write a SQL statement to remove the constraint added in #7.

    ###### My Response:

    ```sql
    ALTER TABLE films DROP CONSTRAINT title_length;
    ```

    ###### LS Response:

    Same.

11. Add a constraint to the table **films** that ensures that all films have a year between 1900 and 2100.

    ###### My Response:

    ```sql
    ALTER TABLE films ADD CONSTRAINT year_range CHECK year BETWEEN 1900 AND 2100;
    ```

    ###### LS Response:

    ```sql
    ALTER TABLE films ADD CONSTRAINT year_range CHECK (year BETWEEN 1900 AND 2100);
    ```

12. How is the constraint added in #11 displayed by `\d films`?

    ###### My Response:

    It appears as a "check constraint":

    ```
    Check constraints:
        "year_range" CHECK (year >= 1900 AND year <= 2100)
    ```

    ###### LS Response:

    Same.

13. Add a constraint to **films** that requires all rows to have a value for `director` that is at least 3 characters long and contains at least one space character (` `).

    ###### My Response:

    ```sql
    ALTER TABLE films ADD CONSTRAINT director_field CHECK (length(director) >= 3 AND director LIKE '% %');
    ```

    ###### LS Response:

    ```sql
    ALTER TABLE films ADD CONSTRAINT director_name
    		CHECK (length(director) >= 3 AND position(' ' in director) > 0);
    ```

14. How does the constraint created in #13 appear in `\d films`?

    ###### My Response:

    It appears as a "check constraint":

    ```
    Check constraints:
        "director_name" CHECK (length(director::text) >= 3 AND "position"(director::text, ' '::text) > 0)
    ```

    ###### LS Response:

    Same.

15. Write an `UPDATE` statement that attempts to change the director for "Die Hard" to "Johnny". Show the error that occurs when this statement is executed.

    ###### My Response:

    ```sql
    UPDATE films SET director = 'Johnny' WHERE title = 'Die Hard';
    
    ERROR:  new row for relation "films" violates check constraint "director_name"
    DETAIL:  Failing row contains (Die Hard, 1988, action, Johnny, 132).
    ```

    ###### LS Response:

    ```sql
    sql-course=# UPDATE films SET director = 'Johnny' WHERE title = 'Die Hard';
    ERROR:  new row for relation "films" violates check constraint "director_name"
    DETAIL:  Failing row contains (Die Hard, 1988, action, Johnny, 132).
    ```

16. List three ways to use the schema to restrict what values can be stored in a column.

    ###### My Response:

    1. Through a column constraint.
    2. Through a table constraint.
    3. Through a check constraint.

    ###### LS Response:

    1. Data type (which can include a length limitation)
    2. NOT NULL Constraint
    3. Check Constraint

17. Is it possible to define a default value for a column that will be considered invalid by a constraint? Create a table that tests this.

    ###### My Response:

    Yes, it is possible to define a default value for a column that will be considered invalid by a constraint; however, any time that default value is attempted to be set an error is raised indicating that the constraint has been violated.

18. How can you see a list of all the constraints on a table?

    ###### My Response:

    You can see a list of all the constraints on a table by using the `\d` command.

    ###### LS Response:

    Using `\d $tablename`, where `$tablename` is the name of the table.

---

### Using Keys

---

* It is entirely possible to have identical rows of data that represent different real-world entities appear in the same table. So how do we distinguish these identical rows?

#### Solving the Problem: Keys

* The solution to this problem is to stop using values within the data to identify rows. Or at least, stop using values that have not been carefully selected to be unique across the entire dataset.
* SQL databases provide something called keys that solve this problem. A **key** uniquely identifies a single row in a database table. There are two types of keys that we'll cover in this course:
  * Natural keys
  * Surrogate keys

#### Natural Keys

* A **natural key** is an existing value in a dataset that can be used to uniquely identify each row of data in that dataset. On the surface there appear to be a lot of values that _might_ be satisfactory for this use: a person's phone number, email address, social security number, or a product number.
* However, in reality most values that _seem_ like they are good candidates for natural keys turn out to not be. A phone number and email address can change hands. A social security number shouldn't change but only some people have them. And products often go through multiple revisions while retaining the same product number.
* There are a variety of solutions to these problems, including using more than one existing value together as a **composite key**. But instead of solving the problems associated with natural keys, this will often just defer the problem until later without actually addressing it.
* Luckily for database users everywhere, there is another option.

#### Surrogate Keys

* A **surrogate key** is a value that is created solely for the purpose of identifying a row of data in a database table. Because it is created specifically for that purpose, it can avoid many of the problems associated with natural keys.

* Perhaps the most common surrogate key in use today is an auto-incrementing integer. This is a value that is added to each row in a table as it is created. With each row, this value increases in order to remain unique in each row.

* Let's create a table with just such a column in PostgreSQL:

  ```sql
  CREATE TABLE colors (id serial, name text);
  ```

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

* A **sequence** is a special kind of relation that generates a series of numbers. A sequence will remember the last number it generated, so it will generate numbers in a predetermined sequence automatically.

#### Primary Keys

* Following conventions in software development saves time, reduces confusion, and minimizes the amount of time needed to get up to speed on a new project. Different teams and software packages may have varying conventions, but contemporary database development within the Ruby, JavaScript, and other communities has settled on the following conventions for working with tables and primary keys:
  1. All tables should have a primary key column called `id`.
  2. The `id` column should automatically be set to a unique value as new rows are inserted into the table.
  3. The `id` column will often be an integer, but there are other data types (such as UUIDs) that can provide specific benefits.
* Do you have to declare a column as a `PRIMARY KEY` in every table? Technically, no. But doing so is generally a good idea.

#### Practice Problems

1. Write a SQL statement that makes a new sequence called "counter".

   ###### My Response:

   ```sql
   CREATE SEQUENCE counter;
   ```

   ###### LS Response:

   Same.

2. Write a SQL statement to retrieve the next value from the sequence created in #1.

   ###### My Response:

   ```sql
   SELECT nextval('counter');
   ```

   ###### LS Response:

   Same.

3. Write a SQL statement that removes a sequence called "counter".

   ###### My Response:

   ```sql
   DROP SEQUENCE counter;
   ```

   ###### LS Response:

   Same.

   

4. Is it possible to create a sequence that returns only even numbers? [The documentation for sequence might be useful](https://www.postgresql.org/docs/9.5/sql-createsequence.html).

   ###### My Response:

   Yes. For example:

   ```sql
   CREATE SEQUENCE counter INCREMENT BY 2 START WITH 2;
   ```

   ###### LS Response:

   ```sql
   sql-course=# CREATE SEQUENCE even_counter INCREMENT BY 2 MINVALUE 2;
   CREATE SEQUENCE
   sql-course=# SELECT nextval('even_counter');
    nextval
   ---------
          2
   (1 row)
   
   sql-course=# SELECT nextval('even_counter');
    nextval
   ---------
          4
   (1 row)
   ```

5. What will the name of the sequence created by the following SQL statement be?

   ```sql
   CREATE TABLE regions (id serial PRIMARY KEY, name text, area integer);
   ```

   ###### My Response:

   ```
   regions_id_seq
   ```

   ###### LS Response:

   `regions_id_seq`.

6. Write a SQL statement to add an auto-incrementing integer primary key column to the `films` table.

   ###### My Response:

   ```sql
   ALTER TABLE films ADD COLUMN id serial PRIMARY KEY;
   ```

   ###### LS Response:

   Same.

7. What error do you receive if you attempt to update a row to have a value for `id` that is used by another row?

   ###### My Response:

   ```
   ERROR:  duplicate key value violates unique constraint "films_pkey"
   DETAIL:  Key (id)=(8) already exists.
   ```

   ###### LS Response:

   ```sql
   films=# UPDATE films SET id = 3 WHERE title = 'Die Hard';
   ERROR: duplicate key value violates unique constraint "films_pkey"
   DETAIL: Key (id)=(3) already exists.
   ```

8. What error do you receive if you attempt to add another primary key column to the `films` table?

   ###### My Response:

   ```
   ERROR:  multiple primary keys for table "films" are not allowed
   ```

   ###### LS Response:

   ```sql
   films=# ALTER TABLE films ADD COLUMN another_id serial PRIMARY KEY;
   ERROR: multiple primary keys for table "films" are not allowed
   ```

9. Write a SQL statement that modifies the table `films` to remove its primary key while preserving the `id` column and the values it contains.

   ###### My Response:

   ```sql
   ALTER TABLE films DROP CONSTRAINT films_pkey;
   ```

   ###### LS Response:

   Same.

----

### GROUP BY and Aggregate Functions

---

1. Import [this file](https://raw.githubusercontent.com/launchschool/sql_course_data/master/sql-and-relational-databases/schema-data-and-sql/group-by-and-aggregate-functions/films4.sql) into a database.

   ###### My Response:

   ```
   $ psql -d sql-course < films4.sql
   ```

   ###### LS Response:

   ```
   $ psql -d films < films4.sql
   ```

2. Write SQL statements that will insert the following films into the database:

   | title           | year | genre     | director          | duration |
   | :-------------- | :--- | :-------- | :---------------- | :------- |
   | Wayne's World   | 1992 | comedy    | Penelope Spheeris | 95       |
   | Bourne Identity | 2002 | espionage | Doug Liman        | 118      |

   ###### My Response:

   ```sql
   INSERT INTO films VALUES (DEFAULT, 'Wayne''s World', 1992, 'comedy', 'Penelope Spheeris', 95);
   
   INSERT INTO films VALUES (DEFAULT, 'Bourne Identity', 2002, 'espionage', 'Doug Liman', 118)
   ```

   ###### LS Response:

   ```sql
   INSERT INTO films (title, year, genre, director, duration)
   	VALUES ('The Bourne Identity', 2002, 'espionage', 'Doug Liman', 118);
   INSERT INTO films (title, year, genre, director, duration)
   	VALUES ('Wayne''s World', 1992, 'comedy', 'Penelope Spheeris', 95);
   ```

3. Write a SQL query that lists all genres for which there is a movie in the `films` table.

   ###### My Response:

   ```sql
   SELECT genre FROM films GROUP BY genre;
   ```

   ###### LS Response:

   ```
   SELECT DISTINCT genre FROM films;
   ```

4. Write a SQL querty that returns the same results as the answer for #3, but without using `DISTINCT`.

   ###### My Response:

   ```sql
   SELECT genre FROM films GROUP BY genre;
   ```

   ###### LS Response:

   Same.

5. Write a SQL query that determines the average duration across all the movies in the `films` table, rounded to the nearest minute.

   ###### My Response:

   ```sql
   SELECT round(avg(duration)) AS average_duration FROM films;
   ```

   ###### LS Response:

   ```sql
   SELECT ROUND(AVG(duration)) FROM films;
   ```

6. Write a SQL query that determines the average duration for each genre in the `films` table, rounded to the nearest minute.

   ###### My Response:

   ```sql
   SELECT genre, ROUND(AVG(duration)) AS average_duration FROM films
    GROUP BY genre;
   ```

   ###### LS Response:

   ```sql
   SELECT genre, ROUND(AVG(duration)) AS average_duration FROM films GROUP BY genre;
   ```

7. Write a SQL query that determines the average duration of movies for each decade represented in the `films` table, rounded to the nearest minute and listed in chronological order.

   ###### My Response:

   ```sql
   SELECT 
   	(EXTRACT(DECADE FROM make_date(year, 1, 1)) * 10) AS decade,
   	ROUND(AVG(duration)) AS average_duration
   	FROM films
    GROUP BY decade
    ORDER BY decade ASC;
   ```

   ###### LS Response:

   ```sql
   SELECT year / 10 * 10 as decade, ROUND(AVG(duration)) as average_duration
   	FROM films GROUP BY decade ORDER BY decade;
   ```

8. Write a SQL query that finds all films whose director has the first name `John`.

   ###### My Response:

   ```sql
   SELECT title, director FROM films
    WHERE director LIKE 'John%';
   ```

   ###### LS Response:

   ```sql
   SELECT * FROM films WHERE director LIKE 'John %';
   ```

9. Write a SQL query that will return the following data:

   ```
      genre   | count
   -----------+-------
    scifi     |     5
    comedy    |     4
    drama     |     2
    espionage |     2
    crime     |     1
    thriller  |     1
    horror    |     1
    action    |     1
   (8 rows)
   ```

   ###### My Response:

   ```sql
   SELECT genre, count(genre) FROM films
   GROUP BY genre
   ORDER BY count DESC;
   ```

   ###### LS Response:

   ```sql
   SELECT genre, count(films.id) FROM films GROUP BY genre ORDER BY count DESC;
   ```

10. Write a SQL query that will return the following data:

    ```
     decade |   genre   |                  films
    --------+-----------+------------------------------------------
       1940 | drama     | Casablanca
       1950 | drama     | 12 Angry Men
       1950 | scifi     | 1984
       1970 | crime     | The Godfather
       1970 | thriller  | The Conversation
       1980 | action    | Die Hard
       1980 | comedy    | Hairspray
       1990 | comedy    | Home Alone, The Birdcage, Wayne's World
       1990 | scifi     | Godzilla
       2000 | espionage | Bourne Identity
       2000 | horror    | 28 Days Later
       2010 | espionage | Tinker Tailor Soldier Spy
       2010 | scifi     | Midnight Special, Interstellar, Godzilla
    (13 rows)
    ```

    ###### My Response:

    ```sql
    SELECT year / 10 * 10 AS decade,
    			 genre,
    			 title
    	FROM films
     GROUP BY decade AND genre;
    ```

    ###### LS Response:

    ```sql
    SELECT year / 10 * 10 AS decade, genre, string_agg(title, ', ') AS films
    	FROM films GROUP BY decade, genre ORDER BY decade, genre;
    ```

11. Write a SQL query that will return the following data:

    ```
       genre   | total_duration
    -----------+----------------
     horror    |            113
     thriller  |            113
     action    |            132
     crime     |            175
     drama     |            198
     espionage |            245
     comedy    |            407
     scifi     |            632
    (8 rows)
    ```

    ###### My Response:

    ```sql
    SELECT genre, sum(duration) AS total_duration FROM films GROUP BY genre ORDER BY total_duration ASC;
    ```

    ###### LS Response:

    ```sql
    SELECT genre, sum(duration) AS total_duration
    	FROM films GROUP BY genre ORDER BY total_duration ASC;
    ```

---

### How PostgreSQL Executes Queries

---

#### A Declarative Language

* SQL is a declarative language. A SQL statement describes _what_ to do, but not _how_ to do it. It is up to the PostgreSQL server to take each query, determine how to best execute it, and return the desired results.  
* The benefit of this approach is that it vastly simplifies database interactions for a user. Databases can perform involved data manipulations with the use of just a few SQL terms, freeing the user to think at a higher level about what data they want returned and in what format.
* The downside to this automated approach is that some of the time, the database will choose to do something in an inefficient way.
* Fortunately, there are ways to give the database hints (or requirements) about how it should execute a query.

#### How PostgreSQL Executes a Query

* While the exact way that a PostgreSQL database server will execute a query will depend on many variables, there is a high-level process that each query goes through. Being familiar with these steps can be a benefit for knowing the difference between two queries that appear to return the same results, or understanding why some queries are rejected by the database.
* The basic process for a `SELECT` query is described below.

###### Rows are collected into a virtual derived table

* You can think of this step as the database creating a new temporary table using the data from all the tables listed in the query's `FROM` clause. This includes tables that are used in `JOIN` clauses.

###### Rows are filtered using `WHERE` conditions

* All the conditions in the `WHERE` clause are evaluated for each row, and those that don't meet these requirements are removed.

###### Rows are divided into groups

* If the query includes a `GROUP BY` clause, the remaining rows are divided into the appropriate groups.

###### Groups are filtered using `HAVING` conditions

* `HAVING` conditions are very similar to `WHERE` conditions, only they are applied to the values that are used to create groups and not individual rows. This means that a column that is mentioned in a `HAVING` clause should almost always appear in a `GROUP BY` clause and/or an aggregate function in the same query. Both `GROUP BY` and aggregate functions perform grouping, and the `HAVING` clause is used to filter that aggregated/grouped data.
* `HAVING` clauses aren't as common as `WHERE` clauses, and we won't be seeing them very much in this course.

###### Compute values to return using select list

* Each element in the select list is evaluated, including any functions, and the resulting values are associated with the name of the column they are from or the name of the last function evaluated, unless a different name is specified in the query with `AS`.

###### Sort Results

* The result set is sorted as specified in an `ORDER BY` clause. Without this clause, the results are returned in an order that is the result of how the database executed the query and the rows' order in the original tables. It's best to always specify order if your application relies on rows being returned in a specific order.

###### Limit Results

* If `LIMIT` or `OFFSET` clauses are included in the query, these are used to adjust which rows in the result set are returned.

---

### Summary

---

* _SQL_ is a _special-purpose, declarative_ language used to manipulate the structure and values of datasets stored in a relational database.

* SQL is comprised of three sublanguages:

  | sub-language                          | controls                       | SQL Constructs                         |
  | :------------------------------------ | :----------------------------- | :------------------------------------- |
  | **DDL** or data definition language   | relation structure and rules   | `CREATE`, `DROP`, `ALTER`              |
  | **DML** or data manipulation language | values stored within relations | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
  | **DCL** or data control language      | who can do what                | `GRANT`                                |

* SQL code is made up of statements, which must be terminated by a semicolon.

* PostgreSQL provides many data types. We've looked at the following subset of types it supports in this lesson:

  | Data Type                   | Type      | Value                             | Example Values          |
  | :-------------------------- | :-------- | :-------------------------------- | :---------------------- |
  | `varchar(length)`           | character | up to `length` characters of text | `canoe`                 |
  | `text`                      | character | unlimited length of text          | `a long string of text` |
  | `integer`                   | numeric   | whole numbers                     | `42`, `-1423290`        |
  | `numeric`                   | numeric   | floating-point numbers            | `24.563`, `-14924.3515` |
  | `decimal(precision, scale)` | numeric   | arbitrary precision numbers       | `123.45`, `-567.89`     |
  | `timestamp`                 | date/time | date and time                     | `1999-01-08 04:05:06`   |
  | `date`                      | date/time | only a date                       | `1999-01-08`            |
  | `boolean`                   | boolean   | true or false                     | `true`, `false`         |

* `NULL` is a special value that represents the absence of any other value.
* `NULL` values must be compared using `IS NULL` or `IS NOT NULL`.
* Database dumps can be loaded using `psql -d database_name < file_to_import.sql`.
* Table columns can have default values, which are specified using `SET DEFAULT`.
* Table columns can be disallowed from storing `NULL` values using `SET NOT NULL`.
* `CHECK` constraints are rules that must be met by the data stored in a table.
* A *natural key* is an existing value in a dataset that can be used to uniquely identify each row of data in that dataset.
* A *surrogate key* is a value that is created solely for the purpose of identifying a row of data in a database table.
* A *primary key* is a value that is used to uniquely identify the rows in a table. It cannot be `NULL` and must be unique within a table. They are created using `PRIMARY KEY`.
* `serial` columns are typically used to create auto-incrementing columns in PostgreSQL.
* `AS` is used to rename tables and columns within a SQL statement.

