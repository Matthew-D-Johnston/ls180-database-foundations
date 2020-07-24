##### LS180 - SQL Fundamentals > Medium: Subqueries and More

---

### 1. Set Up the Database using \copy

---

This set of exercises will focus on an auction. Create a new database called `auction`. In this database there will be three tables, `bidders`, `items`, and `bids`.

After creating the database, set up the 3 tables using the following specifications:

##### bidders

- `id` of type SERIAL: this should be a primary key
- `name` of type text: this should be `NOT NULL`

##### items

- `id` of type SERIAL: this should be a primary key
- `name` of type text: this should be `NOT NULL`
- `initial_price` and `sales_price`: These two columns should both be of type numeric. Each column should be able to hold a number as high as 1000 dollars with 2 decimal points of precision.
- The `initial_price` represents the starting price of an item when it is first put up for auction. This column should never be NULL.
- The `sales_price` represents the final price at which the item was sold. This column may be NULL, as it is possible to have an item that was never sold off.

##### bids

- `id` of type SERIAL: this should be a primary key
- `bidder_id`, `item_id`: These will be of type integer and should not be NULL. This table connects a bidder with an item and each row represents an individual bid. There should never be a row that has `bidder_id` or `item_id` unknown or NULL. Nor should there ever be a bid that references a nonexistent item or bidder. If the item or bidder associated with a bid is removed, that bid should also be removed from the database.
- Create your `bids` table so that both `bidder_id` and `item_id` together form a composite index for faster lookup.
- amount - The amount of money placed for each individual bid by a bidder. This column should be of the same type as `items.initial_price` and have the same constraints.

Finally, use the `\copy` meta-command to import the below files into your `auction` database. You'll have to create these files yourself before you can import them with `\copy`.

**bidders.csv**

```csv
id, name
1,Alison Walker
2,James Quinn
3,Taylor Williams
4,Alexis Jones
5,Gwen Miller
6,Alan Parker
7,Sam Carter
```

**items.csv**

```csv
id, name, initial_price, sales_price
1,Video Game, 39.99, 70.87
2,Outdoor Grill, 51.00, 83.25
3,Painting, 100.00, 250.00
4,Tent, 220.00, 300.00
5,Vase, 20.00, 42.00
6,Television, 550.00,
```

**bids.csv**

```csv
id, bidder_id, item_id, amount
1,1, 1, 40.00
2,3, 1, 52.00
3,1, 1, 53.00
4,3, 1, 70.87
5,5, 2, 83.25
6,2, 3, 110.00
7,4, 3, 140.00
8,2, 3, 150.00
9,6, 3, 175.00
10,4, 3, 185.00
11,2, 3, 200.00
12,6, 3, 225.00
13,4, 3, 250.00
14,1, 4, 222.00
15,2, 4, 262.00
16,1, 4, 290.00
17,1, 4, 300.00
18,2, 5, 21.72
19,6, 5, 23.00
20,2, 5, 25.00
21,6, 5, 30.00
22,2, 5, 32.00
23,6, 5, 33.00
24,2, 5, 38.00
25,6, 5, 40.00
26,2, 5, 42.00
```

##### Approach/Algorithm

