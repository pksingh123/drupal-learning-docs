# MySQL Query List

# Select query

SELECT * FROM tableName;

# Select datafrom databse based on specific condition 

SELECT column1, column2 FROM tableName;

# Select datafrom databse based on specific condition

SELECT * FROM tableName WHERE id = 1;

# Insert – Add Records

INSERT INTO tableName (column1, column2, column3)
VALUES ('value1', 'value2', 'value3');

# Update – Modify Records

UPDATE tableName
SET column1 = 'newValue', column2 = 'newValue2'
WHERE id = 1;

# Delete – Remove Records

DELETE FROM tableName WHERE id = 1;

# Merge / Upsert – Insert or Update

INSERT INTO tableName (id, name, age)
VALUES (1, 'John', 30)
ON DUPLICATE KEY UPDATE name = 'John', age = 30;

Truncate – Remove All Rows
TRUNCATE TABLE tableName;

# Select with Conditions
SELECT * FROM tableName WHERE column = 'value';
SELECT * FROM tableName WHERE column != 'value';
SELECT * FROM tableName WHERE column > 10;
SELECT * FROM tableName WHERE column BETWEEN 10 AND 20;
SELECT * FROM tableName WHERE column IN ('A', 'B', 'C');
SELECT * FROM tableName WHERE column NOT IN ('A', 'B', 'C');
SELECT * FROM tableName WHERE column LIKE '%text%';

# Ordering and Limiting
SELECT * FROM tableName ORDER BY column ASC;
SELECT * FROM tableName ORDER BY column DESC;
SELECT * FROM tableName LIMIT 10;
SELECT * FROM tableName ORDER BY column DESC LIMIT 5;

# Aggregate Functions
SELECT COUNT(*) FROM tableName;
SELECT SUM(price) FROM orders;
SELECT AVG(price) FROM products;
SELECT MIN(age), MAX(age) FROM users;

# Group By and Having
SELECT department, COUNT(*) AS total
FROM employees
GROUP BY department
HAVING total > 5;

# Distinct – Unique Values
SELECT DISTINCT columnName FROM tableName;

# Aliases
SELECT name AS employee_name FROM employees;

# Joins
Inner Join
SELECT employees.name, departments.department_name
FROM employees
INNER JOIN departments ON employees.dept_id = departments.id;

# Left Join
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;

# Right Join
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.id;

# Full Join (MySQL workaround using UNION)
SELECT * FROM table1
LEFT JOIN table2 ON table1.id = table2.id
UNION
SELECT * FROM table1
RIGHT JOIN table2 ON table1.id = table2.id;

# Subquery
SELECT * FROM employees
WHERE dept_id IN (SELECT id FROM departments WHERE location = 'London');

# Exists
SELECT name
FROM customers c
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.id);

# Case Statement
SELECT name,
  CASE
    WHEN salary > 50000 THEN 'High'
    WHEN salary BETWEEN 30000 AND 50000 THEN 'Medium'
    ELSE 'Low'
  END AS salary_level
FROM employees;

# Limit with Offset (Pagination)
SELECT * FROM products LIMIT 10 OFFSET 20;

# Union and Union All
SELECT name FROM employees
UNION
SELECT name FROM customers;

SELECT name FROM employees
UNION ALL
SELECT name FROM customers;

# Create Table
CREATE TABLE employees (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  age INT,
  salary DECIMAL(10,2)
);

# Alter Table
ALTER TABLE employees ADD COLUMN department VARCHAR(100);
ALTER TABLE employees DROP COLUMN department;
ALTER TABLE employees MODIFY COLUMN name VARCHAR(255);

# Drop Table
DROP TABLE employees;

# Rename Table
RENAME TABLE old_table TO new_table;

# Transaction
START TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;
-- or ROLLBACK;

# Index
CREATE INDEX idx_name ON employees (name);
DROP INDEX idx_name ON employees;

# View
CREATE VIEW employee_summary AS
SELECT name, salary FROM employees WHERE salary > 50000;

DROP VIEW employee_summary;

# Stored Procedure Syntax
# A Stored Procedure is a precompiled SQL code block stored in the database that you can call anytime.
# It helps to:

