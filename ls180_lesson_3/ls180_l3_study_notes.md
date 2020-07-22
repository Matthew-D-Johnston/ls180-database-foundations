##### LS180 Database Foundations > Relational Data and JOINs

---

### What is Relational Data?

---

* Relational databases are called **relational** because they persist data in a set of **relations**. What is a relation? A table, which is a set of columns and rows of data, is a relation. PostgreSQL also exposes some other objects as relations, such as sequences and views (which we aren't going to cover in this course). If you can use something in the `FROM` clause in a `SELECT` statement, it's probably a relation.
* The terminology can become confusing, though, when the concept of **relationships** enter a discussion. The words _relation_ and _relationship_ are visually similar, but they name two distinct things within a database. A **relationship** is a connection between entities (rows of data), usually resulting from what those entities represent and how they are related to one another. For example, a **customers** table in a database would probably have a relationship with another table called **orders**.
* To summarize:
  * A **relation** is _usually_ another way to say "table".
  * A **relationship** is an association between the data stored in those relations.
* For a developer, relational data can be translated into a more functional definition: **working with more than one table at a time**. There are many reasons to break data up into multiple tables, and considerations about how tables and their keys and constraints interact affects how rows are retrieved, inserted, updated, and deleted. In this lesson, we will be focusing on situations involving more than one table in order to illustrate many of these topics.

---

### Database Diagrams: Levels of Schema

---

* We can basically think of there being three different levels of schema (i.e. levels of abstraction)
  1. conceptual: bigger objects and higher level concepts; thinking about the data in a very abstract way.
  2. logical: combination of the conceptual and physical levels; often contains a list of all the attributes and their datatypes but in a way that is not specific to any particular database.
  3. physical: concerned primarily with the database specific implementation of a conceptual model; contains all of the different attributes that entity can hold, the data types of those entities, rules about how those different entities and their attributes relate to each other.
* The lower the level, the more detailed.
* **conceptual schema**: high-level design focused on identifying entities and their relationships.
* **entity-relationship** model: a conceptual schema.
* Different types of relationships:
  1. one-to-one
  2. one-to-many
  3. many-to-many

#### Practice Problems

1. What are the three levels of schema?

   ###### My Response:

   Conceptual, logical, and physical.

2. What is a conceptual schema?

   ###### My Response:

   A conceptual schema is a high-level design focused on identifying entities and their relationships.

3. What is a physical schema?

   ###### My Response:

   A physical schema is a low-level design focused on the actual implementation of the relationships between entities. It includes specific information on the attributes and datatypes related to specific entities.

   ###### LS Response:

   A low-level database-specific design focused on implementation.

4. What are the three types of relationships that can be shown in a database diagram?

   1. one-to-one
   2. one-to-many
   3. many-to-many

---

### Database Diagrams: Cardinality and Modality

---

* **cardinality:** the number of objects on each side of the relationship (1:1, 1:M, M:M).
* **modality:** if the relationship is required (1) or optional (0).

#### Practice Problems

1. What is _cardinality_?

   Cardinality describes the number of objects on each side of the relationship.

2. What is _modality_?

   Modality describes whether the relationship is required or optional.

3. If one side of a relationship has a modality of 1, what is the smallest number of instances that can be on that side of the relationship?

   One.

4. What type of notation was described in depth throughout most of the video?

   Crow's foot notation.

---

### Working with Multiple Tables

---

1. Import [this file](https://raw.githubusercontent.com/launchschool/sql_course_data/master/sql-and-relational-databases/relational-data-and-joins/working-with-multiple-tables/theater_full.sql) into an empty PostgreSQL database. Note: the file contains a lot of data and may take a while to run; your terminal should return to the command prompt once the import is complete.

   ###### My Response:

   ```
   $ psql -d sql-course < theater_full.sql
   ```

2. Write a query that determines how many tickets have been sold.

   **Expected Output**

   ```
   count
   -------
   3783
   (1 row)
   ```

   ###### My Response:

   ```sql
   SELECT count(id) FROM tickets;
   ```

   ###### LS Response:

   ```sql
   SELECT COUNT(*) FROM tickets;
   ```

3. Write a query that determines how many different customers purchased tickets to at least one event.

   ###### My Response:

   ```sql
   SELECT COUNT(DISTINCT customer_id) FROM tickets;
   ```

   ###### LS Response:

   Same.

4. Write a query that determines what percentage of the customers in the database have purchased a ticket to one or more of the events.

   ###### My Response:

   ```sql
   SELECT ROUND((COUNT(DISTINCT tickets.customer_id)::decimal / COUNT(DISTINCT customers.id)::decimal * 100), 2) AS percent
   	FROM customers LEFT JOIN tickets
   		ON (tickets.customer_id = customers.id); 
   ```

   ###### LS Response:

   ```sql
   SELECT COUNT(DISTINCT tickets.customer_id)
   			 / COUNT(DISTINCT customers.id)::float * 100
   			 AS percent
     FROM customers
     LEFT OUTER JOIN tickets
     	ON tickets.customer_id = customers.id;
   ```

5. Write a query that returns the name of each event and how many tickets were sold for it, in order from popular to least popular.

   ###### My Response:

   ```sql
   SELECT events.name, count(tickets.id) AS popularity
   	FROM events JOIN tickets ON (events.id = tickets.event_id)
    GROUP BY events.name
    ORDER BY popularity DESC;
   ```

   ###### LS Response:

   ```sql
   SELECT e.name, COUNT(t.id) AS popularity
   	FROM events AS e
   	LEFT OUTER JOIN tickets AS t
   		ON t.event_id = e.id
    GROUP BY e.id
    ORDER BY popularity DESC;
   ```

6. Write a query that returns the user id, email address, and number of events for all customers that have purchased tickets to three events.

   **Expected Output**

   ```
     id   |                email                 | count
   -------+--------------------------------------+-------
     141  | isac.hayes@herzog.net                |     3
     326  | tatum.mraz@schinner.org              |     3
     624  | adelbert.yost@kleinwisozk.io         |     3
     1719 | lionel.feeney@metzquitzon.biz        |     3
     2058 | angela.ruecker@reichert.co           |     3
     3173 | audra.moore@beierlowe.biz            |     3
     4365 | ephraim.rath@rosenbaum.org           |     3
     6193 | gennaro.rath@mcdermott.co            |     3
     7175 | yolanda.hintz@binskshlerin.com       |     3
     7344 | amaya.goldner@stoltenberg.org        |     3
     7975 | ellen.swaniawski@schultzemmerich.net |     3
     9978 | dayana.kessler@dickinson.io          |     3
   (12 rows)
   ```

   ###### My Response:

   ```sql
   SELECT c.id, c.email, COUNT(t.customer_id)
     FROM customers AS c
    LEFT OUTER JOIN tickets AS t
     	ON c.id = t.customer_id
    GROUP BY c.id
    HAVING COUNT(t.event_id) = 3
     ORDER BY c.id;
   ```

   ###### LS Response:

   ```sql
   SELECT customers.id, customers.email, COUNT(DISTINCT tickets.event_id)
   	FROM customers
   	INNER JOIN tickets
   		on tickets.customer_id = customers.id
   	GROUP BY customers.id
   	HAVING COUNT(DISTINCT tickets.event_id) = 3;
   ```

7. Write a query to print out a report of all tickets purchased by the customer with the email address 'gennaro.rath@mcdermott.co'. The report should include the event name and starts_at and the seat's section name, row, and seat number.

   **Expected Output**

   ```
           event        |      starts_at      |    section    | row | seat
   --------------------+---------------------+---------------+-----+------
    Kool-Aid Man       | 2016-06-14 20:00:00 | Lower Balcony | H   |   10
     Kool-Aid Man       | 2016-06-14 20:00:00 | Lower Balcony | H   |   11
     Green Husk Strange | 2016-02-28 18:00:00 | Orchestra     | O   |   14
     Green Husk Strange | 2016-02-28 18:00:00 | Orchestra     | O   |   15
     Green Husk Strange | 2016-02-28 18:00:00 | Orchestra     | O   |   16
     Ultra Archangel IX | 2016-05-23 18:00:00 | Upper Balcony | G   |    7
     Ultra Archangel IX | 2016-05-23 18:00:00 | Upper Balcony | G   |    8
   (7 rows)
   ```
   
   ###### My Response:
   
   ```sql
   SELECT events.name AS event,
   			 events.starts_at,
   			 sections.name AS section,
   			 seats.row,
   			 seats.number AS seat
   	FROM tickets
   	JOIN events ON events.id = tickets.event_id
   	JOIN seats ON seats.id = tickets.seat_id
   	JOIN customers ON customers.id = tickets.customer_id
   	JOIN sections ON seats.section_id = sections.id
    WHERE customers.email = 'gennaro.rath@mcdermott.co';
   ```
   
   ###### LS Response:
   
   ```sql
   SELECT events.name AS event,
   			 events.starts_at,
   			 sections.name AS section,
   			 seats.row,
   			 seats.number AS seat
   	FROM tickets
   	INNER JOIN events
   		ON tickets.event_id = events.id
   	INNER JOIN customers
   		ON tickets.customer_id = customers.id
   	INNER JOIN seats
   		ON tickets.seat_id = seats.id
   	INNER JOIN sections
   		ON seats.section_id = sections.id
   	WHERE customers.email = 'gennaro.rath@mcdermott.co';
   ```

---

### Using Foreign Keys

---

* In database parlance, a _foreign key_ can refer to two different, but related, things:
  * A column that represents a relationship between two rows by pointing to a specific row in another table using its _primary key_. A complete name for these columns is _foreign key column_.
  * A constraint that enforces certain rules about what values are permitted in these foreign key relationships. A complete name for this type of constraint is _foreign key constraint_.

###### Creating Foreign Key Columns

* To create a foreign key _column_, just create a column of the same type as the primary key column it will point to. Since the `products` table shown above uses an `integer` type for its primary key column, `orders.product_id` is also an `integer` column.

###### Creating Foreign Key Constraints

* To create a foreign key _constraint_, there are two syntaxes that can be used. The first is to add a `REFERENCES` clause to the description of a column in a `CREATE TABLE` statement:

  ```sql
  CREATE TABLE orders (
  	id serial PRIMARY KEY,
  	product_id integer REFERENCES products (id),
  	quantity integer NOT NULL
  );
  ```

* The second way is to add the foreign key constraint separately, just as you would any other constraint (note the use of `FOREIGN KEY` instead of `CHECK`):

  ```sql
  ALTER TABLE orders ADD CONSTRAINT orders_product_id_fkey FOREIGN KEY (product_id) REFERENCES products(id);
  ```

###### Referential Integrity

* One of the main benefits of using the foreign key constraints provided by a relational database is to preserve the _referential integrity_ of the data in the database. The database does this by ensuring that every value in a foreign key column exists in the primary key column of the referenced table. Attempts to insert rows that violate the table's constraints will be rejected.

#### Practice Problems

1. Import [this file](https://raw.githubusercontent.com/launchschool/sql_course_data/master/sql-and-relational-databases/relational-data-and-joins/foreign-keys/orders_products1.sql) into a new database.

   ###### My Response:

   ```
   $ psql -d sql-course < orders_products1.sql
   ```

   ###### LS Response:

   ```
   $ createdb foreign-keys
   $ psql -d foreign-keys < orders_products1.sql
   ```

2. Update the `orders` table so that referential integrity will be preserved for the data between `orders` and `products`.

   ###### My Response:

   ```sql
   ALTER TABLE orders ADD CONSTRAINT orders_product_id_fkey FOREIGN KEY (product_id) REFERENCES products(id);
   ```

   ###### LS Response:

   Same.

3. Use `psql` to insert the data shown in the following table into the database:

   | Quantity | Product    |
   | :------- | :--------- |
   | 10       | small bolt |
   | 25       | small bolt |
   | 15       | large bolt |

   ###### My Response:

   ```sql
   INSERT INTO products (name)
   VALUES ('small bolt'),
   ('large bolt');
   
   INSERT INTO orders (product_id, quantity)
   VALUES (1, 10),
   (1, 25),
   (2, 15);
   ```

   ###### LS Response:

   ```sql
   INSERT INTO products (name) VALUES ('small bolt');
   INSERT INTO products (name) VALUES ('large bolt');
   SELECT * FROM products;
   
   INSERT INTO orders (product_id, quantity) VALUES (1, 10);
   INSERT INTO orders (product_id, quantity) VALUES (1, 25);
   INSERT INTO orders (product_id, quantity) VALUES (2, 15);
   ```

4. Write a SQL statement that returns a result like this:

   ```
    quantity |    name
   ----------+------------
          10 | small bolt
          25 | small bolt
          15 | large bolt
   (3 rows)
   ```

   ###### My Response:

   ```sql
   SELECT o.quantity, p.name
   	FROM orders AS o
   	JOIN products AS p
   		ON p.id = o.product_id;
   ```

   ###### LS Response:

   ```sql
   SELECT quantity, name FROM orders INNER JOIN products ON orders.product_id = products.id;
   ```

5. Can you insert a row into `orders` without a `product_id`? Write a SQL statement to prove your answer.

   ###### My Response:

   I didn't know, so I wrote this statement:

   ```sql
   INSERT INTO orders (quantity)
   VALUES (30);
   ```

   It added a row to the `orders` table, so the answer is yes, you can insert a row into `orders` without a `product_id`.

   ###### LS Response:

   Yes:

   ```sql
   INSERT INTO orders (quantity) VALUES (42);
   INSERT 0 1
   ```

6. Write a SQL statement that will prevent NULL values from being stored in `orders.product_id`. What happens if you execute that statement?

   ###### My Response:

   ```sql
   ALTER TABLE orders ALTER COLUMN product_id SET NOT NULL;
   ```

   If I execute that statement it throws up an error: `ERROR: column "product_id" contains null values`

   ###### LS Response:

   ```sql
   ALTER TABLE orders ALTER COLUMN product_id SET NOT NULL;
   ERROR: column "product_id" contains null values
   ```

7. Make any changes needed to avoid the error message encountered in #6.

   ###### My Response:

   ```sql
   DELETE FROM orders
    WHERE product_id IS NULL;
    
   ALTER TABLE orders ALTER COLUMN product_id SET NOT NULL;
   ```

   ###### LS Response:

   ```sql
   DELETE FROM orders WHERE id = 4;
   
   ALTER TABLE orders ALTER COLUMN product_id SET NOT NULL;
   ```

8. Create a new table called `reviews` to store the data shown below. This table should include a primary key and a reference to the `products` table.

   ###### My Response:

   ```sql
   CREATE TABLE reviews (
     id serial PRIMARY KEY,
     product_id integer REFERENCES products (id),
   	product varchar(25),
   	review varchar(50)
   );
   ```

   ###### LS Response:

   ```sql
   CREATE TABLE reviews (
   	id serial PRIMARY KEY,
   	body text NOT NULL,
   	product_id integer REFERENCES products (id)
   );
   ```

9. Write SQL statements to insert the data shown in the table in #8.

   ###### My Response:

   ```sql
   INSERT INTO reviews (body, product_id)
   VALUES ('a little small', 1),
   ('very round!', 1),
   ('could have been smaller', 2);
   ```

   ###### LS Response:

   ```sql
   INSERT INTO reviews (product_id, body) VALUES (1, 'a little small');
   INSERT INTO reviews (product_id, body) VALUES (1, 'very round!');
   INSERT INTO reviews (product_id, body) VALUES (2, 'could have been smaller');
   ```

10. **True** or **false**: A foreign key constraint prevents NULL values from being stored in a column.

    ###### My Response:

    false.

    ###### LS Response:

    **False**. As we saw above, foreign key columns allow NULL values. As a result, it is often necessary to use `NOT NULL` and a foreign key constraint together.

---

### One to Many Relationships

---

* **update anomaly:** an update that makes a database inconsistent, which means that it contains more than one answer for a given question.
* **insertion anomaly:** inability to store information in certain fields unless some other condition has been met.
* **deletion anomaly**: the deletion of certain information that we would like to persist indefinitely when we delete other information that we have know need of.
* **normalization:** the process of designing schema that minimizes or eliminate the possible occurrence of these anomalies. The basic procedure of normalization involves extracting data into additional tables and using foreign keys to tie it back to its associated data.
* Recall that a **foreign key column** is a column that stores references to a primary key column elsewhere in a database. Foreign keys usually point to other tables, but there are cases where they will point to rows in the same table.

#### Practice Problems

1. Write a SQL statement to add the following call data to the database:

   | when                | duration | first_name | last_name | number     |
   | :------------------ | :------- | :--------- | :-------- | :--------- |
   | 2016-01-18 14:47:00 | 632      | William    | Swift     | 7204890809 |

   ###### My Response:

   ```sql
   INSERT INTO calls ("when", duration, contact_id)
   VALUES ('2016-01-18 14:47:00', 632, 6);
   ```

   ###### LS Response:

   Same.

2. Write a SQL statement to retrieve the call times, duration, and first name for all calls **not** made to William Swift.

   ###### My Response:

   ```sql
   SELECT "when", duration, first_name
   	FROM calls
    INNER JOIN contacts
    		ON calls.contact_id = contacts.id
    WHERE first_name <> 'William' AND last_name <> 'Swift';
   ```

   ###### LS Response:

   ```sql
   SELECT calls.when, calls.duration, contacts.first_name
   FROM calls INNER JOIN contacts ON calls.contact_id = contacts.id
   WHERE (contacts.first_name || ' ' || contacts.last_name) != 'William Swift';
   ```

3. Write SQL statements to add the following call data to the database:

   | when                | duration | first_name | last_name | number     |
   | :------------------ | :------- | :--------- | :-------- | :--------- |
   | 2016-01-17 11:52:00 | 175      | Merve      | Elk       | 6343511126 |
   | 2016-01-18 21:22:00 | 79       | Sawa       | Fyodorov  | 6125594874 |

   ###### My Response:

   ```sql
   INSERT INTO contacts (first_name, last_name, number)
   VALUES ('Merve', 'Elk', '6343511126'),
   ('Sawa', 'Fyodorov', '6125594874');
   
   INSERT INTO calls ("when", duration, contact_id)
   VALUES ('2016-01-17 11:52:00', 175, 26),
   ('2016-01-18 21:22:00', 79, 27);
   ```

   ###### LS Response:

   ```sql
   INSERT INTO contacts (first_name, last_name, number) VALUES ('Merve', 'Elk', '6343511126');
   INSERT INTO calls ("when", duration, contact_id) VALUES ('2016-01-17 11:52:00', 175, 26);
   
   INSERT INTO contacts (first_name, last_name, number) VALUES ('Sawa', 'Fyodorov', '612559487');
   INSERT INTO contacts ("when", duration, contact_id) VALUES ('2016-01-18 21:22:00', 79, 27);
   ```

4. Add a constraint to **contacts** that prevents a duplicate value being added in the column `number`.

   ###### My Response:

   ```sql
   ALTER TABLE contacts ADD UNIQUE number;
   ```

   ###### LS Response:

   ```sql
   ALTER TABLE contacts ADD CONSTRAINT number_unique UNIQUE (number);
   ```

5. Write a SQL statement that attempts to insert a duplicate number for a new contact but fails. What error is shown?

   ###### My Response:

   ```sql
   INSERT INTO contacts (first_name, last_name, number)
   VALUES ('Matt', 'Johnston', '6343511126');
   ```

   Produces the following error:

   ```
   ERROR:  duplicate key value violates unique constraint "contacts_number_key"
   DETAIL:  Key (number)=(6343511126) already exists.
   ```

   ###### LS Response:

   ```sql
   one-to-many=# INSERT INTO contacts (first_name, last_name, number) VALUES ('Nivi', 'Petrussen', '6125594874');
   ERROR:  duplicate key value violates unique constraint "number_unique"
   DETAIL:  Key (number)=(6125594874) already exists.
   ```

6. Why does "when" need to be quoted in many of the queries in this lesson?

   ###### My Response:

   Because it is a keyword.

   ###### LS Response:

   It is a reserved word. For a full list, see http://www.postgresql.org/docs/9.5/static/sql-keywords-appendix.html. Note that there are words on that list that are "non-reserved", which means they are allowed as table or column names but are known to the SQL parser as having a special meaning.  

   As a general rule, if you get spurious parser errors for commands that contain any of the listed key words as an identifier you should try to quote the identifier to see if the problem goes away.

7. Draw an entity-relationship diagram for the data we've been working with in this assignment.

   ###### My Response:

   See notebook.

---

### Many to Many Relationships

---

* **Many-to-many** relationships are those where there can be multiple instances on both sides of the relationship. You can think of them as one-to-many relationships that go from the first table to the second **and** from the second table to the first.
* For two tables that have a many-to-many relationship between them, we need a third table that is responsible for storing information about the relationships between the other two. Such a table that is used to persist the state of many-to-many relationships is called a _join table_.

#### Practice Problems

1. Write a SQL statement that will return the following result:

   ```psql
    id |     author      |           categories
   ----+-----------------+--------------------------------
     1 | Charles Dickens | Fiction, Classics
     2 | J. K. Rowling   | Fiction, Fantasy
     3 | Walter Isaacson | Nonfiction, Biography, Physics
   (3 rows)
   ```

   ###### My Solution:

   ```sql
   SELECT books.id, books.author, string_agg(categories.name, ', ') AS categories
   	FROM books
   	JOIN books_categories ON books.id = books_categories.book_id
   	JOIN categories ON books_categories.category_id = categories.id
    GROUP BY books.id
    ORDER BY books.id;
   ```

   ###### LS Solution:

   ```sql
   SELECT books.id, books.author, string_agg(categories.name, ', ') AS categories
   	FROM books
   		INNER JOIN books_categories ON books.id = books_categories.book_id
   		INNER JOIN categories ON books_categories.category_id = categories.id
   	GROUP BY books.id ORDER BY books.id;
   ```

2. Write SQL statements to insert the following new books into the database. What do you need to do to ensure this data fits in the database?

   | Author                        | Title                                      | Categories                               |
   | :---------------------------- | :----------------------------------------- | :--------------------------------------- |
   | Lynn Sherr                    | Sally Ride: America's First Woman in Space | Biography, Nonfiction, Space Exploration |
   | Charlotte Brontë              | Jane Eyre                                  | Fiction, Classics                        |
   | Meeru Dhalwala and Vikram Vij | Vij's: Elegant and Inspired Indian Cuisine | Cookbook, Nonfiction, South Asia         |

   ###### My Solution:

   I first need to insert the categories that are not already in the `categories` table.

   ```sql
   INSERT INTO categories (name)
   VALUES ('Space Exploration'),
   ('Cookbook'),
   ('South Asia');
   ```

   Then I need to add the data for the additional books to the `books` table.

   ```sql
   INSERT INTO books (author, title)
   VALUES ('Lynn Sherr', 'Sally Ride: America''s First Woman in Space'),
   ('Charlotte Brontë', 'Jane Eyre'),
   ('Meeru Dhalwala and Vikram Vij', 'Vij''s: Elegant and Inspired Indian Cuisine');
   ```

   Turns out I need to modify the number of characters allowed in the `title` column.

   ```sql
   ALTER TABLE books
   ALTER COLUMN title
    TYPE varchar(50);
   ```

   Then I need to populate the `books_categories` tables with the appropriate ids from the `books` table and the `categories` table.

   ```sql
   INSERT INTO books_categories (book_id, category_id)
   VALUES (4, 1), (4, 5), (4, 7),
   (5, 2), (5, 4),
   (6, 1), (6, 8), (6, 9);
   ```

3. Write a SQL statement to add a uniqueness constraint on the combination of columns `book_id` and `category_id` of the `books_categories` table. This contraint should be a table constraint; so, it should check for uniqueness on the combination of `book_id` and `category_id` across all rows of the `books_categories` table.

   ###### My Solution:

   ```sql
   CREATE UNIQUE INDEX unique_book_category_ids ON books_categories (book_id, category_id);
   ```

   ###### LS Solution:

   ```sql
   ALTER TABLE books_categories ADD UNIQUE (book_id, category_id);
   ```

4. Write a SQL statement that will return the following result:

   ```psql
         name        | book_count |                                 book_titles
   ------------------+------------+-----------------------------------------------------------------------------
   Biography         |          2 | Einstein: His Life and Universe, Sally Ride: America's First Woman in Space
   Classics          |          2 | A Tale of Two Cities, Jane Eyre
   Cookbook          |          1 | Vij's: Elegant and Inspired Indian Cuisine
   Fantasy           |          1 | Harry Potter
   Fiction           |          3 | Jane Eyre, Harry Potter, A Tale of Two Cities
   Nonfiction        |          3 | Sally Ride: America's First Woman in Space, Einstein: His Life and Universe, Vij's: Elegant and Inspired Indian Cuisine
   Physics           |          1 | Einstein: His Life and Universe
   South Asia        |          1 | Vij's: Elegant and Inspired Indian Cuisine
   Space Exploration |          1 | Sally Ride: America's First Woman in Space
   ```

   ###### My Solution:

   ```sql
   SELECT categories.name, count(books.title) AS book_count, string_agg(books.title, ', ') AS book_titles
    	FROM categories
    INNER JOIN books_categories ON categories.id = books_categories.category_id
    INNER Join books ON books_categories.book_id = books.id
    GROUP BY categories.name
    ORDER BY categories.name;
   ```

   ###### LS Solution:

   ```sql
   SELECT categories.name, count(books.id) AS book_count, string_agg(books.title, ', ') AS book_titles
   	FROM books
   		INNER JOIN books_categories ON books.id = books_categories.book_id
   		INNER JOIN categories ON books_categories.category_id = categories.id
   	GROUP BY categories.name ORDER BY categories.name;
   ```

---

### Converting a 1:M Relationship to a M:M Relationship

---

#### Practice Problems

1. Import [this file](https://raw.githubusercontent.com/launchschool/sql_course_data/master/sql-and-relational-databases/relational-data-and-joins/converting-a-1m-relationship-to-a-mm-relationship/films7.sql) into a database using `psql`.

   ###### My Solution:

   ```psql
   $ psql -d sql-course < films7.sql
   ```

   ###### LS Solution:

   ```psql
   $ createdb om-to-mm
   $ psql -d om-to-mm < films7.sql
   ```

2. Write the SQL statement needed to create a join table that will allow a film to have multiple directors, and directors to have multiple films. Include an `id` column in this table, and add foreign key constraints to the other columns.

   ###### My Solution:

   ```sql
   CREATE TABLE films_directors (
   	id serial PRIMARY KEY,
   	film_id integer REFERENCES films (id),
   	director_id integer REFERENCES directors (id)
   );
   ```

   ###### LS Solution:

   ```sql
   CREATE TABLE directors_films (
   	id serial PRIMARY KEY,
   	director_id integer REFERENCES directors (id),
   	film_id integer REFERENCES films (id)
   );
   ```

   Notice that the table above is named `directors_films` and not `films_directors`. The convention for naming tables in SQL is to use alphabetical order when the table name consists of more than one word.

3. Write the SQL statements needed to insert data into the new join table to represent the existing one-to-many relationships.

   ###### My Solution:

   ```sql
   INSERT INTO directors_films (director_id, film_id) VALUES (1, 1);
   INSERT INTO directors_films (director_id, film_id) VALUES (2, 2);
   INSERT INTO directors_films (director_id, film_id) VALUES (3, 3);
   INSERT INTO directors_films (director_id, film_id) VALUES (4, 4);
   INSERT INTO directors_films (director_id, film_id) VALUES (5, 5);
   INSERT INTO directors_films (director_id, film_id) VALUES (6, 6);
   INSERT INTO directors_films (director_id, film_id) VALUES (3, 7);
   INSERT INTO directors_films (director_id, film_id) VALUES (7, 8);
   INSERT INTO directors_films (director_id, film_id) VALUES (8, 9);
   INSERT INTO directors_films (director_id, film_id) VALUES (4, 10);
   ```

   ###### LS Solution:

   ```sql
   INSERT INTO directors_films (film_id, director_id) VALUES (1, 1);
   INSERT INTO directors_films (film_id, director_id) VALUES (2, 2);
   INSERT INTO directors_films (film_id, director_id) VALUES (3, 3);
   INSERT INTO directors_films (film_id, director_id) VALUES (4, 4);
   INSERT INTO directors_films (film_id, director_id) VALUES (5, 5);
   INSERT INTO directors_films (film_id, director_id) VALUES (6, 6);
   INSERT INTO directors_films (film_id, director_id) VALUES (7, 3);
   INSERT INTO directors_films (film_id, director_id) VALUES (8, 7);
   INSERT INTO directors_films (film_id, director_id) VALUES (9, 8);
   INSERT INTO directors_films (film_id, director_id) VALUES (10, 4);
   ```

4. Write a SQL statement to remove any unneeded columns from `films`.

   ###### My Solution:

   ```sql
   ALTER TABLE films
    DROP COLUMN director_id;
   ```

   ###### LS Solution:

   ```sql
   ALTER TABLE films DROP COLUMN director_id;
   ```

5. Write a SQL statement that will return the following result:

   ```psql
              title           |         name
   ---------------------------+----------------------
    12 Angry Men              | Sidney Lumet
    1984                      | Michael Anderson
    Casablanca                | Michael Curtiz
    Die Hard                  | John McTiernan
    Let the Right One In      | Michael Anderson
    The Birdcage              | Mike Nichols
    The Conversation          | Francis Ford Coppola
    The Godfather             | Francis Ford Coppola
    Tinker Tailor Soldier Spy | Tomas Alfredson
    Wayne's World             | Penelope Spheeris
   (10 rows)
   ```

   ###### My Solution:

   ```sql
   SELECT films.title, directors.name
   	FROM films
   	JOIN directors_films ON films.id = directors_films.film_id
   	JOIN directors ON directors.id = directors_films.director_id
    ORDER BY films.title;
   ```

   ###### LS Solution:

   ```sql
   SELECT films.title, directors.name
   	FROM films
   		INNER JOIN directors_films ON directors_films.film_id = films.id
   		INNER JOIN directors ON directors.id = directors_films.director_id
   	ORDER BY films.title ASC;
   ```

6. Write SQL statements to insert data for the following films into the database:

   | Film                   | Year | Genre   | Duration | Directors                      |
   | :--------------------- | :--- | :------ | :------- | :----------------------------- |
   | Fargo                  | 1996 | comedy  | 98       | Joel Coen                      |
   | No Country for Old Men | 2007 | western | 122      | Joel Coen, Ethan Coen          |
   | Sin City               | 2005 | crime   | 124      | Frank Miller, Robert Rodriguez |
   | Spy Kids               | 2001 | scifi   | 88       | Robert Rodriguez               |

   ###### My Solution:

   ```sql
   INSERT INTO films (title, year, genre, duration)
   VALUES ('Fargo', 1996, 'comedy', 98),
   ('No Country for Old Men', 2007, 'western', 122),
   ('Sin City', 2005, 'crime', 124),
   ('Spy Kids', 2001, 'scifi', 88);
   
   INSERT INTO directors (name)
   VALUES ('Joel Coen'),
   ('Ethan Coen'),
   ('Frank Miller'),
   ('Robert Rodriguez');
   
   INSERT INTO directors_films (director_id, film_id)
   VALUES (9, 11),
   (9, 12),
   (10, 12),
   (11, 13),
   (12, 13),
   (12, 14);
   ```

   ###### LS Solution:

   ```sql
   INSERT INTO films (title, year, genre, duration) VALUES ('Fargo', 1996, 'comedy', 98);
   INSERT INTO directors (name) VALUES ('Joel Coen');
   INSERT INTO directors (name) VALUES ('Ethan Coen');
   INSERT INTO directors_films (director_id, film_id) VALUES (9, 11);
   
   INSERT INTO films (title, year, genre, duration) VALUES ('No Country for Old Men', 2007, 'western', 122);
   INSERT INTO directors_films (director_id, film_id) VALUES (9, 12);
   INSERT INTO directors_films (director_id, film_id) VALUES (10, 12);
   
   INSERT INTO films (title, year, genre, duration) VALUES ('Sin City', 2005, 'crime', 124);
   INSERT INTO directors (name) VALUES ('Frank Miller');
   INSERT INTO directors (name) VALUES ('Robert Rodriguez');
   INSERT INTO directors_films (director_id, film_id) VALUES (11, 13);
   INSERT INTO directors_films (director_id, film_id) VALUES (12, 13);
   
   INSERT INTO films (title, year, genre, duration) VALUES ('Spy Kids', 2001, 'scifi', 88) RETURNING id;
   INSERT INTO directors_films (director_id, film_id) VALUES (12, 14);
   ```

7. Write a SQL statement that determines how many films each director in the database has directed. Sort the results by number of films (greatest first) and then name (in alphabetical order).

   ###### My Solution:

   ```sql
   SELECT directors.name, count(films.id) AS total_films
   	FROM directors
    INNER JOIN directors_films ON directors.id = directors_films.director_id
    INNER JOIN films ON films.id = directors_films.film_id
    GROUP BY directors.name
    ORDER BY total_films DESC, directors.name ASC;
   ```

   ###### LS Solution:

   ```sql
   SELECT directors.name AS director, COUNT(directors_films.film_id) AS films
   	FROM directors
   		INNER JOIN directors_films ON directors.id = directors_films.director_id
     GROUP BY directors.id
     ORDER BY films DESC, directors.name ASC;
   ```

---

### Summary

---

* _Relational databases_ are called relational because they persist data in a set of _relations_, or, as they are more commonly called, _tables_.
* A _relationship_ is a connection between entity instances, or rows of data, usually resulting from a relationship between what those rows of data represent.
* The three levels of schema are _conceptual_, _logical_, and _physical_.
* The three types of relationships are _one to one_, _one to many_, and _many to many_.
* A _conceptual schema_ is a high-level design focused on identifying entities and their relationships.
* A _physical schema_ is a low-level database-specific design focused on implementation.
* _Cardinality_ is the number of objects on each side of the relationship.
* The _modality_ of a relationship indicates if that relationship is required or not.
* _Referential integrity_ is a data property that requires every value in one column of a table to appear in a column of (usually) another table.

#### Exercises

Before you move on, take some time to work through the [LS180 - SQL Fundamentals exercises](https://launchschool.com/exercises#ls180_sql_fundamentals) from the *DDL (Data Definition Lanugage)*, *DML (Data Manipulation Language)*, and *Medium: Many to Many* exercise sets.

---

#### DDL (Data Definition Language)

##### 1. Create an Extrasolar Planetary Database

```sql
$ createdb extrasolar
$ psql -d extrasolar

CREATE TABLE stars (
	id serial PRIMARY KEY,
	name varchar(25) UNIQUE NOT NULL,
	distance integer NOT NULL CHECK (distance > 0),
	spectral_type char(1),
	companions integer NOT NULL CHECK (companions >= 0)
);

CREATE TABLE planets (
	id serial PRIMARY KEY,
	designation char(1),
	mass integer
);
```

##### 2. Relating Stars and Planets

```sql
ALTER TABLE planets
	ADD COLUMN star_id integer NOT NULL REFERENCES stars (id); 
```

##### 3. Increase Star Name Length

```sql
ALTER TABLE stars
ALTER COLUMN name TYPE varchar(50);
```

##### 4. Stellar Distance Precision

```sql
ALTER TABLE stars
ALTER COLUMN distance TYPE numeric;
```

##### 5. Check Values in List

```sql
ALTER TABLE stars
ALTER COLUMN spectral_type 
	SET NOT NULL;

ALTER TABLE stars
	ADD CHECK (spectral_type IN ('O', 'B', 'A', 'F', 'G', 'K', 'M'));
```

##### 6. Enumerated Types

```sql
ALTER TABLE stars
 DROP CONSTRAINT stars_spectral_type_check;
 
CREATE TYPE spectral_type_values AS ENUM ('O', 'B', 'A', 'F', 'G', 'K', 'm');

ALTER TABLE stars
ALTER COLUMN spectral_type TYPE spectral_type_values USING spectral_type::spectral_type_values;
```

##### 7. Planetary Mass Precision

```sql
ALTER TABLE planets
	ADD CHECK (mass >= 0),
ALTER COLUMN mass TYPE numeric,
ALTER COLUMN mass SET NOT NULL,
ALTER COLUMN designation SET NOT NULL;
```

##### 8. Add a Semi-Major Axis Column

```sql
ALTER TABLE planets
	ADD COLUMN semi_major_axis numeric NOT NULL;
```

##### 9. Add a Moons Table

```sql
CREATE TABLE moons (
	id serial PRIMARY KEY,
	designation integer NOT NULL CHECK (designation > 0),
	semi_major_axis numeric CHECK (semi_major_axis > 0.0),
	mass numeric CHECK(mass > 0.0),
  planet_id integer NOT NULL REFERENCES planets (id)
);
```

##### 10. Delete the Database

```sql
$ pg_dump --inserts extrasolar > extrasolar.dump.sql

\c other_database
DROP DATABASE extrasolar;
```

---

#### DML (Data Manipulation Language)

##### 1. Set Up Database

```sql
CREATE DATABASE workshop;

CREATE TABLE devices (
	id serial PRIMARY KEY,
	name text NOT NULL,
	created_at timestamp DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE parts (
	id serial PRIMARY KEY,
	part_number integer UNIQUE NOT NULL,
	device_id integer REFERENCES devices (id)
);
```

##### 2. Insert Data for Parts and Devices

```sql
INSERT INTO devices (name) VALUES ('Accelerometer'), ('Gyroscope');

INSERT INTO parts (part_number, device_id)
VALUES (12, 1),
(14, 1),
(16, 1),
(31, 2),
(33, 2),
(35, 2),
(37, 2),
(39, 2),
(50, NULL),
(54, NULL),
(58, NULL);
```

##### 3. INNER JOIN

```sql
SELECT d.name, p.part_number
	FROM devices AS d
 INNER JOIN parts AS p
 		ON d.id = p.device_id;
```

##### 4. SELECT part_numer

```sql
SELECT * FROM parts
 WHERE parts.part_number::text LIKE '3%';
```

##### 5. Aggregate Functions

```sql
SELECT devices.name, count(parts.part_number)
	FROM devices
	JOIN parts ON devices.id = parts.device_id
 GROUP BY devices.name;
```

##### 6. ORDER BY

```sql
SELECT devices.name, count(parts.device_id)
	FROM devices
	JOIN parts ON devices.id = parts.device_id
 GROUP BY devices.name
 ORDER BY devices.name DESC;
```

##### 7. IS NULL and IS NOT NULL

```sql
SELECT part_number, device_id FROM parts
 WHERE device_id IS NOT NULL;
 
SELECT part_number, device_id FROM parts
 WHERE device_id IS NULL;
```

##### 8. Oldest Device

```sql
SELECT name FROM devices
 ORDER BY age(created_at) DESC
 LIMIT 1;
 
SELECT name FROM devices
 ORDER BY created_at ASC
 LIMIT 1;
```

##### 9. UPDATE device_id

```sql
UPDATE parts
	 SET device_id = 1
 WHERE part_number IN (37, 39);
 
UPDATE parts
	 SET device_id = 2
 WHERE part_number = min(part_number);
```

##### 10. Delete Accelerometer

```sql
DELETE FROM parts
 WHERE device_id = 1;
 
DELETE FROM devices
 WHERE name = 'Accelerometer';
```

---

#### Medium: Many to Many

##### 1. Set Up Database

```sql
CREATE DATABASE billing;

\c billing

CREATE TABLE customers (
	id serial PRIMARY KEY,
	name text NOT NULL,
	payment_token char(8) NOT NULL CHECK (payment_token ~ '^[A-Z]{8}$')
);

CREATE TABLE services (
	id serial PRIMARY KEY,
	description text NOT NULL,
	price numeric(10, 2) NOT NULL CHECK (price >= 0.00)
);

INSERT INTO customers (name, payment_token)
VALUES ('Pat Johnson', 'XHGOAHEQ'),
('Nancy Monreal', 'JKWQPJKL'),
('Lynn Blake', 'KLZXWEEE'),
('Chen Ke-Hua', 'KWETYCVX'),
('Scott Lakso', 'UUEAPQPS'),
('Jim Pornot', 'XKJEYAZA');

INSERT INTO services (description, price)
VALUES ('Unix Hosting', 5.95),
('DNS', 4.95),
('Whois Registration', 1.95),
('High Bandwidth', 15.00),
('Business Support', 250.00),
('Dedicated Hosting', 50.00),
('Bulk Email', 250.00),
('One-to-one Training', 999.00);

CREATE TABLE customers_services (
	id serial PRIMARY KEY,
	customer_id integer REFERENCES customers (id) ON DELETE CASCADE,
	service_id integer REFERENCES services (id),
  CONSTRAINT unique_data UNIQUE (customer_id, service_id)
);

INSERT INTO customers_services (customer_id, service_id)
VALUES (1, 1),
(1, 2),
(1, 3),
(3, 1),
(3, 2),
(3, 3),
(3, 4),
(3, 5),
(4, 1),
(4, 4),
(5, 1),
(5, 2),
(5, 6),
(6, 1),
(6, 6),
(6, 7);

ALTER TABLE customers
	ADD UNIQUE (payment_token);
	
ALTER TABLE customers_services
ALTER COLUMN customer_id
	SET NOT NULL,
ALTER COLUMN service_id
	SET NOT NULL;
```

##### 2. Get Customers With Services

Write a query to retrieve the `customer` data for every customer who currently subscribes to at least one service.

```sql
SELECT DISTINCT customers.* FROM customers
 INNER JOIN customers_services ON customers.id = customers_services.customer_id;
```

##### 3. Get Customers With No Services

```sql
SELECT c.* FROM customers AS c
	LEFT OUTER JOIN customers_services AS cs
		ON cs.customer_id = c.id
 WHERE cs.customer_id IS NULL;
 
SELECT c.*, s.* FROM customers AS c
 	FULL JOIN customers_services AS cs
 		ON cs.customer_id = c.id
 	FULL JOIN services AS s
 		ON cs.service_id = s.id
 		WHERE cs.customer_id IS NULL OR cs.service_id IS NULL;
```

##### 4. Get Services With No Customers

```sql
SELECT s.description FROM customers_services AS cs
 RIGHT OUTER JOIN services AS s
 		ON cs.service_id = s.id
 WHERE cs.service_id is NULL;
```

##### 5. Services for each Customer

```sql
SELECT c.name, string_agg(s.description, ', ') AS services
	FROM customers AS c
	LEFT OUTER JOIN customers_services AS cs
		ON cs.customer_id = c.id
	LEFT OUTER JOIN services AS s
		ON cs.service_id = s.id
 GROUP BY c.name;
```

##### 6. Services With At Least 3 Customers

```sql
SELECT s.description, count(c.id)
	FROM customers AS c
	JOIN customers_services AS cs
		ON cs.customer_id = c.id
	JOIN services AS s
		ON cs.service_id = s.id
 GROUP BY s.description
 HAVING count(c.id) >= 3;
```

##### 7. Total Gross Income

```sql
SELECT sum(services.price) AS gross
	FROM services
 INNER JOIN customers_services
 		ON customers_services.service_id = services.id;
```

##### 8. Add New Customer

```sql
INSERT INTO customers (name, payment_token) VALUES ('John Doe', 'EYODHLCN');

INSERT INTO customers_services (customer_id, service_id)
VALUES (7, 1),
(7, 2),
(7, 3);
```

##### 9. Hypothetically

```sql
SELECT sum(price) FROM customers_services
 INNER JOIN services ON customers_services.service_id = services.id
 WHERE price > 100.00;
 
SELECT sum(price) FROM services
 CROSS JOIN customers
 WHERE price > 100.00;
```

##### 10. Deleting Rows

```sql
DELETE FROM customers_services
 WHERE service_id = 7 OR customer_id = 4;

DELETE FROM customers
 WHERE name = 'Chen Ke-Hua';

DELETE FRom services
 WHERE description = 'Bulk Email';
```

