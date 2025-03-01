# MySQL

## 1. MySQL Overview
DataBase（DB）: Warehouse for storing data, data is stored in an organized manner.

DataBase Management System (DBMS) : Large-scale software to manipulate and manage databases.

Structured Query Language (SQL): A programming language for manipulating relational databases that defines a set of uniform standards for manipulating relational databases.

Relational Database Management System:
 - Oracle
 - MySQL
 - SQL Server
 - PostgreSQL
 - DB2
 - SQLLite
 - MariaDB

```bash
mysql [-h 127.0.0.1] [-P 3306] -u root -p

mysql -u root -p
```

RDBMS:
A database that stores data based on two-dimensional tables becomes a relational database, and a database that does not store data based on two-dimensional tables is a non-relational database.

We can connect to the database management system DBMS through MySQL client and then manipulate the database through DBMS. 

We can use SQL statements to manipulate the database through the DBMS, as well as manipulate the table structure and data in the database.

## 2. SQL
### 2.1 Data Definition Language (DDL)
 - Database operation
```sql
show databases;

select database();

create database lemon;

create database if not exists lemon;

create database student default charset utf8mb4;

drop database if exists lemon;

use lemon;
```
 - Table operation
```sql
show tables;

-- View the specified table structure
desc tb_user;

-- Query The table creation statement for the specified table.
show create table tb_user;

create table tb_user(
  id int comment 'Id',
  name varchar(50) comment 'Name',
  age int comment 'Age',
  gender varchar(1) comment 'Gender'
) comment 'User table';

-- Add field
ALTER TABLE emp ADD nickname varchar(20) COMMENT 'Nickname';
-- Modify field names and field types
ALTER TABLE emp CHANGE nickname username varchar(30) COMMENT 'Username';
-- Delete Field
ALTER TABLE emp DROP username;
-- Change table name
ALTER TABLE emp RENAME TO employee;

-- Delete table
DROP TABLE IF EXISTS tb_user;
```
 - Data type
   - Numeric type: tinyint, smallint, mediumint, int/integer, bigint, float, double, decimal
   - String type: char, varchar, tinyblob, tinytext, blob, text, mediumblob, mediumtext, longblob, longtext
   - Date Time Type: date, time, year, datetime, timestamp
### 2.2 Data Manipulation Language (DML)
```sql
-- Insert data
insert into employee(id,workno,name,gender,age,idcard,entrydate)
values(1,'1','alex','Male',10,'123456789012345678','2000-01-01');

insert into employee values(2,'2','tony','Male',18,'123456789012345670','2005-01-
01');

insert into employee values(3,'3','Luna','Female',38,'123456789012345670','2005-01-
01'),(4,'4','Tom','Male',18,'123456789012345670','2005-01-01');

-- Modify data
update employee set name = 'luna' where id = 1;

update employee set entrydate = '2008-01-01';

-- Delete data
delete from employee where gender = 'Female';
delete from employee;
```
### 2.3 Data Query Language (DQL)
```sql
select name,workno,age from emp;
select id,workno,name,gender,age,idcard,workaddress,entrydate from emp;
select * from emp;

-- alias
select workaddress as w from emp;
-- as can be omitted
select workaddress w from emp;

select distinct workaddress w from emp;

select * from emp where age = 88;
select * from emp where idcard is null;
select * from emp where idcard is not null;

select * from emp where age != 88;
select * from emp where age <> 88;

select * from emp where age >= 15 && age <= 20;
select * from emp where age >= 15 and age <= 20;
select * from emp where age between 15 and 20;

select * from emp where age = 18 or age = 20 or age =40;
select * from emp where age in (18,20,40);

-- Query information about employees with two-character names
select * from emp where name like '__';

-- Query information of employees whose last digit of ID number is X
select * from emp where idcard like '%X';
```
 - Aggregate function
```sql
select count(*) from emp; -- The count is the total number of records.
select count(idcard) from emp; -- The count is the number of records where the idcard field is not null.

select avg(age) from emp;
select max(age) from emp;
select min(age) from emp;
select sum(age) from emp where workaddress = 'paris';
```
 - Group Query
```sql
-- Number of male and female employees according to gender group
select gender, count(*) from emp group by gender;

-- The average age of male and female employees according to gender grouping.
select gender, avg(age) from emp group by gender;

-- Query the employees whose age is less than 45, and according to the grouping of work addresses, get the work addresses with the number of employees greater than or equal to 3.
select workaddress, count(*) address_count from emp where age < 45 group by
workaddress having address_count >= 3;
```
 - Sort Query
```sql
select * from emp order by age asc;
select * from emp order by age;

select * from emp order by entrydate desc;

select * from emp order by age asc , entrydate desc;
```
 - Page Query
   - Starting Index = (Query Page Number - 1) * Number of Records per Page
```sql
select * from emp limit 0,10;
select * from emp limit 10;

-- Query page 2 employee data, show 10 records per page
select * from emp limit 10,10;
```
 - Order of execution
   - from ... where ... group by ... having ... select ... order by ... limit ...

### 2.4 Data Control Language (DCL)
```sql
-- Query user
select * from mysql.user;
```

![result](./Image/01.png)

Where Host represents the host that the current user is accessing, if it is localhost, it only means that it can only be accessed on the current local machine, and cannot be accessed remotely. User represents the username of the user accessing the database.

In MySQL we need to uniquely identify a user by Host and User.

```sql
-- Create user tony, accessible only on current host localhost, password 123456.
create user 'tony'@'localhost' identified by '123456'; 

-- Create user tom, who can access the database from any host, password 123456.
create user 'tom'@'%' identified by '123456'; 

-- Modify the access password for user tom to 1234;
alter user 'tom'@'%' identified with mysql_native_password by '1234'; 

-- Delete the tony@localhost user
drop user 'tony'@'localhost';
```

