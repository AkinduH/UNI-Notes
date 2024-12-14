
```sql
--SETUP AND ENTER
mysql -u root -p
show databases;
use university;
```


```sql
--TABLES
CREATE TABLE employees (
	employee_id INT PRIMARY KEY,
	first_name VARCHAR(50),
	last_name VARCHAR(50),
	hourly_pay DECIMAL(5, 2),
	hire_date DATE
);

RENAME TABLE employees TO workers;

DROP TABLE employees;

ALTER TABLE employees
ADD phone_number VARCHAR(10);

ALTER TABLE employees
RENAME phone_number TO email;

ALTER TABLE employees
MODIFY COLUMN phone_number VARCHAR(12);
AFTER last_name;
```


```sql
--INSERT ROWS
INSERT INTO employees
VALUES  (2, "Squidward", "Tentacles", 15.00, "2023-01-03"),
		(3, "Spongebob", "Squarepants", 12.50, "2023-01-04"),
		(4, "Patrick", "Star", 12.50, "2023-01-05"),
		(5, "Sandy", "Cheeks", 17.25, "2023-01-06");

INSERT INTO employees (employee_id, first_name, last_name)
VALUES  (6, "Sheldon", "Plankton");
```


```sql
--SELECT

SELECT *
FROM employees 
WHERE first_name = "Spongebob";

SELECT *
FROM employees
WHERE hourly_pay >= 15;
```


```sql
--UPDATE

UPDATE employees
SET hourly_pay = 10.50,
	hire_date = "2023-01-07"
WHERE employee_id = 6;
```


```sql
--CHECK

CREATE TABLE employees (
employee_id INT PRIMARY KEY,
first_name VARCHAR(50),
last_name VARCHAR(50),
hourly_pay DECIMAL(5,2),
hire_date DATE,
CONSTRAINT chk_hourly_pay CHECK (hourly_pay >= 12.00)
);

ALTER TABLE employees
ADD CONSTRAINT chk_hourly_pay CHECK(hourly_pay >= 12.00);
```


```sql
--DEFAULT

CREATE TABLE products(
product_id INT,
product_name VARCHAR(25),
price DECIMAL(4, 2) DEFAULT 0
);

ALTER TABLE products
ALTER price SET DEFAULT 0;

CREATE TABLE transactions(
transaction_id INT,
amount DECIMAL(5, 2),
transaction_date DATETIME DEFAULT NOW()
);
```


```sql
--PRIMARY KEY

CREATE TABLE transactions(
transaction_id INT PRIMARY KEY,
amount DECIMAL(5, 2)
);

CREATE TABLE transactions(
transaction_id INT PRIMARY KEY AUTO_INCREMENT,
amount DECIMAL(5, 2)
);
```


```sql
--FOREIGN KEY

CREATE TABLE transactions (
transaction_id INT PRIMARY KEY AUTO_INCREMENT,
amount DECIMAL(5, 2),
customer_id INT,
FOREIGN KEY(customer_id) REFERENCES customers(customer_id)
);
```



#### **JOINS**

```sql
--Inner Join: Returns only the rows that have matching values in both tables.

SELECT *
FROM course INNER JOIN prereq
ON course.course_id = prereq.course_id;

--Left Outer Join (LEFT JOIN): Returns all rows from the left table, and the matched rows from the right table. If no match is found, the result is NULL for the right table.

SELECT * 
FROM course LEFT JOIN prereq 
ON course.course_id = prereq.course_id;

--Right Outer Join (RIGHT JOIN): Returns all rows from the right table, and the matched rows from the left table. If no match is found, the result is NULL for the left table.

SELECT *
FROM course RIGHT JOIN prereq
ON course.course_id = prereq.course_id;

-- points
SELECT name
FROM instructor LEFT JOIN teaches
ON instructor.ID = teaches.ID  -- (USING(ID))
WHERE course_id IS NULL;
```


[[Intermediate SQL]]




```sql
--Extra

SELECT DISTINCT Project
FROM Project
WHERE End_date BETWEEN '2021-02-01' AND '2021-02-07';


SELECT DISTINCT e.Name
FROM Employee e
LEFT JOIN Project p ON e.ID = p.ID
WHERE p.Project = 'eNIC'
AND e.ID NOT IN (
    SELECT p2.ID
    FROM Project p2
    WHERE p2.Project = 'eVisa'
);


SELECT p.Project, p.Project_phase, COUNT(e.ID) AS Number_of_Employees
FROM Project p
LEFT JOIN Employee e ON p.ID = e.ID
GROUP BY p.Project, p.Project_phase
ORDER BY p.Project, p.Project_phase;
```



