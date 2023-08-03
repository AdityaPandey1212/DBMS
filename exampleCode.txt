-- SQLPLUS

CREATE TABLE Employee(
First_Name VARCHAR2(15),
Mid_Name VARCHAR2(2),
Last_Name VARCHAR2(15),
SSN_Number VARCHAR2(9),
Birthday DATE,
Address VARCHAR2(50),
Sex CHAR(1),
Salary NUMBER(7),
Supervisor_SSN VARCHAR2(9),
Department_Number NUMBER(5)
);

CREATE TABLE Department(
Department_Name VARCHAR2(15),
Department_Number NUMBER(5),
Manager_SSN VARCHAR2(9),
ManagerStartDate DATE
);

CREATE TABLE Project(
Project_Name VARCHAR2(15),
Project_Number NUMBER(5),
Project_Location VARCHAR2(15),
Department_Number NUMBER(5)
);

CREATE TABLE location(
Department_Number NUMBER(5),
Department_location VARCHAR2(15)
);

CREATE TABLE WorksOn(
SSN_Number VARCHAR2(9),
Project_Number NUMBER(5),
Hours NUMBER(3,1)
);

CREATE TABLE Dependent(
SSN_Number VARCHAR2(9),
Dependent_Name VARCHAR2(15),
Sex CHAR(1),
Birthday DATE,
Relationship VARCHAR2(8)
);

-- Insert data
INSERT INTO Employee VALUES ('Doug', 'E', 'Gilbert', '123', TO_DATE('1968-06-09', 'YYYY-MM-DD'), 'Chennai', 'M', 80000, NULL, 1);
INSERT INTO Employee VALUES ('Joyce', NULL, 'PAN', '124', TO_DATE('1973-02-07', 'YYYY-MM-DD'), 'Vellore', 'F', 70000, NULL, 1);
INSERT INTO Employee VALUES ('Franklin', 'T', 'Wong', '125', TO_DATE('1972-12-08', 'YYYY-MM-DD'), 'Delhi', 'M', 40000, '123', 2);
INSERT INTO Employee VALUES ('Jennifer', 'S', 'Wallace', '564', TO_DATE('1983-06-20', 'YYYY-MM-DD'), 'Chennai', 'F', 43000, '123', 2);
INSERT INTO Employee VALUES ('John', 'B', 'Smith', '678', TO_DATE('1987-01-09', 'YYYY-MM-DD'), 'Madurai', 'M', 30000, '124', 1);
INSERT INTO Employee VALUES ('Ramesh', 'K', 'Narayan', '234', TO_DATE('1985-09-15', 'YYYY-MM-DD'), 'Bangalore', 'M', 38000, '124', 3);

INSERT INTO Department VALUES ('Administration', 2, '564', TO_DATE('2012-01-03', 'YYYY-MM-DD'));
INSERT INTO Department VALUES ('Headquarter', 1, '678', TO_DATE('2014-12-16', 'YYYY-MM-DD'));
INSERT INTO Department VALUES ('Finance', 3, '234', TO_DATE('2013-05-18', 'YYYY-MM-DD'));
INSERT INTO Department VALUES ('IT', 4, '123', TO_DATE('2015-06-12', 'YYYY-MM-DD'));

INSERT INTO Project VALUES ('ProjectA', 3388, 'Delhi', 1);
INSERT INTO Project VALUES ('ProjectB', 1945, 'Hyderabad', 1);
INSERT INTO Project VALUES ('ProjectC', 6688, 'Chennai', 2);
INSERT INTO Project VALUES ('ProjectD', 2423, 'Chennai', 2);
INSERT INTO Project VALUES ('ProjectE', 7745, 'Bangalore', 3);

INSERT INTO location VALUES (1, 'Houston');
INSERT INTO location VALUES (1, 'Chicago');
INSERT INTO location VALUES (2, 'New York');
INSERT INTO location VALUES (2, 'San Francisco');
INSERT INTO location VALUES (3, 'Salt Lake City');
INSERT INTO location VALUES (4, 'Stafford');
INSERT INTO location VALUES (4, 'Bellaire');
INSERT INTO location VALUES (5, 'Sugarland');
INSERT INTO location VALUES (5, 'Houston');