```sql
-- Query permissions for user 'tom'@'%'
show grants for 'tom'@'%'; 
-- Grant the 'tom'@'%' user lemon all access to all tables in the database.
grant all on lemon.* to 'tom'@'%'; 
-- Revoke all permissions on lemon database for 'tom'@'%' user
revoke all on lemon.* from 'tom'@'%';
```
## 3. Function
### 3.1 String function
```sql
select concat('Hello' , ' MySQL'); 

select lower('Hello'); 

select upper('Hello'); 

-- left fill
select lpad('01', 5, '-'); 

select rpad('01', 5, '-'); 

select trim(' Hello MySQL '); 

select substring('Hello MySQL',1,5);
```
### 3.2 Numeric function
```sql
-- Upward rounding
select ceil(1.1); 

select floor(1.9); 

select mod(7,4); 

select rand(); 

select round(2.344,2); 

-- Generate a six-digit random CAPTCHA
select lpad(round(rand()*1000000 , 0), 6, '0');
```
### 3.3 Date function
```sql
select curdate();

select curtime(); 

-- Current date and time
select now();

select YEAR(now()); 
select MONTH(now()); 
select DAY(now()); 

-- Increase the specified time interval
select date_add(now(), INTERVAL 70 YEAR); 

-- Get the number of days between two dates
select datediff('2021-10-01', '2021-12-01');
```

### 3.4 Process function
```sql
select if(false, 'Ok', 'Error');

select ifnull('Ok','Default');

select ifnull('','Default');

select ifnull(null,'Default'); 
```

## 4. Constraint
 - NOT NULL
 - UNIQUE
 - PRIMARY KEY 
 - DEFAULT
 - CHECK
 - FOREIGN KEY
```sql
CREATE TABLE tb_user(
    id int AUTO_INCREMENT PRIMARY KEY,
    name varchar(10) NOT NULL UNIQUE,
    age int check (age > 0 && age <= 120),
    status char(1) default,
    gender char(1)
);
```
```sql
create table dept(
    id int auto_increment comment 'ID' primary key,
    name varchar(50) not null
);

create table emp(
    id int auto_increment comment 'ID' primary key,
    name varchar(50) not null,
    age int,
    job varchar(20),
    salary int,
    entrydate date,
    managerid int,
    dept_id int
);
```
 - Add foreign key
```sql
alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id);
```
 - Delete foreign key
```sql
alter table emp drop foreign key fk_emp_dept_id;
```

 - Delete/update behavior
   - CASCADE
     - When deleting/updating a corresponding record in the parent table, first check if the record has a corresponding foreign key, and if so, also delete/update the record whose foreign key is in the child table.
      ```sql
      alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references 
      dept(id) on update cascade on delete cascade ;
      ```
   - SET NULL
     - When deleting a corresponding record in the parent table, first check whether the record has a corresponding foreign key, and if so, set the value of the foreign key in the child table to null (this requires that the foreign key be allowed to take null).
      ```sql
      alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references 
      dept(id) on update cascade on delete cascade ;
      ```
   - NO ACTION/RESTRICT/SET DEFAULT
## 5. Multi-table query
### 5.1 Multi-table relationship
 - Many-to-one
   - Create a foreign key on the side of many that points to the primary key on the side of one
 - Many-to-many
   - Create a third intermediate table with at least two foreign keys associated with the primary keys of the two parties
 - One-to-one
   - Add a foreign key to either side, associate it with the primary key of the other side, and set the foreign key to be UNIQUE
### 5.2 Inner Join
 - Implicit inner join
```sql
select emp.name , dept.name from emp , dept where emp.dept_id = dept.id ; 

-- Alias each table to simplify SQL writing. 
select e.name,d.name from emp e , dept d where e.dept_id = d.id; 
```
 - Explicit inner join
```sql
select e.name, d.name from emp e inner join dept d on e.dept_id = d.id; 

-- Alias each table to simplify SQL writing.  
select e.name, d.name from emp e join dept d on e.dept_id = d.id; 
```
### 5.3 Outer join
 - left outer join
```sql
select e.*, d.name from emp e left outer join dept d on e.dept_id = d.id;

select e.*, d.name from emp e left join dept d on e.dept_id = d.id;
```
 - right outer join
```sql
select d.*, e.* from emp e right outer join dept d on e.dept_id = d.id;

select d.*, e.* from dept d left outer join emp e on e.dept_id = d.id;
```
### 5.4 Self join
```sql
select a.name , b.name from emp a , emp b where a.managerid = b.id;
```
### 5.5 Syndicated query
```sql
select * from emp where salary < 5000
union all
select * from emp where age > 50;
```
### 5.6 Subquery
 - Scalar subqueries (subqueries result in a single value)
```sql
-- Find out all the information about the employees of "Sales Department".
select * from emp where dept_id = (select id from dept where name = 'sales'); 
```
 - Column subqueries (subquery results in a column)
```sql
-- Find out about all employees in the "Sales" and "Marketing" departments.
select * from emp where dept_id in (select id from dept where name = 'sales' or
name = 'marketing');
```
 - Row subqueries (subquery result is one row)
```sql
-- Find out about employees with the same salary and direct reports as "tom"
select * from emp where (salary,managerid) = (select salary, managerid from emp
where name = 'tom');
```
 - Table subqueries (subquery results in multiple rows and columns)
```sql
Search for employees with the same job title and salary as "tom" , "jerry"
select * from emp where (job,salary) in ( select job, salary from emp where name =
'tom' or name = 'jerry' );
```