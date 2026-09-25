# database_college_code
this is the code of the database subject of college


this includes the code of assignment 1 and 2 

-- =====================================================
-- ASSIGNMENT 1 + ASSIGNMENT 2
-- SQL SERVER VERSION
-- =====================================================


-- =====================================================
-- DEPARTMENT TABLE
-- =====================================================

CREATE TABLE department (
    dept_id INT NOT NULL PRIMARY KEY,
    dept_name VARCHAR(50) NOT NULL UNIQUE,
    location VARCHAR(50)
);

INSERT INTO department VALUES
(1, 'CSE', 'Delhi'),
(2, 'ECE', 'Noida'),
(3, 'Mechanical', 'Gurgaon');


-- =====================================================
-- STUDENT TABLE
-- =====================================================

CREATE TABLE student (
    roll_no INT NOT NULL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT NOT NULL,
    dept VARCHAR(10) NOT NULL,
    marks DECIMAL(5,2) NOT NULL
);

INSERT INTO student VALUES
(1, 'Aman', 19, 'CSE', 88.50),
(2, 'Priya', 21, 'CSE', 92.00),
(3, 'Rahul', 22, 'ECE', 76.50),
(4, 'Sneha', 20, 'CSE', 95.00),
(5, 'Arjun', 23, 'Mechanical', 81.00),
(6, 'Neha', 21, 'CSE', 89.50),
(7, 'Rohan', 19, 'ECE', 72.00),
(8, 'Kavya', 22, 'CSE', 97.00),
(9, 'Mohit', 24, 'Mechanical', 68.50),
(10, 'Anjali', 21, 'ECE', 85.00);


-- =====================================================
-- EMPLOYEE TABLE
-- =====================================================

CREATE TABLE employee (
    emp_id INT NOT NULL PRIMARY KEY,
    emp_name VARCHAR(50) NOT NULL,
    salary DECIMAL(10,2) NOT NULL,
    dept_id INT,
    emp_type VARCHAR(20),

    CONSTRAINT FK_Employee_Department
        FOREIGN KEY (dept_id)
        REFERENCES department(dept_id)
);

INSERT INTO employee VALUES
(101, 'Raj', 60000, 1, 'Academic'),
(102, 'Priya', 55000, 2, 'Academic'),
(103, 'Amit', 50000, 3, 'Academic'),
(104, 'Neha', 80000, 1, 'Non-Academic'),
(105, 'Rohit', 70000, 2, 'Non-Academic'),
(106, 'Sneha', 45000, 3, 'Non-Academic');


-- =====================================================
-- ASSIGNMENT 1
-- QUESTION A
-- =====================================================

SELECT *
FROM student
WHERE dept = 'CSE';

SELECT name
FROM student
WHERE age > 20;


-- =====================================================
-- QUESTION B
-- =====================================================

SELECT COUNT(*) AS total_students
FROM student;

ALTER TABLE student
ADD email VARCHAR(100);


-- =====================================================
-- QUESTION C
-- =====================================================

SELECT *
FROM student
ORDER BY marks DESC;

SELECT TOP 5 *
FROM student
ORDER BY marks DESC;


-- =====================================================
-- QUESTION D
-- =====================================================

SELECT
    AVG(marks) AS average_marks,
    MAX(marks) AS maximum_marks,
    MIN(marks) AS minimum_marks
FROM student;

SELECT
    dept,
    COUNT(*) AS total_students
FROM student
GROUP BY dept;


-- =====================================================
-- QUESTION E
-- =====================================================

SELECT
    MAX(salary) AS highest_salary
FROM employee;

SELECT
    d.dept_name,
    AVG(e.salary) AS average_salary
FROM employee e
JOIN department d
    ON e.dept_id = d.dept_id
GROUP BY
    d.dept_id,
    d.dept_name;


-- =====================================================
-- QUESTION F
-- =====================================================

SELECT *
FROM employee
WHERE emp_type = 'Non-Academic'
AND salary > (
    SELECT AVG(salary)
    FROM employee
    WHERE emp_type = 'Academic'
);


-- =====================================================
-- ASSIGNMENT 2
-- QUESTION 1(a)
-- =====================================================

ALTER TABLE employee
ADD CONSTRAINT CK_Employee_Salary
CHECK (salary > 20000);


-- =====================================================
-- QUESTION 1(b)
-- RIGHT JOIN
-- =====================================================

SELECT
    d.dept_id,
    d.dept_name,
    COUNT(e.emp_id) AS employee_count
FROM employee e
RIGHT JOIN department d
    ON e.dept_id = d.dept_id
GROUP BY
    d.dept_id,
    d.dept_name
ORDER BY
    d.dept_id;


-- =====================================================
-- QUESTION 2
-- CROSS JOIN
-- =====================================================

SELECT
    e.emp_id,
    e.emp_name,
    e.salary AS employee_salary,
    d.dept_id AS target_dept_id,
    d.dept_name AS target_dept_name,
    AVG(e2.salary) AS target_dept_average_salary
FROM employee e
CROSS JOIN department d
JOIN employee e2
    ON e2.dept_id = d.dept_id
WHERE e.dept_id <> d.dept_id
GROUP BY
    e.emp_id,
    e.emp_name,
    e.salary,
    e.dept_id,
    d.dept_id,
    d.dept_name
HAVING
    e.salary > AVG(e2.salary)
ORDER BY
    e.emp_id,
    d.dept_id;