INSERT INTO WorksOn VALUES ('123', 3388, 32.5);
INSERT INTO WorksOn VALUES ('124', 1945, 7.5);
INSERT INTO WorksOn VALUES ('125', 3388, 40.0);
INSERT INTO WorksOn VALUES ('564', 2423, 20.0);
INSERT INTO WorksOn VALUES ('678', 7745, 10.0);
INSERT INTO WorksOn VALUES ('234', 6688, 10.0);

INSERT INTO Dependent VALUES ('123', 'Alice', 'F', TO_DATE('1976-04-05', 'YYYY-MM-DD'), 'Daughter');
INSERT INTO Dependent VALUES ('124', 'Theodore', 'M', TO_DATE('1973-10-25', 'YYYY-MM-DD'), 'Son');
INSERT INTO Dependent VALUES ('125', 'Joy', 'F', TO_DATE('1948-05-03', 'YYYY-MM-DD'), 'Spouse');
INSERT INTO Dependent VALUES ('564', 'Abner', 'M', TO_DATE('1932-02-29', 'YYYY-MM-DD'), 'Spouse');
INSERT INTO Dependent VALUES ('678', 'Alice', 'F', TO_DATE('1978-12-31', 'YYYY-MM-DD'), 'Daughter');
INSERT INTO Dependent VALUES ('234', 'Elizabeth', 'F', TO_DATE('1957-05-05', 'YYYY-MM-DD'), 'Spouse');


ALTER TABLE Employee MODIFY First_Name VARCHAR2(15) NOT NULL;
ALTER TABLE Employee MODIFY Last_Name VARCHAR2(15) NOT NULL;
ALTER TABLE Employee ADD CONSTRAINT "Employee pk" PRIMARY KEY (SSN_Number);
ALTER TABLE Employee ADD CONSTRAINT "check" CHECK (Sex IN ('M', 'F', 'm', 'f'));
ALTER TABLE Employee MODIFY Salary DEFAULT 800;
ALTER TABLE Employee ADD CONSTRAINT "Employee fk1" FOREIGN KEY (Supervisor_SSN) REFERENCES Employee (SSN_Number) ON DELETE SET NULL;
ALTER TABLE Department MODIFY Department_Name VARCHAR2(15) NOT NULL;
ALTER TABLE Department ADD CONSTRAINT "dept pk" PRIMARY KEY (Department_Number);
ALTER TABLE Department ADD CONSTRAINT "dept fk 1" FOREIGN KEY (Manager_SSN) REFERENCES Employee (SSN_Number) ON DELETE SET NULL;
ALTER TABLE Project MODIFY Project_Name VARCHAR2(15) NOT NULL;
ALTER TABLE Project ADD CONSTRAINT "project pk" PRIMARY KEY (Project_Number);
ALTER TABLE Project ADD CONSTRAINT "project fk1" FOREIGN KEY (Department_Number) REFERENCES Department (Department_Number) ON DELETE SET NULL;
ALTER TABLE Employee ADD CONSTRAINT "employee fk2" FOREIGN KEY (Department_Number) REFERENCES Department (Department_Number) ON DELETE CASCADE;
ALTER TABLE location ADD CONSTRAINT "loc_cons" FOREIGN KEY (Department_Number) REFERENCES Department (Department_Number) ON DELETE CASCADE;
ALTER TABLE WorksOn MODIFY Hours NUMBER(3,1) NOT NULL;
ALTER TABLE WorksOn ADD CONSTRAINT "fk_wo" FOREIGN KEY (Project_Number) REFERENCES Project (Project_Number) ON DELETE CASCADE;
ALTER TABLE WorksOn ADD CONSTRAINT "fk_WOemp" FOREIGN KEY (SSN_Number) REFERENCES Employee (SSN_Number) ON DELETE CASCADE;
ALTER TABLE Dependent ADD CONSTRAINT "fk_dep" FOREIGN KEY (SSN_Number) REFERENCES Employee (SSN_Number) ON DELETE CASCADE;
ALTER TABLE Dependent ADD CONSTRAINT "checkSD" CHECK (Sex IN ('M', 'F', 'm', 'f'));


