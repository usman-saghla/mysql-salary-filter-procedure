# MySQL Salary Filter Procedure

This repository contains a MySQL stored procedure that filters employee salaries within a given range and returns the **average salary grouped by gender and department**.

## 📌 Features
- Filter salaries between a minimum and maximum range.
- Join employee, salary, and department tables.
- Get aggregated results grouped by department and gender.

## 🗂️ Schema Requirements
The procedure assumes the following tables exist in your database (`employees_mod`):
- `t_employees` (employee data, includes `emp_no`, `gender`)
- `t_salaries` (salary data, includes `emp_no`, `salary`)
- `t_departments` (department data, includes `dept_no`, `dept_name`)
- `t_dept_emp` (relationship between employees and departments, includes `emp_no`, `dept_no`)

## ⚙️ Stored Procedure

```sql
USE employees_mod;

DROP PROCEDURE IF EXISTS filter_salary;

DELIMITER $$
CREATE PROCEDURE filter_salary (IN p_min_salary FLOAT, IN p_max_salary FLOAT)
BEGIN
    SELECT
        e.gender,
        d.dept_name,
        AVG(s.salary) as avg_salary
    FROM
        t_salaries s
    JOIN
        t_employees e ON s.emp_no = e.emp_no
    JOIN
        t_dept_emp de ON de.emp_no = e.emp_no
    JOIN
        t_departments d ON d.dept_no = de.dept_no
    WHERE s.salary BETWEEN p_min_salary AND p_max_salary
    GROUP BY d.dept_no, e.gender;
END$$

DELIMITER ;
