--LISTED TABLES:

select *
from public.department_data dd;

select *
from public.employee_data ed; 

select *
from manager_data md;

select *
from project_data pd; 

--'GROUP BY' STATEMENTS

select salary, count(salary) as count_of_sal
from public.employee_data ed 
group by salary;

-- 'WHERE' STATEMENTS

select *
from public.employee_data ed 
where salary between 20000 and 29000
order by salary desc;

--'IN' STATEMENTS

select *
from PUBLIC.department_data dd
where dept_name in ('Admin', 'HR', 'Media');

select *
from PUBLIC.department_data dd
where dept_name like 'I%';

select salary, manager_id 
from public.employee_data ed 
where salary < 20000 and salary > 10000  and manager_id NOT IN ('D3');

select *
from public.employee_data ed 
order by emp_id desc 
limit 5;

--CASE STATEMENT

select emp_id, salary,
case when salary >= 40000 then 'High pay'
	when salary between 30000 and 39000 then 'Median pay'
	when salary < 30000 then 'Low pay' 
	end as range 
from public.employee_data ed 
order by 2 desc;

--JOIN CLAUSES

--1. Fetch all employee names, salary and their department names:

select ed.emp_name, salary,  dd.dept_name 
from employee_data ed 
join department_data dd on ed.dept_id = dd.dept_id;

--2.Fetch all employee names, salary and their department names, where salaries are over 30000:
select ed.emp_name, salary,  dd.dept_name 
from employee_data ed 
join department_data dd on ed.dept_id = dd.dept_id
where salary >= 30000
and dept_name = 'Admin';

-- JOINING MULTIPLE TABLES: fetch all employee and manager names, department name and project names where employee earns under £20000:
select ed.emp_name, md.manager_name, dd.dept_name, pd.project_name
from employee_data ed 
join department_data dd on ed.dept_id = dd.dept_id
join manager_data md  on md.dept_id = dd.dept_id
join project_data pd on ed.manager_id  = pd.team_member_id; 
where salary <20000;


-- UNION: fetch all employees id AND name for those  who earn >20000, and fetch managers id their departments and projects and project id:

select ed.emp_id, ed.emp_name, md.manager_name, md.manager_id, dd.dept_name, pd.project_name
from employee_data ed 
join department_data dd on ed.dept_id = dd.dept_id
join manager_data md  on md.dept_id = dd.dept_id
join project_data pd on ed.manager_id  = pd.team_member_id; 
where salary >20000;

--joins, left, right, outer

select *
from employee_data ed 
join manager_data md on ed.manager_id  = md.manager_id 

select 
from employee_data ed 
join manager_data md on ed.manager_id  = md.manager_id 

select *
from employee_data ed 
join project_data pd on ed.manager_id  = pd.team_member_id

select *
from employee_data ed 
left join project_data pd on ed.manager_id  = pd.team_member_id

select *
from employee_data ed 
right join project_data pd on ed.manager_id  = pd.team_member_id

select *
from employee_data ed 
full outer join project_data pd on ed.manager_id  = pd.team_member_id

--UNIONS: Union of employee and manager data - will only process as tables MUST have same number of columns:

select ed.emp_id, ed.emp_name, ed.dept_id 
from public.employee_data ed
union all
select *
from public.manager_data md
order by 1;

--CASE STATEMENTS: None of tables contain null values, however this calls for case where 'no null'.
select ed.emp_id, ed.emp_name, ed.salary,
case
	when ed.salary > 29000 then 'High'
	else 'Low'
end as salary_category
from public.employee_data ed
where ed.emp_name is not null; 

-- CASE STATEMENT FROM 
select ed.emp_name, ed.dept_id, ed.salary, 
case
	when ed.dept_id = 'D1' then salary + (salary * .50)
	when ed.dept_id = 'D2' then salary + (salary * .40)
	else salary + (salary * .20)
end as salary_wincrease
from public.employee_data ed
order by 1;

--HAVING CLAUSE: where can not contain an aggregate statement, having is conditional on the 'group by': 
select dd.dept_name , COUNT(dd.dept_name)
from employee_data ed 
join department_data dd on ed.dept_id = dd.dept_id
group by dd.dept_name 
having COUNT(dd.dept_name) > 2

select dd.dept_name, avg(round(ed.salary,1)) as avg_salary
from employee_data ed
join department_data dd
on ed.dept_id = dd.dept_id
group by dd.dept_name
having avg(round(ed.salary,1))  > 30000
order by avg(round(ed.salary,1));

--UPDATING:
select *
from employee_data ed 

update public.employee_data 
set salary = 18000
where emp_id = 'E1' and emp_name = 'Cyndia';

--DELETING: Delete row for employee named Jonas, using unique value 'E2':
--DELETE 
--FROM public.employee_data 
--WHERE emp_id = 'E2'

