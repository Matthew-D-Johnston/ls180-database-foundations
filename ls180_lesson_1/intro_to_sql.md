##### LS180 Database Foundations > Launch School Bookshelf > Introduction to SQ

---

## Introduction to SQL

### Structured Data

*  Structured data attempts to solve the problem of the limitations of unstructured data.
* One way to structure data is by using a tabular format of rows and columns, like a spreadsheet.
* A common way of storing data in a structured manner is to use a **relational database**. A basic definition of a database is simply 'a structured set of data held in a computer'.

### Spreadsheet as Database

* Spreadsheets as a whole can be thought of as a database, and the worksheets within the spreadsheet can be used to describe tables within a database. 
* A table, in the context of a database, can be defined as a list of individual but related data entries (the table rows), each of which store values for a set of defined, shared attributes (the table columns).
* The rows and columns within a worksheet can be seen as analogous to the rows and columns in a table. Each row represents a single set of related data, while the columns represent a standardized way to store data for that particular attribute.
* This simplified analogy serves perfectly to describe, conceptually, a database. Keep this analogy in mind as we go forward in the book:

| Spreadsheet      | Database     |
| ---------------- | ------------ |
| Worksheet        | Table        |
| Worksheet Column | Table Column |
| Worksheet Row    | Table Record |

### Relational Database Management Systems

* A **relational database** is a database organized according to the _relational model_ of data. In simple terms, the relational model defines a set of relations (which we can think of as analogous to tables) and describes the relationships, or connections, between them in order to determine how the data stored in them can interact. Using the relational model elevates our database from data that is represented in just a flat, two-dimensional table, to one where we can describe the data in a more complex and detailed way. Using a relational database helps us to cut down on duplicated data and provides a much more useful data structure for us to interact with.
* A **relational database management system (RDBMS)** is essentially a software application, or system, for managing relational databases. An RDBMS allows a user, or another application, to interact with a database by issuing commands using syntax that conforms to a certain set of conventions or standards.
* Examples of RDBMS: SQLite, MS SQL, PostgreSQL, and MySQL. There are various differences between these but the one thing they share in common is the underlying language they all use: SQL.

### SQL

* **SQL**, which stands for _Structured Query Language_, is the programming language used to communicate with a relational database.
* SQL is a powerful language that uses simple English sentences that, with a few lines, allow you to _Select_ (find), _Insert_ (add), _Update_ (change), and _Delete_ (remove) a large amount of data.
* SQL is a **declarative** language; when you write an SQL statement you describe _what_ needs to be done, but not exactly _how_ to do it--the exact details of how the query is executed are handled internally by the RDBMS you are using.

### Why learn SQL?

* Relational databases are widespread. We use them every day without realizing it.
* Creating a well-designed database is like laying the foundations of a house, and learning SQL and relational database concepts will help you build your applications on a strong foundation. Since databases are such a key part of almost all web applications, understanding the language of databases and how they work is a vital step towards becoming a well-rounded web-developer.

### Vocabulary

* One should be familiar with the following terms:

| Term                | Definition                                                   |
| :------------------ | :----------------------------------------------------------- |
| Relational Database | A structured collection of data that follows the *relational model*. |
| RDBMS               | Relational Database Management System. A software application for managing relational databases, such as PostgreSQL. |
| Relation            | A set of individual but related data entries; analogous to a database table. |
| SQL                 | Structured Query Language. The language used by RDBMSs.      |
| SQL Statement       | A SQL command used to access/use the database or the data within that database via the SQL language. |
| SQL query           | A subset of a "SQL Statement". A query is a way to search, or lookup data within a database, as opposed to updating or changing data. |

### Installing PostgreSQL

* To start PostgreSQL in the command line, type: `brew services start postgresql`
* To stop it, type: `brew services stop postgresql`

### Interacting With a Database

* There are many interfaces, or clients, that you can use to interact with a RDBMS such as PostgreSQL. You may access the database from a programming language, through a GUI application that allows access and administration, or through the command-line interface. One thing these interfaces all have in common is the underlying architecture they use to interact with the database: they issue a request, or declaration, and receive a response in return. This is generally referred to as "client-server" architecture. PostgreSQL is a "client-server" database, which is an architecture design used by most relational databases.
* Whichever type of PostgreSQL client you choose to use, that interface is basically an abstraction; ultimately they all work by issuing queries to the database, using SQL syntax. Depending on the client, that SQL syntax may be 'wrapped' in some other commands or even abstracted altogether by some sort of visual interface, but underneath it all the same thing is essentially happening.

### PostgreSQL Client Applications

* PostgreSQL comes packaged with a number of what are called 'client applications'. These client applications are used to interact with PostgreSQL in various ways by issuing a command via the command line. Some commonly used PostgreSQL client applications are `createdb`, `dropdb`, `pg_dump`, `pg_restore`, and `pg_bench`. Some of these client applications are essentially wrappers around SQL commands; for example `createdb`, which can be used to create a new PostgreSQL database, is a wrapper around the SQL command `CREATE DATABASE`.
* One client application that we will be using throughout this book is `psql`. `psql` is a PostgreSQL interactive console, or a terminal-based front-end to PostgreSQL. It allows you to write queries in SQL syntax, issue those queries to a PostgreSQL database, and see the results of those queries in the terminal window. In that sense the `psql` console is essentially a REPL; you may already be familiar with other REPLs, such as IRB.

### The psql console

* Type `psql postgres` to enter the psql console.

* There are two different types of commands you can issue from the psql console prompt:
  1. You can issue psql console meta-commands.
  2. You can run SQL queries using standard SQL syntax.

###### Meta-commands

