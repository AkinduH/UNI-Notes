
### JDBC and ODBC Overview

#### What are JDBC and ODBC?
- **ODBC (Open Database Connectivity)** and **JDBC (Java Database Connectivity)** are APIs that allow applications to interact with databases. 
- Both provide mechanisms to connect to a database, send SQL queries, fetch results, and update the database.

### ODBC
- **ODBC** works with languages such as **C, C++, C#, and Visual Basic**.
- It acts as a common API to interact with multiple databases (like SQL Server, Oracle, MySQL) by abstracting the database specifics.

### JDBC
- **JDBC** is a Java API for connecting and executing queries in databases.
- It is widely used for **SQL-based database management systems** in **Java applications**.

---

### JDBC Workflow
1. **Open a Connection**: Connect to the database using a `Connection` object.
2. **Create a Statement**: SQL queries are executed via a `Statement` object.
3. **Execute Queries**: The statement is used to run SQL queries such as `SELECT`, `INSERT`, `UPDATE`, and `DELETE`.
4. **Fetch Results**: Queries that return results use a `ResultSet` object.
5. **Close the Connection**: After operations are done, the connection is closed to release resources.
6. **Error Handling**: JDBC uses exceptions (`SQLException`) to manage errors.

---

### JDBC Example - Insert Operation:

```java
try {
    statement.executeUpdate("INSERT INTO instructor VALUES ('77987', 'Kim', 'Physics', 98000)");
} catch (SQLException e) {
    log.error("Unable to access the database server: ", e);
}
```
This inserts a new instructor into the `instructor` table. If an error occurs (e.g., a violation of constraints), it throws an `SQLException`.

### Fetching Results:
To retrieve and process query results, a `ResultSet` is used:

```java
ResultSet resultSet = statement.executeQuery("SELECT dept_name, AVG(salary) FROM instructor GROUP BY dept_name");
while (resultSet.next()) {
    System.out.println(resultSet.getString("dept_name") + " : " + resultSet.getFloat(2));
}
```
This fetches and prints the department names and their average salaries.

---

### Prepared Statements
- **Prepared Statements** are used to execute SQL queries with parameters, preventing **SQL injection** attacks and optimizing performance.
  
Example of inserting data using a `PreparedStatement`:

```java
PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO instructor VALUES (?, ?, ?, ?)");
preparedStatement.setString(1, "88877");
preparedStatement.setString(2, "Perry");
preparedStatement.setString(3, "Finance");
preparedStatement.setInt(4, 125000);
preparedStatement.executeUpdate();
```

- Prepared statements **sanitize user inputs** to prevent SQL injection and are more efficient for queries executed multiple times with different values.

---

### SQL Injection Example
If user input is directly concatenated into an SQL query, it can lead to SQL injection:

```sql
"SELECT * FROM instructor WHERE name = '" + name + "'"
```
- If a malicious user enters `X' or 'Y' = 'Y`, the query becomes:
  ```sql
  SELECT * FROM instructor WHERE name = 'X' OR 'Y' = 'Y'
  ```
  This returns all rows because `'Y' = 'Y'` is always true.

Prepared statements avoid this by sanitizing inputs:

```java
PreparedStatement ps = connection.prepareStatement("SELECT * FROM instructor WHERE name = ?");
ps.setString(1, "X' OR 'Y' = 'Y");
```

---

### Transaction Control in JDBC
- **Auto-commit** is enabled by default, meaning each SQL statement is treated as a separate transaction.
- In JDBC, you can disable auto-commit for multi-statement transactions to control when commits or rollbacks occur:
  
Example of turning off auto-commit:
 
```java
conn.setAutoCommit(false);  // Disable auto-commit
// Execute multiple updates...
conn.commit();  // Commit the transaction
// Or rollback in case of failure
conn.rollback();  // Rollback if needed
```
This ensures atomicity and consistency when executing multiple SQL statements.

---

### Other JDBC Features
1. **Handling Large Objects**:
   - You can handle large objects (like blobs and clobs) with the `getBlob()` and `getClob()` methods.
   
2. **Metadata Features**:
   - Metadata of a result set can be accessed using `ResultSetMetaData`. It provides information such as column names and types:

     ```java
     ResultSetMetaData rsmd = rs.getMetaData();
     for (int i = 1; i <= rsmd.getColumnCount(); i++) {
         System.out.println(rsmd.getColumnName(i));
         System.out.println(rsmd.getColumnTypeName(i));
     }
     ```


