
#### **Views**

```sql
CREATE VIEW faculty AS
SELECT ID, name, dept_name 
FROM instructor;

CREATE VIEW departments_total_salary(dept_name, total_salary) AS
SELECT dept_name, SUM(salary)
FROM instructor
GROUP BY dept_name;

--View Dependencies

CREATE VIEW physics_fall_2009 AS
SELECT course.course_id, sec_id, building, room_number 
FROM course, section 
WHERE course.course_id = section.course_id 
AND course.dept_name = 'Physics' 
AND section.semester = 'Fall' 
AND section.year = '2009';

CREATE VIEW physics_fall_2009_watson AS 
SELECT course_id, room_number 
FROM physics_fall_2009 
WHERE building = 'Watson';
```
##### Updating Views
- A view can only be updated if there is a **one-to-one** relationship between rows in the view and the base table.
- For **updatable views**:
    - The `FROM` clause must refer to only one table.
    - The `SELECT` clause can only contain column names, not expressions, aggregates, or `DISTINCT`.
    - No `GROUP BY` or `HAVING` clauses are allowed.

##### Materialized Views
- Unlike regular views, **materialized views** store the results of a query in a physical table.
- **Benefits**: Improved performance, since the data does not need to be dynamically retrieved every time the view is queried.
- **Challenge**: If the underlying tables are updated, the materialized view becomes outdated and must be refreshed.
- **Not supported in MySQL**.

#### **Transactions**

A **transaction** in a database is a sequence of operations performed as a single logical unit of work. These operations must satisfy the **ACID** properties to ensure data integrity and reliability. 

Atomicity  -  Either all the operations succeed, or none of them take effect (rolled back if an error occurs)
Consistency  -  Ensures that a transaction leaves the database in a valid state
Isolation  -  Transactions operate independently and transparent to each other
Durability  -  Once a transaction is committed, the changes are permanent, even if there’s a system crash


```sql
START TRANSACTION;

-- Step 1: Check if the seat is available
SELECT Availability 
FROM Seat 
WHERE Seat_ID = 'S001' 
  AND Airplane_ID = 123 
  AND Availability = 'Available'
FOR UPDATE;

-- Step 2: Update seat availability to 'Unavailable'
UPDATE Seat 
SET Availability = 'Unavailable' 
WHERE Seat_ID = 'S001' 
  AND Airplane_ID = 123;

-- Step 3: Insert the booking record
INSERT INTO Booking (Booking_ID, Flight_ID, User_ID, Passenger_ID, Seat_ID, Price) 
VALUES ('B1001', 'F001', 'U001', 200, 'S001', 350.00);

-- Commit the transaction if all steps are successful
COMMIT;

-- If any step fails, roll back the entire transaction
ROLLBACK;
```


#### **Integrity Constraints**

```sql
NOT NULL
PRIMARY KEY
UNIQUE
CHECK
```

**Cascading Actions in Referential Integrity**

```sql
ON DELETE CASCADE
ON DELETE RESTRICT
ON DELETE SET NULL
ON DELETE SET DEFAULT
ON UPDATE CASCADE
```


```sql
--Rename
SELECT ID, name, salary/12 AS monthly_salary 
FROM instructor;


--Ordering
SELECT DISTINCT name 
FROM instructor 
ORDER BY name DESC;


--String Operations
SELECT name 
FROM instructor 
WHERE name LIKE '%dar%';


--Aggregate Functions
SELECT AVG(salary) 
FROM instructor 
WHERE dept_name = 'Comp. Sci.';

SELECT COUNT(DISTINCT ID) 
FROM teaches 
WHERE semester = 'Spring' AND year = 2010;
```


#### **Built-in Data Types in SQL**

1. **Date**: Represents a calendar date (year, month, day).

```sql
DATE '2005-07-27'
```

2. **Time**: Represents the time of day in hours, minutes, and seconds (with optional fractional seconds).

```sql
TIME '09:00:30'
TIME '09:00:30.75'  -- with fractional seconds
```

3. **Timestamp**: Combines both date and time into one data type.

```sql
TIMESTAMP '2005-07-27 09:00:30.75'
```

4. **Interval**: Represents a period of time (like days, months, or years) and can be used for date/time arithmetic.

```sql
INTERVAL '1' DAY

--Can be used for operations like subtracting dates:

SELECT CURRENT_DATE - INTERVAL '1' DAY;  -- Subtracts one day from the current date
```

#### **User-Defined Types**

**Custom Data Types**: SQL allows users to create their own types with specific constraints.

```sql
CREATE TYPE Dollars AS NUMERIC(12,2) FINAL;
```

This creates a `Dollars` type to store monetary values with two decimal places.

#### **Domains**

**Domain Creation**: Domains are user-defined types that can have additional constraints like `NOT NULL` or `CHECK`.

```sql
CREATE DOMAIN person_name CHAR(20) NOT NULL;

--You can specify conditions on the domain using `CHECK` constraints.

CREATE DOMAIN degree_level VARCHAR(10)
CONSTRAINT degree_level_test CHECK (VALUE IN ('Bachelors', 'Masters', 'Doctorate'));
```

#### **Large-Object Types**

- **BLOB (Binary Large Object)**: Used for storing large collections of binary data (e.g., images, videos). These are uninterpreted by the database and handled by applications outside the database.

```sql 
BLOB
```

- **CLOB (Character Large Object)**: Used for storing large collections of character data, such as text documents.

```sql
CLOB
```

- **Pointers for Large Objects**: When querying large objects, instead of returning the entire data, the database returns a pointer to the large object, improving performance.


#### **Indexing in Databases**

**What is an Index?**

- An **index** is a data structure used to quickly locate and access the data rows in a database table based on the values in one or more columns.
- Indices help speed up queries, particularly for `SELECT` queries that search for records with specific column values.

**Example: Creating an Index**

To create an index on a column (or a combination of columns), the `CREATE INDEX` statement is used.

```sql
CREATE INDEX student_dept_name_index ON student(dept_name);
```

This index on the `dept_name` column allows the database to quickly find rows where `dept_name = 'Physics'` without having to scan the entire table.

**Types of Index Data Structures:**

- **B-Trees**: Most common indexing method. B-Trees maintain a sorted structure, enabling efficient range queries and sorted data retrieval.
- **Hash Tables**: Useful for exact match lookups, but not for range queries.
- **R-Trees**: Used for indexing multi-dimensional information, like spatial data (e.g., geographical coordinates).

---

**Advantages of Using an Index:**

1. **Faster search time**: Indices allow the database to skip scanning entire tables and directly access the rows with the required values.
    - Beneficial in `SELECT` queries or when performing `JOIN` operations.
2. **Improves query performance**: Especially effective when dealing with large datasets where queries need to filter or sort records based on certain columns.

---

**Disadvantages of Using an Index:**

1. **Takes up space**: Indexes consume additional disk space, and the size grows with the number of rows in the table.
2. **Slower write operations**: Inserting, updating, or deleting rows becomes slower because the index needs to be updated alongside the data.


[[Advanced SQL]]