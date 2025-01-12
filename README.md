# SQL-Queries
### Create table Departments:[using create command]
 create table if not exists Departments(
							DepartmentId int primary key, 
                            DepartmentName varchar(50),
                            Location varchar(30));
### Insert values into Departments Table:[using insert command]
insert into Departments(DepartmentId,DepartmentName,Location) values (1, 'IT', 'New York'),
                                                                     (2, 'HR', 'London'),
                                                                     (3, 'Sales', 'San Francisco'),
																	                                   (4, 'Finance', 'Sydney');
### Showcase Departments Table:[using select command]
select * from Departments;
### Result:
![Departments](https://github.com/user-attachments/assets/87ff62d3-cbb9-464b-aebe-1189754d18fd)



### Create table Employee:[using create command]
create table Employee(
                     EmployeeId int primary key,
                     FirstName varchar(30),
                     LastName varchar(30),
                     DepartmentId int,
                     JobTitle varchar(30),
                     Salary decimal(10,2),
                     HireDate date,
                     ManagerId int,
                    foreign key (DepartmentId) references Departments(DepartmentId));
### Insert values into Employee Table:[using insert command]
insert into Employee(EmployeeId,FirstName,LastName,DepartmentId,JobTitle,Salary,HireDate,ManagerId) values (1, 'John', 'Doe', 1, 'Software Engineer', 85000, '2020-05-10', NULL),
(2, 'Jane', 'Smith', 2, 'HR Manager', 95000, '2018-07-20', NULL),
(3, 'Alice', 'Brown', 1, 'DevOps Engineer', 78000, '2021-02-15', 1),
(4, 'Bob', 'Johnson', 3, 'Sales Executive', 65000, '2019-11-05', NULL),
(5, 'Charlie', 'Davis', 1, 'QA Engineer', 72000, '2022-03-12', 1),
(6, 'Emily', 'White', 4, 'Financial Analyst', 88000, '2020-09-01', NULL),
(7, 'Michael', 'Green', 1, 'Software Engineer', 85000, '2021-06-25', 1),
(8, 'Sarah', 'Lee', 2, 'HR Specialist', 62000, '2021-08-30', 2);

### Showcase Employee Table:[using select command]
select * from Employee;
### Result:
![Employee](https://github.com/user-attachments/assets/b86637d6-3385-4bd1-88c4-c437047b3530)

### Query 1) Find the Second Highest Salary
- Solution:
  
  select max(salary)as second_highest_salary from employee where salary<(select max(salary) from employee);
- Result

  ![output](https://github.com/user-attachments/assets/7acfb24d-d7b6-4ccd-8402-0dee71a9f1ad)

### Query 2) Retrieve names of employees whose names start with "A" and end with "n":
- Solution:

  select FirstName,LastName,JobTitle,Salary from employee where FirstName like 's%h';                
- Result

  
  ![output 1](https://github.com/user-attachments/assets/0e50e223-ca81-4cfb-a969-0df5f352adb3)

### Query 3) Display the total count of employees grouped by their department:
- Solution:

   select JobTitle, COUNT(*) AS EmployeeCount from Employee group by JobTitle;      
- Result

  
   ![output 2](https://github.com/user-attachments/assets/e8ce3119-66af-4d1f-aa11-288ed4dddb53)

### Query 4) Retrieve the list of duplicate records in a table:
- Solution:

  select JobTitle, COUNT(*) from Employee group by JobTitle having count(*) > 1;
- Result

   
   ![output3](https://github.com/user-attachments/assets/7b610e27-5c16-4812-ad06-070550cefb54)

### Query 5) List all employees and their departments (including employees with no orders):
- Solution:

   select Employee.EmployeeID, Employee.FirstName, Departments.DepartmentID, Departments.DepartmentName from Employee
   left join Departments on Employee.EmployeeID = Departments.DepartmentID;
- Result

   ![output4](https://github.com/user-attachments/assets/6f62e329-44e3-4d67-8c6b-0c0ad0b85bad)

### Query 6) Find the maximum, minimum, and average salary of employees in each department:
- Solution:

    select JobTitle, MAX(Salary) AS MaxSalary, MIN(Salary) AS MinSalary, AVG(Salary) AS AvgSalary from Employee group by JobTitle;
- Result

   ![output 5](https://github.com/user-attachments/assets/4582d9bc-f8ac-45ae-b30a-4f3b533e893d)

### Query 7) Fetch names of employees who earn more than the average salary in their jobtitle:
- Solution:

   select FirstName,LastName,Salary from Employee where Salary > (select avg(Salary) from Employee where JobTitle = Employee.JobTitle);
- Result

   ![output 6](https://github.com/user-attachments/assets/342aa089-d1dd-4315-9357-5a49906a4cb9)

### Query 8) Fetch the 3 jobtitle:
- Solution:

   select EmployeeID, FirstName, Salary from Employee order by Salary desc limit 3;
- Result

   ![output7](https://github.com/user-attachments/assets/0a5c8150-6029-4397-8397-a1966904d323)

### Query 9) Calculate the running total of salaries:
- Solution:

   select EmployeeID, FirstName, LastName, Salary, SUM(Salary) over (order by Salary) as RunningTotal from Employee;
- Result

    ![output 8](https://github.com/user-attachments/assets/b1a5b709-82e8-492a-b89d-447aa134d949)

### Query 10) Fetch EmployeeId that is distinct:
- Solution:

    select EmployeeID, JobTitle from Employee where EmployeeID not in(select distinct DepartmentID from Departments);
- Result

    
     ![output 9](https://github.com/user-attachments/assets/bea46c22-e7b0-486c-83ca-2778c49943fe)

### Query 11) To list Employee hired in the year 2021:
- Solution:

    select EmployeeId, FirstName, HireDate from Employee where year(HireDate) = 2021;
- Result

     
    ![output 10](https://github.com/user-attachments/assets/a1cf48d3-0d43-469a-9b78-1344655610e7)


### Query 12) Find the EmployeeId with highest average salary:
- Solution:

     select EmployeeId, AVG(Salary) AS AvgSalary from Employee group by EmployeeId having avg(Salary) = (select max(AvgSalary) from (select EmployeeId, AVG(Salary) AS AvgSalary
     from Employee group by EmployeeId) as Sub);
- Result

    
     ![output 11](https://github.com/user-attachments/assets/60fcaeed-45f0-4229-a2f4-c643a0e9b51b)


### Query 13) Find employees whose salary is greater than the jobtitle average:
- Solution:

    select FirstName, Salary, JobTitle frm Employee where Salary > (select AVG(Salary) from Employee where JobTitle = Employee.JobTitle);
- Result

     ![output 12](https://github.com/user-attachments/assets/21ba11eb-5156-4bc8-8417-685f4ca94984)

### Query 14) To find the departmentid according to jobtitle from departments table:
- Solution:

    select DepartmentId,JobTitle from employee where DepartmentId IN (SELECT DepartmentId from Departments);
- Result

    ![output 14](https://github.com/user-attachments/assets/24524e14-45ba-404d-981f-ef94247a6af8)


### Query 15) To union the department name from departments table and firstname from employee:
- Solution:

    select departmentname from departments union select firstname from employee;
- Result

    ![output 15](https://github.com/user-attachments/assets/03fdb5e6-bf06-4ec2-9230-814407738c22)








  

    
    

    




   

  

   


   

   



   



   


   

  










