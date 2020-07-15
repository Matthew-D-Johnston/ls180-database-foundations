##### LS180 - SQL Fundamentals > DML (Data Manipulation Language)

---

### 1. Set Up Database

---

For this set of exercises we'll want to create a new database and some tables to work with. The theme for the exercise is that of a workshop. Create a database to store information and tables related to this workshop.

One table should be called `devices`. This table should have columns that meet the following specifications:

- Includes a primary key called `id` that is auto-incrementing.
- A column called `name`, that can contain a String. It cannot be `NULL`.
- A column called `created_at` that lists the date this device was created. It should be of type `timestamp` and it should also have a default value related to when a device is created.

In the workshop, we have several devices, and each device should have many different parts. These parts are unique to each device, so one device can have various parts, but those parts won't be used in any other device. Make a table called `parts` that reflects the information listed above. Table `parts` should have the following columns that meet the following specifications:

- An `id` which auto-increments and acts as the primary key
- A `part_number`. This column should be unique and not null.
- A foreign key column called `device_id`. This will be used to associate various parts with a device.

###### My Solution:

`devices` table:

```sql
CREATE TABLE devices (
	id serial PRIMARY KEY,
	name text NOT NULL,
	created_at timestamp DEFAULT now
);
```

`parts` table:

```sql
CREATE TABLE parts (
	id serial PRIMARY KEY,
	part_number UNIQUE NOT NULL,
	device_id integer REFERENCES devices (id)
);
```

###### LS Solution:

```sql
-- Create our workshop database
CREATE DATABASE workshop;

-- Make workshop the current database
\c workshop

-- Create a devices table
CREATE TABLE devices (
	id serial PRIMARY KEY,
	name text NOT NULL,
	created_at timestamp DEFAULT CURRENT_TIMESTAMP
);

-- Create a parts table
CREATE TABLE parts (
	id serial PRIMARY KEY,
	part_number integer UNIQUE NOT NULL,
	device_id integer REFERENCES devices (id)
);
```

---

### 2. Insert Data for Parts and Devices

---

Now that we have the infrastructure for our workshop set up, let's start adding in some data. Add in two different devices. One should be named, "Accelerometer". The other should be named, "Gyroscope".

The first device should have 3 parts (this is grossly simplified). The second device should have 5 parts. The part numbers may be any number between 1 and 10000. There should also be 3 parts that don't belong to any device yet.

###### My Solution:

```sql
INSERT INTO devices (name) VALUES ('Accelerometer');
INSERT INTO devices (name) VALUES ('Gyroscope');

INSERT INTO parts (part_number, device_id) VALUES (100, 1);
INSERT INTO parts (part_number, device_id) VALUES (101, 1);
INSERT INTO parts (part_number, device_id) VALUES (102, 1);

INSERT INTO parts (part_number, device_id) VALUES (200, 2);
INSERT INTO parts (part_number, device_id) VALUES (201, 2);
INSERT INTO parts (part_number, device_id) VALUES (202, 2);
INSERT INTO parts (part_number, device_id) VALUES (203, 2);
INSERT INTO parts (part_number, device_id) VALUES (204, 2);

INSERT INTO parts (part_number) VALUES (300);
INSERT INTO parts (part_number) VALUES (301);
INSERT INTO parts (part_number) VALUES (302);
```

###### LS Solution:

```sql
INSERT INTO devices (name) VALUES ('Accelerometer');
INSERT INTO devices (name) VALUES ('Gyroscope');

INSERT INTO parts (part_number, device_id) VALUES (12, 1);
INSERT INTO parts (part_number, device_id) VALUES (14, 1);
INSERT INTO parts (part_number, device_id) VALUES (16, 1);

INSERT INTO parts (part_number, device_id) VALUES (31, 2);
INSERT INTO parts (part_number, device_id) VALUES (33, 2);
INSERT INTO parts (part_number, device_id) VALUES (35, 2);
INSERT INTO parts (part_number, device_id) VALUES (37, 2);
INSERT INTO parts (part_number, device_id) VALUES (39, 2);

INSERT INTO parts (part_number) VALUES (50);
INSERT INTO parts (part_number) VALUES (54);
INSERT INTO parts (part_number) VALUES (58);
```

---

### 3. INNER JOIN

---

Write an SQL query to display all devices along with all the parts that make them up. We only want to display the name of the `devices`. Its `parts` should only display the `part_number`.

###### My Solution:

```sql
SELECT d.name, p.part_number
	FROM devices AS d
 INNER JOIN parts AS p
 		ON d.id = p.device_id;
```

###### LS Solution

```sql
SELECT devices.name, parts.part_number FROM devices
INNER JOIN parts ON devices.id = parts.device_id;
```

---

### 4. SELECT Part Number

---

We want to grab all parts that have a `part_number` that starts with 3. Write a `SELECT` query to get this information. This table may show all attributes of the `parts` table.

###### My Solution

```sql
SELECT * FROM parts
 WHERE part_number::text LIKE '3%';
```

###### LS Solution

```sql
SELECT * FROM parts WHERE part_number::text LIKE '3%';
 id | part_number | device_id
----+-------------+-----------
  4 |          31 |         2
  5 |          33 |         2
  6 |          35 |         2
  7 |          37 |         2
  8 |          39 |         2
(5 rows)
```

---

### 5. Aggregate Functions

---

Write an SQL query that returns a result table with the name of each device in our database together with the number of parts for that device.

