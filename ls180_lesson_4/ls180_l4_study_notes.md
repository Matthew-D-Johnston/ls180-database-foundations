##### LS180 Database Foundations > Optimizing SQL Queries

---

### What to Focus On

---

###### Optimization is something to be aware of.

* A database-backed application can be slow for a variety of reasons. In general, an application that makes fewer queries will be faster than one that makes more, but optimizing application performance is a complex topic that is beyond the scope of this course.
* You should understand what indexes are, how to implement them, and the trade-offs involved in using them.
* You should be aware of the concept of comparing different SQL statements.

###### Subqueries can be useful.

* You should know what a subquery is and how to use it, but you aren't expected to know everything about them. Many queries can be accomplished using JOINS _or_ subqueries, and knowing which is the best requires understanding a lot about the specific scenario being considered. This isn't something you are expected to master at this point in your studies.

---

### Indexes

---

#### What are indexes?

* In the context of a database, an index is a mechanism that SQL engines use to speed up queries. They do this by storing indexed data in a table-like structure, which can be quickly searched using particular search algorithms. The results of the search provide a link back to the record(s) to which the indexed data belongs. Using an index means that the the database engine can locate column values more efficiently since it doesn't have to search through every record in a table in sequence.

#### When to use an index?

* The question you might be asking is, if indexes are so useful for speeding up queries then why not just index every column in a table? People often do this, and end up with slower tables. There are numerous reasons for this slowness; one reason being that when you build an index of a field, reads become faster, but every time a row is updated or inserted, the index must be updated as well. Now you're updating not only the table but also the index, so that's a performance cost. As with many database optimization decisions, there are a lot of choices involved in how to decide whether or not a column should be designated an index.
* Making such choices is often about trade-offs. There are some useful rules of thumb that can help when assessing what those trade-offs are:
  * Indexes are best used in cases where sequential reading is inadequate. For example: columns that aid in mapping relationships (such as Foreign Key columns), or columns that are frequently used as part of an `ORDER BY` clause, are good candidates for indexing.
  * They are best used in tables and/ or columns where the data will be read much more frequently than it is created or updated.

###### Types of Index

* Another decision when using indexes is the type of index to use. Different types of index use different data structures and different search algorithms. Some of the index types available within PostgreSQL are B-tree, Hash, GiST, and GIN. For the purposes of this course, you don't need to remember these types, understand how they work, or know the differences between them.

#### Creating Indexes

* At this point in the course you will already have created indexes on columns, though not explicitly. When you define a `PRIMARY KEY` constraint, or a `UNIQUE` constraint, on a column you automatically create an index on that column. In fact, the index is the mechanism by which these constraints enforce uniqueness. 

* Unlike `PRIMARY KEY` and `UNIQUE` constraints, `FOREIGN KEY` constraints do not automatically create an index on a column. Foreign Key columns are good candidates for indexing, however you would need to explicitly create the index on the column. Let's look at how to do that next.

* The general form for adding an index to table is:

  ```sql
  CREATE INDEX index_name ON table_name (field_name);
  ```

###### Unique and Non-unique

* When an index is created via `PRIMARY KEY` or `UNIQUE` constraints, the index created is a _unique_ index. When an index is unique, multiple table rows with equal values for that index are not allowed. Indexes added that don't enforce uniquess allow for the same value to occur multiple times in the indexed column. This is sometimes referred to as a non-unique index.

###### Multicolumn Indexes

* As well as indexing a single column, indexes can also be created on more than one column. The form is almost the same as a single-column index, except instead of specifying a single column name, you specify more than one:

  ```sql
  CREATE INDEX index_name ON table_name (field_name_1, field_name_2);
  ```

* Only certain index types support multi-column indexes, and there is a limit to the number of columns that can be combined in an index.

###### Partial Indexes