* The syntax for a psql console meta-command is a backslash `\` followed by the command and any optional arguments.
* Meta-commands can be used for a number of different things, such as connecting to a different database, listing tables, describing the structure of a particular table, setting environment variables, and so on.
* We'll look at some examples of meta-commands later in this book. For now, one useful command to remember is `\q`, which quits the sql console and returns to the normal command line.

###### SQL Statements

* SQL statements are commands issued to the database using SQL syntax. A simple statement might look something like this:

  ```
  postgres-# SELECT name FROM people WHERE id = 1;
  ```

* SQL statements always terminate in a semicolon. This means that you can actually write statements over multiple lines; PostgreSQL will not execute the statement until it reaches the semicolon.
* A `SELECT` statement is used to retrieve data from a database.

### SQL Sub-languages

* SQL can be thought of as comprising of three separate sub-languages, each concerned with a specific aspect of manipulating or interacting with a database. The three sub-languages are:
  * **DDL: Data Definition Language**. Used to define the structure of a database and the tables and columns within it.
  * **DML: Data Manipulation Language**. Used to retrieve or modify data stored in a database. `SELECT` queries are part of DML.
  * **DCL: Data Control Language**. Used to determine what various users are allowed to do when interacting with a database.

---

## SQL Basics tutorial

### Some SQL Keywords

* `SELECT`. This is a keyword that identifies the type of statement being issued. Since the statement starts with `SELECT` we know it is a select statement, and its purpose is to retrieve data.
* `*`. This is a _wild card_ character that acts as an identifier for _all_ of the columns in a given table.
* `FROM`. This is another keyword. It is used as a clause within a `SELECT` statement to identify the table from which to retrieve the data.
* `WHERE`. This is a keyword that informs the `SELECT` statement that only rows that match a certain condition should be returned.
* Note: in the `WHERE` clause of SQL queries, `=` is treated as an 'equality' operator, in that it compares things. In most programming languages, a single `=` operator is used solely for assignment, but not so in SQL.

### Data vs. Schema

* Schema is concerned with the _structure_ of a database. This structure is defined by things such as the names of tables and table columns, the data types of those columns and any constraints that they may have.
* Data is concerned with the _contents_ of a database. These are the actual values associated with specific rows and columns in a database table.
* Schema and data work together in order to let us interact with a database in useful ways. Schema without data would just be a buncy of empty tables.
* If we had data without schema, we'd be back to the idea of unstructured data.

### Exercises

1. Write a query that returns all of the customer names for the `orders` table.

```sql
SELECT customer_name FROM orders;
```

2. Write a query that retuns all of the orders that include a Chocolate Shake.

```sql
SELECT * FROM orders WHERE drink = 'Chocolate Shake'
```

3. Write a query that returns the burger, side, and drink for the order with an `id` of `2`.

```sql
SELECT burger, side, drink FROM orders WHERE id = 2
```

4. Write a query that returns the name of anyone who ordered Onion Rings.

```sql
SELECT customer_name FROM orders WHERE side = 'Onion Rings'
```

### Create a database

* Use the `createdb` command in the terminal followed by the name of the database you want to create (e.g. `createdb sql_book`).

* We can connect to the database using the `psql` command, passing it the `-d` option with the database name `sql_book`: `psql -d sql_book`. The `-d` option is the flag used to specify the database.

* `createdb` acts as a 'wrapper' around the actual SQL syntax `CREATE DATABASE`, which is used to create a database within the psql console:

  ```sql
  sql_book=# CREATE DATABASE another_database;
  ```

###### Database naming

* As a guideline, always try to keep database names self-descriptive. A descriptive name is especially helpful if you end up having a lot of databases. A database containing information about Employees could be named 'employees' or 'employee_database'. A less descriptive name might be 'emp' or 'records'. Also, database names should be written in **snake_case**, that is, lowercase with words separated by underscores.

### Connecting to a Database

* In order to be able to issue commands to a database we need to be connected to it. At the start of this chapter, we saw that from terminal we can open the psql console and connect to a database using the `psql` command with the `-d` option and a database name.
* When we are in the psql console itself, we can connect to a different database by using the `\c` or `\connect` meta-commands (these both do the same thing). Let's try this out.

### Delete the Database

* We can use the SQL command `DROP DATABASE` to do delete a database.

  ```sql
  yet_another_database=# DROP DATABASE another_database;
  DROP DATABASE
  yet_another_database=#
  ```

* In the same way that `createdb` is a PostgreSQL client application that acts as a wrapper for the `CREATE DATABASE` SQL command, `dropdb` performs the same role for the `DROP DATABASE` SQL command.
* To use `dropdb`, we need to issue the command from terminal rather than the psql console.

### A Summary of Some Useful Commands

* First we have some commands that can be used within a `psql` session:

  | PSQL Command                         | Notes                                                        |
  | :----------------------------------- | :----------------------------------------------------------- |
  | `\l` or `\list`                      | displays all databases                                       |
  | `\c sql_book` or `\connect sql_book` | connects to the *sql_book* database                          |
  | `\q`                                 | exits the PostgreSQL session and return to the command-line prompt |

* We also have some commands that are programs installed by PostgreSQL on our system:

  | Command-line Command | Notes                                                        |
  | :------------------- | :----------------------------------------------------------- |
  | `psql -d sql_book`   | starts a `psql` session and connect to the *sql_book* database |
  | `createdb sql_book`  | creates a new database called *sql_book* using a psql utility |
  | `dropdb my_database` | permanently deletes the database named *my_database* and all its data |

* Remember, that some of the utilities we've shown such as `createdb` and `dropdb` are wrapper functions for actual SQL statements:

  | SQL Statement               | Notes                                                        |
  | :-------------------------- | :----------------------------------------------------------- |
  | `CREATE DATABASE sql_book`  | creates a new database called *sql_book*                     |
  | `DROP DATABASE my_database` | permanently deletes the database named *my_database* and all its data |

### Exercises

1. From the Terminal, create a database called `database_one`.

   ```
   createdb database_one
   ```

2. From the Terminal, connect via the psql console to the database that you created in the previous question.

   ```
   psql -d database_one
   ```

3. From the psql console, create a database called `database_two`.

   ```sql
   CREATE DATABASE database_two;
   ```

4. From the psql console, connect to `database_two`.

   ```sql
   \c database_two
   ```

5. Display all of the databases that currently exist.

   ```
   \l
   ```

6. From the psql console, delete `database_two`.

   ```sql
   DROP DATABASE database_two;
   ```

7. From the Terminal, delete the `database_one` and `ls_burger` databases.

   ```
   dropdb database_one
   dropdb ls_burger
   ```

### Table Creation Syntax

* To create a table we can use the `CREATE TABLE` SQL statement. In its most simple form, it's very similar to the `CREATE DATABASE` SQL statement we looked at in the previous chapter:

  ```sql
  CREATE TABLE some_table();
  ```

* To create a table with columns, we need to place column definitions in between the parentheses. Each column definition is generally written on a separate line, separated by a comma. The basic format of a `CREATE TABLE` statement is:

  ```sql
  CREATE TABLE table_name (
  	column_1_name column_1_data_type [constraints, ...],
  	column_2_name column_2_data_type [constraints, ...],
  	.
  	.
  	.
  	constraints
  );
  ```

* Column names and data types are a required part of each column definition; constraints are optional. One thing to note from the above format is that constraints can be defined either at the column level or at the table level.

### Data Types

* A data type classifies particular values that are allowed for that column. This can help protect our database from data of an invalid type being entered.

* Some common data types:

  | Column Data Type          | Description                                                  |
  | :------------------------ | :----------------------------------------------------------- |
  | serial                    | This data type is used to create identifier columns for a PostgreSQL database. These identifiers are integers, auto-incrementing, and cannot contain a null value. |
  | char(N)                   | This data type specifies that information stored in a column can contain strings of up to N characters in length. If a string less than length N is stored, then the remaining string length is filled with space characters. |
  | varchar(N)                | This data type specifies that information stored in a column can contain strings of up to N characters in length. If a string less than length N is stored, then the remaining string length isn't used. |
  | boolean                   | This is a data type that can only contain two values "true" or "false". In PostgreSQL, boolean values are often displayed in a shorthand format, t or f |
  | integer or INT            | An integer is simply a "whole number." An example might be 1 or 50, -50, or 792197 depending on what storage type is used. |
  | decimal(precision, scale) | The decimal type takes two arguments, one being the total number of digits in the entire number on both sides of the decimal point (the precision), the second is the number of the digits in the fractional part of the number to the right of the decimal point (the scale). |
  | timestamp                 | The timestamp type contains both a simple date and time in YYYY-MM-DD HH:MM:SS format. |
  | date                      | The date type contains a date but no time.                   |

### Keys and Constraints

* One of the key functions of a database is to maintain the integrity and quality of the data that it is storing. Keys and Constraints are rules that define what data values are allowed in certain columns. They are an important database concept and are part of a database's schema definition. Defining Keys and Constraints is part of the database design process and ensures that the data within a database is reliable and maintains its integrity. Constraints can apply to a specific column, an entire table, more than one table, or an entire schema.
* Let's go over some constraints:
  * UNIQUE: The `id` column also has a `UNIQUE` constraint, which prevents any duplicate values from being entered into that column.
  * NOT NULL: The `id` column also has a `NOT NULL` constraint, which essentially means that when adding data to the table a value MUST be specified for this column, it cannot be left empty.
  * DEFAULT: The `enabled` column has an extra property `DEFAULT` with a value of `TRUE`. If no value is set in this field when a record is created then the value of `TRUE` is set in that field.
* We haven't run into keys just yet, but we will later on. They come into play when we have to set up relationships between different database tables. They're also useful for keeping track of unique rows within a database table.

### View the Table

* In a large database with lots of different tables, we might want to view a list of all the tables that exist in the database. We can use the `\dt` meta-command to show us a list of all the tables, or relations, in the database.
* For more detailed information about a particular table, such as the names of its columns, and the column data types and properties, we can use the `\d` meta-command. This lets us describe a table.

### Summary of Table Commands

| Command              | Notes                                    |
| :------------------- | :--------------------------------------- |
| CREATE TABLE users.. | Creates a new table called *users*       |
| \dt                  | Shows the tables in the current database |
| \d users             | Shows the schema of the table *users*    |

### Exercises

1. From the Terminal, create a database called `encylcopedia` and connect to it via the sql console.

   ```
   $ createdb encyclopedia
   $ psql -d encyclopedia
   ```

2. Create a table called `countries`. It should have the following columns:

   * An `id` column of type `serial`
   * A `name` column of type `varchar(50)`
   * A `capital` column of type `varchar(50)`
   * A `population` column of type `integer`

   The `name` column should have a `UNIQUE` constraint. The `name` and `capital` columns should both have `NOT NULL` constraints.

   ```sql
   CREATE TABLE countries (
   id serial,
   name varchar(50) UNIQUE NOT NULL,
   capital varchar(50) NOT NULL,
   population integer
   );
   ```

3. Create a table called `famous_people`. It should have the following columns:

   * An `id` column that contains auto-incrementing values
   * A `name` column. This should contain a string up to 100 characters in length
   * An `occupation` column. This should contain a string up to 150 characters in length
   * A `date_of_birth` column that should contain each person's date of birth in a string of up to 50 characters
   * A `deceased` column that contains either `true` or `false`

   The table should prevent `NULL` values being added to the `name` column. If the value of the `deceased` column is unknown then `false` should be used.

   ```sql
   CREATE TABLE famous_people (
   id serial,
   name varchar(100) NOT NULL,
   occupation varchar(150),
   date_of_birth varchar(50),
   deceased boolean DEFAULT false
   );
   ```

4. Create a table called `animals` that could contain the sample data below:

   | Name         | Binomial Name     | Max Weight (kg) | Max Age (years) | Conservation Status |
   | :----------- | :---------------- | :-------------- | :-------------- | :------------------ |
   | Lion         | Pantera leo       | 250             | 20              | VU                  |
   | Killer Whale | Orcinus orca      | 6,000           | 60              | DD                  |
   | Golden Eagle | Aquila chrysaetos | 6.35            | 24              | LC                  |

* The database table should also contain an auto-incrementing `id` column.
* Each animal should always have a name and a binomial name.
* Names and binomial names vary in length but never exceed 100 characters.
* The max weight column should be able to hold data in the range 0.001kg to 40,000kg
* Conservation Status is denoted by a combination of two letters (CR, EN, VU, etc).

```sql
CREATE TABLE animals (
id serial,
name varchar(100) NOT NULL,
binomial_name varchar(100) NOT NULL,
max_weight_kg decimal(8, 3),
max_age_years integer,
conservation_status char(2)
);
```

5. List all of the tables in the `encyclopedia` database.

   ```sql
   \dt
   ```

6. Display the schema for the `animals` table.

   ```sql
   \d animals
   ```

7. Create a database called `ls_burger` and connect to it.

   ```sql
   CREATE DATABASE ls_burger;
   \c ls_burger
   ```

8. Create a table in the `ls_burger` database called `orders`. The table should have the following columns:

   * An `id` column, that should contain an auto-incrementing integer value.
   * A `customer_name` column, that should contain a string of up to 100 characters.
   * A `burger` column, that should hold a string of up to 50 characters.
   * A `side` column, that should hold a string of up to 50 characters.
   * A `drink` column, that should hold a string of up to 50 characters.
   * An `order_total` column, that should hold a numeric value in dollars and cents. Assume that all orders will be less than $100.

   The `customer_name` and `order_total` columns should always contain a value.

   ```sql
   CREATE TABLE orders (
   id serial,
   customer_name varchar(100) NOT NULL,
   burger varchar(50),
   side varchar(50),
   drink varchar(50),
   order_total decimal(4, 2) NOT NULL
   );
   ```


### Alter Table Syntax

* Existing tables can be altered with an `ALTER TABLE` statement. An `ALTER TABLE` statement is part of DDL, and is for altering a table **schema** only ; we'll look at updating _data_ in a table later in this book.

* The basic format of an `ALTER TABLE` statement is:

  ```
  > ALTER TABLE table_to_change HOW TO CHANGE THE TABLE additional arguments
  ```

### Renaming a Table

* We can rename a table using the `RENAME` clause.

  ```sql
  sql_book=# ALTER TABLE users
  sql_book=# RENAME TO all_users;
  ```

### Renaming a Column

* `RENAME` can also be used to rename a column. But we need to include a `COLUMN` clause in between the `RENAME` and `TO` clauses.

  ```sql
  sql_book=# ALTER TABLE all_users
  sql_book-# RENAME COLUMN username TO full_name;
  ```

### Changing a Column's Datatype

* To change a column's datatype we again use the `ALTER TABLE` statement, but this time in conjunction with `ALTER COLUMN` to target a specific column for change. In the statement below, the datatype of `full_name` is changed from `char(25)` to `varchar(25)`. 

  ```sql
  sql_book=# ALTER TABLE all_users
  sql_book-# ALTER COLUMN full_name TYPE varchar(25);
  ```

### Adding a Constraint

* Constraints are a little different from the other changes we have been making in that rather than changing them we add them to, or remove them from, the column definition. Another difference with constraints is that whereas a column can only have one name and one datatype, it can have more than one constraint.

* The syntax for adding constraints can vary depending on the type of constraint we're adding. Some types of constraint are considered 'table constraints' (even if they apply to a specific column) and others, such as `NOT NULL` are considered 'column constraints'.

* The form for adding a column constraint is as follows:

  ```
  > ALTER TABLE table_name ALTER COLUMN column_name SET constraint clause
  ```

* Whereas the form for adding a table constraint is:

  ```
  > ALTER TABLE table_name ADD CONSTRAINT constraint_name constraint clause
  ```

* We'll look at this second form a bit later, for now let's add a `NOT NULL` constraint to our `full_name` column.

  ```sql
  sql_book=# ALTER TABLE all_users
  sql_book-# ALTER COLUMN full_name SET NOT NULL;
  ```

### Removing a Constraint

* Just as we can add constraints to a table after creating it, we may also remove them. The syntax is the same for both column and table constraints:

  ```sql
  ALTER TABLE table_name DROP CONSTRAINT constraint_name
  ```

* When we used a specified data type of `serial` for our `id` column, this automatically added a `DEFAULT` clause that uses the `nextval` function to set `id` to the next available value if no value is specified. This is quite a useful set-up for our `id` column, as we'll see later when we start adding data to our table. If we did want to remove the `DEFAULT` clause, it's not technically a constraint, so the syntax is a little different:

  ```sql
  sql_book=# ALTER TABLE all_users
  sql_book-# ALTER COLUMN id DROP DEFAULT;
  ```

### Adding a Column

* There may be situations were you need to add an entirely new column.

* If you need to add a column to the table you've created, one that was not specified in the original schema, you can use an `ADD COLUMN` clause in an `ALTER TABLE` statement.

* Run this command in your psql console and follow along to see how the database changes.

  ```sql
  ALTER TABLE all_users
  	ADD COLUMN last_login timestamp NOT NULL DEFAULT NOW();
  ```

* Note: `NOW()` is a SQL function. It provides the current date and time when it is called. There are many such functions available and we will look at some of the common ones in subsequent chapters.

### Removing a Column

* The command to remove a column from a table also uses the `ALTER TABLE` clause. Say for example we wanted to remove the `enabled` column from the `all_users` table.

  ```sql
  sql_book=# ALTER TABLE all_users DROP COLUMN enabled;
  ```

### Dropping Tables

* Deleting a table has a relatively straightforward command and the syntax for deleting a table is much like the command for dropping a database, `DROP`. Run the statement and command below on your psql console. If you try to `describe` the deleted table, you will get an error.

  ```sql
  sql_book=# DROP TABLE all_users;
  ```

### Summary of Commands

* Although the SQL statements for all of these actions use the same initial `ALTER TABLE` clause, the specific syntax for each varies according to the action.

| Action                           | Command                                                      | Notes                                                        |
| :------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Add a column to a table          | ALTER TABLE table_name ADD COLUMN column_name data_type CONSTRAINTS; | Alters a table by adding a column with a specified data type and optional constraints. |
| Alter a column's data type       | ALTER TABLE table_name ALTER COLUMN column_name TYPE data_type; | Alters the table by changing the datatype of column.         |
| Rename a table                   | ALTER TABLE table_name RENAME TO new_table_name;             | Changes the name of a table in the currently connected to database. |
| Rename a column within a table   | ALTER TABLE table_name RENAME COLUMN column_name TO new_column_name; | Renames a column of the specified table.                     |
| Add column constraint            | ALTER TABLE table_name ALTER COLUMN column_name SET constraint clause; | Adds a specified constraint to the specified table column.   |
| Add table constraint             | ALTER TABLE table_name ADD CONSTRAINT constraint_name constraint clause; | Adds a specified constraint to the specified table.          |
| Remove a column constraint       | ALTER TABLE table_name ALTER COLUMN column_name DROP CONSTRAINT; | Removes a constraint from the specified table.               |
| Remove a table constraint        | ALTER TABLE table_name DROP CONSTRAINT constraint_name;      | Removes a constraint from the specified table.               |
| Remove a column from a table     | ALTER TABLE table_name DROP COLUMN column_name               | Removes a column from the specified table.                   |
| Delete a table from the database | DROP TABLE table_name;                                       | Permanently deletes the specified table from its database.   |

### Exercises

1. Make sure you are connected to the `encyclopedia` database. Rename the `famous_people` table to `celebrities`.

   ```sql
   ALTER TABLE famous_people
   RENAME TO celebrities;
   ```

2. Change the name of the `name` column in the `celebrities` table to `first_name`, and change its data type to `varchar(80)`.

   ```sql
   ALTER TABLE celebrities
   RENAME COLUMN name TO first_name;
   ```

   ```sql
   ALTER TABLE celebrities
   ALTER COLUMN first_name TYPE varchar(80);
   ```

3. Create a new column in the `celebrities` table called `last_name`. It should be able to hold strings of lengths up to 100 characters. This column should always hold a value.

   ```sql
   ALTER TABLE celebrities
   ADD COLUMN last_name varchar(100) NOT NULL;
   ```

4. Change the `celebrities` table so that the `date_of_birth` column uses a data type that holds an actual date value rather than a string. Also ensure that this column must hold a value.

   ```sql
   ALTER TABLE celebrities
   	ALTER COLUMN date_of_birth TYPE date
   		USING date_of_birth::date,
   	ALTER COLUMN date_of_birth SET NOT NULL;
   ```

5. Change the `max_weight_kg` column in the `animals` table so that it can hold values in the range 0.0001kg to 200,000kg.

   ```sql
   ALTER TABLE animals
   ALTER COLUMN max_weight_kg TYPE decimal(10,4);
   ```

6. Change the `animals` table so that the `binomial_name` column cannot contain duplicate values.

   ```sql
   ALTER TABLE animals
   ADD CONSTRAINT unique_binomial_name UNIQUE (binomial_name); 
   ```

7. Connect to the `ls_burger` database. Add the following columns to the `orders` table:

   * A column called `customer_email`; it should hold strings of up to 50 characters.
   * A column called `customer_loyalty_points` that should hold integer values. If no value is specified for this column, then a value of `0` should be applied.

   ```sql
   ALTER TABLE orders
   ADD COLUMN customer_email varchar(50),
   ADD COLUMN customer_loyalty_points integer DEFAULT 0;
   ```

8. Add three columns to the `orders` table called `burger_cost`, `side_cost`, and `drink_cost` to hold monetary values in dollars and cents (assume that all values will be less than $100). If no value is entered for these columns, a value of `0` dollars should be used.

   ```sql
   ALTER TABLE orders
   ADD COLUMN burger_cost decimal(4,2) DEFAULT 0,
   ADD COLUMN side_cost decimal(4,2) DEFAULT 0,
   ADD COLUMN drink_cost decimal(4,2) DEFAULT 0;
   ```

9. Remove the `order_total` column from the `orders` table.

   ```sql
   ALTER TABLE orders
   DROP COLUMN order_total;
   ```

### Data and DML

* DML is a sub-language of SQL which incorporates the various key words, clauses and syntax used to write Data Manipulation Statements.
* Data Manipulation Statements are used for accessing and manipulating data in the database. Data Manipulation Statements can be categorized into four different types:
  * `INSERT` statements: These add new data into a database.
  * `SELECT` statements: Also referred to as Queries; these retrieve existing data from database tables. We've worked with this type a bit already.
  * `UPDATE` statements: These update existing data in a database table.
  * `DELETE` statements: These delete existing data from a database table.

* We'll be working with all of these types of statements in this and the following chapters. The actions performed by these four types of statement are sometimes also referred to as CRUD operations.

###### A Bit About CRUD

* The term `CRUD` is a commonly used acronym in the database world. The letters in CRUD stand for the words CREATE, READ, UPDATE, and DELETE. These four words are analogous to our `INSERT`, `SELECT`, `UPDATE` and `DELETE` statements, and we can think of these statements as performing their equivalent CRUD operations. Web applications whose main purpose is to provide an interface to perform these operations are often referred to as 'CRUD apps'.

### Insertion Statement Syntax

* Here is the general form of an `INSERT` SQL statement:

  ```sql
  INSERT INTO table_name (column1_name, column2_name, ...)
  	VALUES (data_for_column1, data_for_column2, ...);
  ```

* When using an `INSERT` statement, we have to provide three key pieces of information:
  1. The table name we wish to store data in.
  2. The names of the columns we're adding data to.
  3. The values we wish to store in the columns listed directly after the table name.
* When inserting data into a table, you may specify all the columns from the table, just a few of them, or none at all. Depending on how your table is structured, and how your data row is ordered, not specifying columns can sometimes lead to unexpected results or errors, so it is generally best to specify which columns you want to insert data into.
* When specifying columns, for each column specified you _must_ supply a value for it in the `VALUES` clause, otherwise you'll get an error back. If you don't specify a column for data insertion, then null or a default value will be added to the record you wish to store instead.

### Adding Rows of Data

###### Rows

* If we think of columns as giving structure to our table, then we can think of rows (sometimes referred to as 'tuples') as actually containing the data. Each row in a table is an individual entity, which can perhaps be seen as the logical equivalent of a record, and has a corresponding value for each column in the table. The rows and columns work together. It is the intersection of the structure provided by our columns and the data in our rows that create the structured data that we need.

###### Adding a Single Row

* Let's add a record, or row, into the `users` table with the following values:

  * id: We'll use the default value for this; an integer generated by the `nextval` function
  * username: 'John Smith'
  * enabled: false
  * last_login: We'll use the default value for this; The current time and date.

* The first one, id, we can just allow the default value to be set, which we can also do for the last one, last_login.

  ```sql
  sql_book=# INSERT INTO users (full_name, enabled)
  sql_book-# VALUES ('John Smith', false);
  ```

* Note that the order of the columns must match the order of the values to be inserted.

###### Adding Multiple Rows

* If you're adding lots of data, you probably won't want to execute a separate `INSERT` statement for each row. Fortunately we can use a single `INSERT` statement to add multiple rows of data to a table. Let's add in two more records to our table using a single statement:

  ```sql
  sql_book=# INSERT INTO users (full_name)
  sql_book-# VALUES ('Jane Smith'), ('Harry Potter');
  ```

### Constraints and Adding Data

* Although constraints are set at the level of the table structure, or schema, and so are part of DDL, they are primarily concerned with controlling what data can be added to a table.

###### Default Values

* Setting a `DEFAULT` value for a column ensures that if a value is not specified for that column in an `INSERT` statement, then the default value will be used instead. Three columns in our `users` table, `id`, `enabled` and `last_login`, have `DEFAULT` values set.

###### NOT NULL Constraints

* It doesn't always make sense for a column to have a default value. For example, a column like `full_name` in the `users` table should contain a name that is specific to each user record, rather than some generic, default name. `NOT NULL` constraints can be used to ensure that when a new row is added, a value must be specified for that column.

###### Unique Constraints

* Sometimes, rather than simply ensuring that a column has a value in it, we want to ensure that the value added for that column is unique; to do this we can use a `UNIQUE` constraint.
* When we created our `users` table, we added a `UNIQUE` constraint to the `id` column. This type of constraint ensures that you can't have duplicate values in that column of the table. When we first created the table, we explained that an Index is created as a result of the `UNIQUE` constraint; on our `users` table, this Index is called `users_id_key`. Whenever we try to insert a row into our `users` table, the value that we specify for the `id` column is checked against existing values in the `users_id_key` Index; if the value already exists in there then we can't insert it into that column for our new row.

###### CHECK Constraints

* Certain columns in our table may not need unique values, but we may well want to ensure that the values entered into those columns conform to some other specific rules. In such a situation we can use a `CHECK` constraint. Check constraints limit the type of data that can be included in a column based on some condition we set in the constraint. Each time a new record is in the process of being added to a table, that constraint is first *checked* to ensure that data being added conforms to it.

* Adding a check constraint to ensure that no blank names are added to the `full_name` column:

  ```sql
  sql_book=# ALTER TABLE users ADD CHECK (full_name <> '');
  ```

* In case you haven't seen it before `<>` is an operator in SQL. It's a 'not equal' to operator (and alternative syntax for `!=`).

### Summary of Commands

| Command                                                      | Notes                                                        |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| INSERT INTO table_name (column1_name, column2_name, ...) VALUES (data_for_column1, data_for_column2, ...); | creates a new record in *table_name* with the specified columns and their associated values. |
| ALTER TABLE table_name ADD UNIQUE (column_name);             | Adds a constraint to `table_name` that prevent non-unique values from being added to the table for `column_name` |
| ALTER TABLE table_name ADD CHECK (expression);               | Adds a constraint to `table_name` that prevents new rows from being added if they don't pass a *check* based on a specified expression. |

### Exercises

1. Make sure you are connected to the `encyclopedia` database. Add the following data to the `countries` table:

   | Name   | Capital | Population |
   | :----- | :------ | :--------- |
   | France | Paris   | 67,158,000 |

```sql
INSERT INTO countries (name, capital, population)
VALUES ('France', 'Paris', 67158000);
```



2. Now add the following additional data to the `countries` table:

| Name    | Capital         | Population  |
| :------ | :-------------- | :---------- |
| USA     | Washington D.C. | 325,365,189 |
| Germany | Berlin          | 82,349,400  |
| Japan   | Tokyo           | 126,672,000 |

```sql
INSERT INTO countries (name, capital, population)
VALUES ('USA', 'Washington D.C.', 325365189),
('Germany', 'Berlin', 82349400),
('Japan', 'Tokyo', 126672000);
```

3. Add an entry to the `celebrities` table for the singer and songwriter Bruce Springsteen, who was born on September 23rd 1949 and is still alive.

```sql
INSERT INTO celebrities (first_name, last_name, occupation, date_of_birth)
VALUES ('Bruce', 'Springsteen', 'singer/songwriter', 'Sep-23-1949');
```

4. Add an entry for the actress Scarlett Johansson, who was born on November 22nd 1984. Use the default value for the `deceased` column.

```sql
INSERT INTO celebrities (first_name, last_name, occupation, date_of_birth)
VALUES ('Scarlett', 'Johansson', 'actress', 'Nov-22-1984');
```

5. Add the following two entries to the `celebrities` table with a single `INSERT` statement. For Frank Sinatra set `true` as the value for the deceased column. For Tom Cruise, don't set an explicit value for the `deceased` column, but use the default value.

- - [
     Update Data in a Table](https://launchschool.com/books/sql/read/update_and_delete_data)
- WORKING WITH MULTIPLE TABLES
  - [Create Multiple Tables](https://launchschool.com/books/sql/read/table_relationships)
  - [SQL Joins](https://launchschool.com/books/sql/read/joins)
- CONCLUSION
  - [Summary and Additional Resources](https://launchschool.com/books/sql/read/resources)
- SHARE ON
  -  
  - 

# Inserting Data into a Table

In the previous section of this book we looked at creating a database, and creating, altering, and even deleting tables. These things are all concerned with the structure, or schema, of our database. This is only half the story though; the reason for creating that structure in the first place is to set the stage for how we can store data, in what format we can store data, and the format we can expect when we try to retrieve data.

Although we've mentioned data a lot, and the idea of data has been in the background of everything we've talked about so far, we've not yet spoken in detail about what we actually mean by data in the context of a database. In this section we're going to focus on that 'data' piece of the puzzle, and explore some of the various ways that we can use Data Manipulation Language (DML) to add, query, change, and remove data. We've mentioned DML before, and you may already have some idea of what it means and what we can do with it. Before we start working with it, let's just define it a little more clearly.

## [Data and DML](https://launchschool.com/books/sql/read/add_data#dmldata)

DML is a sub-language of SQL which incorporates the various key words, clauses and syntax used to write Data Manipulation Statements.

Data Manipulation Statements are used for accessing and manipulating data in the database. Data Manipulation Statements can be categorized into four different types :

- `INSERT` statements - These add new data into a database table
- `SELECT` statements - Also referred to as Queries; these retrieve existing data from database tables. We've worked with this type a bit already.
- `UPDATE` statements - These update existing data in a database table.
- `DELETE` statements - These delete existing data from a database table.

We'll be working with all of these types of statements in this and the following chapters. The actions performed by these four types of statement are sometimes also referred to as CRUD operations.

### A Bit About CRUD

The term `CRUD` is a commonly used acronym in the database world. The letters in CRUD stand for the words CREATE, READ, UPDATE, and DELETE. These four words are analogous to our `INSERT`, `SELECT`, `UPDATE` and `DELETE` statements, and we can think of these statements as performing their equivalent CRUD operations. Web applications whose main purpose is to provide an interface to perform these operations are often referred to as 'CRUD apps'.

The first of these operations we'll look at is creating, or adding, data. Before we do that though, we need to put back the table that we removed at the end of the previous chapter.

## [Setup](https://launchschool.com/books/sql/read/add_data#setup)

First of all, make sure that you're connected to the `sql_book` database via the psql console. Your command prompt should look like this:

```sql
sql_book=#
```

Now execute the following SQL statement:

```sql
CREATE TABLE users (
    id serial UNIQUE NOT NULL,
    full_name character varying(25) NOT NULL,
    enabled boolean DEFAULT true,
    last_login timestamp without time zone DEFAULT now()
);
```

You should receive the `CREATE TABLE` response, and a prompt ready to receive the next statement:

```sql
CREATE TABLE
sql_book=#
```

Thus, we've got our `users` table back but it's currently empty of data.



![Empty Table](https://d186loudes4jlv.cloudfront.net/sql/images/add_data/add-data-empty-table.png)



If we execute a SQL statement to retrieve all of the data in the table, the response tells us that there are `(0 rows)`:

```sql
sql_book=# SELECT * FROM users;
 id | full_name | enabled | last_login
