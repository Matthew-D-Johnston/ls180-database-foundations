##### LS180 - SQL Fundamentals > DDL (Data Definition Language)

---

### 1. Create an Extrasolar Planetary Database

---

For this exercise set, we will be working with the DDL statements to create and modify tables in a database that tracks planets around stars other than our Sun. To get started, first create a postgresql database named "extrasolar", and then create two tables in the database as follows:

`stars` table

- `id`: a unique serial number that auto-increments and serves as a primary key for this table.
- `name`: the name of the star,e,g., "Alpha Centauri A". Allow room for 25 characters. Must be unique and non-null.
- `distance`: the distance in light years from Earth. Define this as a whole number (e.g., 1, 2, 3, etc) that must be non-null and greater than 0.
- `spectral_type`: the spectral type of the star: O, B, A, F, G, K, and M. Use a one character string.
- `companions`: how many companion stars does the star have? A whole number will do. Must be non-null and non-negative.

`planets` table

- `id`: a unique serial number that auto-increments and serves as a primary key for this table.
- `designation`: a single alphabetic character that uniquely identifies the planet in its star system ('a', 'b', 'c', etc.)
- `mass`: estimated mass in terms of Jupiter masses; use an integer for this value.

###### My Solution:

First, create the database:

```
$ createdb extrasolar
```

Now, create the `stars` table:

```sql
CREATE TABLE stars (
	id serial PRIMARY KEY,
	name varchar(25) UNIQUE NOT NULL,
	distance integer NOT NULL,
	spectral_type char(1),
	companions integer NOT NULL
);

ALTER TABLE stars ADD CHECK (distance > 0);
ALTER TABLE stars ADD CHECK (companions >= 0);
```

Now, create the `planets` table:

```sql
CREATE TABLE planets (
	id serial PRIMARY KEY,
	designation char(1),
	mass integer
);
```

----

### 2. Relating Stars and Planets

---

You may have noticed that, when we created the `stars` and `planets` tables, we did not do anything to establish a relationship between the two tables. They are simply standalone tables that are related only by the fact that they both belong to the `extrasolar` database. However, there is no relationship between the rows of each table.

To correct this problem, add a `star_id` column to the `planets` table; this column will be used to relate each planet in the `planets` table to its home star in the `stars` table. Make sure the row is defined in such a way that it is required and must have a value that is present as an `id` in the `stars` table.

###### My Solution:

```sql
ALTER TABLE planets ADD COLUMN star_id integer NOT NULL REFERENCES stars (id); 
```

---

### 3. Increase Star Name Length

---

Hmm... it turns out that 25 characters isn't enough room to store a star's complete name. For instance, the star "Epsilon Trianguli Australis A" requires 30 characters. Modify the column so that it allows star names as long as 50 characters.

###### My Solution:

```sql
ALTER TABLE stars ALTER COLUMN name TYPE varchar(50);
```

##### Further Exploration:

Assume the `stars` table already contains one or more rows of data. What will happen when you try to run the above command? To test this, revert the modification and add a row or two of data.  

###### My Response:

The command will execute without any issues regardless of whether or not the table is already populated with data.

---

#### 4. Stellar Distance Precision

---

For many of the closest stars, we know the distance from Earth fairly accurately; for instance, Proxima Centauri is roughly 4.3 light years away. Our database, as currently defined, only allows integer distances, so the most accurate value we can enter is 4. Modify the `distance` column in the `stars` table so that it allows fractional light years to any degree of precision required.

###### My Solution:

```sql
ALTER TABLE stars ALTER COLUMN distance TYPE numeric;
```

##### Further Exploration:

Assume the `stars` table already contains one or more rows of data. What will happen when you try to run the above command? To test this, revert the modification and add a row or two of data.  

###### My Response:

The above command will execute without an error being raised. However, I noticed that if their is a decimal number in the data and the data type is changed from type numeric to type integer, the decimal number will be truncated so that it is just an integer being displayed in the table.

---

### 5. Check Values in List

---

The `spectral_type` column in the `stars` table is currently defined as a one-character string that contains one of the following 7 values: `'O'`, `'B'`, `'A'`, `'F'`, `'G'`, `'K'`, and `'M'`. However, there is currently no enforcement on the values that may be entered. Add a constraint to the table `stars` that will enforce the requirement that a row must hold one of the 7 listed values above. Also, make sure that a value is required for this column.

###### My Solution:

```sql
ALTER TABLE stars
	ADD CHECK spectral_type = 'O'
	 OR spectral_type = 'B'
	 OR spectral_type = 'A'
	 OR spectral_type = 'F'
	 OR spectral_type = 'G'
	 OR spectral_type = 'K'
	 OR spectral_type = 'M';

ALTER TABLE stars ALTER COLUMN spectral_type SET NOT NULL;
```

