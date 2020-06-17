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