----+-----------+---------+------------
(0 rows)
```

We'll talk a bit more about what we mean by rows shortly, what this basically means though is that our table has no data in it. Let's change that.

## [Insertion Statement Syntax](https://launchschool.com/books/sql/read/add_data#insertionstatementsyntax)

Here is the general form of an `INSERT` SQL statement:

```sql
INSERT INTO table_name (column1_name, column2_name, ...)
  VALUES (data_for_column1, data_for_column2, ...);
```

When using an `INSERT` statement, we have to provide three key pieces of information:

1. The table name we wish to store data in.
2. The names of the columns we're adding data to.
3. The values we wish to store in the columns listed directly after the table name.

When inserting data into a table, you may specify all the columns from the table, just a few of them, or none at all. Depending on how your table is structured, and how your data row is ordered, not specifying columns can sometimes lead to unexpected results or errors, so it is generally best to specify which columns you want to insert data into.

When specifying columns, for each column specified you *must* supply a value for it in the `VALUES` clause, otherwise you'll get an error back. If you don't specify a column for data insertion, then null or a default value will be added to the record you wish to store instead.

## [Adding Rows of Data](https://launchschool.com/books/sql/read/add_data#addingrows)

### Rows

If we think of columns as giving structure to our table, then we can think of rows (sometimes referred to as 'tuples') as actually containing the data. Each row in a table is an individual entity, which can perhaps be seen as the logical equivalent of a record, and has a corresponding value for each column in the table. The rows and columns work together. It is the intersection of the structure provided by our columns and the data in our rows that create the structured data that we need.

### Adding a Single Row

Let's revisit what our table looks like before we add the data, so we know what columns we have and what type of data we need to add to it. The structure of the table is also referred to as the schema of a table.

```sql
sql_book=# \d users
                   Table "public.users"
   Column   |            Type             |   Modifiers
------------+-----------------------------+---------------
 id         | integer                     | not null
 full_name  | character varying(25)       | not null
 enabled    | boolean                     | default true
 last_login | timestamp without time zone | default now()
```

Now we want to add a record, or row, into the `users` table with the following values:

- id: We'll use the default value for this; an integer generated by the `nextval` function
- username: 'John Smith'
- enabled: false
- last_login: We'll use the default value for this; The current time and date.

First let's try to add this row of data to `users` without specifying the columns by running the `INSERT` statement below.

```sql
sql_book=# INSERT INTO users VALUES ('John Smith', false);
```

If you execute this statement you should receive the following error:

```sql
ERROR:  invalid input syntax for integer: "John Smith"
LINE 1: INSERT INTO users VALUES ('John Smith', false);
                                  ^
```

This is basically telling us that we are trying to insert an invalid value, the string `"John Smith"`, into an integer column. The reason for this is the values we've included in our `INSERT` statement don't match up with the defined order of the columns in our `users` table. PostgreSQL thinks we wanted to insert `"John Smith"` into our `id` column, which has a type of `integer`, since this is the first column in our table and `"John Smith"` was the first value in our `VALUES` list.

There's a couple of things we could do here.

1. We could use the keyword `DEFAULT` as the 'value' for our `id` column in our `VALUES` list. This would indicate that we want PostgreSQL to use the `nextval` function that we've set as the default for this column. Note that we wouldn't need to use `DEFAULT` for `last_login`; for any columns that we omit, PostgreSQL will either use the default (if one has been set) or set the column to `NULL`.
2. We could specify the columns in our `INSERT` statement (ensuring that the order of those columns matches up with our values).

Let's take this second approach.

```sql
sql_book=# INSERT INTO users (full_name, enabled)
sql_book-# VALUES ('John Smith', false);
```

Note that the order of the columns must match the order of the values to be inserted, but by specifying both the columns *and* the values, it is much easier to ensure that the order matches up correctly. Looking at our statement above, we can clearly see that the columns we specified match up with the data that we want to insert.

This time our `INSERT` statement should be executed successfully, and we should get the following command tag back in response:

```sql
INSERT 0 1
```

The first digit after `INSERT` in this tag is the `oid`, which we won't cover in this book. The second digit is the `count` of rows that were inserted; since we inserted one row, the count is `1`.

### Adding Multiple Rows

If you're adding lots of data, you probably won't want to execute a separate `INSERT` statement for each row. Fortunately we can use a single `INSERT` statement to add multiple rows of data to a table. Let's add in two more records to our table using a single statement:

```sql
sql_book=# INSERT INTO users (full_name)
sql_book-# VALUES ('Jane Smith'), ('Harry Potter');
INSERT 0 2
```

The syntax is very similar to when adding a single row, except each row of values is comma separated. As can be seen from the above example, you don't necessarily need to have each row on a separate line. It is generally good practice to do so though, as it enables you to clearly see the rows that you are adding and the values of those rows (in the case of our example this isn't too much of an issue since we are only adding two rows with one value each). Since we inserted two rows here, the `count` in the response is `2`.

One thing to note is that even though we are adding multiple rows at the same time, PostgreSQL adds them in the order that we specified in our statement. The `nextval` function therefore knows to set an `id` of `2` for 'Jane Smith' and `id` of `3` for 'Harry Potter'.

Here's what our table now looks like with all three rows of data added:



![Table with Data Added](https://d186loudes4jlv.cloudfront.net/sql/images/add_data/add-data-3-rows-added.png)



When inserting these three rows into our table, we've relied on a constraint, `DEFAULT`, for setting the `last_login` value for our first row, the `enabled` and `last_login` values for our second and third rows, and the `id` value for all three. Let's look at exactly what that means, and go over that and some other constraints you may encounter.

## [Constraints and Adding Data](https://launchschool.com/books/sql/read/add_data#constraintsdata)

We've covered constraints very briefly when setting up our table structure, but haven't yet really explained too much about them. Although constraints are set at the level of the table structure, or schema, and so are part of DDL, they are primarily concerned with controlling what data can be added to a table.

### Default Values

Setting a `DEFAULT` value for a column ensures that if a value is not specified for that column in an `INSERT` statement, then the default value will be used instead. Three columns in our `users` table, `id`, `enabled` and `last_login`, have `DEFAULT` values set.

In our first `INSERT` statement we specified a value for `enabled`, but not for `last_login`, so our specified value was used for `enabled` and the default value used for `last_login`:



![Adding one row with one default column](https://d186loudes4jlv.cloudfront.net/sql/images/add_data/add-data-add-row-1.png)



In our second `INSERT` statement we didn't specify a value for `enabled` or `last_login`, so the default values were used for both columns:



![Adding two rows with two default columns](https://d186loudes4jlv.cloudfront.net/sql/images/add_data/add-data-adding-rows-2-and-3.png)



### NOT NULL Constraints

It doesn't always make sense for a column to have a default value. For example, a column like `full_name` in the `users` table should contain a name that is specific to each user record, rather than some generic, default name. `NOT NULL` constraints can be used to ensure that when a new row is added, a value must be specified for that column.

If we try to execute the following `INSERT` statement:

```sql
sql_book=# INSERT INTO users (id, enabled)
sql_book-# VALUES (1, false);
```

we receive the following response:

```sql
ERROR:  null value in column "full_name" violates not-null constraint
DETAIL:  Failing row contains (1, null, f, 2017-10-18 12:20:02.067639).
```

There are two things of interest here, the `ERROR` which tells us that we are in violation of the `not-null constraint` on the `full_name` column, and the `DETAIL` which shows the values in our failing row and specifically that the value we were trying to insert into the `full_name` column was `null`.

If our `INSERT` statement specifies both columns and values but we don't specify a particular column, SQL will try to insert `null` into that column by default. Since we have a `NOT NULL` constraint on our `full_name` column, that `null` gets rejected and we get an error.



![Not Null constraint](https://d186loudes4jlv.cloudfront.net/sql/images/add_data/add-data-null-constraint.png)



### Unique Constraints

Sometimes, rather than simply ensuring that a column has a value in it, we want to ensure that the value added for that column is unique; to do this we can use a `UNIQUE` constraint.

When we created our `users` table, we added a `UNIQUE` constraint to the `id` column. This type of constraint ensures that you can't have duplicate values in that column of the table. When we first created the table, we explained that an Index is created as a result of the `UNIQUE` constraint; on our `users` table, this Index is called `users_id_key`. Whenever we try to insert a row into our `users` table, the value that we specify for the `id` column is checked against existing values in the `users_id_key` Index; if the value already exists in there then we can't insert it into that column for our new row.



![Unique constraint](https://d186loudes4jlv.cloudfront.net/sql/images/add_data/add-data-unique-constraint.png)



We already have a row in our users table where the value in the `id` column is `1`, so if we try to add another row with this same value for the `id` column, our `UNIQUE` constraint should prevent us from doing so.

```sql
sql_book=# INSERT INTO users (id, full_name, enabled)
sql_book-# VALUES (1, 'Alissa Jackson', true);
ERROR:  duplicate key value violates unique constraint "unique_id"
DETAIL:  Key (id)=(1) already exists.
```

Just as intended, that unique constraint prevented duplicate data in our table. We can even check our current data within our table just to be sure:

```sql
sql_book=# SELECT * FROM users;
 id |  full_name   | enabled |         last_login
