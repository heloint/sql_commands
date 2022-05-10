## 1.Run the following anonymous block. It should execute successfully.

    DECLARE
        v_emp_lname employees.last_name%TYPE;
        v_emp_salary employees.salary%TYPE;
    BEGIN
        SELECT last_name, salary INTO v_emp_lname, v_emp_salary
        FROM employees
        WHERE job_id = 'AD_PRES';
        DBMS_OUTPUT.PUT_LINE(v_emp_lname || ' ' || v_emp_salary);
    END;

a) Now modify the block to use ‘IT_PROG’ instead of ‘AD_PRES’ and re-run it. Why does it fail this time?

b) Now modify the block to use ‘IT_PRAG’ instead of ‘IT_PROG’ and re-run it. Why does it still fail?

    ANSWER a.,:  It fails,because there are more people with job_id='IT_PROG' and in the original code there's only one person. If we want to display all the employees with job_id='IT_PROG', then we should use a FOR LOOP.

    ANSWER b.,:  Fails, because there is no such a job_id "IT_PRAG" in the employees table.

---

## 2. 
Use (but don't execute) the following code to answer this question:

    DECLARE

        last_name VARCHAR2(25) := 'Fay';

    BEGIN

        UPDATE emp_dup

        SET first_name = 'Jennifer'

        WHERE last_name = last_name;

    END;

---

## 3.
What do you think would happen if you ran the above code?

    It will change the first_name of that person who has the last_name as "Fay" in the DB. SQL knows how to differ the column name from the variable, 
    but the code is not well-written. That's why we should use "v_" before each variables.

---

## 4.
Modify the following code so that for the employee whose last_name = ”Fay”, the first_name is updated to Jennifer.

Before, create a emp_dup table as copy of employees table.

    DECLARE

        last_name VARCHAR2(25) := 'Fay';

    BEGIN

        UPDATE emp_dup

        SET first_name = 'Jennifer'

        WHERE last_name = last_name;

    END;
    
Solution:

    -- Duplicate the employee table.

        CREATE TABLE emp_dup AS (SELECT * FROM employees);

    -Update the given field. *The original code was correct.*

    DECLARE

        v_last_name VARCHAR2(25) := 'Fay';

    BEGIN

        UPDATE emp_dup

        SET first_name = 'Jennifer'

        WHERE last_name = v_last_name;

    END;

---

## 5. 
Is it possible to have a column, table, and variable, all with the same name?

    ANSWER: Nope, "invalid identifier" error will be raised.

---

## 6. 
Create a copy table from departments table called new_depts.

Create a PL/SQL block to insert a new department called "New department" to table new_depts. This new department must have as department_id the maximum current department_id value plus 10.

After insert, show "The maximum department id is: <new_value>" ans how many rows are inserted.

    CREATE TABLE new_depts as (SELECT * FROM departments);
    DECLARE
        --Declare the variables
        v_max_id NUMBER(5);
        v_old_max_rownum NUMBER(5);
        v_new_max_rownum NUMBER(5);
        v_result_rownum NUMBER(5);
    BEGIN
        -- The initial number of rows.
        SELECT MAX(ROWNUM) INTO v_old_max_rownum FROM new_depts;

        -- The largest department_id
        SELECT MAX(department_id) INTO v_max_id FROM departments;
        
        --Display the max dep. ID    
        DBMS_OUTPUT.PUT_LINE('The maximum department id is: '||v_max_id||'.');
        
        -- Add 10 to the largest dep. ID
        v_max_id := v_max_id+10;

        -- Insert the "New department" into the duplicate table.
        INSERT INTO new_depts (department_id,department_name) VALUES (v_max_id,'New department');

        --Re-assign the post-insert number of rows.
        SELECT MAX(ROWNUM) INTO v_new_max_rownum FROM new_depts;
        
        --Count the difference of rownums between pre-insert and post-insert
        v_result_rownum := v_new_max_rownum - v_old_max_rownum;


        -- Display result
        DBMS_OUTPUT.PUT_LINE('Number of rows inserted: '||v_result_rownum||'.');

    END;

---

## 7. 
Create a block that UPDATE all rows with location_id = 1700 to location_id = 1400 from new_depts table.
If the change is done, show how many rows are affected if not show the message "Update not done".

    DECLARE
        v_counter NUMBER(5):=0;
    BEGIN
        -- For each department register in new_depts...
        FOR dep in (SELECT * FROM new_depts)


        LOOP
            -- If location id match, then update it and add 1 to the counter variable.
            IF dep.location_id = 1700 THEN
                UPDATE new_depts
                SET location_id = 1400
                WHERE department_id = dep.department_id;
                
                v_counter := v_counter + 1;
            END IF;
        END LOOP;
        -- If v_counter is bigger than 0, then display the v_counter result.
        IF v_counter > 0 THEN
            DBMS_OUTPUT.PUT_LINE('Number of rows affected: '||v_counter);
        -- If the v_counter is 0 then display the "Update not done" message.
        ELSIF v_counter = 0 THEN
            DBMS_OUTPUT.PUT_LINE('Update not done');
        END IF;
    END;

---
