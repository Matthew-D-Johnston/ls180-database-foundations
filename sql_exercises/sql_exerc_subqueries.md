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

For this exercise, use a scalar subquery to determine the number of bids on each item. The entire query should return a table that has the `name` of each item along with the number of bids on an item.

Here is the expected output:

```plain text
    name      | count
--------------+-------
Video Game    |     4
Outdoor Grill |     1
Painting      |     8
Tent          |     4
Vase          |     9
Television    |     0
(6 rows)
```

##### Approach/Algorithm

Refer to the PostgreSQL documentation on [scalar subqueries](https://www.postgresql.org/docs/9.5/static/sql-expressions.html#SQL-SYNTAX-SCALAR-SUBQUERIES) to solve this exercise. Keep a few key facts in mind:

- You may reference columns within your subquery from the outer `SELECT` query. Those values will act as constants for the current subquery evaluation.
- A scalar subquery must only return one column and one row.

###### My Solution:

```sql
SELECT name, (SELECT count(item_id) 
              	FROM bids 
               WHERE item_id = items.id)
	FROM items;
```

###### LS Solution:

```sql
SELECT name,
			 (SELECT COUNT(item_id) FROM bids WHERE item_id = items.id)
	FROM items;
```

##### Further Exploration

If we wanted to get an equivalent result, without using a subquery, then we would have to use a `LEFT OUTER JOIN`. Can you come up with the equivalent query that uses a `JOIN` clause?

###### My Solution:

```sql
SELECT items.name, count(bids.item_id) FROM items
	LEFT OUTER JOIN bids
			 ON bids.item_id = items.id
 GROUP BY items.name;
```

---

### 7. Row Comparison

---

We want to check that a given item is in our database. There is one problem though: we have all of the data for the item, but we don't know the `id` number. Write an SQL query that will display the `id` for the item that matches all of the data that we know, but does not use the `AND` keyword. Here is the data we know:

`'Painting', 100.00, 250.00`

###### My Solution:

```sql
SELECT id FROM items
 WHERE ROW(name, initial_price, sales_price) = ROW('Painting', 100.00, 250.00);
```

###### LS Solution:

```sql
SELECT id FROM items
WHERE ROW('Painting', 100.00, 250.00) =
  ROW(name, initial_price, sales_price);
```

---

### 8. EXPLAIN

---

For this exercise, let's explore the `EXPLAIN` PostgreSQL statement. It's a very useful SQL statement that lets us analyze the efficiency of our SQL statements. More specifically, use `EXPLAIN` to check the efficiency of the query statement we used in the exercise on `EXISTS`:

```sql
SELECT name FROM bidders
WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);
```

First use just `EXPLAIN`, then include the `ANALYZE` option as well. For your answer, list any SQL statements you used, along with the output you get back, and your thoughts on what is happening in both cases.

###### My Solution:

First, I just ran the code as is to inspect the output;

```sql
SELECT name FROM bidders
WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);

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

The output is a single column with six separate rows corresponding to each person that made a bid.  

Now, let's use the `EXPLAIN` statement in our query:

```sql
EXPLAIN SELECT name FROM bidders
WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);

                                QUERY PLAN                                
--------------------------------------------------------------------------
 Hash Join  (cost=33.38..66.47 rows=635 width=32)
   Hash Cond: (bidders.id = bids.bidder_id)
   ->  Seq Scan on bidders  (cost=0.00..22.70 rows=1270 width=36)
   ->  Hash  (cost=30.88..30.88 rows=200 width=4)
         ->  HashAggregate  (cost=28.88..30.88 rows=200 width=4)
               Group Key: bids.bidder_id
               ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4)
(7 rows)
```

Based on the top-most node of the 'Query Plan' we see that the estimated total cost of our query is `66.47`. The estimated start-up cost is `33.38`. The query type for the top-most node is a `Hash Join`.  

Now, let's add the `ANALYZE` statement to our query:

```sql
EXPLAIN ANALYZE SELECT name FROM bidders
WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);

                                                     QUERY PLAN                                                      
---------------------------------------------------------------------------------------------------------------------
 Hash Join  (cost=33.38..66.47 rows=635 width=32) (actual time=0.244..0.250 rows=6 loops=1)
   Hash Cond: (bidders.id = bids.bidder_id)
   ->  Seq Scan on bidders  (cost=0.00..22.70 rows=1270 width=36) (actual time=0.068..0.070 rows=7 loops=1)
   ->  Hash  (cost=30.88..30.88 rows=200 width=4) (actual time=0.097..0.097 rows=6 loops=1)
         Buckets: 1024  Batches: 1  Memory Usage: 9kB
         ->  HashAggregate  (cost=28.88..30.88 rows=200 width=4) (actual time=0.069..0.072 rows=6 loops=1)
               Group Key: bids.bidder_id
               ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4) (actual time=0.039..0.047 rows=26 loops=1)
 Planning Time: 0.744 ms
 Execution Time: 3.297 ms