----+--------------+---------+----------------------------
  1 | John Smith   | f       | 2017-10-25 10:26:10.015152
  2 | Jane Smith   | t       | 2017-10-25 10:26:50.295461
  3 | Harry Potter | t       | 2017-10-25 10:26:50.295461
(3 rows)
```

It's not unusual for a column such as `id` to have a `UNIQUE` constraint. Having some sort of 'id' column in a database table is a common, and useful, practice. Such a column is generally used to store a unique identifier for each row of data. In order for it to work effectively though, we need to ensure that each value in such a column is actually unique. Thus far, we've added data in such a way so that each `id` was unique and each record distinct, but we don't want to have to manually keep track of every value we add to that column; using a `UNIQUE` constraint lets PostgreSQL do the work for us.

Looking up values for `UNIQUE` constraints is just one use for indexes in a database. Database Indexes is a large and complex topic, worthy of a book on its own, and not one that we cover in any detail in this book, just remember that they come into play when a table column has a `UNIQUE` constraint.

### CHECK Constraints

Certain columns in our table may not need unique values, but we may well want to ensure that the values entered into those columns conform to some other specific rules. In such a situation we can use a `CHECK` constraint. Check constraints limit the type of data that can be included in a column based on some condition we set in the constraint. Each time a new record is in the process of being added to a table, that constraint is first *checked* to ensure that data being added conforms to it.

Let's try this out by using a `CHECK` constraint on the `full_name` column. We want to ensure that every user record has a name. Right now, we ensure that `null` values can't be entered for a users's full name, but we don't guard against empty strings. For example, the values in following statement would be perfectly valid:

```sql
INSERT INTO users (id, full_name) VALUES (4, '');
```

Don't execute the above statement just yet, let's first fix this potential issue by adding a `CHECK` constraint to our `users` table:

```sql
sql_book=# ALTER TABLE users ADD CHECK (full_name <> '');
ALTER TABLE
```

Now, if we were to try and add in an user with a blank name, we'll get an error back, similar to the one we received when we tried to add a record with a duplicate id.

```sql
sql_book=# INSERT INTO users (id, full_name) VALUES (4, '');
ERROR:  new row for relation "users" violates check constraint "users_full_name_check"
DETAIL:  Failing row contains (4, , t, 2017-10-25 10:32:21.521183).
```



![Check constraint](https://d186loudes4jlv.cloudfront.net/sql/images/add_data/add-data-check-constraint.png)



A couple of things here that should be clarified. In case you haven't seen it before `<>` is an operator in SQL. It's a 'not equal' to operator (and alternative syntax for `!=`), and here we're using it to specify that any value we try to insert for `full_name` cannot be equal to an empty string.

Also, notice that we didn't specify a name for our constraint. We mentioned this short-hand syntax earlier, and now we're putting it to good use. If we don't need a specific name for our check constraint, then it's fine to leave the naming up to PostgreSQL.

**A Bit About Quote Marks**

A string in PostgreSQL is defined as a sequence of characters bounded by single quotes `'`. For example, `'This is a string'`. What happens though if the string itself *contains* a single-quote character, such as in the name `'O'Leary'`?

If we tried to use such a string in an `INSERT` statement, the statement would not execute properly since PostgreSQL would think that the second quote mark (after the `O`) was terminating a string, and the third one (after the `y`) was denoting the start of another string.

The way to deal with this situation is to use a second quote mark to escape the first, in the following manner `'O''Leary'`.

## [Summary](https://launchschool.com/books/sql/read/add_data#summary)

In this chapter we've talked about one of the four types of DML interactions you can have with a database table, as well as adding, 'creating', data using `INSERT` statements. We've covered a number of different aspects of adding data:

- `INSERT` statement syntax
- Table rows
- Adding a single row of data
- Adding multiple rows of data
- Various types of constraint we can use to control what data is added:
  - `DEFAULT` values
  - `NOT NULL` constraints
  - `UNIQUE` constraints
  - `CHECK` constraints

Let's quickly recap some of the main commands:

| Command                                                      | Notes                                                        |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| INSERT INTO table_name (column1_name, column2_name, ...) VALUES (data_for_column1, data_for_column2, ...); | creates a new record in *table_name* with the specified columns and their associated values. |
| ALTER TABLE table_name ADD UNIQUE (column_name);             | Adds a constraint to `table_name` that prevent non-unique values from being added to the table for `column_name` |
| ALTER TABLE table_name ADD CHECK (expression);               | Adds a constraint to `table_name` that prevents new rows from being added if they don't pass a *check* based on a specified expression. |

That's all for now on adding data to a table and constraints. In the following chapters we'll work on expanding our knowledge of querying a database using `SELECT`.

## Exercises

1. Make sure you are connected to the `encyclopedia` database. Add the following data to the `countries` table:

   | Name   | Capital | Population |
   | :----- | :------ | :--------- |
   | France | Paris   | 67,158,000 |

   #### Solution

   ```sql
   INSERT INTO countries (name, capital, population)
                VALUES ('France', 'Paris', 67158000);
   ```

2. Now add the following additional data to the `countries` table:

   | Name    | Capital         | Population  |
   | :------ | :-------------- | :---------- |
   | USA     | Washington D.C. | 325,365,189 |
   | Germany | Berlin          | 82,349,400  |
   | Japan   | Tokyo           | 126,672,000 |

   #### Solution

   Here we could use separate `INSERT` statments for each row, or use a single statement to add multiple rows (as below).

   ```sql
   INSERT INTO countries (name, capital, population)
                VALUES ('USA', 'Washington D.C.', 325365189),
                       ('Germany', 'Berlin', 82349400),
                       ('Japan', 'Tokyo', 126672000);
   ```

3. Add an entry to the `celebrities` table for the singer and songwriter Bruce Springsteen, who was born on September 23rd 1949 and is still alive.

   Hint

   PostgreSQL can accept date data in many different formats, including:

   ```sql
   1999-01-08
   1999-Jan-08
   Jan-08-1999
   08-Jan-1999
   99-Jan-08
   08-Jan-99
   Jan-08-99
   19990108
   ```

   More detailed information about the way PostrgeSQL deals with date and time inputs is outlined in the [PostgreSQL documentation](https://www.postgresql.org/docs/current/static/datatype-datetime.html#DATATYPE-DATETIME-INPUT).

   #### Solution

   ```sql
   INSERT INTO celebrities (first_name, last_name, occupation, date_of_birth, deceased)
                 VALUES ('Bruce', 'Springsteen', 'Singer, Songwriter', '1949-09-23', false);
   ```

4. Add an entry for the actress Scarlett Johansson, who was born on November 22nd 1984. Use the default value for the `deceased` column.

   #### Solution

   We can either omit the `deceased` column from our column list, in which case the default value for that column will automatically be used.

   ```sql
   INSERT INTO celebrities (first_name, last_name, occupation, date_of_birth)
                    VALUES ('Scarlett', 'Johansson', 'Actress', '1984-11-22');
   ```

   Alternatively we can include the `deceased` column, but use the `DEFAULT` keyword as the value for that column.

   ```sql
   INSERT INTO celebrities (first_name, last_name, occupation, date_of_birth, deceased)
                    VALUES ('Scarlett', 'Johansson', 'Actress', '1984-11-22', DEFAULT);
   ```

5. Add the following two entries to the `celebrities` table with a single `INSERT` statement. For Frank Sinatra set `true` as the value for the deceased column. For Tom Cruise, don't set an explicit value for the `deceased` column, but use the default value.

   | First Name | Last Name | Occupation    | D.O.B.            |
   | :--------- | :-------- | :------------ | :---------------- |
   | Frank      | Sinatra   | Singer, Actor | December 12, 1915 |
   | Tom        | Cruise    | Actor         | July 03, 1962     |

```sql
INSERT INTO celebrities (first_name, last_name, occupation, date_of_birth, deceased)
VALUES ('Frank', 'Sinatra', 'singer, actor', 'Dec-12-1915', true),
('Tom', 'Cruise', 'actor', 'Jul-03-1962', DEFAULT);
```

6. Look at the schema of the `celebrities` table. What do you think will happen if we try to insert the following data?

   | First Name | Last Name | Occupation                          | D.O.B.       | Deceased |
   | :--------- | :-------- | :---------------------------------- | :----------- | :------- |
   | Madonna    |           | Singer, Actress                     | '08/16/1958' | false    |
   | Prince     |           | Singer, Songwriter, Musician, Actor | '06/07/1958' | true     |

* We will get an error because the last_name column has a NOT NULL constraint and we have left it empty.

7. Update the `last_name` column of the `celebrities` table so that the data in the previous question can be entered, and then add the data to the table.

   ```sql
   ALTER TABLE celebrities
   ALTER COLUMN last_name
   DROP NOT NULL;
   ```

8. Check the schema of the `celebrities` table. What would happen if we specify a `NULL` value for `deceased` column, such as with the data below?

9. | First Name | Last Name | Occupation              | D.O.B.       | Deceased |
   | :--------- | :-------- | :---------------------- | :----------- | :------- |
   | Elvis      | Presley   | Singer, Musician, Actor | '01/08/1935' | NULL     |

   Check the schema of the `animals` table. What would happen if we tried to insert the following data to the table?

   | Name             | Binomial Name            | Max Weight (kg) | Max Age (years) | Conservation Status |
   | :--------------- | :----------------------- | :-------------- | :-------------- | :------------------ |
   | Dove             | Columbidae Columbiformes | 2               | 15              | LC                  |
   | Golden Eagle     | Aquila Chrysaetos        | 6.35            | 24              | LC                  |
   | Peregrine Falcon | Falco Peregrinus         | 1.5             | 15              | LC                  |
   | Pigeon           | Columbidae Columbiformes | 2               | 15              | LC                  |
   | Kakapo           | Strigops habroptila      | 4               | 60              | CR                  |

* The problem is that the `binomial_name` column has a `UNIQUE` constraint on it, but Doves and Pigeons have the same binomial name. Thus, we need to drop that constraint.

```sql
ALTER TABLE animals
DROP CONSTRAINT unique_binomial_name;
```

* Now we can insert the data:

```sql
INSERT INTO animals (name, max_weight_kg, max_age_years, conservation_status, binomial_name)
VALUES ('Dove', 2, 15, 'LC', 'Columbidae Columbiformes'),
('Golden Eagle', 6.35, 24, 'LC', 'Aquila Chrysaetos'),
('Peregrine Falcon', 1.5, 15, 'LC', 'Falco Peregrinus'),
('Pigeon', 2, 15, 'LC','Columbidae Columbiformes'),
('Kakapo', 4, 60, 'CR', 'Strigops habroptila');
```

10. Connect to the `ls_burger` database and examine the schema for the `orders` table.

```sql
INSERT INTO orders (customer_name, customer_email, burger, side, drink, burger_cost, side_cost, drink_cost, customer_loyalty_points)
VALUES ('James Bergman', 'james1998@email.com', 'LS Chicken Burger', 'Fries', 'Cola', 4.50, 0.99, 1.50, 28),
('Natasha O''Shea', 'natasha@osheafamily.com', 'LS Cheeseburger', 'Fries', NULL, 3.50, 0.99, DEFAULT, 18),
('Natasha O''Shea', 'natasha@osheafamily.com', 'LS Double Deluxe Burger', 'Onion Rings', 'Chocolate Shake', 6.00, 1.50, 2.00, 42),
('Aaron Muller', NULL, 'LS Burger', NULL, NULL, 3.00, DEFAULT, DEFAULT, 10);
```

### Select Queries

* In the previous chapter we used `INSERT` to add some data to our `users` table. Now that our table contains data, we can use `SELECT` to access, or _query_, that data in various ways. Querying data forms the **Read** part of our **CRUD** operations, and is arguably the most common operation in database-backed applications.

### Select Query Syntax

* Let's start by breaking down the `SELECT` statement into individual generalized parts, as we've done for SQL statements in previous chapters.

  ```sql
  SELECT [*, (column_name1, column_name2, ...)]
  FROM table_name WHERE (condition);
  ```

##### Identifiers and Keywords

* In a SQL statement such as `SELECT enabled, full_name FROM users;` there are _identifiers_ and _keywords_. The identifiers, such as `enabled`, `full_name`, and `users`, identify tables or columns within a table. The keywords, such as `SELECT` and `FROM`, tell PostgreSQL to do something specific.
* Since SQL is not a case-sensitive language, the case differences in our example can't be used by PostgreSQL to differentiate between identifiers and keywords. Instead it assumes that anything which is not a keyword (or operator, or function) is an identifier and so treats it as such. What do we do if we want to use an identifier that is the same as a keyword? For example, we might have a column called `year`, which is actually a reserved word in PostgreSQL. If used in certain SQL statements, depending on the context, this identifier would cause an error.
* Generally it's best to try and avoid naming columns the same as keywords for this very reason. If it's unavoidable however, you can double quote the identifier in your statement, like so `"year"`. PostgreSQL then knows to treat it specifically as an identifier rather than as a keyword.  

### Order By

* `ORDER BY` displays the results of a query in a particular sort order.
* SQL allows returning sorted data by adding the `ORDER BY column_name` clause to a query. Let's add to our `SELECT` query syntax example within an `ORDER BY` clause.

```sql
SELECT [*, (column_name1, column_name2, ...)]
FROM table_name WHERE (condition)
ORDER BY column_name;
```

* You can fine tune your ordering with the `ORDER BY` clause by specifying the sort direction, either _ascending_ or _descending_, using keywords `ASC` or `DESC`. If omitted, then the default is `ASC`, which is why our previous query was sorted by `enabled` in _ascending_ order.
* You can fine tune your ordering even further by returning results ordered by more than one column. This is done by having comma-separated expressions in the `ORDER BY` clause. If we add `id DESC` to our `ORDER BY` clause, the rows will first be ordered by `enabled` as in our previous example, but then _within_ any sets of rows which have identical values for `enabled` a second level of ordering will be applied, this time by `id` in descending order.

### Operators

* Operators are generally used as part of an expression in a `WHERE` clause. We'll briefly look at a few different operators and how to use them as part of a `SELECT` query by grouping some of them into the following different types:
  1. Comparison
  2. Logical
  3. String Matching
* The operators we discuss below are only a selection of those available within PostgreSQL; they do however represent some of the most commonly used operators and fundamental use cases that you will encounter.

###### Comparison Operators

* These operators are used to compare one value to another. Often these values are numerical, but other data types can also be compared. Examples of comparison operator would be 'less than' `<` or 'not equal' `<>` (both of which we've already encountered).
* Within the expression of a `WHERE` clause, the comparison operator is placed in between the two things being compared; i.e. the column name and the specific value to be compared against the values in that column.
* Let's look at an example, using the 'greater than or equal to' operator `>=`.

```sql
SELECT full_name, enabled, last_login FROM users WHERE id >= 2;
```

* Some other comparison operators that work in a similar way are listed below:

| Operator     | Description              |
| :----------- | :----------------------- |
| `<`          | less than                |
| `>`          | greater than             |
| `<=`         | less than or equal to    |
| `>=`         | greater than or equal to |
| `=`          | equal                    |
| `<>` or `!=` | not equal                |

* As well as the comparison operators listed above, there are what is termed _comparison predicates_ which behave much like operators but have special syntax. Examples include `BETWEEN`, `NOT BETWEEN`, `IS DISTINCT FROM`, `IS NOT DISTINCT FROM`. We won't discuss these in this book, though there are two important ones which we will briefly cover: `IS NULL` and `IS NOT NULL`.

* `NULL` is a special value in SQL which actually represents an **unknown value**. Don't worry too much about the finer details of what this means for now, we'll explore this some more later in the curriculum. On a practical level though, what this means is that we can't simply treat `NULL` as we would any other value. We couldn't, for example, have a `WHERE` clause in the form `WHERE column_name = NULL`. When identifying `NULL` values we must instead use the `IS NULL` comparison predicate.

  ```sql
  SELECT * FROM my_table WHERE my_column IS NULL;
  ```

* The above example would select all rows for which the column `my_column` had a `NULL` value.
* Using `IS NOT NULL` would do the opposite; i.e. it would select all rows for which the column had any value _other than_ `NULL`.

###### Logical Operators

* Logical operators can be used to provide more flexibility to your expressions. There are three logical operators:

  1. `AND`
  2. `OR`
  3. `NOT`

* The third one, `NOT` is less commonly used than the other two, so we won't cover it here. The `AND` and `OR` operators allow you to combine multiple conditions in a single expression. Let's try them out with some quick examples:

  ```sql
  SELECT * FROM users WHERE full_name = 'Harry Potter' OR enabled = 'false';
  ```

* The above statement should retrieve two rows. This is because there is one row where the value of `full_name` is `"Harry Potter"` and so matches the first condition, and another row where `enabled` is `false` and so matches the second condition. Since by using `OR` we are interested in rows that satisfy **either** condition, both of these rows should be returned. The other row in our table didn't satisfy either of the conditions and so shouldn't be returned.
* If we use `AND` we are saying that we are only interested in rows that satisfy **both** conditions. Since there are no rows where `full_name` is `"Harry Potter"` and `enabled` is also `false`, no rows satisfy our overall condition and so `0` rows are returned.

###### String Matching Operators

* String, or pattern, matching allows you to add flexibility to your conditional expressions in another way, by searching for a sub-set of the data within a column. For instance, let's say we wanted to find all users with the last name Smith. We can't directly check if `full_name` is equal to Smith, since Smith is only part of the entire name. We need a way to look at a substring within the entire name. As the name suggests, string matching can only be done when the data type of the column is a string type. It is most often carried out using the `LIKE` operator. Let's try it out by using our `full_name` example:

  ```sql
  sql_book=# SELECT * FROM users WHERE full_name LIKE '%Smith';
  ```

* Our `WHERE` clause here looks very much like other `WHERE` clauses we've seen so far in this book, except where the `=` operator would normally be, we use the `LIKE` operator instead. Also notice the use of the `%` character in the value that we want to match against; this is a wildcard character. By putting `%` just before Smith we are saying: Match all users that have a full name with any number of characters followed by "Smith".
* As well as the `%` character, the underscore `_` can also be used as a wildcard with `LIKE`, however `_` stands in for only a _single_ character whereas `%` stands in for _any number_ of characters.
* An alternative to `LIKE` for pattern matching is to use `SIMILAR TO`. It works in much the same way as `LIKE`, except that it compares the target column to a Regex (Regular Expression) pattern. 

### Summary of `SELECT` Clauses

| SELECT Clause                            | Notes                                                        |
| :--------------------------------------- | :----------------------------------------------------------- |
| ORDER BY column_name [ASC, DESC]         | Orders the data selected by a column name within the associated table. Data can be ordered in descending or ascending order; if neither are specified, the query defaults to ascending order. |
| WHERE column_name [>,=, <=, =, <>] value | Filters a query result based on some comparison between a column's value and a specified literal value. There are several comparison operators available for use, from "greater than" to "not equal to". |
| WHERE expression1 [AND, OR] expression2  | Filters a query result based whether one expression is true [and,or] another expression is true. |
| WHERE string_column LIKE '%substring'    | Filters a query result based on whether a substring is contained within string_column's data and has any number of characters before that substring. Those characters are matched using the wildcard `%`. `%` doesn't have to come before a substring, you can also put it after one as well, matching the substring first and then any number of characters after that substring. |

### Exercises

1. Make sure you are connected to the `encyclopedia` database. Write a query to retrieve the population of the USA.

   ```sql
   SELECT population FROM countries
   WHERE name = 'USA';
   ```

2. Write a query to return the population and the capital (with the columns in that order) of all the countries in the table.

   ```sql
   SELECT population, capital FROM countries;
   ```

3. Write a query to return the names of all the countries ordered alphabetically.

   ```sql
   SELECT name
   FROM countries
   ORDER BY name ASC;
   ```

4. Write a query to return the names and the capitals of all the countries in order of population, from lowest to highest.

   ```sql
   SELECT name, capital
   FROM countries
   ORDER BY population;
   ```

5. Write a query to return the same information as the previous query, but ordered from highest to lowest.

   ```sql
   SELECT name, capital
   FROM countries
   ORDER BY population DESC;
   ```

6. Write a query on the `animals` table, using `ORDER BY`, that will return the following output:

   ```
        name       |      binomial_name       | max_weight_kg | max_age_years
   ------------------+--------------------------+---------------+---------------
    Peregrine Falcon | Falco Peregrinus         |        1.5000 |            15
    Pigeon           | Columbidae Columbiformes |        2.0000 |            15
    Dove             | Columbidae Columbiformes |        2.0000 |            15
    Golden Eagle     | Aquila Chrysaetos        |        6.3500 |            24
    Kakapo           | Strigops habroptila      |        4.0000 |            60
   (5 rows)
   ```

   Use only the columns that can be seen in the above output for ordering purposes.

   ```sql
   SELECT name, binomial_name, max_weight_kg, max_age_years
   FROM animals
   ORDER BY max_age_years, max_weight_kg, name DESC;
   ```

7. Write a query that returns the names of all the countries with a population greater than 70 million.

   ```sql
   SELECT name
   FROM countries
   WHERE population > 70000000;
   ```

8. Write a query that returns the names of all the countries with a population greater than 70 million but less than 200 million.

   ```sql
   SELECT name
   FROM countries
   WHERE population > 70000000 AND population < 200000000;
   ```

9. Write a query that will return the first name and last name of all entries in the celebrities table where the value of the deceased columns is not true.

   ```sql
   SELECT first_name, last_name
   FROM celebrities
   WHERE deceased <> true OR deceased IS NULL;
   ```

10. Write a query that will return the first and last names of all the celebrities who sing.

    ```sql
    SELECT first_name, last_name
    FROM celebrities
    WHERE occupation LIKE '%singer%';
    ```

11. Write a query that will return the first and last names of all the celebrities who act.

    ```sql
    SELECT first_name, last_name
    FROM celebrities
    WHERE occupation LIKE '%act%';
    ```

12. Write a query that will return the first and last names of all the celebrities who both sing and act.

    ```sql
    SELECT first_name, last_name
    FROM celebrities
    WHERE (occupation LIKE '%actor%' AND occupation LIKE '%singer%')
    OR (occupation LIKE '%actress%' AND occupation LIKE '%singer%');
    ```

13. Connect to the `ls_burger` database. Write a query that lists all of the burgers that have been ordered, from cheapest to most expensive, where the cost of the burger is less than $5.00.

    ```sql
    SELECT burger
    FROM orders
