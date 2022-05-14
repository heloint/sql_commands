# 1.
Create a function called full_name. 
Pass two parameters to the function, an employee’s last name and first name. 
The function should return the full name in the format, last name, comma, space, first name (for example: Smith, Joe).
Save your code.

Now call the function from within a SELECT statement. Your SELECT statement should display the first_name, last_name, and full name (using the function) of all employees in department 50.

    CREATE OR REPLACE FUNCTION full_name
        (p_last_name employees.last_name%TYPE,
         p_first_name employees.first_name%TYPE)
        RETURN VARCHAR2 IS
        v_full_name VARCHAR2(100);
    BEGIN
        v_full_name := p_last_name ||' '||p_first_name;
        RETURN v_full_name;
    END full_name;

    SELECT 
        first_name as "First name", 
        last_name as "Last name", 
        full_name(first_name,last_name) as "Full name"
    FROM employees
    WHERE department_id = 50;

---

# 2.

Create a function called divide that accepts two numbers as input 
and returns the result of dividing the first number by the second number, 
rounded to two decimal places.

Function should handle all possible exceptions.

    CREATE OR REPLACE FUNCTION divide
        (p_first_number NUMBER, p_second_number NUMBER) 
        RETURN NUMBER IS
        v_result NUMBER(30,2);
    BEGIN
        v_result := ROUND(p_first_number/p_second_number,2);
        RETURN v_result;
    END;

---

# 3.
Create a function which accepts a character string as input and returns the same character string but with the order of the letters reversed. For example, "Smith" would be returned as "htimS." Save your code.
Test your function using the following SQL statements: 

    SELECT last_name, reverse_string(last_name) FROM employees; 

    SELECT country_name, reverse_string(country_name) FROM countries


    CREATE OR REPLACE FUNCTION reverse_string (p_input VARCHAR2)
        RETURN VARCHAR2 IS
        v_result VARCHAR2(200);
    BEGIN
        SELECT reverse(p_input) INTO v_result FROM dual;
        RETURN v_result;
    END reverse_string;


---

# 4.
Create a function check_dept that accepts a department id as an 
input parameter and checks whether the department exists in the departments table.


    CREATE OR REPLACE FUNCTION check_dept
     (p_department_id departments.department_id%TYPE)

     RETURN BOOLEAN IS
     v_department_id departments.department_id%TYPE;

    BEGIN
     SELECT department_id INTO v_department_id
     FROM departments WHERE department_id = p_department_id;

     RETURN TRUE;

    EXCEPTION
     WHEN NO_DATA_FOUND THEN
         RETURN FALSE;
    END;


---

# 5.
Write a procedure called insert_emp which inserts a new employee. Pass the employee id, last name, salary, and department id to the procedure as IN parameters. The procedure should call your check_dept function to verify that the passed department id exists. If it exists, insert the employee. If it does not exist, 
use DBMS_OUTPUT.PUT_LINE to display a suitable error message. Save your code.

    CREATE OR REPLACE PROCEDURE insert_emp
        (p_employee_id IN employees.employee_id%TYPE,
         p_last_name IN employees.last_name%TYPE,
         p_salary IN employees.salary%TYPE,
         p_department_id IN employees.department_id%TYPE)
    IS
    BEGIN
        IF check_dept(p_department_id) THEN
            INSERT INTO employees(employee_id, last_name, salary, department_id)
            VALUES (p_employee_id, p_last_name, p_salary, p_department_id);
        ELSE
            DBMS_OUTPUT.PUT_LINE('No department_id found.');
        END IF;
    END insert_emp;

---

# 6.
Create a procedure that given an id and a department name, adds a new department.
Manage all possible exception scenarios. 


    CREATE OR REPLACE PROCEDURE add_department
        (p_dep_id IN departments.department_id%TYPE,
         p_dep_name IN departments.department_name%TYPE)
    IS
    BEGIN
        IF check_dept(p_dep_id) THEN
            DBMS_OUTPUT.PUT_LINE('Department with the given ID already exists.');
        ELSE
           INSERT INTO departments(department_id, department_name)
           VALUES(p_dep_id, p_dep_name);
        END IF;
        EXCEPTION
        WHEN OTHERS THEN
            RAISE_APPLICATION_ERROR(-06502, 'Wrong type or value as a parameter.');
    END;


---
---
---