###### LS Solution:

```sql
ALTER TABLE stars
ADD CHECK (spectral_type IN ('O', 'B', 'A', 'F', 'G', 'K', 'M')),
ALTER COLUMN spectral_type SET NOT NULL;
```

##### Further Exploration

Assume the `stars` table contains one or more rows that are missing a `spectral_type` value, or that have an illegal value. What will happen when you try to alter the table schema? How would you go about adjusting the data to work around this problem. To test this, revert the modification and add some data.

###### My Response

For this part of the code,

```sql
ALTER TABLE stars
ADD CHECK (spectral_type IN ('O', 'B', 'A', 'F', 'G', 'K', 'M'));
```

the following error is raised:

```
ERROR:  check constraint "stars_spectral_type_check" is violated by some row
```

And for the `NOT NULL` constraint,

```sql
ALTER TABLE stars
ALTER COLUMN spectral_type SET NOT NULL;
```

the following error is raised:

```
ERROR:  column "spectral_type" contains null values
```

To fix this problem, we could insert in the appropriate spectral type for the star that currently contains a null value, and for the star that has an invalid spectral type we could change it to a valid one.  For example,

```sql
UPDATE stars
	 SET spectral_type = 'A'
 WHERE spectral_type IS NULL;
 
UPDATE stars
	 SET spectral_type = 'B'
 WHERE spectral_type = 'X';

ALTER TABLE stars
	ADD CHECK (spectral_type IN ('O', 'B', 'A', 'F', 'G', 'K', 'M')),
ALTER COLUMN spectral_type
	SET NOT NULL;
```

---

### 6. Enumerated Types

---

In the previous exercise, we added a `CHECK` constraint to the `stars` table to enforce that the value stored in the `spectral_type` column for each row is valid. In this exercise, we will use an alternate approach to the same problem.

PostgreSQL provides what is called an enumerated data type; that is, a data type that must have one of a finite set of values. For instance, a traffic light can be red, green, or yellow: these are enumerate values for the color of the currently lit signal light.

Modify the `stars` table to remove the `CHECK` constraint on the `spectral_type` column, and then modify the `spectral_type` column so it becomes an enumerated type that restricts it to one of the following 7 values: `'O'`, `'B'`, `'A'`, `'F'`, `'G'`, `'K'`, and `'M'`.

###### My Solution:

```sql
ALTER TABLE stars
 DROP CONSTRAINT stars_spectral_type_check;

CREATE TYPE spectral_type_values AS ENUM ('O', 'B', 'A', 'G', 'F', 'K', 'M');

ALTER TABLE stars
ALTER COLUMN spectral_type TYPE spectral_type_values;
```

###### LS Solution:

```sql
ALTER TABLE stars
DROP CONSTRAINT stars_spectral_type_check;

CREATE TYPE spectral_type_enum AS ENUM ('O', 'B', 'A', 'F', 'G', 'K', 'M');

ALTER TABLE stars
ALTER COLUMN spectral_type TYPE spectral_type_enum
													 USING spectral_type::spectral_type_enum;
```

---

### 7. Planetary Mass Precision

---

We will measure Planetary masses in terms of the mass of Jupiter. As such, the current data type of `integer` is inappropriate; it is only really useful for planets as large as Jupiter or larger. These days, we know of many extrasolar planets that are smaller than Jupiter, so we need to be able to record fractional parts for the mass. Modify the `mass` column in the `planets` table so that it allows fractional masses to any degree of precision required. In addition, make sure the mass is required and positive.

While we're at it, also make the `designation` column required.

###### My Solution:

```sql
ALTER TABLE planets
ALTER COLUMN mass
 TYPE numeric;

ALTER TABLE planets
ALTER COLUMN mass
  SET NOT NULL;
  
ALTER TABLE planets
	ADD CHECK (mass >= 0);
  
ALTER TABLE planets
ALTER COLUMN designation
	SET NOT NULL;
```

###### LS Solution:

```sql
ALTER TABLE planets
ALTER COLUMN mass TYPE numeric,
ALTER COLUMN mass SET NOT NULL,
ADD CHECK (mass > 0.0),
ALTER COLUMN designation SET NOT NULL;
```

---

### 8. Add a Semi-Major Axis Column

---

Add a `semi_major_axis` column for the semi-major axis of each planet's orbit; the semi-major axis is the average distance from the planet's star as measured in astronomical units (1 AU is the average distance of the Earth from the Sun). Use a data type of `numeric`, and require that each planet have a value for the `semi_major_axis`.