WHERE burger_cost < 5.00
    ORDER BY burger_cost;
    ```
    
14. Write a query to return the customer name and email address and loyalty points from any order worth 20 or more loyalty points. List the results from the highest number of points to the lowest.

    ```sql
    SELECT customer_name, customer_email, customer_loyalty_points
    FROM orders
    WHERE customer_loyalty_points >= 20
    ORDER BY customer_loyalty_points DESC;
    ```

15. Write a query that returns all the burgers ordered by Natasha O'Shea.

    ```sql
    SELECT burger
    FROM orders
    WHERE customer_name = 'Natasha O''Shea';
    ```

16. Write a query that returns the customer name from any order which does not include a drink item.

    ```sql
    SELECT customer_name
    FROM orders
    WHERE drink IS NULL;
    ```

17. Write a query that returns the three meal items for any order which does not include fries.

    ```sql
    SELECT burger, side, drink
    FROM orders
    WHERE side <> 'Fries' OR side IS NULL;
    ```

18. Write a query that returns the three meal items for any order that includes both a side and a drink.

    ```sql
    SELECT burger, side, drink
    FROM orders
    WHERE side IS NOT NULL AND drink IS NOT NULL;
    ```

### LIMIT and OFFSET

* Displaying portions of data as separate 'pages' is a user interface pattern used in many web applications, generally referred to as 'pagination'. An example of this can be seen in the Launch School forum pages, where twelve forum posts are displayed on the first 'page' and you need to navigate to the next 'page' to see the next twelve.

* The `LIMIT` and `OFFSET` clauses of `SELECT` are the base on which pagination is built. Let's look at how it works.

* Example:

  ```sql
  SELECT * FROM users LIMIT 1 OFFSET 1;
  
   id |  full_name   | enabled |         last_login
  ----+--------------+---------+----------------------------
    2 | Jane Smith   | t       | 2017-10-25 10:26:50.295461
  (1 row)
  ```

  

### DISTINCT

* Sometimes duplicate data is unavoidable. For example, we might get duplication when joining more than one table together. We'll delve into working with multiple tables later in this book, for now let's look at a way to deal with duplication by using the `DISTINCT` qualifier.

* After adding some duplicate data to our `users` table, if we select the `full_name` column from the table, we get five rows back, two of which contain duplicate names.

  ```sql
  sql_book=# SELECT full_name FROM users;
  
   full_name
  --------------
   John Smith
   Jane Smith
   Harry Potter
   Harry Potter
   Jane Smith
  (5 rows)
  ```

  

* We can use `DISTINCT` as part of our `SELECT` query to only return distinct, or unique, values.

  ```sql
  SELECT DISTINCT full_name FROM users;
  
   full_name
  --------------
   John Smith
   Jane Smith
   Harry Potter
  (3 rows)
  ```

### Functions

* Functions are a way of working with data in SQL that may seem a little more familiar if you're coming to SQL from a programming language such as Ruby or JavaScript. Functions are a set of commands included as part of the RDBMS that perform particular operations on fields or data. Some functions provide data transformations that can be applied before returning results. Others simply return information on the operations carried out.
* These functions can generally be grouped into different types. Some of the most commonly used types of functions are:
  1. String
  2. Date/Time
  3. Aggregate

###### String Functions

* String Functions, as their name suggests, perform some sort of operation on values whose data type is String. Some examples are:

| Function | Example                                               | Notes                                                        |
| :------- | :---------------------------------------------------- | :----------------------------------------------------------- |
| `length` | `SELECT length(full_name) FROM users;`                | This returns the length of every user's name. You could also use `length` in a `WHERE` clause to filter data based on name length. |
| `trim`   | `SELECT trim(leading ' ' from full_name) FROM users;` | If any of the data in our `full_name` column had a space in front of the name, using the `trim` function like this would remove that leading space. |

###### Date/ Time Functions

Just as string functions perform operations on String data, Date/ Time functions, for the most part, perform operations on date and time data. Many of the date/ time functions take time or timestamp inputs. Our `last_login` column, for example, has a data type of `timestamp` and so data in that column can act as an argument to such functions:

| Function    | Example                                                      | Notes                                                        |
| :---------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `date_part` | `SELECT full_name, date_part('year', last_login) FROM users;` | `date_part` allow us to view a table that only contains a part of a user's timestamp that we specify. The above query allows us to see each user's name along with the year of the `last_login` date. Sometimes having date/time data down to the second isn't needed |
| `age`       | `SELECT full_name, age(last_login) FROM users;`              | The `age` function, when passed a single `timestamp` as an argument, calculates the time elapsed between that timestamp and the current time. The above query allows us to see how long it has been since each user last logged in. |

###### Aggregate Functions

Aggregate functions perform _aggregation_; that is, they compute a single result from a set of input values. We briefly looked at one of these, `count`, a little earlier in this chapter.

| Function | Example                              | Notes                                                        |
| :------- | :----------------------------------- | :----------------------------------------------------------- |
| `count`  | `SELECT count(id) FROM users;`       | Returns the number of values in the column passed in as an argument. This type of function can be very useful depending on the context. We could find the number of users who have enabled account, or even how many users have certain last names if we use the above statement with other clauses. |
| `sum`    | `SELECT sum(id) FROM users;`         | Not to be confused with `count`. This *sums* numeric type values for all of the selected rows and returns the total. |
| `min`    | `SELECT min(last_login) FROM users;` | This returns the lowest value in a column for all of the selected rows. Can be used with various data types such as numeric, date/ time, and string. |
| `max`    | `SELECT max(last_login) FROM users;` | This returns the highest value in a column for all of the selected rows. Can be used with various data types such as numeric, date/ time, and string. |
| `avg`    | `SELECT avg(id) FROM users;`         | Returns the average (arithmetic mean) of numeric type values for all of the selected rows. |

* Aggregate functions really start to be useful when grouping table rows together. The way we do that is by using the `GROUP BY` clause.

### GROUP BY

* Sometimes you need to combine data results together to form more meaningful information.

  ```sql
  sql_book=# SELECT enabled, count(id) FROM users GROUP BY enabled;
  
   enabled | count
  ---------+-------
   f       |     1
   t       |     4
  (2 rows)
  ```

### Summary

* In this chapter we've built on our knowledge of `SELECT`, and looked at a number of different ways we can make our `SELECT` queries more flexible and powerful:
  - Returning portions of a dataset using `LIMIT` and `OFFSET`
  - Returning unique values using `DISTINCT`
  - Using SQL functions to work with data in various ways
  - Aggregating data using `GROUP BY`

### Exercises

1. Make sure you are connected to the `encyclopedia` database. Write a query to retrieve the first row of data from the `countries` table.

   ```sql
   SELECT * FROM countries LIMIT 1;
   ```

2. Write a query to retrieve the name of the country with the largest population.

   ```sql
   SELECT name FROM countries
   ORDER BY population DESC
   LIMIT 1;
   ```

3. Write a query to retrieve the name of the country with the second largest population.

   ```sql
   SELECT name FROM countries
   ORDER BY population DESC
   LIMIT 1 OFFSET 1;
   ```

4. Write a query to retrieve all of the unique values from the `binomial_name` column of the `animals` table.

   ```sql
   SELECT DISTINCT binomial_name FROM animals;
   ```

5. Write a query to return the longest binomial name from the `animals` table.

   ```sql
   SELECT binomial_name FROM animals
   ORDER BY length(binomial_name) DESC
   LIMIT 1;
   ```

6. Write a query to return the first name of any celebrity born in 1958.

   ```sql
   SELECT first_name
   FROM celebrities
   WHERE date_part('year', date_of_birth) = 1958;
   ```

7. Write a query to return the highest maximum age from the `animals` table.

   ```sql
   SELECT max_age_years
   FROM animals
   ORDER BY max_age_years DESC
   LIMIT 1;
   
   or 
   
   SELECT max(max_age_years)
   FROM animals;
   ```

8. Write a query to return the average maximum weight from the `animals` table.

   ```sql
   SELECT avg(max_weight_kg) FROM animals;
   ```

9. Write a query to return the number of rows in the `countries` table.

   ```sql
   SELECT count(id) FROM countries;
   ```

10. Write a query to return the total population of all the countries in the `countries` table.

    ```sql
    SELECT sum(population) FROM countries;
    ```

11. Write a query to return each unique conversation status code alongside the number of animals that have that code.

    ```sql
    SELECT conservation_status, count(conservation_status)
    FROM animals
    GROUP BY conservation_status;
    ```

12. Connect to the `ls_burger` database. Write a query that returns the average burger cost for all orders that include fries.

    ```sql
    SELECT avg(burger_cost)
    FROM orders
    WHERE side = 'Fries';
    ```

13. Write a query that returns the cost of the cheapest side ordered.

    ```sql
    SELECT min(side_cost) FROM orders
    WHERE side IS NOT NULL;
    ```

14. Write a query that returns the number of orders that include Fries and the number of orders that include Onion Rings.

    ```sql
    SELECT count(id)
    FROM orders
    WHERE side = 'Fries' OR side = 'Onion Rings';
    ```

### Updating Data

* An `UPDATE` statement can be written with the following syntax:

  ```sql
  UPDATE table_name SET [column_name1 = value1, ...]
  WHERE (expression);
  ```

* This statement can be read as "Set column(s) to these values in a table when an expression evaluates to true." We can specify any table in our database to update and specify any number of columns within that table.
* The `WHERE` clause in the above syntax example is optional. If omitted, PostgreSQL will update **every** row in the target table, so before executing such a query be sure that this is actually what you want to do. Even when using a `WHERE` clause care must be taken to ensure that it is restrictive or specific enough to target only the rows that you want to modify. You can always test your `WHERE` clause in a `SELECT` statement to check which rows are being targeted, before using it in an `UPDATE` statement.

###### Update All Rows

* One thing we might want to do in our `users` table is disable all of our users at once, for example in response to a security issue.

  ```sql
  UPDATE users SET enabled = false;
  ```

###### Update Specific Rows

* Using a `WHERE` clause lets us update only the specific rows that meet the condition(s) set in that clause. We previously set all users to be disabled. Let's re-enable a few of those accounts.

  ```sql
  UPDATE users SET enabled = true
  WHERE full_name = 'Harry Potter' OR full_name = 'Jane Smith';
  ```

### Deleting Data

* Sometimes simply updating the data in a row isn't enough to fix a particular data discrepancy, and you need to remove that row altogether. This is where the `DELETE` statement comes in.

* The `DELETE` statement is used to remove entire rows from a database table. The form of a delete statement is somewhat similar to `UPDATE`:

  ```sql
  DELETE FROM table_name WHERE (expression);
  ```

* As with `UPDATE`, the `WHERE` clause in a `DELETE` statement is used to target specific rows. Let's try this out before moving on to look at how to delete all of the rows in a table.

###### Delete Specific Rows

* We resolted our duplicate 'Jane Smith' issue by updating the `full_name` of one of those rows, but we still have two Harry Potters. Let's delete one of the rows that contian our duplicate name.

  ```sql
  DELETE FROM users
  WHERE full_name='Harry Potter' AND id > 3;
  ```

###### Delete all Rows

* It's rare that you will want to delete all the rows in a table. If you did want to do this however, it can be done with a very simple statement. Just as with `UPDATE`, the `WHERE` clause in a `DELETE` statement is optional. If omitted, _all_ the rows in the table will be deleted.

  ```sql
  DELETE FROM users;
  ```

### Update vs Delete

* One key difference to keep in mind between how `UPDATE` works and how `DELETE` works: with `UPDATE` you can update one or more columns within one or more rows by using the `SET` clause; with `DELETE` you can only delete one or more _entire rows_, and not particular pieces of data from within those rows.

* Although it's not possible to _delete_ specific values within a row, we can approximate this by using `NULL`. You may remember in an earlier chapter we explained that `NULL` is a special value which actually represents an **unknown value**. By using an `UPDATE` statement to `SET` a specific value to `NULL`, although not _deleting_ it as such, we are effectively removing that value.

* This would be done in the form:

  ```sql
  UPDATE table_name SET column_name1 = NULL
  WHERE (expression);
  ```

* A couple of things to note here:
  * Unlike with a `WHERE` clause, with our `SET` clause we can use `=` with `NULL` since it's not being used as comparison operator in this situation.
  * If a column has a `NOT NULL` constraint, then it's not possible to set its value to `NULL`. An error will be thrown.

### Use Caution

* Even if you are using a `WHERE` clause in your `UPDATE` or `DELETE` statements it's sensible to be a bit cautious. It's typical to first do a `SELECT` to verify which rows you are targeting. You can then issue the `UPDATE` or `DELETE` with the same modifiers, being confident that you will only affect the rows that you intend to. It's rare to just issue an `UPDATE` or `DELETE` command without verifying first, and probably not a good idea. This is especially the case with `DELETE` since you will remove the entire row from the table.

### Summary of Commands

| Statement                                                    | Notes                                                        |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| UPDATE table_name SET [column_name1 = value1, ...] WHERE (expression); | Update specified fields within a table. The rows updated are dependent on the `WHERE` clause. We may update all rows by leaving out the `WHERE` clause. |
| DELETE FROM table_name WHERE (expression);                   | Delete rows in the specified table. Which rows are deleted is dependent on the `WHERE` clause. We may delete all rows by leaving out the `WHERE` clause. |

### Exercises

1. Make sure you are connected to the `encyclopedia` database. Add a column to the `animals` table called `class` to hold strings of up to 100 characters.

   Update all the rows in the table so that this column holds the value `Aves`.

   ```sql
   ALTER TABLE animals
   ADD COLUMN class varchar(100);
   
   UPDATE animals SET class = 'Aves';
   ```

2. Add two more columns to the `animals` table called `phylum` and `kingdom`. Both should hold strings of up to 100 characters.

   Update all the rows in the table so that `phylum` holds the value of `Chordata` and `kingdom` holds `Animalia` for all the rows in the table.

   ```sql
   ALTER TABLE animals
   ADD COLUMN phylum varchar(100);
   
   ALTER TABLE animals
   ADD COLUMN kingdom varchar(100);
   
   UPDATE animals SET phylum='Chordata', kingdom="Animalia";
   ```

3. Add a column to the `countries` table called `continent` to hold strings of up to 50 characters.

   Update all the rows in the table so France and German have a value of `Europe` for this column, Japan has a value of `Asia` and the USA has a value of `North America`.

   ```sql
   ALTER TABLE countries
   ADD COLUMN continent varchar(50);
   
   UPDATE countries SET continent='Europe'
   WHERE name = 'France' OR name = 'Germany';
   
   UPDATE countries SET continent='Asia'
   WHERE name = 'Japan';
   
   UPDATE countries SET continent = 'North America'
   WHERE name = 'USA';
   ```

4. In the `celebrities` table, update the Elvis row so that the value in the `deceased` column is true. Then change the column so that it no longer allows `NULL` values.  

   ```sql
   UPDATE celebrities SET deceased = true
   WHERE first_name = 'Elvis';
   
   ALTER TABLE celebrities
   ALTER COLUMN deceased SET NOT NULL;
   ```

5. Remove Tom Cruise from the `celebrities` table.

   ```sql
   DELETE FROM celebrities
   WHERE first_name = 'Tom';
   ```

6. Change the name of the `celebrities` table to `singers`, and remove anyone who isn't a singer.

   ```sql
   ALTER TABLE celebrities
   RENAME TO singers;
   
   DELETE FROM singers
   WHERE occupation NOT LIKE '%singer%';
   ```

7. Remove all the rows from the `countries` table.

   ```sql
   DELETE FROM countries;
   ```

8. Connect to the `ls_burger` database. Change the drink on James Bergman's order from a Cola to a Lemonade.

   ```sql
   UPDATE orders SET drink = 'Lemonade'
   WHERE customer_name = 'James Bergman';
   ```

9. Add Fries to Aaron Muller's order. Make sure to add the cost ($0.99) to the appropriate field and add 3 loyalty points to the current total.

   ```sql
   UPDATE orders
   SET side = 'Fries', side_cost = 0.99, customer_loyalty_points = 13
   WHERE customer_name = 'Aaron Muller';
   ```

10. The cost of Fries has increased to $1.20. Update the data in the table to reflect this.

    ```sql
    UPDATE orders
    SET side_cost = 1.20
    WHERE side = 'Fries';
    ```

### Table Relationships

* Thus far in this book, all the work we've done has been with a single database table. The majority of databases you'll work with as a developer will have more than one table, and those tables will be connected together in various ways to form table relationships. In this chapter we'll explore the reasons for having multiple tables in a database, look at how to define relationships between different tables, and outline the different types of table relationships that can exist.

### Normalization

* At this point, our `users` table doesn't need to hold that much data for each user in our system. It stores a name for the user, whether their account is enabled or not, when they last logged in, and an id to identify each user record. In reality, the requirements of our application will mean that we need to store a lot more data than that. Our app will be used to manage a library of SQL books and allow users to check out the books and also review them.
* To implement some of these requirements we could simply try adding more columns to our `users` table. But that's a lot of information in one table. There are other issues here as well, such as duplication of data (often referred to as 'redundancy'). For each book that a user checks out, we have to repeate all of the user data in our table.
* Duplicating data in this way can lead to issues with data integrity.
* How do we deal with this situation? The answer is to split our data up across multiple different tables, and create relationships between them. The process of splitting up data in this way to remove duplication and improve data integrity is known as _normalization_. 
* Normalization is a deep topic, and there are complex sets of rules which dictate the extent to which a database is judged to be *normalized*. These rule-sets, known as 'normal forms', are beyond the scope of this book; for now there are two important things to remember:
  - The *reason* for normalization is to reduce data redundancy and improve data integrity
  - The *mechanism* for carrying out normalization is arranging data in multiple tables and defining relationships between them

### Database Design

* At a high level, the process of database design involves defining **entities** to represent different sorts of data and designing **relationships** between those entities. But what do we mean by entities, and how do different entities relate to each other? Let's find out.

###### Entities

* An entity represents a real world object, or a set of data that we want to model within our database; we can often identify these as the major nouns of the system we're modeling. For the purposes of this book we're going to try and keep things simple and draw a direct correlation between an entity and a single table of data; in a real database however, the data for a single entity might be stored in more than one table.

###### Relationships

* We're making good progress with our database design. We've decided on the entities we want and have formed a picture of the tables we need, the columns in those tables, and even examples of the data that those columns will hold. There's something missing though, and that's the relationships between our entities.
* We can build simple Entity Relationship Diagrams, or **ERDs**, to graphically represent entities and their relationships to each other, and is a commonly used tool within database design.

### Keys

* Okay, so we now know the tables that we need and we've also defined the relationships that should exist between those tables in our ERD, but how do we actually implement those relationships in terms of our table schema? The answer to that is to use *keys*.
* In an earlier section of this book we looked at an aspect of schema called constraints, and explored how constraints act on and work with the data in our database tables. Keys are a special type of constraint used to establish relationships and uniqueness. They can be used to *identify a specific row in the current table*, or to *refer to a specific row in another table*. In this chapter we'll look at two types of keys that fulfil these particular roles: **Primary Keys**, and **Foreign Keys**.

###### Primary Keys

* A necessary part of establishing relationships between two entities or two pieces of data is being able to identify the *data* correctly. In SQL, uniquely identifying data is critical. **A Primary Key is a unique identifier for a row of data**.

* In order to act as a unique identifier, a column must contain some data, and that data should be unique to each row. If you're thinking that those requirements sound a lot like our `NOT NULL` and `UNIQUE` constraints, you'd be right; in fact, making a column a `PRIMARY KEY` is essentially equivalent to adding `NOT NULL` and `UNIQUE` constraints to that column.

* The `id` column in our `users` table has both of these constraints, and we've used that column in many of our `SELECT` queries in order to uniquely identify rows; we've effectively had `id` as a primary key all along although we haven't explicitly set it as the Primary Key. Let's do that now:

  ```sql
  ALTER TABLE users ADD PRIMARY KEY (id);
  ```

* Although any column in a table can have `UNIQUE` and `NOT NULL` constraints applied to them, each table can have only one Primary Key. It is common practice for that Primary Key to be a column named `id`. If you look at the other tables we've defined for our database, most of them have an `id` column. While a column of any name can serve as the primary key, using a column named `id` is useful for mnemonic reasons and so is a popular convention.
* Being able to uniquely identify a row of data in a table via that table's Primary Key column is only half the story when it comes to creating relationships between tables. The other half of this story is the Primary Key's partner, the Foreign Key.

###### Foreign Keys

* A Foreign Key allows us to associate a row in one table to a row in another table. This is done by setting a column in one table as a Foreign Key and having that column reference another table's Primary Key column. Creating this relationship is done using the `REFERENCES` keyword in this form:

  ```sql
  FOREIGN KEY (fk_col_name) REFERENCES target_table_name (pk_col_name)
  ```

* By setting up this reference, we're ensuring the *referential integrity* of a relationship. Referential integrity is the assurance that a column value within a record must reference an existing value; if it doesn't then an error is thrown. In other words, PostgreSQL won't allow you to add a value to the Foreign Key column of a table if the Primary Key column of the table it is referencing does not already contain that value. We'll discuss this concept in a bit more detail later on.

* The specific way in which a Foreign Key is used as part of a table's schema depends on the type of relationship we want to define between our tables. In order to implement that schema correctly it is useful to formally describe the relationships we need to model between our entities:

  1. A User can have **ONE** address. An address has only **ONE** user.
  2. A review can only be about **ONE** Book. A Book can have **MANY** reviews.
  3. A User can have **MANY** books that he/she may have checked out or returned. A Book can be/ have been checked out by **MANY** users.

  The entity relationships described above can be classified into three relationship types:

  - One to One
  - One to Many
  - Many to Many

### One-to-One

* A one-to-one relationship between two entities exists when a particular entity instance exists in one table, and it can have only one associated entity instance in another table.

* **Example:** A user can have only one address, and an address belongs to only one user.

* In the database world, this sort of relationship is implemented like this: the `id` that is the `PRIMARY KEY` of the `users` table is used as both the `FOREIGN KEY` _and_ `PRIMARY KEY` of the `addresses` table.

  ```sql
  /*
  one to one: User has one address
  */
  
  CREATE TABLE addresses (
    user_id int, -- Both a primary and foreign key
    street varchar(30) NOT NULL,
    city varchar(30) NOT NULL,
    state varchar(30) NOT NULL,
    PRIMARY KEY (user_id),
    FOREIGN KEY (user_id) REFERENCES users (id) ON DELETE CASCADE
  );
  ```

### Referential Integrity

* Referential integrity is a concept used when discussing relational data, and which states that table relationships must always be consistent. Different RDBMSes might enforce referential integrity rules differently, but the concept is the same.
* The constraints we've defined for our `addresses` table enforce the one to one relationship we want between it and our `users` table, whereby a user can only have one address and an address must have one, and only one, user. This is an example of _referential integrity_.

###### The ON DELETE clause

* Adding this clause to the `FOREIGN KEY` definition within a table creation statement, and setting it to `CASCADE` basically means that if the row being referenced is deleted, the row referencing it is also deleted. There are alternatives to `CASCADE` such as `SET NULL` or `SET DEFAULT` which instead of deleting the referencing row will set a new value in the appropriate column for that row.
* Determining what to do in situations where you delete a row that is referenced by another row is an important design decision, and is part of the concept of maintaining referential integrity. If we don't set such clauses we leave the decision of what to do up to the RDBMS we are using. In the case of PostgreSQL, if we try to delete a row that is being referenced by a row in another table and we have no `ON DELETE` clause for that reference, then an error will be thrown.

### One-to-Many

* A one-to-many relationship exists between two entities if an entity instance in one of the tables can be associated with multiple records (entity instances) in the other table. The opposite relationship does not exist; that is, each entity instance in the second table can only be associated with one entity instance in the first table.

* **Example:** A review belongs to only one book. A book has many reviews.

  ```sql
  CREATE TABLE books (
    id serial,
    title varchar(100) NOT NULL,
    author varchar(100) NOT NULL,
    published_date timestamp NOT NULL,
    isbn char(12),
    PRIMARY KEY (id),
    UNIQUE (isbn)
  );
  
  /*
   one to many: Book has many reviews
  */
  
  CREATE TABLE reviews (
    id serial,
    book_id integer NOT NULL,
    reviewer_name varchar(255),
    content varchar(255),
    rating integer,
    published_date timestamp DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (id),
    FOREIGN KEY (book_id) REFERENCES books(id) ON DELETE CASCADE
  );
  ```

* These table creation statements for our `books` and `reviews` tables are fairly similar to our previous example. There's a key difference worth pointing out in the statement for our `reviews` table however:
  
  - Unlike our `addresses` table, the `PRIMARY KEY` and `FOREIGN KEY` reference different columns, `id` and `book_id` respectively. This means that the `FOREIGN KEY` column, `book_id` is not bound by the `UNIQUE` constraint of our `PRIMARY KEY` and so the same value from the `id` column of the `books` table can appear in this column more than once. In other words a book can have many reviews.

### Many-to-Many

* A many-to-many relationship exists between two entities if for one entity instance there may be multiple records in the other table, and vice versa.

* **Example:** A user can check out many books. A book can be checked out by many users (over time).

* In order to implement this sort of relationship we need to introduce a third, cross-reference, table. This table holds the relationship between the two entities, by having **two** `FOREIGN KEY`s, each of which references the **PRIMARY KEY** of one of the tables for which we want to create this relationship. We already have our `books` and `users` tables, so we just need to create the cross-reference table: `checkouts`.

* Here, the `user_id` column in `checkouts` references the `id` column in `users`, and the `book_id` column in `checkouts` references the `id` column in `books`. Each row of the `checkouts` table uses these two Foreign Keys to create an association between rows of `users` and `books`.

* Let's create our `checkouts` table and add some data to it.

  ```sql
  CREATE TABLE checkouts (
    id serial,
    user_id int NOT NULL,
    book_id int NOT NULL,
    checkout_date timestamp,
    return_date timestamp,
    PRIMARY KEY (id),
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (book_id) REFERENCES books(id) ON DELETE CASCADE
  );
  ```

### Exercises

1. Make sure you are connected to the `encyclopedia` database. We want to hold the continent data in a separate table from the country data.
   1. Create a `continents` table with an auto-incrementing `id` column (set as the Primary Key), and a `continent_name` column which can hold the same data as the `continent` column from the `countries` table.
   2. Remove the `continent` column from the `countries` table.
   3. Add a `continent_id` column to the `countries` table of type integer.
   4. Add a Foreign Key constraint to the `continent_id` column which references the `id` field of the `continents` table.

   ```sql
   CREATE TABLE continents (
   id serial PRIMARY KEY,
   continent_name varchar(50)
   );
   
   ALTER TABLE countries DROP COLUMN continent;
   
   ALTER TABLE countries ADD COLUMN continent_id integer;
   
   ALTER TABLE countries ADD FOREIGN KEY (continent_id) REFERENCES continents(id);
   ```

2. Write queries to add data to the `countries` and `continents` tables so that the data below is correctly represented across the two tables. Add both the countries and the continents to their respective tables in alphabetical order.

   | Name    | Capital         | Population  | Continent     |
   | :------ | :-------------- | :---------- | :------------ |
   | France  | Paris           | 67,158,000  | Europe        |
   | USA     | Washington D.C. | 325,365,189 | North America |
   | Germany | Berlin          | 82,349,400  | Europe        |
   | Japan   | Tokyo           | 126,672,000 | Asia          |
   | Egypt   | Cairo           | 96,308,900  | Africa        |
   | Brazil  | Brasilia        | 208,385,000 | South America |

```sql
INSERT INTO continents (continent_name) VALUES
('Africa'),
('Asia'),
('Europe'),
('North America'),
('South America');