* Another form of index is a partial index. Partial indexes are built from a subset of the data in a table. The subset of data is defined by a conditional expression, and the index contains entries only for the rows from the table where the value in the indexed column satisfies the condition.
* Partial indexes can be useful in certain situations, but most of the time you will be using single-column or perhaps multi-column indexes.

#### Deleting Indexes

* The `DROP INDEX` command can be used to delete the index that was created. In order to execute the command you need to refer to the index by its name.
* Rather than displaying the entire schema for a table, another way of listing indexes is to use the `\di` psql console command. This is similar to the `\d` command, except rather than listing the tables, views, and sequences of a database, it lists the indexes.

---

### Comparing SQL Statements

---

#### Assessing a Query with EXPLAIN

* PostgreSQL provides a useful command called `EXPLAIN` which gives a step-by-step analysis of how the query will be run internally by PostgreSQL.

* In order to execute each query that it receives, PostgreSQL devises an appropriate **query plan**. The creation of this query plan is one step in the path of executing a query as outlined in the [PostgreSQL Documentation](https://www.postgresql.org/docs/9.5/query-path.html). What `EXPLAIN` does is allow you to access and read that query plan.

* To use `EXPLAIN` you prepend the query with the `EXPLAIN` keyword. This does not actually execute the query, but instead returns the query plan for that query.

  ```sql
  EXPLAIN SELECT * FROM books;
  					QUERY PLAN
  --------------------------------------------------------
  Seq Scan on books (cost=0.00..12.60 rows=260 width=282)
  (1 row)
  ```

* The structure of the query plan is a node-tree. The more 'elements' that there are to your query, the more nodes there will be in the tree. The example above explains a very simple query, so there is only one node in the plan tree. Each node consists of the node type (in this case a sequential scan on the `books` table) along with estimated cost for that node (start-up cost, followed by total cost), the estimated number of rows to be output by the node, and the estimated average width of the rows in bytes.

* The cost values are calculated using arbitrary units determined by the planner's cost parameters, and represent the estimated amount of effort or resources required to execute the query as planned. Understanding cost parameters is beyond the scope of this course, but is explained in detail in the [PostgreSQL documentation](https://www.postgresql.org/docs/current/runtime-config-query.html#RUNTIME-CONFIG-QUERY-CONSTANTS).

* The output for a more complex query would have more nodes:

  ```sql
  EXPLAIN SELECT books.title FROM books
  JOIN authors ON books.author_id = authors.id
  WHERE authors.name = 'William Gibson';
  						 QUERY PLAN
  --------------------------------------------------------
  Hash Join  (cost=14.03..27.62 rows=2 width=218)
   Hash Cond: (books.author_id = authors.id)
   -> Seq Scan on books (cost=0.00..12.60 rows=260 width=222)
   -> Hash  (cost=14.00..14.00 rows=2 width=4)
   			-> Seq Scan on authors  (cost=0.00..14.00 rows=2 width=4)
   					 Filter: ((name)::text = 'William Gibson'::text)
  (6 rows)
  ```

* However many nodes the query plan has, one of the key pieces of information to look out for in order to compare queries is the estimated 'total cost' value of the top-most node. For our first query this value is `12.60`. Our second, more complex, query understandably has a higher estimated cost of `27.62`.

* For a more interesting comparison, we can replace our second example with an equivalent subquery:

  ```sql
  my_books=# EXPLAIN SELECT title FROM books
  my_books-# WHERE author_id =
  my_books-# (SELECT id FROM authors
  my_books(# WHERE name = 'William Gibson');
                                       QUERY PLAN
  -------------------------------------------------------------------------------------
   Index Scan using books_author_id_idx on books  (cost=14.15..22.16 rows=1 width=218)
     Index Cond: (author_id = $0)
     InitPlan 1 (returns $0)
       ->  Seq Scan on authors  (cost=0.00..14.00 rows=2 width=4)
             Filter: ((name)::text = 'William Gibson'::text)
  (5 rows)
  ```

* Looking at the two outputs, the estimated cost for the subquery, `22.16`, is lower than that for the join, `27.62`. This is not necessarily always the case, and often joins are more efficient than subqueries.

###### EXPLAIN ANALYZE

* An important thing to remember is that when you use `EXPLAIN`, the query is not actually run. The values that `EXPLAIN` outputs are estimates, based on the planner's knowledge of the schema and assumptions on PostgreSQL system statistics. In order to assess a query using actual data, you can add the `ANALYZE` option to an `EXPLAIN` command.

  ```sql
  my_books=# EXPLAIN ANALYZE SELECT books.title FROM books
  my_books-# JOIN authors ON books.author_id = authors.id
  my_books-# WHERE authors.name = 'William Gibson';
                                                    QUERY PLAN
  --------------------------------------------------------------------------------------------------------------
   Hash Join  (cost=14.03..27.62 rows=2 width=218) (actual time=0.029..0.034 rows=3 loops=1)
     Hash Cond: (books.author_id = authors.id)
     ->  Seq Scan on books  (cost=0.00..12.60 rows=260 width=222) (actual time=0.009..0.012 rows=7 loops=1)
     ->  Hash  (cost=14.00..14.00 rows=2 width=4) (actual time=0.010..0.010 rows=1 loops=1)
           Buckets: 1024  Batches: 1  Memory Usage: 9kB
           ->  Seq Scan on authors  (cost=0.00..14.00 rows=2 width=4) (actual time=0.006..0.007 rows=1 loops=1)
                 Filter: ((name)::text = 'William Gibson'::text)
                 Rows Removed by Filter: 2
   Planning time: 0.201 ms
   Execution time: 0.074 ms
  (10 rows)
  ```

* Using the `ANALYZE` option actually runs the query and, in addition to the output normally returned by `EXPLAIN`, includes the actual time (in milliseconds) required to run the query and its constituent parts, as well as the actual rows returned by each plan node rather than just a number of rows based on default statistics.

---

### Subqueries

---

#### Subquery Expressions

###### EXISTS

* `EXISTS` effectively checks whether _any_ rows at all are returned by the nested query.
* If at least one row is returned then the result of `EXISTS` is 'true', otherwise it is 'false'.

###### IN

* `IN` compares an evaluated expression to every row in the subquery result.
* If a row equal to the evaluated expression is found, then the result of `IN` is 'true', otherwise it is 'false'.

###### NOT IN

* `NOT IN` is similar to `IN` except that the result of `NOT IN` is 'true' if an equal row is **not** found, and 'false' otherwise.

###### ANY/ SOME

* `ANY` and `SOME` are synonyms, and can be used interchangeably. These expressions are used along with an operator (e.g. `=`, `<`, `>`, etc). The result of `ANY` / `SOME` is 'true' if _any_ true result is obtained when the expression to the left of the operator is evaluated using that operator against the results of the nested query.
* Note: when the `=` operator is used with `ANY` / `SOME`, this is equivalent to `IN`.

###### ALL

* As with `ANY` / `SOME`, `ALL` is used along with an operator.
* The result of `ALL` is true only if _all of_ the results are true when the expression to the left of the operator is evaluated using that operator against the results of the nested query.
* Note: when the `<>` / `!=` operator is used with `ALL`, this is equivalent to `NOT IN`.

#### When to use Subqueries

* We've seen that when comparing subqueries and joins that produce the same results, one can sometimes be faster than the other. When first formulating your queries however, you probably aren't testing them for speed; optimization usually comes as a later step in a project.
* When you're not yet at the optimization stage and performance isn't a factor, the decision over whether to use a subquery over a join will often come down to personal preference. There are however valid arguments to say that subqueries are more readable or make more logical sense in some situations. For example, if you want to return data from one table conditional on data from another table, but don't need to return any data from the second table, then a subquery may make more logical sense and be more readable. If you need to return data from both tables then you would need to use a join.

---