###### My Solution:

```sql
ALTER TABLE planets
	ADD COLUMN semi_major_axis numeric NOT NULL;
```

##### Further Exploration

Assume the `planets` table already contains one or more rows of data. What will happen when you try to run the above command? What will you need to do differently to obtain the desired result? To test this, delete the `semi_major_axis` column and then add a row or two of data (note: your `stars` table will also need some data that corresponds to the `star_id` values):

```sql
ALTER TABLE planets
DROP COLUMN semi_major_axis;

DELETE FROM stars;
INSERT INTO stars (name, distance, spectral_type, companions)
           VALUES ('Alpha Centauri B', 4.37, 'K', 3);
INSERT INTO stars (name, distance, spectral_type, companions)
           VALUES ('Epsilon Eridani', 10.5, 'K', 0);

INSERT INTO planets (designation, mass, star_id)
           VALUES ('b', 0.0036, 1); -- check star_id; see note below
INSERT INTO planets (designation, mass, star_id)
           VALUES ('c', 0.1, 2); -- check star_id; see note below

ALTER TABLE planets
ADD COLUMN semi_major_axis numeric NOT NULL;
```

NOTE: The `semi_major_axis` for Alpha Centauri B planet b is 0.04 AU while that for Epsilon Eridani planet c is about 40 AU. Note also that the `star_id` values shown above may be different from what is in your database. Check your `stars` table to see which `star_id` values to use.

###### My Response:

We could first add the `semi_major_axis` column without the `NOT NULL` constraint. Then, insert the appropriate values for the entries that already exist in the tables. Once they have values, we can then impose a `NOT NULL` constraint:

```sql
ALTER TABLE planets
	ADD COLUMN semi_major_axis numeric;
	
UPDATE planets
	 SET semi_major_axis = 0.04
 WHERE star_id = 8;
 
UPDATE planets
	 SET semi_major_axis = 40
 WHERE star_id = 9;
 
ALTER TABLE planets
ALTER COLUMN semi_major_axis SET NOT NULL;
```

---

### 9. Add a Moons Table

---

Someday in the future, technology will allow us to start observing the moons of extrasolar planets. At that point, we're going to need a `moons` table in our `extrasolar` database. For this exercise, your task is to add that table to the database. The table should include the following data:

- `id`: a unique serial number that auto-increments and serves as a primary key for this table.
- `designation`: the designation of the moon. We will assume that moon designations will be numbers, with the first moon discovered for each planet being moon 1, the second moon being moon 2, etc. The designation is required.
- `semi_major_axis`: the average distance in kilometers from the planet when a moon is farthest from its corresponding planet. This field must be a number greater than 0, but is not required; it may take some time before we are able to measure moon-to-planet distances in extrasolar systems.
- `mass`: the mass of the moon measured in terms of Earth Moon masses. This field must be a numeric value greater than 0, but is not required.

Make sure you also specify any foreign keys necessary to tie each moon to other rows in the database.

###### My Solution:

```sql
CREATE TABLE moons (
	id serial PRIMARY KEY,
	designation integer NOT NULL CHECK (designation > 0),
	semi_major_axis numeric CHECK (semi_major_axis > 0),
	mass numeric CHECK (mass > 0),
  planets_id integer REFERENCES planets (id)
);
```

###### LS Solution:

```sql
CREATE TABLE moons (
	id serial PRIMARY KEY,
	designation integer NOT NULL CHECK (designation > 0),
	semi_major_axis numeric CHECK (semi_major_axis > 0.0),
	mass numeric CHECK (mass > 0.0),
	planet_id integer NOT NULL REFERENCES planets (id)
);
```

---

### 10. Delete the Database

---

Delete the `extrasolar` database. Use the `psql` console -- don't use the terminal level commands.

###### My Solution:

Make sure to switch to a different database.

```sql
\c sql-course
```

Then drop the database:

```sql
DROP DATABASE extrasolar;
```

###### LS Solution:

```sql
\c other_database
DROP DATABASE extrasolar;
```

To delete a database, you can use the `DROP DATABASE` statement; this will delete the entire database, including any tables and other objects associated with the database, and the data associated with these tables and objects. However, before you can do this, you must first make sure the database you want to delete isn't open: to do this, you must use the `\c` console command to connect to another database.

Note that `DROP DATABASE` is extremely destructive: there is no confirmation prompt, and no backups are made of the database. Use this statement with extreme care. In particular, it's a good idea to make a backup of your database before you drop it (also called a database dump). You can make a backup like this from the terminal:

```
$ pg_dump --inserts extrasolar > extrasolar.dump.sql
```