alter table Project add constraint "Project uq1" UNIQUE ("Project Name")
alter table Employee modify Sex not null
alter table Employee modify Salary NUMBER

EX3
-- Query 1
SELECT First_Name, Mid_Name, Last_Name, Salary
FROM Employee
WHERE Salary > 25000;

-- Query 2
SELECT First_Name, Mid_Name, Last_Name
FROM Employee
WHERE Salary > 30000 AND Salary < 70000;

-- Query 3
SELECT First_Name, Mid_Name, Last_Name
FROM Employee
WHERE Supervisor_SSN IS NULL;

-- Query 4
SELECT TO_CHAR(Birthday, 'DDth Month YYYY') AS Birthday
FROM Employee;

-- Query 5
SELECT First_Name, Mid_Name, Last_Name
FROM Employee
WHERE Birthday <= DATE '1978-12-31';

-- Query 6
SELECT First_Name, Mid_Name, Last_Name
FROM Employee
WHERE Address LIKE '%Chennai%';

-- Query 7
SELECT Department_Name
FROM Department
WHERE Department_Name LIKE 'A%';

-- Query 8
SELECT Department_Name
FROM Department
WHERE Department_Name LIKE '%r';

-- Query 9
SELECT First_Name, Mid_Name, Last_Name
FROM Employee
WHERE SSN_Number = 123 OR SSN_Number = 124;

-- Query 10
SELECT UPPER(Department_Name) FROM Department;

-- Query 11
SELECT LOWER(Department_Name) FROM Department;

-- Query 12
SELECT SUBSTR(Department_Name, 1, 4) AS "First four", SUBSTR(Department_Name, -4) AS "Last four" FROM DEPT;

-- Query 13
SELECT SUBSTR(Address, 5, 7) AS address FROM Employee;

-- Query 14
SELECT ManagerStartDate + INTERVAL '3' MONTH AS MgrStartdate FROM DEPARTMENT;

-- Query 15
SELECT ROUND((MONTHS_BETWEEN(SYSDATE, Birthday) / 12), 2) AS age FROM Employee;

-- Query 16
SELECT LAST_DAY(ManagerStartDate) AS NEXT_DAY, ManagerStartDate + INTERVAL '1' DAY AS LAST_DAY FROM DEPARTMENT;

-- Query 17
SELECT SUBSTR('Harini', 2, 4) AS substring FROM DUAL;

-- Query 18
SELECT REPLACE('Harini', 'ni', 'sh') FROM DUAL;

-- Query 19
SELECT LENGTH(Department_Name) FROM Department;

-- Query 20
SELECT ADD_MONTHS(SYSDATE, 10) FROM DUAL;

-- Query 21
SELECT NEXT_DAY(SYSDATE + INTERVAL '6' DAY, 'SUNDAY') FROM DUAL;

-- Query 22
SELECT NEXT_DAY(SYSDATE + INTERVAL '6' DAY, 'SUNDAY')
FROM DUAL
WHERE NEXT_DAY(SYSDATE + INTERVAL '6' DAY, 'SUNDAY') <= DATE '2021-08-29';

-- Query 23
SELECT LPAD(Project_Location, LENGTH(Project_Location) + 4, '****') FROM Project;


-- 1. How many different departments are there in the 'employee' table?
SELECT COUNT(DISTINCT Department_Number) AS DepartmentCount
FROM Employee;

-- 2. For each department, display the minimum and maximum employee salaries.
SELECT Department_Number, MIN(Salary) AS MinimumSalary, MAX(Salary) AS MaximumSalary
FROM Employee
GROUP BY Department_Number;

-- 3. Print the average annual salary.
SELECT AVG(Salary*12) AS AverageSalary
FROM Employee;

-- 4. Count the number of employees over 30 years of age.
SELECT COUNT(*) AS EmployeeCount
FROM Employee
WHERE (SYSDATE - Birthday) > 30 * 365.25;