--ALIASING: keep in mind column types must be the same for aliasing to work:
SELECT emp_id::varchar(255) || ' ' || emp_name AS staff_mem
FROM public.employee_data ed;

-- PARTITION
select ed.emp_name, salary, md.manager_name, dd.dept_name, count(dept_name) over (partition by dept_name) as total_dept
from employee_data ed 
join department_data dd on ed.dept_id = dd.dept_id
join manager_data md on dd.dept_id = md.dept_id 

--using GROUP BY instead of PARTITION does not give the response above:
select dd.dept_name, count(dd.dept_name)
from employee_data ed 
join department_data dd on ed.dept_id = dd.dept_id
join manager_data md on dd.dept_id = md.dept_id 
group by dd.dept_name;

--COMMON TABLE EXPRESSION(CTE) - ONLY IN MEMORY:
with CTE_employees as
(select ed.emp_name, salary, md.manager_name, dd.dept_name, count(dept_name) over (partition by dept_name) as total_dept
from employee_data ed 
join department_data dd on ed.dept_id = dd.dept_id
join manager_data md on dd.dept_id = md.dept_id )
select salary, total_dept
from CTE_employees 

--TEMP TABLES: 
--TEMP TABLE THAT IS DRAWING FROM AN EXISTING TABLE -- TEMP TABLE -- :

DROP TABLE IF EXISTS TEMP_employeesn;
CREATE TABLE TEMP_employeesn (
  emp_id VARCHAR (10),
  emp_name VARCHAR(20),
  salary INT,
  dept_id VARCHAR (10)
);

insert into temp_employeesn 
select emp_id, emp_name, salary, dept_id
from PUBLIC.employee_data;

select *
from temp_employeesn; 

create table temp_employeesn2(
dept_id VARCHAR (10)
, dept_count int
, salary int
, avg_salary int
)

insert into temp_employeesn2
select md.dept_id, count(md.dept_id), ed.salary, avg(ed.salary)
from public.employee_data ed
join public.manager_data md on ed.dept_id = md.dept_id
group by md.dept_id, ed.salary

select *
from temp_employeesn2

--TEMP TABLE THAT IS  NEWLY CREATED
drop table if EXISTS TEMP_employees;
create table TEMP_employees(
First_name varchar (30)
, Surname_name varchar (30)
,Employee_id int
, Job_title varchar(100)
, Salary int
);
select *
from TEMP_employees



insert into TEMP_employees values 
('THomas', 'Stanley', '100', 'Analyst', '30000')
, ('Samantha', 'Jyordan', '  101', 'HR', '29000')
,('Hanna', 'Ahmed  ', '102', 'Customer Services - temporary', '20000');

select *
from TEMP_employees


-- STRING FUNCTION TRIMS NOTE TRIM ONLY WORKS ON VARCHAR SO CONVERTING OF DTYPE MAY BE NECESSARY:

select
  Employee_id,
  btrim(Employee_id::varchar) AS trimmed_employee_id
from temp_employees te;


select Surname_name, ltrim(Surname_name)
from temp_employees te;

--REPLACE USE:
select Surname_name, replace(Surname_name, 'Jyordan', 'Jordan') as LastNameFix
from temp_employees te;

UPDATE temp_employees
SET Surname_name = REPLACE(Surname_name, 'Jyordan', 'Jordan')
WHERE Surname_name LIKE '%Jyordan%';

select replace(first_name, 'THomas', 'Thomas') 
from temp_employees te;

-- REPLACE AND UPDATE
UPDATE temp_employees
SET first_name = REPLACE(first_name, 'THomas', 'Thomas')
WHERE first_name LIKE '%THomas%';

--JOB TITLE REPLACE
select job_title, replace(job_title, 'Customer Services - temporary', 'Customer Service') as fixedjtitle
from temp_employees te;

UPDATE temp_employees
SET job_title = REPLACE(job_title, 'Customer Services - temporary', 'Customer Service')
WHERE job_title LIKE '%C%';


--SUBQUERIES : From all of the salaries for employees, return employees earning less than the average salary amount:

select emp_name, salary
from PUBLIC.employee_data ed
where salary < (select avg(salary) from employee_data ed2)

--SUBQUERIES: RETURN THE COLLEAGUE NAMES FROM EACH DEPARTMENT RECEIVING THE MAX SALARY FOR THEIR DEPARTMENTS

SELECT *
FROM employee_data ed
JOIN department_data dd ON ed.dept_id = dd.dept_id
WHERE (ed.salary, dd.dept_name)  IN (
    SELECT MAX(salary), dept_name
    FROM employee_data ed
    JOIN department_data dd ON ed.dept_id = dd.dept_id
    GROUP BY dd.dept_name
);