### ODBC - Open Database Connectivity Standard
**ODBC** is a standard API that enables communication between application programs and database servers. It is designed to allow applications to:
- **Open a connection** to a database.
- **Send queries and updates** to the database.
- **Receive results** from the database.

#### Key Features of ODBC:
1. **Cross-language support**: Initially designed for Basic and C, ODBC versions are available for many languages.
2. **Driver-based**: Each database system provides an ODBC driver, which acts as a library linked to the client program.
3. **API Structure**:
   - SQL environment must be initialized with a handle.
   - A connection handle is allocated to establish a database connection.
   - SQL commands are executed, and results are fetched through this connection.

#### ODBC Example:
Here’s a basic example of an ODBC program in C:
```c
int ODBCexample() {
    RETCODE error;
    HENV env;       // Environment handle
    HDBC conn;      // Connection handle
    
    // Allocate environment and connection handles
    SQLAllocEnv(&env);
    SQLAllocConnect(env, &conn);
    
    // Connect to the database
    SQLConnect(conn, "db.yale.edu", SQL_NTS, "avi", SQL_NTS, "avipasswd", SQL_NTS);
    
    // ... Perform work (queries, updates, etc.) ...
    
    // Disconnect from the database and free resources
    SQLDisconnect(conn);
    SQLFreeConnect(conn);
    SQLFreeEnv(env);
}
```
In this example:
- `SQLAllocEnv()` and `SQLAllocConnect()` allocate the SQL environment and connection handles.
- `SQLConnect()` establishes a connection to the specified database.
- After work is done, `SQLDisconnect()`, `SQLFreeConnect()`, and `SQLFreeEnv()` are used to release the resources.

---

### Embedded SQL

**Embedded SQL** refers to the integration of SQL commands within a host programming language (like C, Java, or Cobol). SQL is embedded within the code of the host language, allowing the application to interact directly with a database.

#### Features of Embedded SQL:

1. **Host Language**: The programming language that embeds SQL commands is referred to as the host language.
2. **Preprocessing**: Before compiling the program, a **preprocessor** replaces the embedded SQL statements with the necessary host language code to run SQL queries at runtime.
3. **EXEC SQL**: In embedded SQL, the `EXEC SQL` statement is used to mark embedded SQL commands.

#### Embedded SQL Example:

```c
EXEC SQL SELECT * FROM instructor WHERE dept_name = 'Physics' END_EXEC;
```
This example fetches all rows from the `instructor` table where the department name is 'Physics'. The preprocessor translates the embedded SQL code into host language commands for execution.

---

### SQLJ (Embedded SQL in Java)

**SQLJ** is a standard for embedding static SQL statements directly into Java code. Unlike JDBC, which is dynamic, **SQLJ** allows for **compile-time error checking**.

#### Why SQLJ Over JDBC?

- **Compile-time error detection**: SQLJ catches errors (such as SQL syntax or type mismatches) during the compilation process, whereas JDBC errors are caught only at runtime.
  
#### SQLJ Example:

```java
#sql iterator deptInfoIter (String dept_name, int avgSal);
deptInfoIter iter = null;
#sql iter = { SELECT dept_name, AVG(salary) 
              FROM instructor 
              GROUP BY dept_name };
while (iter.next()) {
    String deptName = iter.dept_name();
    int avgSal = iter.avgSal();
    System.out.println(deptName + " " + avgSal);
}
iter.close();
```
In this example:
- The SQL query retrieves the department name and the average salary from the `instructor` table.
- The results are iterated over using the `deptInfoIter` iterator.

---

### JDBC vs. SQLJ

- **JDBC**: Dynamic and flexible but errors are detected only at runtime.
- **SQLJ**: More static, but catches errors at compile time, making it safer for embedding SQL in Java applications.


### Procedural Extensions and Stored Procedures in SQL

**SQL** provides a module language that supports procedural logic such as `if-then-else`, loops, and more, enhancing the flexibility and functionality of databases.

#### Stored Procedures:
- **Definition**: A stored procedure is a set of SQL statements stored in the database that can be executed as needed.
- **Purpose**: Stored procedures allow external applications to perform operations on the database without needing to understand its internal structure.
- **Execution**: Stored procedures can be called using the `CALL` statement.

---

### SQL Functions

SQL:1999 introduced **functions and procedures**, which allow embedding complex logic into SQL. These functions can be written in SQL or external languages such as C.

#### Example of an SQL Function:
The function below calculates the number of instructors in a department.
```sql
CREATE FUNCTION dept_count (dept_name VARCHAR(20))
RETURNS INTEGER
BEGIN
   DECLARE d_count INTEGER;
   SELECT COUNT(*) INTO d_count
   FROM instructor
   WHERE instructor.dept_name = dept_name;
   RETURN d_count;
END;
```
This function returns the count of instructors in a given department.