# Reuse complex SQL logic
# Improve performance
# Maintain cleaner application code
# Enforce business rules at the database level

DELIMITER //

CREATE PROCEDURE procedure_name (parameters)
BEGIN
  -- SQL statements go here
END //

DELIMITER ;

# Examples

# Simple Stored Procedure
DELIMITER //

CREATE PROCEDURE GetAllUsers()
BEGIN
  SELECT * FROM users;
END //

DELIMITER ;


Call it:

CALL GetAllUsers();

# Stored Procedure with Parameters
DELIMITER //

CREATE PROCEDURE GetUserById(IN userId INT)
BEGIN
  SELECT * FROM users WHERE id = userId;
END //

DELIMITER ;


Call it:

CALL GetUserById(5);

# Stored Procedure with OUT Parameter
DELIMITER //

CREATE PROCEDURE GetUserCount(OUT total INT)
BEGIN
  SELECT COUNT(*) INTO total FROM users;
END //

DELIMITER ;


Call it and get output:

CALL GetUserCount(@count);
SELECT @count;

# Stored Procedure with IN and OUT Parameters
DELIMITER //

CREATE PROCEDURE GetTotalOrders(IN customerId INT, OUT totalOrders INT)
BEGIN
  SELECT COUNT(*) INTO totalOrders
  FROM orders
  WHERE customer_id = customerId;
END //

DELIMITER ;


Call it:

CALL GetTotalOrders(10, @total);
SELECT @total;

# Insert Record Using Stored Procedure
DELIMITER //

CREATE PROCEDURE AddUser(
  IN p_name VARCHAR(100),
  IN p_email VARCHAR(100)
)
BEGIN
  INSERT INTO users (name, email)
  VALUES (p_name, p_email);
END //

DELIMITER ;


Call it:

CALL AddUser('John Doe', 'john@example.com');

# Update Record Using Stored Procedure
DELIMITER //

CREATE PROCEDURE UpdateUserEmail(
  IN p_userId INT,
  IN p_newEmail VARCHAR(100)
)
BEGIN
  UPDATE users
  SET email = p_newEmail
  WHERE id = p_userId;
END //

DELIMITER ;


Call it:

CALL UpdateUserEmail(5, 'newmail@example.com');

# Stored Procedure with Conditional Logic
DELIMITER //

CREATE PROCEDURE CheckUserStatus(IN p_userId INT)
BEGIN
  DECLARE userStatus VARCHAR(20);
  SELECT status INTO userStatus FROM users WHERE id = p_userId;

  IF userStatus = 'active' THEN
    SELECT 'User is active' AS message;
  ELSE
    SELECT 'User is inactive' AS message;
  END IF;
END //

DELIMITER ;


Call it:

CALL CheckUserStatus(1);

# Stored Procedure with Loop
DELIMITER //

CREATE PROCEDURE PrintNumbers()
BEGIN
  DECLARE x INT DEFAULT 1;
  WHILE x <= 5 DO
    SELECT x;
    SET x = x + 1;
  END WHILE;
END //

DELIMITER ;


Call it:

CALL PrintNumbers();

# Delete Record Using Stored Procedure
DELIMITER //

CREATE PROCEDURE DeleteUser(IN p_userId INT)
BEGIN
  DELETE FROM users WHERE id = p_userId;
END //

DELIMITER ;

Call it:

CALL DeleteUser(3);

# How to managed store Procedure
# Show all stored procedures
SHOW PROCEDURE STATUS;

# What is a Trigger?
#A Trigger in MySQL is a special stored program that automatically executes (fires) when a specific event occurs on a table - such as an INSERT, UPDATE, or DELETE

# Syntax fo creating Triger

CREATE TRIGGER trigger_name
trigger_time trigger_event
ON table_name
FOR EACH ROW
BEGIN
   -- SQL statements
END;
# Log for emplyee
CREATE TRIGGER after_employee_insert
AFTER INSERT
ON employees
FOR EACH ROW
BEGIN
  INSERT INTO employee_log (employee_id, action, created_at)
  VALUES (NEW.id, 'Inserted', NOW());
END;

