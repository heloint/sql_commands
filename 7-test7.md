#1. Enter the following code to create a copy of the employees table for this and the next question.

CREATE TABLE emp_temp AS SELECT * FROM employees;

In the new emp_temp table, delete all but one of the employees in department 10.

SELECT * FROM emp_temp WHERE department_id = 10;

Complete following block in order to manage all possible scenarios (departments with employees, departments with a single employee, departments with no employees).

-- Delete all employees from new_emps in department 10, except 1.

DELETE FROM new_emps WHERE employee_id IN (SELECT employee_id FROM new_emps WHERE department_id = 10 AND employee_id != 200);


DECLARE

    v_employee_id emp_temp.employee_id%TYPE;

    v_last_name emp_temp.last_name%TYPE;

BEGIN

    SELECT employee_id, last_name INTO v_employee_id, v_last_name

    FROM emp_temp

    WHERE department_id = 10; -- run with values 10, 20, and 30

    DBMS_OUTPUT.PUT_LINE('The SELECT was successful');

    
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            DBMS_OUTPUT.PUT_LINE('There are no registers to assign.');

        WHEN TOO_MANY_ROWS THEN
            DBMS_OUTPUT.PUT_LINE('There are too many registers to assign.');
            
END;

---

#2.
Modify the following block to handle this exception:

DECLARE

    v_number NUMBER(6, 2) := 100;

    v_region_id regions.region_id%TYPE;

    v_region_name regions.region_name%TYPE;

BEGIN

    SELECT region_id, region_name INTO v_region_id, v_region_name

    FROM regions

    WHERE region_id = 1;

    DBMS_OUTPUT.PUT_LINE('Region: ' || v_region_id || ' is: ' || v_region_name);

    v_number := v_number / 0;
    
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('There are no registers to display.');
END;

---

#3.
Modify the following block to handle this exception.

DECLARE

    v_number NUMBER(6, 2) := 100;

    v_region_id regions.region_id%TYPE;

    v_region_name regions.region_name%TYPE;

BEGIN

    SELECT region_id, region_name INTO v_region_id, v_region_name

    FROM regions

    WHERE region_id = 29;

    DBMS_OUTPUT.PUT_LINE('Region: ' || v_region_id || ' is: ' || v_region_name);

    v_number := v_number / 0;
    
    EXCEPTION
        WHEN ZERO_DIVIDE THEN
            DBMS_OUTPUT.PUT_LINE('ERROR: Cannot divide with zero.');
END;

---

#4.
Fix the following block

DECLARE

    CURSOR regions_curs IS

    SELECT * FROM regions

    WHERE region_id < 20

    ORDER BY region_id;

    regions_rec regions_curs%ROWTYPE;

    v_count NUMBER(6);

BEGIN
    OPEN regions_curs;
        LOOP

            FETCH regions_curs INTO regions_rec;

            EXIT WHEN regions_curs%NOTFOUND;

            DBMS_OUTPUT.PUT_LINE('Region: ' || regions_rec.region_id

            || ' Name: ' || regions_rec.region_name);

        END LOOP;

    CLOSE regions_curs;

END;

#5.
Fix following block adding a non_predefined exception handler to trap the ORA-01400 exception. 
Name your exception e_null_not_allowed

DECLARE
    e_null_not_allowed EXCEPTION;
    PRAGMA EXCEPTION_INIT(e_null_not_allowed, -1400 );

BEGIN

    INSERT INTO languages(language_id, language_name)

    VALUES(80, null);

    EXCEPTION
        WHEN e_null_not_allowed THEN
         DBMS_OUTPUT.PUT_LINE('The register you want to insert has null value, and the column does not allow it.');
        
END;

#6.
All the questions in this exercise use a copy of the employees table. Create this copy by running the following SQL statement:

CREATE TABLE excep_emps AS SELECT * FROM employees;

Create a PL/SQL block that updates the salary of every employee to a new value of 10000 in a chosen department.
Include a user-defined exception handler that handles the condition where no rows are updated and displays a custom message.
Also include an exception handler that will trap any other possible error condition and display the corresponding SQLCODE and SQLERRM. 
Test your code three times, using department_ids 20, 30, and 40.

DECLARE
    v_department_id := :choose_dep;
BEGIN
    UPDATE emp_temp
    SET salary = 10000
    WHERE department_id = v_department_id;
END;

Copyright © 2018, Oracle and/or its affiliates. All rights reserved. Oracle and Java are registered trademarks of Oracle and/
or its affiliates. Other names may be trademarks of their
respective owners.
DECLARE

    no_rows_updated EXEPTION;

BEGIN
    UPDATE
    excep_emps
    SET salary
    = 10000
    WHERE department_id = 20;
    IF SQL%NOTFOUND THEN
    IF SQL%ROWCOUNT = 0
    Raise E_NO_ROWS_UPDATED;
    END IF;
    EXCEPTION
        WHEN e_no_rows_updated THEN
        DBMS_OUTPUT.PUT_LINE(‘No hay empleados en ese departamento .’);

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE(‘Ha ocurrido un error:’||SQLCODE||’-‘||SQLERRM);
END

## HAY QUE TERMINARLO..