#### Using the Function:
```sql
SELECT dept_name, budget
FROM department
WHERE dept_count(dept_name) > 12;
```
This query retrieves the department name and budget for departments that have more than 12 instructors.

---

### Table Functions (SQL:2003)

**Table functions** return an entire relation (table) as the result of a query, which can be extremely useful for complex data retrieval tasks.

#### Example:
The following function returns all instructors in a specific department.
```sql
CREATE FUNCTION instructors_of (dept_name CHAR(20))
RETURNS TABLE (ID VARCHAR(5), name VARCHAR(20), dept_name VARCHAR(20), salary NUMERIC(8,2))
RETURN TABLE
(SELECT ID, name, dept_name, salary
 FROM instructor
 WHERE instructor.dept_name = instructors_of.dept_name);
```
Usage:
```sql
SELECT *
FROM TABLE (instructors_of('Music'));
```

---

### SQL Procedures

SQL procedures work similarly to functions but allow for **input and output parameters**.

#### Example:
The same `dept_count` function can be written as a procedure:
```sql
CREATE PROCEDURE dept_count_proc (IN dept_name VARCHAR(20), OUT d_count INTEGER)
BEGIN
   SELECT COUNT(*) INTO d_count
   FROM instructor
   WHERE instructor.dept_name = dept_count_proc.dept_name;
END;
```
Usage:
```sql
DECLARE d_count INTEGER;
CALL dept_count_proc('Physics', d_count);
```
This procedure calculates the count of instructors in a department and outputs it into the variable `d_count`.

---

### Procedural Constructs in SQL

SQL supports common procedural constructs such as loops, conditionals, and variable declarations.

#### Example: **While Loop**
```sql
DECLARE n INTEGER DEFAULT 0;
WHILE n < 10 DO
   SET n = n + 1;
END WHILE;
```

#### Example: **For Loop**
```sql
DECLARE n INTEGER DEFAULT 0;
FOR r AS
   SELECT budget FROM department
   WHERE dept_name = 'Music'
DO
   SET n = n - r.budget;
END FOR;
```

---

### External Language Functions and Procedures

**SQL:1999** allows for procedures and functions to be written in external languages like C or C++.

#### Example of External Procedure:
```sql
CREATE PROCEDURE dept_count_proc (IN dept_name VARCHAR(20), OUT count INTEGER)
LANGUAGE C
EXTERNAL NAME '/usr/avi/bin/dept_count_proc';
```
This procedure is written in C and can be linked to the database for execution.

#### Pros and Cons of External Language Functions:
- **Benefits**: They can be more efficient and expressive for specific operations.
- **Drawbacks**: 
  - They pose risks, such as accidental corruption of database structures or security vulnerabilities.
  - Alternatives such as running the external code in a separate process provide better security but may have worse performance.


### Triggers in SQL

**Triggers** are a set of instructions in SQL that automatically execute (or "fire") in response to certain events on a table, such as `INSERT`, `DELETE`, or `UPDATE`. They are primarily used to enforce integrity constraints, automate workflows, and respond to changes in database data.

---

### Trigger Example 1: Enforcing Integrity Constraints
In the case where a foreign key constraint can't be directly enforced, triggers can be used as an alternative to maintain data integrity.

#### Example:
The following trigger ensures that when a new section is added, the `time_slot_id` must exist in the `time_slot` table.
```sql
CREATE TRIGGER timeslot_check1
AFTER INSERT ON section
REFERENCING NEW ROW AS nrow
FOR EACH ROW
WHEN (nrow.time_slot_id NOT IN (
   SELECT time_slot_id
   FROM time_slot))
BEGIN
   ROLLBACK;
END;
```
This trigger rolls back the `INSERT` operation if the inserted `time_slot_id` in the `section` table does not exist in the `time_slot` table.

---

### Trigger Example 2: Maintaining Referential Integrity on Delete
This trigger ensures that when a `time_slot_id` is deleted from the `time_slot` table, it isn't still referenced in the `section` table.

```sql
CREATE TRIGGER timeslot_check2
AFTER DELETE ON time_slot
REFERENCING OLD ROW AS orow
FOR EACH ROW
WHEN (orow.time_slot_id NOT IN (
   SELECT time_slot_id
   FROM time_slot) 
AND orow.time_slot_id IN (
   SELECT time_slot_id
   FROM section))
BEGIN
   ROLLBACK;
END;
```
This trigger prevents the deletion of a `time_slot_id` that is still referenced by a `section`.