`psql` provides a useful command, `\copy` which allows you to import `csv` files. Read the [documentation](https://www.postgresql.org/docs/10/static/app-psql.html) for `\copy` before proceeding with this exercise. Notice that the data files are of type csv and they have headers. Keep this in mind when deciding how to write the `\copy` command.

###### My Solution:

First,

```psql
$ createdb auction
$ psql -d auction
```

Then,

```sql
CREATE TABLE bidders (
	id serial PRIMARY KEY,
	name text NOT NULL
);

CREATE TABLE items (
	id serial PRIMARY KEY,
	name text NOT NULL,
	initial_price decimal(6, 2) NOT NULL,
	sales_price decimal(6, 2)
);

CREATE TABLE bids (
	id serial PRIMARY KEY,
	bidder_id integer NOT NULL REFERENCES bidders (id) ON DELETE CASCADE,
	item_id integer NOT NULL REFERENCES items (id) ON DELETE CASCADE,
  amount decimal(6, 2) NOT NULL
);
```

Then,

```sql
COPY bidders FROM bidders.csv;
COPY items FROM items.csv;
COPY bids FROM bids.csv;
```

But, after reading the "Approach/Algorithm" section:

```sql
\copy bidders from 'bidders.csv'
\copy items from 'items.csv'
\copy bids from 'bids.csv'
```

###### LS Solution:

```psql
createdb auction
```

```sql
CREATE TABLE bidders (
	id SERIAL PRIMARY KEY,
	name TEXT NOT NULL
);

CREATE TABLE items (
	id SERIAL PRIMARY KEY,
	name TEXT NOT NULL,
	initial_price DECIMAL(6,2) NOT NULL CHECK(initial_price BETWEEN 0.01 AND 1000.00),
	sales_price DECIMAL(6,2) CHECK(sales_price BETWEEN 0.01 AND 1000.00)
);

CREATE TABLE bids (
	id SERIAL PRIMARY KEY,
	bidder_id integer NOT NULL REFERENCES bidders(id) ON DELETE CASCADE,
	item_id integer NOT NULL REFERENCES items(id) ON DELETE CASCADE,
	amount DECIMAL(6,2) NOT NULL CHECK(amount BETWEEN 0.01 AND 1000.00)
);

CREATE INDEX ON bids (bidder_id, item_id);
```

```sql
\copy bidders FROM 'bidders.csv' WITH HEADER CSV;
\copy items FROM 'items.csv' WITH HEADER CSV;
\copy bids FROM 'bids.csv' WITH HEADER CSV;
```

---

### 2. Conditional Subqueries: IN

---

Write a SQL query that shows all items that have had bids put on them. Use the logical operator `IN` for this exercise, as well as a subquery.

Here is the expected output:

```plain text
 Bid on Items
---------------
 Video Game
 Outdoor Grill
 Painting
 Tent
 Vase
(5 rows)
```

###### My Solution:

```sql
SELECT name FROM items
WHERE id IN (SELECT item_id FROM bids);
```

###### LS Solution:

```sql
SELECT name AS "Bid on Items" FROM items
WHERE items.id IN (SELECT DISTINCT item_id FROM bids);
```

---

### 3. Conditional Subqueries: NOT IN

---

Write a SQL query that shows all items that have not had bids put on them. Use the logical operator `NOT IN` for this exercise, as well as a subquery.

Here is the expected output:

```plain text
 Not Bid On
------------
 Television
(1 row)
```

###### My Solution:

```sql
SELECT name AS "Not Bid On" FROM items
WHERE items.id NOT IN (SELECT DISTINCT item_id FROM bids);
```

###### LS Solution:

```sql
SELECT name AS "Not Bid On" FROM items
WHERE items.id NOT IN (SELECT item_id FROM bids);
```

---

### 4. Conditional Subqueries: EXISTS

---

Write a `SELECT` query that returns a list of names of everyone who has bid in the auction. While it is possible (and perhaps easier) to do this with a `JOIN` clause, we're going to do things differently: use a subquery with the `EXISTS` clause instead. Here is the expected output:

```plain text
      name
-----------------
 Alison Walker
 James Quinn
 Taylor Williams
 Alexis Jones
 Gwen Miller
 Alan Parker
(6 rows)
```

###### My Solution:

```sql
SELECT name FROM bidders
WHERE EXISTS (SELECT DISTINCT bidder_id FROM bids);
```

I'm missing something here, big time.

###### LS Solution:

```sql
SELECT name FROM bidders
WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);
```

##### Further Exploration

More often than not, we can get an equivalent result by using a `JOIN` clause, instead of a subquery. Can you figure out a `SELECT` query that uses a `JOIN` clause that returns the same output as our solution above?

###### My Solution

```sql
SELECT bidders.name FROM bidders
 INNER JOIN bids
 		ON bids.bidder_id = bidders.id
 GROUP BY bidders.id
 ORDER BY bidders.id;
```

---

### 5. Query From a Virtual Table

---

For this exercise, we'll make a slight departure from *how* we've been using subqueries. We have so far used subqueries to filter our results using a WHERE clause. In this exercise, we will build that filtering into the table that we will query. Write an SQL query that finds the largest number of bids from an individual bidder.

For this exercise, you must use a subquery to generate a result table (or virtual table), and then query that table for the largest number of bids.

Your output should look like this:

```plain text
  max
------
    9
(1 row)
```

###### My Solution:

```sql
SELECT max(number_of_bidders)
  FROM (SELECT count(bidders.id) AS number_of_bidders
          FROM bidders
          JOIN bids ON bids.bidder_id = bidders.id
         GROUP BY bidders.name) AS bidder_count;
```

###### LS Solution:

```sql
SELECT MAX(bid_counts.count) FROM
	(SELECT COUNT(bidder_id) FROM bids GROUP BY bidder_id) AS bid_counts;
```

---

### 6. Scalar Subqueries

---