###### My Solution

```sql
SELECT d.name, count(p.part_number)
	FROM devices AS d
 INNER JOIN parts AS p
 		ON d.id = p.device_id
 GROUP BY d.name;
```

###### LS Solution

```sql
SELECT devices.name AS name, COUNT(parts.device_id) FROM devices
	LEFT OUTER JOIN parts ON devices.id = parts.device_id GROUP BY devices.name;
```

---

### 6. ORDER BY

---

In the previous exercise, we had to use a `GROUP BY` clause to obtain the expected output. In that exercise, we used an SQL query like:

```sql
SELECT devices.name AS name, COUNT(parts.device_id)
FROM devices
JOIN parts ON devices.id = parts.device_id
GROUP BY devices.name;
```

Now, we want to work with the same query, but we want to guarantee that the devices and the count of their parts are listed in descending alphabetical order. Alter the SQL query above so that we get a result table that looks like the following.

```
name          | count
--------------+-------
Gyroscope     |     5
Accelerometer |     3
(2 rows)
```

###### My Solution

```sql
SELECT d.name, count(p.device_id)
	FROM devices AS d
	LEFT OUTER JOIN parts AS p
		ON d.id = p.device_d
 GROUP BY d.name
 ORDER BY d.name DESC;
```

---

### 7. IS NULL and IS NOT NULL

---

Write two SQL queries:

- One that generates a listing of parts that currently belong to a device.
- One that generates a listing of parts that don't belong to a device.

Do not include the `id` column in your queries.

**Expected Output:**

```psql
part_number | device_id 
------------+-----------
         12 |         1
         14 |         1
         16 |         1
         31 |         2
         33 |         2
         35 |         2
         37 |         2
         39 |         2
(8 rows)
```

```psql
part_number | device_id 
------------+-----------
         50 |          
         54 |          
         58 |        
(3 rows)
```

###### My Solution

Listing of parts that _do_ belong to a device:

```sql
SELECT part_number, device_id
	FROM parts
 WHERE device_id IS NOT NULL;
```

Listing of parts that _do not_ belong to a device

```sql
SELECT part_number, device_id
  FROM parts
 WHERE device_id IS NULL;
```

---

### 8. Oldest Device

---

Insert one more device into the `devices` table. You may use this SQL statement or create your own.

```sql
INSERT INTO devices (name) VALUES ('Magnetometer');
INSERT INTO parts (part_number, device_id) VALUES (42, 3);
```

Assuming nothing about the existing order of the records in the database, write an SQL statement that will return the name of the oldest device from our `devices` table.

###### My Solution

```sql
SELECT name FROM devices
 ORDER BY age(created_at) DESC
 LIMIT 1;
```

###### LS Solution

```sql
SELECT name AS oldest_device
	FROM devices
 ORDER BY created_at ASC
 LIMIT 1;
```

---

### 9. UPDATE device_id

---

We've realized that the last two parts we're using for device number 2, "Gyroscope", actually belong to an "Accelerometer". Write an SQL statement that will associate the last two parts from our `parts` table with an "Accelerometer" instead of a "Gyroscope".

###### My Solution

```sql
UPDATE parts
	 SET device_id = 1
 WHERE part_number = 37 OR part_number = 39;
```

###### LS Solution

```sql
UPDATE parts SET device_id = 1
 WHERE part_number=37 OR part_number=39;
```

##### Further Exploration

What if we wanted to set the part with the smallest `part_number` to be associated with "Gyroscope"? How would we go about doing that?

###### My Solution

```sql
UPDATE parts
	 SET device_id = 2
 WHERE part_number = min(part_number);
```

This raises the following error:

```psql
ERROR:  aggregate functions are not allowed in WHERE
LINE 3: WHERE part_number = min(part_number);
```

I could just use the `min(part_number)` to retrieve the smallest part number and then once I have it use it in my `SELECT` query. But surely there is a way that I can retrieve it and update it at the same time???

Other submitted solution:

```sql
UPDATE parts SET device_id = 2
WHERE part_number = (SELECT MIN(part_number) FROM parts);
```

---

### 10. Delete Accelerometer

---

Our workshop has decided to not make an Accelerometer after all. Delete any data related to "Accelerometer", this includes the parts associated with an Accelerometer.

###### My Solution

```sql
DELETE FROM devices
 WHERE name = 'Accelerometer';
 
DELETE FROM parts
 WHERE device_id = 1;
```

The statements are correct, but I need to reverse the order in which they are executed so as to avoid an error.

##### Further Exploration

This process may have been a bit simpler if we had initially defined our devices tables a bit differently. We could delete both a device and its associated parts with one SQL statement if that were the case. What change would have to be made to table `parts` to make this possible? Also, what SQL statement would you have to write that can delete both a device and its parts all in one go?

###### My Solution

We would need to change the `parts` table so that the parts associated with the device that is being deleted would also be deleted. I think that this could be achieved by some sort of additional constraint applied to the `device_id` foreign key. We can achieve this by first dropping the foreign key constraint on `device_id` and then reapplying the constraint with the added `ON DELETE CASCADE` attribute.

```sql
ALTER TABLE parts
DROP CONSTRAINT parts_device_id_fkey;

ALTER TABLE parts
	ADD CONSTRAINT parts_device_id_fkey FOREIGN KEY (device_id) REFERENCES devices (id)
	 ON DELETE CASCADE;
```