(10 rows)
```

Here, we have all of the same information given by the `EXPLAIN` statement, but with additional time information. We can see that the Planning Time of the query was `0.744 ms` and the Execution Time was `3.297 ms`.

###### LS Discussion:

`EXPLAIN` is used to show statistics about the query plan for a SQL statement. Here is the general form for `EXPLAIN`s syntax:

```
EXPLAIN sql_expression;
```

Let's go through the output of each `EXPLAIN` statement and see what is going on.

The first `EXPLAIN` statement contains a fair amount of information. Each row represents an operation taken. The following items are listed in each row of information.

1. The name of the node used to perform the SQL statement. A node represents some operation taken to run the SQL statement. An example would be the name in the first row of our query plan, `Hash Join`
2. The estimated startup cost and estimated total cost. These can be seen here: `Hash Join (cost=33.38..62.84 rows=635 width=32)` The first number after `cost` is the estimated startup cost, and the second is the estimated total cost.
3. The estimated number of rows to be shown when the SQL statement we are explaining is run. This is the number right after `rows=` above.
4. The width is the estimated amount in bytes taken up by rows for the SQL statement we are explaining.

Did you notice how some nodes are nested further in than others? A nested node represents one that is a child of the one above it. That means that nested nodes were operations necessary to allow the parent node(operation) to run its course. One other important fact is that all of these numbers represent estimates. The SQL statement we're explaining isn't actually run, so all information listed above is an approximation of what will actually happen. Another thing to consider are the units used for describing the estimated startup and total costs. These units are arbitrary and are used by PostgreSQL internally to create the query plan. Their main purpose is to give some measure of the efficiency of using certain nodes, taking certain operations to execute a SQL statement. If the cost is greater than some other group of operations then it will probably be dropped for an alternative approach.

Next, let's take a look at the information that was added when we used the `ANALYZE` option. The information here is mostly the same: cost, rows, and width are still there. But there is one other bit of information that has been added to each node: the `actual_time` required for the startup and execution of that node. At the end of our query plan, the planning and execution time have also been added: these represent the total time required to set up the SQL statement along with the total time it took to execute that SQL statement.

Since the SQL statement is actually run when we use `EXPLAIN ANALYZE`, the results we get from our query plan are the actual results, and not estimates; that includes the costs and the measures of time elapsed per operation.

Now, one might wonder why bother with the `EXPLAIN` statement at all? Well, this statement can actually be really useful. We can compare the costs of running different SQL statements, which can help us with optimizing our DB calls. There may be some SQL statements that we don't actually want to run, but just want some estimated data on: in that case, use `EXPLAIN`. But, if we're ok with running the statement, and we need some extra data(maybe to compare the elapsed execution and setup time between two equivalent SQL statements), then we should use `EXPLAIN ANALYZE`.

---

### 9. Comparing SQL Statements

---

In this exercise, we'll use `EXPLAIN ANALYZE` to compare the efficiency of two SQL statements. These two statements are actually from the "Query From a Virtual Table" exercise in this set. In that exercise, we stated that our subquery-based solution:

```sql
SELECT MAX(bid_counts.count) FROM
  (SELECT COUNT(bidder_id) FROM bids GROUP BY bidder_id) AS bid_counts;
```

was actually faster than the simpler equivalent without subqueries:

```sql
SELECT COUNT(bidder_id) AS max_bid FROM bids
  GROUP BY bidder_id
  ORDER BY max_bid DESC
  LIMIT 1;
```

In this exercise, we will demonstrate this fact.

Run `EXPLAIN ANALYZE` on the two statements above. Compare the planning time, execution time, and the total time required to run these two statements. Also compare the total "costs". Which statement is more efficient and why?

###### My Solution:

First statement (Subquery):

```sql
EXPLAIN ANALYZE SELECT MAX(bid_counts.count) FROM
  (SELECT COUNT(bidder_id) FROM bids GROUP BY bidder_id) AS bid_counts;
```

Output:

```plain text
                                                  QUERY PLAN                                                   
---------------------------------------------------------------------------------------------------------------
 Aggregate  (cost=37.15..37.16 rows=1 width=8) (actual time=0.097..0.097 rows=1 loops=1)
   ->  HashAggregate  (cost=32.65..34.65 rows=200 width=12) (actual time=0.087..0.091 rows=6 loops=1)
         Group Key: bids.bidder_id
         ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4) (actual time=0.055..0.061 rows=26 loops=1)
 Planning Time: 0.436 ms
 Execution Time: 0.460 ms
(6 rows)
```

* planning time: 0.436 ms
* execution time: 0.460 ms
* total time: 0.896 ms, or 0.097 ms
* startup cost: 37.15
* total cost: 37.16

Second statement (ORDER BY and LIMIT):

```sql
EXPLAIN ANALYZE SELECT COUNT(bidder_id) AS max_bid FROM bids
  GROUP BY bidder_id
  ORDER BY max_bid DESC
  LIMIT 1;
