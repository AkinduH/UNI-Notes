
1.
```sql
CREATE TABLE grade_point (
    Grade VARCHAR(2) PRIMARY KEY,
    Point DECIMAL(2,1) CHECK (Point >= 0 AND Point <= 4.2)
);
```

2.
```sql
INSERT INTO grade_point (Grade, Point) VALUES 
('A+', 4.2),
('A', 4.0),
('A-', 3.7),
('B+', 3.5),
('B', 3.0),
('B-', 2.7),
('C+', 2.3),
('C', 2.0),
('C-', 1.5),
('D', 1.0);
```

3.
```sql
SELECT 
    s.name AS student_name,
    CASE 
        WHEN COUNT(gp.Point) = 0 THEN NULL
        ELSE SUM(gp.Point * c.credits)
    END AS total_gp
FROM 
    student s
LEFT JOIN 
    takes t ON s.ID = t.ID
LEFT JOIN 
    grade_point gp ON t.grade = gp.Grade
LEFT JOIN 
    course c ON t.course_id = c.course_id
GROUP BY 
    s.name
HAVING 
    SUM(c.credits) > 0 
ORDER BY 
    total_gp DESC;
```

4.
```sql
DELIMITER //

CREATE FUNCTION count_students_by_course(course_id_input VARCHAR(8))
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE student_count INT;
    
    SELECT COUNT(DISTINCT ID)
    INTO student_count
    FROM takes
    WHERE course_id = course_id_input;
    
    RETURN student_count;
END //

DELIMITER ;
```

5.
```sql
SELECT course_id
FROM course
WHERE count_students_by_course(course_id) > 5
ORDER BY course_id ASC;
```

6.
```sql
DELIMITER //

CREATE TRIGGER check_grade_before_insert
BEFORE INSERT ON takes
FOR EACH ROW
BEGIN
    IF NEW.grade NOT IN ('A+', 'A', 'A-', 'B+', 'B', 'B-', 'C+', 'C', 'C-', 'D') THEN
        SET NEW.grade = NULL;
    END IF;
END //

DELIMITER ;
```

7.
```sql
CREATE VIEW faculty AS
SELECT ID, name, dept_name
FROM instructor;
```

8.
```sql
CREATE USER 'uomcse'@'localhost' IDENTIFIED BY 'uomcse123';
```

9.
```sql
GRANT SELECT ON university.faculty TO 'uomcse'@'localhost';
FLUSH PRIVILEGES;
```

10.
```sql
GRANT ALL PRIVILEGES ON university.takes TO 'uomcse'@'localhost';
FLUSH PRIVILEGES;
```








