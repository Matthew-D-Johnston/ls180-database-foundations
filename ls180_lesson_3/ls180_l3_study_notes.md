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

