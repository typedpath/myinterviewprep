# SQL Interview Questions

This collection of SQL interview questions covers fundamental and advanced topics, including data manipulation, joins, aggregations, indexing, and database design. These questions are suitable for assessing candidates for roles involving HR and workforce management systems like elementsuite, which rely on relational databases to manage employee data, schedules, and performance metrics.

## Basic SQL Questions

1. **Write a SQL query to insert a new employee record into an `EMPLOYEE` table with columns `employee_id`, `first_name`, `last_name`, `email`, and `hire_date`.**
    - **Tests**: Understanding of `INSERT` statements and handling basic data types.
    - **Example Answer**:
      ```sql
      INSERT INTO EMPLOYEE (employee_id, first_name, last_name, email, hire_date)
      VALUES (1001, 'John', 'Doe', 'john.doe@company.com', '2025-06-30');
      ```

2. **How do you retrieve all columns from a table called `DEPARTMENT`?**
    - **Tests**: Familiarity with basic `SELECT` queries.
    - **Example Answer**:
      ```sql
      SELECT * FROM DEPARTMENT;
      ```

3. **Write a query to update the `email` of an employee with `employee_id = 1001` in the `EMPLOYEE` table.**
    - **Tests**: Knowledge of `UPDATE` statements and `WHERE` clause usage.
    - **Example Answer**:
      ```sql
      UPDATE EMPLOYEE
      SET email = 'john.new@company.com'
      WHERE employee_id = 1001;
      ```

4. **What is the difference between `DELETE` and `TRUNCATE` in SQL?**
    - **Tests**: Understanding of data deletion methods and their implications.
    - **Expected Answer**: `DELETE` removes specific rows based on a condition and can be rolled back, while `TRUNCATE` removes all rows, resets auto-increment counters, and is faster but cannot be rolled back in some databases.

## Intermediate SQL Questions

5. **Write a query to retrieve all employees from the `EMPLOYEE` table who were hired after January 1, 2025, and belong to the 'HR' department, assuming a `department_id` links to a `DEPARTMENT` table.**
    - **Tests**: Joins and date filtering.
    - **Example Answer**:
      ```sql
      SELECT e.employee_id, e.first_name, e.last_name, e.hire_date
      FROM EMPLOYEE e
      JOIN DEPARTMENT d ON e.department_id = d.department_id
      WHERE e.hire_date > '2025-01-01' AND d.name = 'HR';
      ```

6. **Write a query to find the total number of shifts per employee in the `SHIFT` table for June 2025, grouped by employee and ordered by the number of shifts descending.**
    - **Tests**: Aggregation, grouping, and date functions.
    - **Example Answer**:
      ```sql
      SELECT s.employee_id, COUNT(*) as shift_count
      FROM SHIFT s
      JOIN SCHEDULE sch ON s.schedule_id = sch.schedule_id
      WHERE sch.start_date BETWEEN '2025-06-01' AND '2025-06-30'
      GROUP BY s.employee_id
      ORDER BY shift_count DESC;
      ```

7. **Explain the difference between `INNER JOIN`, `LEFT JOIN`, and `RIGHT JOIN`. Provide an example of an `INNER JOIN`.**
    - **Tests**: Understanding of join types and their use cases.
    - **Example Answer**:
        - `INNER JOIN` returns only matching records from both tables.
        - `LEFT JOIN` returns all records from the left table and matching records from the right, with `NULL` for non-matches.
        - `RIGHT JOIN` is the opposite, returning all records from the right table.
        - Example:
          ```sql
          SELECT e.first_name, d.name
          FROM EMPLOYEE e
          INNER JOIN DEPARTMENT d ON e.department_id = d.department_id;
          ```

8. **Write a query to find employees who have not completed any training courses, assuming `TRAINING` links employees to courses.**
    - **Tests**: Use of `LEFT JOIN` and handling nulls for non-matching records.
    - **Example Answer**:
      ```sql
      SELECT e.employee_id, e.first_name
      FROM EMPLOYEE e
      LEFT JOIN TRAINING t ON e.employee_id = t.employee_id
      WHERE t.training_id IS NULL;
      ```

## Advanced SQL Questions

9. **Design a SQL query to calculate the average KPI score for each employee in the `KPI` table, considering only KPIs from 2025, and include only employees with an average score above 80.**
    - **Tests**: Advanced aggregation, filtering, and date handling.
    - **Example Answer**:
      ```sql
      SELECT k.employee_id, AVG(k.value) as avg_kpi_score
      FROM KPI k
      JOIN PERFORMANCE_RECORD pr ON k.record_id = pr.record_id
      WHERE YEAR(pr.review_date) = 2025
      GROUP BY k.employee_id
      HAVING AVG(k.value) > 80;
      ```

10. **Write a query to insert multiple job applications into the `JOB_APPLICATION` table in a single statement, ensuring `application_date` is the current date.**
    - **Tests**: Bulk insertion and use of date functions.
    - **Example Answer**:
      ```sql
      INSERT INTO JOB_APPLICATION (employee_id, position_id, application_date, status)
      VALUES
          (1001, 201, CURRENT_DATE, 'Pending'),
          (1002, 202, CURRENT_DATE, 'Pending'),
          (1003, 201, CURRENT_DATE, 'Pending');
      ```