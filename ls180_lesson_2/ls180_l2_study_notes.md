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

   

