# employment_management_Data_Analysis

# 📚 SQL Employee Database Queries

This project showcases a collection of practical SQL queries for an employee management database. Each query is written to solve a specific real-world business problem, commonly encountered in data analytics and data engineering roles.

---

## 🧠 Question & Answers

### 1️⃣ Retrieve the first and last names of all employees.
```sql
SELECT FirstName, LastName FROM Employee;

2️⃣ Retrieve the first and last names of employees who work as 'Software Engineer'.

SELECT FirstName, LastName 
FROM Employee E
INNER JOIN JobTitle J ON E.JobTitleID = J.JobTitleID
WHERE JobTitleName = 'Software Engineer';

3️⃣ Retrieve first names and last names of last 7 hires.

SELECT TOP 7 FirstName, LastName, HireDate 
FROM Employee
ORDER BY HireDate DESC;


4️⃣ Get the count of employees in each job title.

SELECT JobTitleName, COUNT(EmployeeID) AS NoOfEmployees 
FROM Employee E
INNER JOIN JobTitle J ON J.JobTitleID = E.JobTitleID
GROUP BY JobTitleName;


5️⃣ Retrieve full name and personal info of employees in 'Engineering' department.

SELECT CONCAT(FirstName, ' ', LastName) AS FullName, DateOfBirth, Gender, PhoneNumber, Email 
FROM Employee E
INNER JOIN Department D ON E.DepartmentID = D.DepartmentID
WHERE DepartmentName = 'Engineering';


6️⃣ List job titles that have more than 3 employees.

SELECT JobTitleName, COUNT(EmployeeID) AS NoOfEmployees 
FROM Employee E
INNER JOIN JobTitle J ON J.JobTitleID = E.JobTitleID
GROUP BY JobTitleName
HAVING COUNT(EmployeeID) > 3;


7️⃣ Retrieve all employee names along with their department names.

SELECT CONCAT(FirstName, ' ', LastName) AS FullName, DepartmentName 
FROM Employee E
INNER JOIN Department D ON D.DepartmentID = E.DepartmentID;



8️⃣ Retrieve first names of employees and the projects they are working on, along with their role.

SELECT FirstName, ProjectName AS Project_Name, JobTitleName AS Project_Role 
FROM Employee E
INNER JOIN ProjectAllocation PA ON E.EmployeeID = PA.EmployeeID
INNER JOIN Project P ON P.ProjectID = PA.ProjectID
INNER JOIN JobTitle J ON J.JobTitleID = E.JobTitleID;


9️⃣ Get the count of employees in each department.

SELECT DepartmentName, COUNT(EmployeeID) AS NoOfEmployees 
FROM Employee E
INNER JOIN Department D ON D.DepartmentID = E.DepartmentID
GROUP BY DepartmentName;


🔟 List all departments with more than 5 employees.

SELECT DepartmentName, COUNT(EmployeeID) AS NoOfEmployees 
FROM Employee E
INNER JOIN Department D ON D.DepartmentID = E.DepartmentID
GROUP BY DepartmentName
HAVING COUNT(EmployeeID) > 5;


1️⃣1️⃣ Retrieve the full names of employees and their managers.

SELECT CONCAT(E.FirstName, ' ', E.LastName) AS EmployeeName,
       CONCAT(M.FirstName, ' ', M.LastName) AS ManagerName
FROM Employee E
INNER JOIN Employee M ON E.ManagerID = M.EmployeeID;


1️⃣2️⃣ Which manager is managing the most employees?

SELECT CONCAT(M.FirstName, ' ', M.LastName) AS ManagerName,
       COUNT(E.EmployeeID) AS NumberOfEmployees
FROM Employee E
INNER JOIN Employee M ON E.ManagerID = M.EmployeeID
GROUP BY CONCAT(M.FirstName, ' ', M.LastName);


1️⃣3️⃣ Retrieve names of employees working on projects as 'Software Engineer', ordered by project start date.

SELECT CONCAT(FirstName, ' ', LastName) AS Name, JobTitleName, StartDate 
FROM Employee E
INNER JOIN ProjectAllocation PA ON E.EmployeeID = PA.EmployeeID
INNER JOIN Project P ON P.ProjectID = PA.ProjectID
INNER JOIN JobTitle J ON J.JobTitleID = E.JobTitleID
WHERE JobTitleName = 'Software Engineer'
ORDER BY StartDate;


1️⃣4️⃣ Retrieve names of employees working on 'Project Delta'.

SELECT CONCAT(FirstName, ' ', LastName) AS Name, ProjectName, StartDate 
FROM Employee E
INNER JOIN ProjectAllocation PA ON E.EmployeeID = PA.EmployeeID
INNER JOIN Project P ON P.ProjectID = PA.ProjectID
WHERE P.ProjectName = 'Project Delta'
ORDER BY StartDate;


1️⃣5️⃣ Retrieve employee names, department name, and total salary.

SELECT CONCAT(FirstName, ' ', LastName) AS FullName, DepartmentName, 
       (BaseSalary + Bonus) - Deductions AS Total_Salary
FROM Employee E
INNER JOIN Department D ON D.DepartmentID = E.DepartmentID
INNER JOIN Salary S ON S.EmployeeID = E.EmployeeID
ORDER BY Total_Salary DESC;


1️⃣6️⃣ Create a function to find employees with a birthday in the given month and calculate their age.

CREATE FUNCTION GetBirthday(@Month INT)
RETURNS TABLE
RETURN (
    SELECT FirstName, LastName, DateOfBirth,
           YEAR(GETDATE()) - YEAR(DateOfBirth) AS Age
    FROM Employee
    WHERE MONTH(DateOfBirth) = @Month
);

-- Use the function
SELECT * FROM DBO.GetBirthday(1);
