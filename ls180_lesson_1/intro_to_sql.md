##### LS180 Database Foundations > Launch School Bookshelf > Introduction to SQ

---

## Introduction to SQL

### Structured Data

*  Structured data attempts to solve the problem of the limitations of unstructured data.
* One way to structure data is by using a tabular format of of rows and columns, like a spreadsheet.
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

