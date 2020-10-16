# sql-challenge

#### Data Engineering

    -- Table: public.departments

    -- DROP TABLE public.departments;

CREATE TABLE public.departments
(
    dept_no character varying(10) COLLATE pg_catalog."default" NOT NULL,
    dept_name character varying(30) COLLATE pg_catalog."default",
    CONSTRAINT departments_pkey PRIMARY KEY (dept_no)
)

TABLESPACE pg_default;

ALTER TABLE public.departments
    OWNER to postgres;
    
    
    -- Table: public.dept_emp

    -- DROP TABLE public.dept_emp;

CREATE TABLE public.dept_emp
(
    emp_no integer NOT NULL,
    dept_no character varying(10) COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT dept_emp_pkey PRIMARY KEY (emp_no, dept_no)
)

TABLESPACE pg_default;

ALTER TABLE public.dept_emp
    OWNER to postgres;
    
    
    -- Table: public.dept_manager

    -- DROP TABLE public.dept_manager;

CREATE TABLE public.dept_manager
(
    dept_no character varying(10) COLLATE pg_catalog."default" NOT NULL,
    emp_no integer NOT NULL,
    CONSTRAINT dept_manager_pkey PRIMARY KEY (dept_no, emp_no)
)

TABLESPACE pg_default;

ALTER TABLE public.dept_manager
    OWNER to postgres;
    
    
    -- Table: public.employees

    -- DROP TABLE public.employees;

CREATE TABLE public.employees
(
    emp_no integer NOT NULL,
    emp_title_id character varying(10) COLLATE pg_catalog."default" NOT NULL,
    birth_date timestamp without time zone,
    first_name name COLLATE pg_catalog."C",
    last_name name COLLATE pg_catalog."C",
    sex character varying(1) COLLATE pg_catalog."default",
    hire_date timestamp without time zone,
    CONSTRAINT employees_pkey PRIMARY KEY (emp_no, emp_title_id)
)

TABLESPACE pg_default;

ALTER TABLE public.employees
    OWNER to postgres;
    
    
    -- Table: public.salaries

    -- DROP TABLE public.salaries;

CREATE TABLE public.salaries
(
    emp_no integer NOT NULL,
    salary integer,
    CONSTRAINT salaries_pkey PRIMARY KEY (emp_no)
)

TABLESPACE pg_default;

ALTER TABLE public.salaries
    OWNER to postgres;
    
    
    -- Table: public.titles

    -- DROP TABLE public.titles;

CREATE TABLE public.titles
(
    title_id character varying(10) COLLATE pg_catalog."default" NOT NULL,
    title character varying(30) COLLATE pg_catalog."default",
    CONSTRAINT titles_pkey PRIMARY KEY (title_id)
)

TABLESPACE pg_default;

ALTER TABLE public.titles
    OWNER to postgres;

#### Data Analysis

1. List the following details of each employee: employee number, last name, first name, sex, and salary.

SELECT employees.emp_no, employees.last_name, employees.first_name, employees.sex, salaries.salary
FROM employees
JOIN salaries
ON employees.emp_no = salaries.emp_no;

2. List first name, last name, and hire date for employees who were hired in 1986.

SELECT employees.first_name, employees.last_name, employees.hire_date
FROM employees
WHERE employees.hire_date >= '1986-01-01' AND employees.hire_date <= '1986-12-31';

3. List the manager of each department with the following information: department number, department name, the manager's employee number, last name, first name.

SELECT dept_manager.dept_no, departments.dept_name, dept_manager.emp_no, employees.last_name, employees.first_name
FROM employees 
JOIN dept_manager
ON employees.emp_no = dept_manager.emp_no
JOIN departments
ON departments.dept_no = dept_manager.dept_no

4. List the department of each employee with the following information: employee number, last name, first name, and department name.

SELECT dept_emp.emp_no, employees.last_name, employees.first_name, departments.dept_name 
FROM employees 
JOIN dept_emp
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON departments.dept_no = dept_emp.dept_no

5. List first name, last name, and sex for employees whose first name is "Hercules" and last names begin with "B."

SELECT employees.first_name, employees.last_name, employees.sex 
FROM employees 
WHERE employees.first_name = 'Hercules' AND employees.last_name LIKE 'B%'

6. List all employees in the Sales department, including their employee number, last name, first name, and department name.

SELECT dept_emp.emp_no, employees.last_name, employees.first_name, departments.dept_name 
FROM employees 
JOIN dept_emp
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON departments.dept_no = dept_emp.dept_no
WHERE dept_name LIKE 'Sales%'

7. List all employees in the Sales and Development departments, including their employee number, last name, first name, and department name.

SELECT dept_emp.emp_no, employees.last_name, employees.first_name, departments.dept_name 
FROM employees 
JOIN dept_emp
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON departments.dept_no = dept_emp.dept_no
WHERE dept_name IN ('Sales', 'Development');

8. In descending order, list the frequency count of employee last names, i.e., how many employees share each last name.

SELECT employees.last_name, COUNT(employees.last_name)
FROM employees
GROUP BY employees.last_name
ORDER BY COUNT(employees.last_name) DESC;

## Bonus (Optional)

EmployeeSQL.ipynb file added on repository sql-challenge