-- 5. Print the Department name and average salary of each department.
SELECT D.Department_Name, AVG(E.Salary) AS AverageSalary
FROM Employee E, Department D
WHERE E.Department_Number = D.Department_Number
GROUP BY D.Department_Name;
{
    SELECT DEPARTMENT_NAME, AVG(SALARY) 
    FROM EMPLOYEE E JOIN DEPARTMENT D ON E.DEPARTMENT_NUMBER = D.DEPARTMENT_NUMBER
    GROUP BY D.DEPARTMENT_NUMBER; 
    -- WE CANT DO THIS BECAUSE WE CAN ONLY HAVE ATTRIBUTE IN GROUP BY OR AGGRATE FUNCTION
}
-- 6. Display the department name which contains more than 30 employees.
SELECT D.Department_Name
FROM Department D, Employee E
WHERE E.Department_Number = D.Department_Number
GROUP BY D.Department_Name
HAVING COUNT(*) > 30;

-- 7. Calculate the average salary of employees by department and age.
SELECT D.Department_Name, (SYSDATE - E.Birthday) / 365.25 AS Age, AVG(E.Salary) AS AverageSalary
FROM Employee E, Department D
WHERE E.Department_Number = D.Department_Number
GROUP BY D.Department_Name, (SYSDATE - E.Birthday) / 365.25;

-- 8. Count separately the number of employees in the finance and research departments.
SELECT D.Department_Name, COUNT(*) AS EmployeeCount
FROM Employee E, Department D
WHERE E.Department_Number = D.Department_Number
  AND D.Department_Name IN ('Finance', 'Research')
GROUP BY D.Department_Name;

-- 9. List out the employees based on their seniority.
SELECT First_Name, Mid_Name, Last_Name, Birthday
FROM Employee
ORDER BY Birthday;

-- 10. List out the employees who work in the 'manufacture' department, grouped by first name.
SELECT First_Name, COUNT(*) AS EmployeeCount
FROM Employee E, Department D
WHERE E.Department_Number = D.Department_Number
AND D.Department_Name = 'Manufacture'
GROUP BY First_Name;


----------------------------------------------------------------------------
--EX5
-- 1. Find the employee who is getting the highest salary in the department research
SELECT e.*
FROM Employee e
WHERE e.Salary = (SELECT MAX(Salary) FROM Employee WHERE Department_Number = 2);

-- 2. Find the employees who earn the same salary as the minimum salary for each Department
SELECT e.*
FROM Employee e
WHERE e.Salary = (SELECT MIN(Salary) FROM Employee WHERE Department_Number = e.Department_Number);

-- 3. Find the employee whose salary is greater than the average salary of department 2
SELECT e.*
FROM Employee e
WHERE e.Salary > (SELECT AVG(Salary) FROM Employee WHERE Department_Number = 2);

-- 4. List out all the department names with their individual employee strength
SELECT d.Department_Name, COUNT(e.First_Name) AS Employee_Count
FROM Department d
LEFT JOIN Employee e ON d.Department_Number = e.Department_Number
GROUP BY d.Department_Name;

-- 5. Find out the department name having the highest employee strength
 SELECT Department_Name
 FROM (
     SELECT d.Department_Name, COUNT(e.First_Name) AS Employee_Count
     FROM Department d
     LEFT JOIN Employee e ON d.Department_Number = e.Department_Number
     GROUP BY d.Department_Name
     ORDER BY COUNT(e.First_Name) DESC
 ) WHERE ROWNUM = 1;

-- 6. List out all the departments and the average salary drawn by their employees
SELECT d.Department_Name, AVG(e.Salary) AS Average_Salary
FROM Department d
LEFT JOIN Employee e ON d.Department_Number = e.Department_Number
GROUP BY d.Department_Name;

-- 7. Find the maximum average salary for each department
SELECT d.Department_Name, MAX(avg_salaries.Average_Salary) AS Maximum_Average_Salary
FROM Department d
LEFT JOIN (
    SELECT Department_Number, AVG(Salary) AS Average_Salary
    FROM Employee
    GROUP BY Department_Number
) avg_salaries ON d.Department_Number = avg_salaries.Department_Number
GROUP BY d.Department_Name;