---

### Triggering Events and Actions in SQL

- **Triggering Events**: Events that can trigger a trigger include:
  - `INSERT`
  - `DELETE`
  - `UPDATE`
  
- **Before and After Update**: Triggers on updates can specify whether they should run before or after the event. They can also be restricted to specific attributes.
  
#### Example: Handling Update of a Specific Attribute
This trigger sets the `grade` to `NULL` if the grade is updated to an empty string.
```sql
CREATE TRIGGER setnull_trigger
BEFORE UPDATE OF takes
REFERENCING NEW ROW AS nrow
FOR EACH ROW
WHEN (nrow.grade = '')
BEGIN ATOMIC
   SET nrow.grade = NULL;
END;
```

- **Row-Level Triggers**: The default behavior is to execute a trigger action for each affected row.
  
- **Statement-Level Triggers**: Instead of operating on individual rows, you can use statement-level triggers to perform a single action for all rows affected by a transaction. This is useful for efficiently handling bulk updates.
  - Use `FOR EACH STATEMENT` instead of `FOR EACH ROW`.
  - Temporary transition tables (`referencing old table` or `referencing new table`) can be used to handle all affected rows in a single transaction.

---

### When Not to Use Triggers

While triggers are powerful, there are situations where their use is discouraged:
- **Summary Data**: Triggers were historically used to maintain summary data (e.g., total salaries of departments), but modern databases provide **materialized views** that are more efficient for this purpose.
  
- **Database Replication**: In the past, triggers were used to replicate databases, but now most database systems offer built-in replication support.
  
- **Encapsulation**: Instead of using triggers, encapsulation through methods/functions can be a more maintainable and predictable way to handle updates.

#### Risks of Triggers:
- **Unintended Execution**: Triggers may fire unexpectedly during certain operations, such as:
  - Loading data from a backup.
  - Replicating updates at a remote site.
  - Trigger execution can be temporarily disabled during these operations.
  
- **Error Propagation**: If a trigger fails, it can cause the failure of critical transactions.
  
- **Cascading Execution**: Triggers may unintentionally cause a cascade of further triggers to fire, leading to complex and hard-to-trace behaviors.


---

### Ranking in SQL

Ranking functions in SQL allow you to assign a rank to each row based on the values of a specified column. They are typically used with the `ORDER BY` clause to arrange rows according to a particular sorting rule. SQL provides multiple ranking functions, such as `RANK()`, `DENSE_RANK()`, and `ROW_NUMBER()`.

#### Example: Ranking Students by GPA
Consider a `student_grades` table with `ID` and `GPA` columns. The following query ranks students based on their GPA:
```sql
SELECT ID, RANK() OVER (ORDER BY GPA DESC) AS s_rank
FROM student_grades;
```
This query ranks students with the highest GPA first (`ORDER BY GPA DESC`). If two students have the same GPA, they will receive the same rank, but the next rank will have a gap.

#### Example: Sorting by Rank
To display the students in order of their rank, add an additional `ORDER BY` clause:
```sql
SELECT ID, RANK() OVER (ORDER BY GPA DESC) AS s_rank
FROM student_grades
ORDER BY s_rank;
```

#### `DENSE_RANK()`
Unlike `RANK()`, the `DENSE_RANK()` function does not leave gaps in ranking. For example, if two students are tied for rank 1, the next rank will be 2, not 3:
```sql
SELECT ID, DENSE_RANK() OVER (ORDER BY GPA DESC) AS s_rank
FROM student_grades;
```

#### Supported Version
These ranking functions are supported starting from **MySQL 8.0.2**. Be sure to check the syntax for your specific database system, as it may vary.

---

### Windowing in SQL

Windowing functions are used to calculate values over a specified "window" of rows related to the current row, typically to perform aggregations like moving averages. This can smooth out random variations in data, such as sales over time.

#### Example: Moving Average
Suppose we have a `sales` table with `date` and `value` columns, and we want to calculate a moving average over a three-day window (including the previous day, the current day, and the next day):
```sql
SELECT date, SUM(value) OVER (ORDER BY date ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS moving_avg
FROM sales;
```
This query calculates the sum of the sales value for each date, the day before, and the day after, and returns it as a moving average.

#### Other Window Specifications
1. **Between Unbounded Preceding and Current Row**: This includes all rows from the start of the partition up to the current row:
   ```sql
   SELECT date, SUM(value) OVER (ORDER BY date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
   FROM sales;
   ```

