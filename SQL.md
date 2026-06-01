query to find the employee whose salary is highest.(When all salaries are different)
SELECT name, MAX(salary) as salary FROM employee 

SELECT name, MAX(salary) AS salary FROM employee WHERE salary < (SELECT MAX(salary) FROM employee); 
SELECT salary FROM employee ORDER BY salary desc limit n-1,1

query to find the employee 4th highest salary.(When multiple employee have same salary. salaries are different)
SELECT * FROM employee WHERE salary= (SELECT DISTINCT(salary) employee ORDER BY salary desc LIMIT 3,1);


DeptID      EmpName   Salary
Engg        Sam       1000
Engg        Smith     2000
HR          Denis     1500
HR          Danny     3000
IT          David     2000
IT          John      3000

how to get the max salary from each department
SELECT DeptID, MAX(Salary) FROM EmpDetails GROUP BY DeptID

If you want the employee(s) in each department with the highest salary, 
SELECT empName,empDept,EmpSalary FROM Employee WHERE empSalary IN (SELECT max(empSalary) AS salary From Employee GROUP BY EmpDept)

SELECT Dept, MAX(salary) Salary FROM Employee e RIGHT JOIN Dept d ON e.Dept = d.deptid GROUP BY Dept;

========================================================

| StudentName | StudentMarks |
+-------------+--------------+
| Adam        |           56 |
| Chris       |           78 |
| Adam        |           89 |
| Robert      |           98 |
| Chris       |           65 |
| Robert      |           34 |
| Kali        |          110 |
+-------------+--------------+

Average of all the students with dublicate(same student exists)
select avg(StudentMarks) from Student; // 75.56

Average marks of each the student
select avg(StudentMarks), studentname from Student group by studentname ;
Robert	66 ,Chris 71.5,kali 110, Adam 72.5

Below query is average marks of all the students.
select avg(Marks) from (select avg(StudentMarks) as Marks from Student group by studentname);
//80

Below is to find the students which are greater than average marks
select studentname from Student where StudentMarks > (select avg(Marks) from (select avg(StudentMarks) as Marks from Student group by studentname));
//Adam 89, Robert 98,Kali 110