```

Output:

```plain text
                                                     QUERY PLAN                                                      
---------------------------------------------------------------------------------------------------------------------
 Limit  (cost=35.65..35.65 rows=1 width=12) (actual time=0.093..0.093 rows=1 loops=1)
   ->  Sort  (cost=35.65..36.15 rows=200 width=12) (actual time=0.092..0.092 rows=1 loops=1)
         Sort Key: (count(bidder_id)) DESC
         Sort Method: top-N heapsort  Memory: 25kB
         ->  HashAggregate  (cost=32.65..34.65 rows=200 width=12) (actual time=0.060..0.062 rows=6 loops=1)
               Group Key: bidder_id
               ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4) (actual time=0.043..0.046 rows=26 loops=1)
 Planning Time: 0.394 ms
 Execution Time: 0.233 ms
(9 rows)
```

* planning time: 0.394 ms
* execution time: 0.233 ms
* total time: 0.627 ms, or 0.093 ms
* startup cost: 35.65
* total cost: 35.65

It seems that the second statement is actually more efficient; It takes much less time to plan and execute, and its total cost is lower.

| Query Type                      | Planing Time | Execution Time | Total Costs |
| :------------------------------ | :----------- | :------------- | :---------- |
| **Subquery** (first)            | 0.436 ms     | 0.460 ms       | 37.16       |
| **ORDER BY and LIMIT** (second) | 0.394 ms     | 0.233 ms       | 35.65       |

##### Further Exploration

We mentioned earlier that using a scalar subquery was faster than using an equivalent `JOIN` clause. Determining that `JOIN` statement was part of the "Further Exploration" for that exercise. For this "Further Exploration", compare the times and costs of those two statements. The SQL statement that uses a scalar subquery is listed below.

Scalar Subquery:

```sql
SELECT name,
(SELECT COUNT(item_id) FROM bids WHERE item_id = items.id)
FROM items;
```

JOIN statement:

```sql
SELECT items.name, count(bids.item_id) FROM items
	LEFT OUTER JOIN bids
			 ON bids.item_id = items.id
 GROUP BY items.name;
```

EXPLAIN ANALYZE with Scalar Subquery

```sql
EXPLAIN ANALYZE SELECT name,
(SELECT COUNT(item_id) FROM bids WHERE item_id = items.id)
FROM items;
```

Output:

```plain text
                                                 QUERY PLAN                                                  
-------------------------------------------------------------------------------------------------------------
 Seq Scan on items  (cost=0.00..25455.20 rows=880 width=40) (actual time=0.033..0.066 rows=6 loops=1)
   SubPlan 1
     ->  Aggregate  (cost=28.89..28.91 rows=1 width=8) (actual time=0.007..0.007 rows=1 loops=6)
           ->  Seq Scan on bids  (cost=0.00..28.88 rows=8 width=4) (actual time=0.003..0.005 rows=4 loops=6)
                 Filter: (item_id = items.id)
                 Rows Removed by Filter: 22
 Planning Time: 0.124 ms
 Execution Time: 1.004 ms
(8 rows)
```

EXPLAIN ANALYZE with JOIN statement:

```sql
EXPLAIN ANALYZE SELECT items.name, count(bids.item_id) FROM items
	LEFT OUTER JOIN bids
			 ON bids.item_id = items.id
 GROUP BY items.name;
```

Output:

```plain text
                                                     QUERY PLAN                                                      
---------------------------------------------------------------------------------------------------------------------
 HashAggregate  (cost=66.44..68.44 rows=200 width=40) (actual time=0.185..0.189 rows=6 loops=1)
   Group Key: items.name
   ->  Hash Right Join  (cost=29.80..58.89 rows=1510 width=36) (actual time=0.134..0.160 rows=27 loops=1)
         Hash Cond: (bids.item_id = items.id)
         ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4) (actual time=0.035..0.040 rows=26 loops=1)
         ->  Hash  (cost=18.80..18.80 rows=880 width=36) (actual time=0.068..0.069 rows=6 loops=1)
               Buckets: 1024  Batches: 1  Memory Usage: 9kB
               ->  Seq Scan on items  (cost=0.00..18.80 rows=880 width=36) (actual time=0.052..0.055 rows=6 loops=1)
 Planning Time: 0.479 ms
 Execution Time: 0.428 ms
(10 rows)
```

Comparison:

| Query Type                  | Planing Time | Execution Time | Total Costs |
| :-------------------------- | :----------- | :------------- | :---------- |
| **Scalar Subquery** (first) | 0.124 ms     | 1.004 ms       | 25455.20    |
| **Join Statement** (second) | 0.479 ms     | 0.428 ms       | 68.44       |

Only the planning time for the scalar subquery is lower than the Join Statement. But the execution time is greater than combined planing time and execution time of the Join Statement. Also, the total cost of the Scalar Subquery is astronomical compared to that of the Join Statement.