2. **Range Specification**: Instead of counting rows, you can specify a range of values. For example, the following query calculates the sum of all rows with a value between the current row's value minus 10 and the current value:
   ```sql
   SELECT date, SUM(value) OVER (ORDER BY value RANGE BETWEEN 10 PRECEDING AND CURRENT ROW) AS range_sum
   FROM sales;
   ```

3. **Date Interval Specification**: You can use time intervals in your window specification. For example, the following query calculates the sum of sales values over the 10 days preceding the current row:
   ```sql
   SELECT date, SUM(value) OVER (ORDER BY date RANGE INTERVAL 10 DAY PRECEDING) AS ten_day_sum
   FROM sales;
   ```

#### Supported Version
These windowing functions are also supported from **MySQL 8.0.2** onwards. Different databases may have their own syntax or additional options, so it's always good to consult the documentation for the specific system in use.


---

### Authorization in SQL

SQL provides various forms of authorization that control how users can interact with data or modify the database schema. These authorizations can be granted or revoked by users with the appropriate privileges.

#### Data Authorization Types:
1. **Read**: Allows reading data, but no modifications.
2. **Insert**: Allows adding new rows, but no changes to existing data.
3. **Update**: Allows modifying existing data but not deleting rows.
4. **Delete**: Allows removing rows from a table.

#### Schema Modification Authorizations:
1. **Index**: Allows creation and deletion of indexes.
2. **Resources**: Permits creating new tables and relations.
3. **Alteration**: Allows adding or removing attributes (columns) in a table.
4. **Drop**: Allows deletion of tables.

---

### Authorization Specification in SQL

SQL uses the `GRANT` statement to give specific privileges to users or roles.

#### Syntax for Granting Privileges:
```sql
GRANT <privilege_list>
ON <relation_name or view_name>
TO <user_list>;
```
- `<user_list>` can be a specific user (`user-id`), a role, or `public` (to grant privileges to all users).
- Granting a privilege on a view does **not** grant privileges on the underlying relations.

Example: Granting `SELECT` permission to users U1, U2, and U3 on the `instructor` table:
```sql
GRANT SELECT 
ON instructor 
TO U1, U2, U3;
```

---

### Revoking Authorization

SQL allows revoking privileges using the `REVOKE` statement.

#### Syntax for Revoking Privileges:
```sql
REVOKE <privilege_list>
ON <relation_name or view_name>
FROM <user_list>;
```

Example: Revoking `SELECT` permission from users U1, U2, and U3 on the `branch` table:
```sql
REVOKE SELECT 
ON branch 
FROM U1, U2, U3;
```

- If the privilege was granted to `public`, revoking it will remove the privilege from all users except those who were granted it explicitly.
- Privileges granted by multiple users may persist if only one of the grants is revoked.
- When a privilege is revoked, all dependent privileges are also revoked.

---

### Roles in SQL

Roles allow grouping of privileges, which can simplify privilege management by granting and revoking roles rather than individual privileges.

#### Creating and Using Roles:
1. **Creating a Role**:
```sql
CREATE ROLE instructor;
```

2. **Granting Privileges to a Role**:
```sql
GRANT SELECT ON takes TO instructor;
```

3. **Assigning Roles to Users**:
```sql
GRANT instructor TO Amit;
```

4. **Inheriting Roles**:
   Roles can inherit privileges from other roles. For example, assigning the `teaching_assistant` role to `instructor`:
```sql
CREATE ROLE teaching_assistant;
GRANT teaching_assistant TO instructor;
```

   In this case, `instructor` will inherit all the privileges of `teaching_assistant`.

---

### Other Authorization Features

1. **References Privilege**:
   The `REFERENCES` privilege is used to create foreign keys:
```sql
GRANT REFERENCES (dept_name) ON department TO Mariano;
```

2. **Transferring Privileges**:
   Privileges can be granted with the option to further transfer those privileges to others:
```sql
GRANT SELECT ON department TO Amit WITH GRANT OPTION;
```

3. **Revoking Privileges with Cascade and Restrict**:
   - **Cascade**: When revoking a privilege, it cascades and revokes from all dependent users:
```sql
REVOKE SELECT ON department FROM Amit, Satoshi CASCADE;
```
   - **Restrict**: Prevents revoking a privilege if other dependent privileges exist:
```sql
REVOKE SELECT ON department FROM Amit, Satoshi RESTRICT;
```

### Conclusion

SQL's authorization system offers fine-grained control over data access and schema modifications. By using the `GRANT` and `REVOKE` statements along with roles, database administrators can efficiently manage user privileges while maintaining data security and integrity.