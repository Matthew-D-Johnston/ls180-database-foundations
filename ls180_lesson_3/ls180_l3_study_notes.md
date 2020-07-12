

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