INSERT INTO countries (name, capital, population, continent_id) VALUES
('Brazil', 'Brasilia', 208385000, 5),
('Egypt', 'Cairo', 96308900, 1),
('France', 'Paris', 67158000, 3),
('Germany', 'Berlin', 82349400, 3),
('Japan', 'Tokyo', 126672000, 2),
('USA', 'Washington D.C.', 325365189, 4);
```

3. Examine the data below:

   | Album Name        | Released         | Genre                           | Label                     | Singer Name       |
   | :---------------- | :--------------- | :------------------------------ | :------------------------ | :---------------- |
   | Born to Run       | August 25, 1975  | Rock and roll                   | Columbia                  | Bruce Springsteen |
   | Purple Rain       | June 25, 1984    | Pop, R&B, Rock                  | Warner Bros               | Prince            |
   | Born in the USA   | June 4, 1984     | Rock and roll, pop              | Columbia                  | Bruce Springsteen |
   | Madonna           | July 27, 1983    | Dance-pop, post-disco           | Warner Bros               | Madonna           |
   | True Blue         | June 30, 1986    | Dance-pop, Pop                  | Warner Bros               | Madonna           |
   | Elvis             | October 19, 1956 | Rock and roll, Rhythm and Blues | RCA Victor                | Elvis Presley     |
   | Sign o' the Times | March 30, 1987   | Pop, R&B, Rock, Funk            | Paisley Park, Warner Bros | Prince            |
   | G.I. Blues        | October 1, 1960  | Rock and roll, Pop              | RCA Victor                | Elvis Presley     |

   We want to create an `albums` table to hold all the above data except the singer name, and create a reference from the `albums` table to the `singers` table to link each album to the correct singer. Write the necessary SQL statements to do this and to populate the table with data. Assume Album Name, Genre, and Label can hold strings up to 100 characters. Include an auto-incrementing `id` column in the `albums` table.

   ```sql
   ALTER TABLE singers
   ADD CONSTRAINT unique_id UNIQUE (id);
   
   CREATE TABLE albums (
   id serial PRIMARY KEY,
   album_name varchar(100),
   released date,
   genre varchar(100),
   label varchar(100),
   singer_id int,
   FOREIGN KEY (singer_id) REFERENCES singers(id)
   );
   ```

4. Connect to the `ls_burger` database. If you run a simple `SELECT` query to retreive all the data from the `orders` table, you will see it is very unnormalized. We have repetition of item names and costs and of customer data.

   ```sql
   ls_burger=# SELECT * FROM orders;
    id | customer_name  |         burger          |    side     |      drink      |     customer_email      | customer_loyalty_points | burger_cost | side_cost | drink_cost
   ----+----------------+-------------------------+-------------+-----------------+-------------------------+-------------------------+-------------+-----------+------------
     3 | Natasha O'Shea | LS Double Deluxe Burger | Onion Rings | Chocolate Shake | natasha@osheafamily.com |                      42 |        6.00 |      1.50 |       2.00
     2 | Natasha O'Shea | LS Cheeseburger         | Fries       |                 | natasha@osheafamily.com |                      18 |        3.50 |      1.20 |       0.00
     1 | James Bergman  | LS Chicken Burger       | Fries       | Lemonade        | james1998@email.com     |                      28 |        4.50 |      1.20 |       1.50
     4 | Aaron Muller   | LS Burger               | Fries       |                 |                         |                      13 |        3.00 |      1.20 |       0.00
   (4 rows)
   ```

   We want to break this table up into multiple tables. First of all create a `customers` table to hold the customer name data and an `email_addresses` table to hold the customer email data. Create a one-to-one relationship between them, ensuring that if a customer record is deleted so is the equivalent email address record. Populate the tables with the appropriate data from the current `orders` table.

   ```sql
   CREATE TABLE customers (
   id serial PRIMARY KEY,
   customer_name varchar(100)
   );
   
   CREATE TABLE email_addresses (
   customer_id integer PRIMARY KEY,
   customer_email varchar(50),
   FOREIGN KEY (customer_id)
   REFERENCES customers(id)
   ON DELETE CASCADE
   );
   
   INSERT INTO customers (customer_name)
   VALUES ('James Bergman'),
   ('Natasha O''Shea'),
   ('Aaron Muller');
   
   INSERT INTO email_addresses (customer_id, customer_email)
   VALUES (1, 'james1998@email.com'),
   (2, 'natasha@osheafamily.com');
   ```

5. We want to make our ordering system more flexible, so that customers can order any combination of burgers, sides and drinks. The first step towards doing this is to put all our products data into a separate table called `products`. Create a table and populate it with the following data:

   | Product Name            | Product Cost | Product Type | Product Loyalty Points |
   | :---------------------- | :----------- | :----------- | :--------------------- |
   | LS Burger               | 3.00         | Burger       | 10                     |
   | LS Cheeseburger         | 3.50         | Burger       | 15                     |
   | LS Chicken Burger       | 4.50         | Burger       | 20                     |
   | LS Double Deluxe Burger | 6.00         | Burger       | 30                     |
   | Fries                   | 1.20         | Side         | 3                      |
   | Onion Rings             | 1.50         | Side         | 5                      |
   | Cola                    | 1.50         | Drink        | 5                      |
   | Lemonade                | 1.50         | Drink        | 5                      |
   | Vanilla Shake           | 2.00         | Drink        | 7                      |
   | Chocolate Shake         | 2.00         | Drink        | 7                      |
   | Strawberry Shake        | 2.00         | Drink        | 7                      |

   The table should also have an auto-incrementing `id` column which acts as its `PRIMARY KEY`. The `product_type` column should hold strings of up to 20 characters. Other than that, the column types should be the same as their equivalent columns from the `orders` table.

   ```sql
   CREATE TABLE products (
   id serial PRIMARY KEY,
   product_name varchar(50),
   product_cost numeric(4,2) DEFAULT 0,
   product_type varchar(20),
   product_loyalty_points integer
   );
   
   INSERT INTO products (product_name, product_cost, product_type, product_loyalty_points)
   VALUES ('LS Burger', 3.00, 'Burger', 10),
   ('LS Cheeseburger', 3.50, 'Burger', 15),
   ('LS Chicken Burger', 4.50, 'Burger', 20),
   ('LS Double Deluxe Burger', 6.00, 'Burger', 30),
   ('Fries', 1.20, 'Side', 3),
   ('Onion Rings', 1.50, 'Side', 5),
   ('Cola', 1.50, 'Drink', 5),
   ('Lemonade', 1.50, 'Drink', 5),
   ('Vanilla Shake', 2.00, 'Drink', 7),
   ('Chocolate Shake', 2.00, 'Drink', 7),
   ('Strawberry Shake', 2.00, 'Drink', 7);
   ```

6. To associate customers with products, we need to do two more things:

   1. Alter or replace the `orders` table so that we can associate a customer with one or more orders (we also want to record an order status in this table).
   2. Create an `order_items` table so that an order can have one or more products associated with it.

   Based on the order descriptions below, amend and create the tables as necessary and populate them with the appropriate data.

   James has one order, consisting of a Chicken Burger, Fries, Onion Rings, and a Lemonade. It has a status of 'In Progress'.

   Natasha has two orders. The first consists of a Cheeseburger, Fries, and a Cola, and has a status of 'Placed'; the second consists of a Double Deluxe Burger, a Cheeseburger, two sets of Fries, Onion Rings, a Chocolate Shake and a Vanilla Shake, and has a status of 'Complete'.

   Aaron has one order, consisting of an LS Burger and Fries. It has a status of 'Placed'.

   Assume that the `order_status` field of the `orders` table can hold strings of up to 20 characters.

   ```sql
   DROP TABLE orders;
   
   CREATE TABLE orders (
   id serial PRIMARY KEY,
   customer_id integer,
   order_status varchar(20),
   FOREIGN KEY (customer_id)
   REFERENCES customers(id)
   ON DELETE CASCADE
   );
   
   CREATE TABLE order_items (
   id serial PRIMARY KEY,
   order_id integer,
   product_id integer,
   FOREIGN KEY (order_id)
   REFERENCES orders(id)
   ON DELETE CASCADE,
   FOREIGN KEY (product_id)
   REFERENCES products(id)
   ON DELETE CASCADE
   );
   
   INSERT INTO orders (customer_id, order_status)
   VALUES (1, 'In Progress'),
   (2, 'Placed'),
   (2, 'Complete'),
   (3, 'Placed');
   
   INSERT INTO order_items (order_id, product_id)
   VALUES (1, 3),
   (1, 5),
   (1, 6),
   (1, 8),
   (2, 2),
   (2, 5),
   (2, 7),
   (3, 4),
   (3, 2),
   (3, 5),
   (3, 5),
   (3, 6),
   (3, 10),
   (3, 9),
   (4, 1),
   (4, 5);
   ```

### What is a SQL Join?

* SQL handles queries across more than one table through the use of JOINs. JOINs are clauses in SQL statements that link two tables together, usually based on the keys that define the relationship between those two tables. There are several types of JOINs: INNER, LEFT OUTER, RIGHT OUTER, FULL OUTER and CROSS; they all do slightly different things, but the basic theory behind them all is the same. We'll take a look at each type of `JOIN` in turn, but first lets go over the general syntax that JOIN statements share.

### Join Syntax

* The general syntax of a `JOIN` statement is as follows:

  ```sql
  SELECT [table_name.column_name1, table_name.column_name2,..] FROM table_name1
  join_type JOIN table_name2 ON (join_condition);
  ```

* The first part of this:

  ```sql
  SELECT [table_name.column_name1, table_name.column_name2,..] FROM
  ```
  is essentially the `SELECT column_list FROM` form that you've already seen in previous SELECT queries, with the slight difference that column names are prepended by table names in the column list.

* Let's focus on the second part of the statement, the part that joins the tables:

  ```sql
  table_name1 join_type JOIN table_name2 ON (join_condition)
  ```

  To join one table to another, PostgreSQL needs to know several pieces of information:

  * The name of the first table to join;
  * The type of join to use;
  * The name of the second table to join;
  * The join condition.

  These pieces of information are combined together using the `JOIN` and `ON` keywords. The part of the statement that comes after the `ON` keyword is the _join condition_; this defines the logic by which a row in one table is joined to a row in another table. In most cases this join condition is created using the primary key of one table and the foreign key of the table we want to join it with.

### Types of Joins

* As mentioned earlier, a `JOIN` statement can come in various forms. To specify which type of join to use, you can add either `INNER`, `LEFT`, `RIGHT`, `FULL` or `CROSS` just before the keyword `JOIN`. We'll look at an example of each of those types of join using the tables in our `sql_book` database.  

###### INNER JOIN

* An `INNER JOIN` returns a result set that contains the common elements of the tables, i.e. the intersection where they match on the joined condition. INNER JOINs are the most frequently used JOINs; in fact if you don't specify a join type and simply use the `JOIN` keyword, then PostgreSQL will assume you want an inner join. Our `shapes` and `colors` example from earlier used an `INNER JOIN` in this way.

* In the query below, the line `INNER JOIN (addresses) ON (users.id = addresses.user_id)` creates the intersection between the two tables, which means that the join table contains only rows where there is a definite match between the values in the two columns used in the condition.

  ```sql
  SELECT users.*, addresses.*
  FROM users
  INNER JOIN addresses
  ON (users.id = addresses.user_id);
  ```

###### LEFT JOIN

* A LEFT JOIN or a LEFT OUTER JOIN takes all the rows from one table, defined as the `LEFT` table, and joins it with a second table. The `JOIN` is based on the conditions supplied in the parentheses. A `LEFT JOIN` will always include the rows from the `LEFT` table, even if there are no matching rows in the table it is JOINed with.

* Let's try and use the same JOIN query as before, but this time we'll use a left join:

  ```sql
  SELECT users.*, addresses.*
  FROM users
  LEFT JOIN addresses
  ON (users.id = addresses.user_id)
  ```

* Note that using either `LEFT JOIN` or `LEFT OUTER JOIN` does exactly the same thing, and the `OUTER` part is often omitted. Even so, it is still common to refer to this type of join as an 'outer' join in order to differentiate it from an 'inner' join. Another type of outer join is a `RIGHT JOIN`, which we'll look at next.

###### RIGHT JOIN

* A `RIGHT JOIN` is similar to a `LEFT JOIN` except that the roles between the two tables are reversed, and all the rows on the second table are included along with any matching rows from the first table. In the last chapter we mentioned that in our `sql_book` database we have books, and also reviews for those books. Not all of our books have reviews, however. Let's make a `RIGHT JOIN` or `RIGHT OUTER JOIN` that displays all reviews and their associated books, along with any books that don't have a review.

  ```sql
  SELECT reviews.book_id, reviews.content,
  reviews.rating, reviews.published_date,
  books.id, books.title, books.author
  FROM reviews RIGHT JOIN books ON (reviews.book_id = books.id);
  ```

###### FULL JOIN

* A `FULL JOIN` or `FULL OUTER JOIN` is essentially a combination of `LEFT JOIN` and `RIGHT JOIN`. This type of join contains all of the rows from both of the tables. Where the join condition is met, the rows of the two tables are joined, just as in the previous examples we've seen. For any rows on either side of the join where the join condition is not met, the columns for the _other_ table have `NULL` values for that row.
* A `FULL JOIN` is a little less common than the other ones we've looked at so far and so we won't show an example for this.
* Another uncommon type of join is a `CROSS JOIN`; let's take a look.

###### CROSS JOIN

* A `CROSS JOIN`, also known as a Cartesian JOIN, returns all rows from one table crossed with every row from the second table. In other words, the join table of a cross join contains every possible combination of rows from the tables that have been joined. Since it returns all combinations, a `CROSS JOIN` does not need to match rows using a join condition, therefore it does not have an `ON` clause.

* The way this join works is sometimes a little difficult to envisage, so it's worth looking at an example in this case. This SQL query has the similar syntax to other `JOIN`s, but without the `ON` clause:

  ```sql
  sql_book=# SELECT * FROM users CROSS JOIN addresses;
  ```

* The query above returns the `addresses` and `users` tables, cross joined. The result set consists of every record in `users` mapped to every record in `addresses`. For 4 users and 3 addresses, we get a total of `4x3=12` records. In mathematical terms, this is the _cross product_ of a set.
* In an application, it's very unlikely that you would use a `CROSS JOIN`. Most of the time, it's more important to match rows together through a join condition in order to return a meaningful result. It's still important to be aware of `CROSS JOIN`s however, since you may occassionally encounter them.

### Multiple Joins

* It is possible, and indeed common, to join more than just two tables together. This is done by adding additional `JOIN` clauses to your `SELECT` statement. To join multiple tables in this way, there must be a logical relationship between the tables involved. One example would be joining our `users`, `checkouts`, and `books` tables.

  ```sql
  SELECT users.full_name, books.title, checkouts.checkout_date
  FROM users
  INNER JOIN checkouts ON (users.id = checkouts.user_id)
  INNER JOIN books ON (books.id = checkouts.book_id);
  ```

* Here we are using two `INNER JOIN`s. One between `users` and `checkouts` and one between `books` and checkouts. In both cases the `JOIN` is implemented by using the Primary Key of one table (either `users` or `books`) and the Foreign Key for that table in the `checkouts` table.

### Aliasing

* You may have noticed that some of the queries we list above can get a bit long. We can cut back on the length of these queries by using aliasing. Aliasing allows us to specify another name for a column or table and then use that name in later parts of a query to allow for more concise syntax. Let's use our three table join from above as an example. Using aliasing, the query would look like this:

  ```sql
  SELECT u.full_name, b.title, c.checkout_date
  FROM users AS u
  INNER JOIN checkouts AS c ON (u.id = c.user_id)
  INNER JOIN books AS b ON (b.id = c.book_id);
  ```

* Here we specify single letter aliases for our tables, and use those aliases instead of our table names in order to prepend the columns from those tables in the column list and in the join conditions. This is commonly referred to as 'table aliasing'.
* We can even use a shorthand for aliasing by leaving out the `AS` keyword entirely. `FROM users u` and `FROM users AS u` are equivalent SQL clauses.

###### Column Aliasing

* Aliasing isn't just useful for shortening SQL queries. We can also use it to display more meaningful information in our result table. For instance, if we want to display the number of checkouts from the library we could write something like:

  ```sql
  sql_book=# SELECT count(id) AS "Number of Books Checked Out"
  sql_book-# FROM checkouts;
  Number of Books Checked Out
  ----------------------------
                            4
  (1 row)
  ```

* If we hadn't used aliasing above then we lose context about what was counted.

### Subqueries

* Imagine executing a `SELECT` query, and then using the results of that SELECT query as a condition in _another_ `SELECT` query. This is called nesting, and the query that is nested is referred to as a **subquery**.

* For example, suppose we need to select users that have no books checked out. We could do this by finding `users` whose `user_id` is not in the `checkouts` table. If no relation is found, that would mean that the user has not checked out any books.

  ```sql
  sql_book=# SELECT u.full_name FROM users u
  sql_book-# WHERE u.id NOT IN (SELECT c.user_id FROM checkouts c);
    full_name
  -------------
   Harry Potter
  (1 row)
  ```

* In the code above, the `NOT IN` clause compares the current `user_id` to all of the rows in the result of the subquery. If that `id` number isn't part of the subquery results, then the `full_name` for current row is added to the result set.
* **Subquery Expressions:** PostgreSQL provides a number of expressions that can be used specifically with sub-queries as `IN`, `NOT IN`, `ANY`, `SOME`, and `ALL`. These all work slightly differently, but essentially they all compare values to the results of a subquery.

###### Subqueries vs Joins

* As you write more queries, you may find that there is more than one way to write a query and achieve the same results. The most common choices are between subqueries and JOINs.
* When creating queries that return the same result, a differentiator between them may be their performance when compared to each other. As a general rule, JOINs are faster to run than subqueries. This may be something to bear in mind if working with large datasets.

### Summary

* One of the most important things to remember about **how joins work** is that we set a condition that compares a value from the first table (usually a primary key), with one from the second table (usually a foreign key). If the condition that uses these two values evaluates to true, then the row that holds the first value is joined with the row that holds the second value.

* Let's quickly recap on some of the different **types of joins** we can use:

  | Join Type |                            Notes                             |
  | :-------- | :----------------------------------------------------------: |
  | INNER     | Combines rows from two tables whenever the join condition is met. |
  | LEFT      | Same as an inner join, except rows from the first table are added to the join table, regardless of the evaluation of the join condition. |
  | RIGHT     | Same as an inner join, except rows from the second table are added to the join table, regardless of the evaluation of the join condition. |
  | FULL      |          A combination of left join and right join.          |
  | CROSS     | Doesn't use a join condition. The join table is the result of matching every row from the first table with the second table, the cross product of all rows across both tables. |

### Exercises

1. Connect to the `encyclopedia` database. Write a query to return all of the country names along with their appropriate continent names.

   ```sql
   SELECT countries.name, continents.continent_name
   FROM countries
   INNER JOIN continents ON (countries.continent_id = continents.id);
   ```

2. Write a query to return all of the names and capitals of the European countries.

   ```sql
   SELECT countries.name, countries.capital
   FROM countries
   JOIN continents ON (countries.continent_id = continents.id)
   WHERE continents.continent_name = 'Europe';
   ```

3. Write a query to return the first name of any singer who had an album released under the Warner Bros label.

   ```sql
   SELECT DISTINCT singers.first_name
   FROM singers JOIN albums
   ON singers.id = albums.singer_id
   WHERE albums.label LIKE '%Warner Bros%';
   ```

4. Write a query to return the first name and last name of any singer who released an album in the 80s and who is still living, along with the names of the album that was released and the release date. Order the results by the singer's age (youngest first).

   ```sql
   SELECT singers.first_name, singers.last_name, albums.album_name, albums.released
   FROM singers
   JOIN albums
   ON singers.id = albums.singer_id
   WHERE (date_part('year', albums.released) >= '1980' AND date_part('year', albums.released) < '1990')
   AND singers.deceased = 'false'
   ORDER BY age(singers.date_of_birth);
   ```

5. Write a query to return the first name and last name of any singer without an associated album entry.

   ```sql
   SELECT singers.first_name, singers.last_name
   FROM singers
   WHERE singers.id NOT IN (SELECT albums.singer_id FROM albums);
   
   # or...
   
   SELECT singers.first_name, singers.last_name
   FROM singers LEFT JOIN albums
   ON singers.id = albums.singer_id
   WHERE albums.id IS NULL;
   ```

6. Rewrite the query for the last question as a sub-query.

   ```sql
   SELECT first_name, last_name
   FROM singers
   WHERE id NOT IN (SELECT singer_id FROM albums);
   ```

7. Connect to the `ls_burger` database. Return a list of all orders and their associated product items.

   ```sql
   SELECT orders.id, products.product_name
   FROM order_items
   JOIN orders ON (order_items.order_id = orders.id)
   JOIN products ON (order_items.product_id = products.id);
   ```

8. Return the id of any order that includes Fries. Use table aliasing in your query.

   ```sql
   SELECT o.order_id FROM order_items AS o
   JOIN products AS p
   ON o.product_id = p.id
   WHERE p.product_name = 'Fries';
   ```

9. Build on the query from the previous question to return the name of any customer who ordered fries. Return this in a column called 'Customers who like Fries.' Don't repeat the same customer name more than once in the results.

   ```sql
   SELECT DISTINCT c.customer_name AS "Customers who like Fries"
   FROM customers AS c
   JOIN orders AS o ON c.id = o.customer_id
   JOIN order_items AS oi ON o.id = oi.order_id
   JOIN products AS p ON oi.product_id = p.id
   WHERE p.product_name = 'Fries';
   ```

10. Write a query to return the total cost of Natasha O'Shea's orders.

    ```sql
    SELECT sum(products.product_cost) FROM products
    JOIN order_items ON products.id = order_items.product_id
    JOIN orders ON order_items.order_id = orders.id
    JOIN customers ON orders.customer_id = customers.id
    WHERE customer_name = 'Natasha O''Shea';
    ```

11. Write a query to return the name of every product included in an order alongside the number of times it has been ordered.

    ```sql
    SELECT products.product_name, count(order_items.product_id) 
    FROM products
    JOIN order_items ON products.id = order_items.product_id
    GROUP BY products.product_name;
    ```

    