-- 8. Create a view to display the employee details who are working in the IT department
CREATE VIEW IT_Employees AS
SELECT e.*
FROM Employee e
WHERE e.Department_Number = (
    SELECT Department_Number
    FROM Department
    WHERE Department_Name = 'IT'
);

-- 9. Create a logical table to store employee details who are getting a salary more than 10000
CREATE TABLE HighSalary_Employees AS
SELECT *
FROM Employee
WHERE Salary > 10000;

-- 10. Create a table to store the employee details based on the department number
CREATE TABLE Department_Employees AS
SELECT e.*
FROM Employee e
JOIN Department d ON e.Department_Number = d.Department_Number;

-- EX 6

-- 1. Retrieve the names of all employees in department 5 who work more than 10 hours per week on ProductX project.
SELECT e.First_Name, e.Last_Name
FROM Employee e
JOIN WorksOn w ON e.SSN_Number = w.SSN_Number
JOIN Project p ON w.Project_Number = p.Project_Number
WHERE e.Department_Number = 5
  AND w.Hours > 10
  AND p.Project_Name = 'ProductX';

-- 2. List the names of all employees who have a dependent with the same first name as themselves.
SELECT e.First_Name, e.Last_Name
FROM Employee e
JOIN Dependent d ON e.SSN_Number = d.SSN_Number
WHERE d.Dependent_Name = e.First_Name;

-- 3. Find the names of all employees who are directly supervised by 'Franklin Wong'.
SELECT e.First_Name, e.Last_Name
FROM Employee e
JOIN Employee s ON e.Supervisor_SSN = s.SSN_Number
WHERE s.First_Name = 'Franklin' AND s.Last_Name = 'Wong';

-- 4. Retrieve the names of all who do not work on any project.
SELECT e.First_Name, e.Last_Name
FROM Employee e
LEFT JOIN WorksOn w ON e.SSN_Number = w.SSN_Number
WHERE w.SSN_Number IS NULL;

-- 5. Find the names and addresses of all employees who work on at least one project located in Houston but whose department has no location in Houston.
SELECT e.First_Name, e.Last_Name, e.Address
FROM Employee e
JOIN WorksOn w ON e.SSN_Number = w.SSN_Number
JOIN Project p ON w.Project_Number = p.Project_Number
LEFT JOIN Location l ON e.Department_Number = l.Department_Number
WHERE p.Project_Location = 'Houston'
  AND l.Department_location IS NULL;

-- 6. List the names of all managers who have no dependents.
SELECT e.First_Name, e.Last_Name
FROM Employee e
WHERE e.SSN_Number IN (
    SELECT Supervisor_SSN
    FROM Employee
    WHERE SSN_Number NOT IN (
        SELECT SSN_Number
        FROM Dependent
    )
);

-- 7. List the employee's names and the department names if they happen to manage a department.
SELECT e.First_Name, e.Last_Name, d.Department_Name
FROM Employee e
LEFT JOIN Department d ON e.SSN_Number = d.Manager_SSN;

-- 8. For each project, retrieve the project number, project name, and the number of employees who work on that project.
SELECT p.Project_Number, p.Project_Name, COUNT(w.SSN_Number) AS Employee_Count
FROM Project p
LEFT JOIN WorksOn w ON p.Project_Number = w.Project_Number
GROUP BY p.Project_Number, p.Project_Name;

-- 9. For each project, list the project name and the total hours per week (by all employees) spent on that project.
SELECT p.Project_Name, SUM(w.Hours) AS Total_Hours
FROM Project p
JOIN WorksOn w ON p.Project_Number = w.Project_Number
GROUP BY p.Project_Name;

-- 10. Retrieve the names of the employees who have 2 or more dependents.
SELECT e.First_Name, e.Last_Name
FROM Employee e
JOIN (
    SELECT SSN_Number
    FROM Dependent
    GROUP BY SSN_Number
    HAVING COUNT(*) >= 2
) d ON e.SSN_Number = d.SSN_Number;
