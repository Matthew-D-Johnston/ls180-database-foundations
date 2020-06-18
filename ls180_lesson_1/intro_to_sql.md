##### LS180 Database Foundations > Launch School Bookshelf > Introduction to SQL

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

   

