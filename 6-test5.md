## 1.
Write and test a PL/SQL block to read and display all the rows 
in the COUNTRIES table for all countries in region 5 (South America region). 
For each selected country, display the country_name, national_holiday_date, 
and national_holiday_name. Display only 
those countries having a national holiday date that is not null. 
Save your code (you will need it later in next practice)

    DECLARE
        --Assign content to cursor.
        CURSOR c_countries IS 
            SELECT country_name, national_holiday_date, national_holiday_name
            FROM countries WHERE region_id = 5 
            AND national_holiday_date IS NOT NULL;
        
        --Record structures.
        v_country_name countries.country_name%TYPE;
        v_holiday_date countries.national_holiday_date%TYPE;
        v_holiday_name countries.national_holiday_name%TYPE;

    BEGIN
        OPEN c_countries;
            LOOP

                FETCH c_countries INTO v_country_name,
                                       v_holiday_date,
                                       v_holiday_name;

                --Display results.
                DBMS_OUTPUT.PUT_LINE('In '||v_country_name||' the '||v_holiday_name||' is held on '||v_holiday_date||'.');
                EXIT WHEN c_countries%NOTFOUND;

            END LOOP;
        CLOSE c_countries;
    END;

---

## 2.
Write a PL/SQL block to read and display the names of world regions, with a count of the number of countries in each region. Include only those regions having at least 10 countries. 
**Order your output by ascending region name.**

    DECLARE
        CURSOR c_regions IS SELECT region_name, count(*) 
                            FROM regions reg, countries co
                            WHERE reg.region_id = co.region_id
                            GROUP BY region_name
                            HAVING count(*) > 10;

        v_region_name regions.region_name%TYPE;
        v_result NUMBER(5);

    BEGIN
        OPEN c_regions;
            LOOP
                FETCH c_regions into v_region_name, v_result;

                -- Display the results
                DBMS_OUTPUT.PUT_LINE('Region: '||v_region_name||'; Total of its countries: '||v_result||'.');
                
                -- Break loop, when there's no more row to display
                EXIT WHEN c_regions%NOTFOUND;
            END LOOP;
        CLOSE c_regions;
    END;

---

## 3.
Write a PL/SQL block to read through rows in the countries table for all countries in region 5 (South America region). For each selected country, 
display the country_name, national_holiday_date, and national_holiday_name. 
Use a record structure to hold all the columns selected from the countries table.

    DECLARE
        --Assign content to cursor.
        CURSOR c_countries IS 
            SELECT country_name, national_holiday_date, national_holiday_name
            FROM countries WHERE region_id = 5 
            AND national_holiday_date IS NOT NULL;
        
        --Record structures.
        v_country_record c_countries%ROWTYPE;

    BEGIN
        OPEN c_countries;
            LOOP

                FETCH c_countries INTO v_country_name,
                                       v_holiday_date,
                                       v_holiday_name;

                --Display results.
                DBMS_OUTPUT.PUT_LINE('In '||v_country_name||' the '||v_holiday_name||' is held on '||v_holiday_date||'.');
                EXIT WHEN c_countries%NOTFOUND;

            END LOOP;
        CLOSE c_countries;
    END;

---

## 4.
For this exercise, you use the employees table. 
Create a PL/SQL block that fetches and displays the six employees with the highest salary. 
For each of these employees, display the first name, last name, job id, and salary. 
Order your output so that the employee with the highest salary is displayed first. 
Use %ROWTYPE and the explicit cursor attribute %ROWCOUNT.


    DECLARE
        CURSOR c_emps IS SELECT first_name, last_name, job_id, salary
                         FROM employees ORDER BY salary DESC; -- Flip the salary column to descend.
        
        v_emp_record c_emps%ROWTYPE;

    BEGIN
        OPEN c_emps; 
            LOOP
                FETCH c_emps into v_emp_record;
                EXIT WHEN c_emps%ROWCOUNT > 6 OR c_emps%NOTFOUND; --Iterate until only 6 rows.
                DBMS_OUTPUT.PUT_LINE(v_emp_record.first_name||';'||v_emp_record.last_name||';'||v_emp_record.job_id||';'||v_emp_record.salary); 
            END LOOP;
        CLOSE c_emps; 
    END;

---

## 5.
In real life we would not know how many rows the table contained. 
Modify your last block so that it will exit from the loop when either 41 rows have been fetched and displayed, 
or when there are no more rows to fetch. 
Test the block again.

    DECLARE
        CURSOR c_emps IS SELECT first_name, last_name, job_id, salary
                         FROM employees ORDER BY salary DESC; -- Flip the salary column to descend.

        v_emp_record c_emps%ROWTYPE;

    BEGIN
        OPEN c_emps;
            LOOP
                FETCH c_emps into v_emp_record;
                EXIT WHEN c_emps%ROWCOUNT > 41 OR c_emps%NOTFOUND; --Iterate until only 6 rows.
                DBMS_OUTPUT.PUT_LINE(v_emp_record.first_name||';'||v_emp_record.last_name||';'||v_emp_record.job_id||';'||v_emp_record.salary);
            END LOOP;
        CLOSE c_emps;
    END;

---

## 6.
Modify the following PL/SQL block so that it uses a cursor FOR loop. Keep the explicit cursor declaration in the DECLARE section. 
Test your changes.

    DECLARE

        CURSOR countries_cur IS

            SELECT country_name, national_holiday_name, national_holiday_date
            FROM countries
            WHERE region_id = 5;

        countries_rec countries_cur%ROWTYPE;

    BEGIN

        OPEN countries_cur;
        LOOP

            FETCH countries_cur INTO countries_rec;

            EXIT WHEN countries_cur%NOTFOUND;

            DBMS_OUTPUT.PUT_LINE ('Country: ' || countries_rec.country_name || ' National holiday: '|| countries_rec.national_holiday_name
            || ', held on: '|| countries_rec.national_holiday_date);

        END LOOP;

        CLOSE countries_cur;
    END;

---

    DECLARE

        CURSOR countries_cur IS SELECT country_name, national_holiday_name, national_holiday_date
                                FROM countries
                                WHERE region_id = 5;

    countries_rec countries_cur%ROWTYPE;

    BEGIN
        FOR country in countries_cur
            LOOP

                DBMS_OUTPUT.PUT_LINE ('Country: ' || country.country_name || ' National holiday: '|| country.national_holiday_name
                || ', held on: '|| country.national_holiday_date);

            END LOOP;
    END;

---

## 7.
Modify your answer to previous question to declare the cursor 
using a subquery in the FOR…LOOP statement, rather 
than in the declaration section. 
Test your changes again


    BEGIN
        FOR country IN (SELECT country_name, national_holiday_name, national_holiday_date
                       FROM countries WHERE region_id = 5)

            LOOP

                DBMS_OUTPUT.PUT_LINE ('Country: ' || country.country_name || ' National holiday: '|| country.national_holiday_name
                || ', held on: '|| country.national_holiday_date);

            END LOOP;
    END;

---

## 8.
Using the COUNTRIES table, write a cursor that returns countries 
with a highest_elevation greater than 8,000 m. 
For each country, display the country_name, highest_elevation, and climate. 
Use a cursor FOR loop, declaring the cursor using a subquery in the FOR…LOOP statement.

    BEGIN
        FOR country IN (SELECT country_name, highest_elevation, climate
                        FROM countries WHERE highest_elevation > 8000)
            LOOP
                DBMS_OUTPUT.PUT_LINE(country.country_name||';'||country.highest_elevation||';'||country.climate);
            END LOOP;
    END;

---

## 9.
Write a PL/SQL block to display the country name and the area of each country 
in a chosen region. The region_id should be passed to the cursor as a parameter. 
Test your block using two region_ids: 5 (South America) and 30 (Eastern Asia). 
Do not use a cursor FOR loop.

    DECLARE
        CURSOR c_country(p_region_id NUMBER) IS SELECT country_name, area
                                                   FROM countries WHERE region_id = p_region_id;

        v_country_record c_country%ROWTYPE;

    BEGIN
        OPEN c_country (5);
            LOOP
                FETCH c_country INTO v_country_record;
                EXIT WHEN c_country%NOTFOUND;
                DBMS_OUTPUT.PUT_LINE(v_country_record.country_name||';'||v_country_record.area);
            END LOOP;
        CLOSE c_country;
    END;

---

## 10.
Modify your answer to previous question to use a cursor FOR loop. 
You must still declare the cursor explicitly in the DECLARE section. 
Test it again using regions 5 and 30.

    DECLARE
        CURSOR c_country(p_region_id NUMBER) IS SELECT country_name, area
                                                   FROM countries WHERE region_id = p_region_id;

        v_country_record c_country%ROWTYPE;

    BEGIN
        FOR country IN c_country (5)
            LOOP
                DBMS_OUTPUT.PUT_LINE(country.country_name||';'||country.area);
            END LOOP;
    END;

---

## 11.
Modify your answer to previous question to display the country_name 
and area of each country in a chosen region that has an area greater than a specific value. 
The region_id and specific area should be passed to the cursor as two parameters. 
Test your block twice using region_id 5 (South America): 
the first time with area = 200000 and the second time with area = 1000000.

    DECLARE
        CURSOR c_country(p_region_id NUMBER, p_area NUMBER) IS SELECT country_name, area
                                                   FROM countries WHERE region_id = p_region_id
                                                   AND area > p_area;

        v_country_record c_country%ROWTYPE;

    BEGIN
        FOR country IN c_country (5, 200000)
            LOOP
                DBMS_OUTPUT.PUT_LINE(country.country_name||';'||country.area);
            END LOOP;
    END;

---

## 12.
Write a PL/SQL block that inserts a row into PROPOSED_RAISES for each eligible employee. 
The eligible employees are those whose salary is below a chosen value. 
The salary value is passed as a parameter to the cursor. 
For each eligible employee, insert a row into PROPOSED_RAISES with date_proposed = today’s date, date_appoved null, 
and proposed_new_salary 5% greater than the current salary. 
The cursor should LOCK the employees rows so that no one can modify the employee data while the cursor is open. 
Test your code using a chosen salary value of 5000.

    DECLARE
        CURSOR c_proposes (p_salary NUMBER) IS SELECT employee_id, department_id, salary FROM employees
                                        WHERE salary < p_salary FOR UPDATE NOWAIT;

        v_proposes_record c_proposes%ROWTYPE;
        v_new_salary NUMBER(5);
    BEGIN
        OPEN c_proposes (5000);
            LOOP
                FETCH c_proposes INTO v_proposes_record;
                EXIT WHEN c_proposes%NOTFOUND;

                -- Calculate the new salary proposal for each employee.
                v_new_salary := ROUND(v_proposes_record.salary * 1.05, 2);

                -- Insert each eligible employee datas into the corresp. table.
                INSERT INTO proposed_raises
                VALUES(SYSDATE, '', v_proposes_record.employee_id, v_proposes_record.department_id, v_proposes_record.salary,v_new_salary);

            END LOOP;
        CLOSE c_proposes;
    END;

---

## 13.
Imagine these proposed salary increases have been approved by company management.
Write and execute a PL/SQL block to read each row from the PROPOSED_RAISES table. 
For each row, UPDATE the date_approved column with today’s date. 
Use the WHERE CURRENT OF... syntax to UPDATE each row. 
After running your code, SELECT from the PROPOSED_RAISES table to view the updated data.

    DECLARE
       CURSOR c_proposes IS SELECT * FROM proposed_raises
                            FOR UPDATE OF date_approved NOWAIT;
    BEGIN
        FOR prop IN c_proposes
            LOOP
                UPDATE proposed_raises
                SET date_approved = SYSDATE
                WHERE CURRENT OF c_proposes;
            END LOOP;
    END;

---

## 14.
Management has now decided that employees in department 50 cannot have a salary increase after all. 
Modify your code from last question to DELETE employees in department 50 from PROPOSED_RAISES. 
This could be done by a simple DML statement (DELETE FROM proposed_raises WHERE department_id = 50;), 
but we want to do it using a FOR UPDATE cursor. 
Test your code, and view the PROPOSED_RAISES table again to check that the rows have been deleted.

    DECLARE
       CURSOR c_proposes IS SELECT * FROM proposed_raises
                            WHERE department_id = 50
                            FOR UPDATE OF department_id NOWAIT;
    BEGIN
        FOR prop IN c_proposes
            LOOP
                DELETE FROM proposed_raises
                WHERE CURRENT OF c_proposes;
            END LOOP;
    END;

---

## 15.
Write and run a PL/SQL block which produces a listing of departments and their employees. 
Use the DEPARTMENTS and EMPLOYEES tables. 
In a cursor FOR loop, retrieve and display the department_id and department_name for each department, 
and display a second line containing ‘----------‘ as a separator. 

In a nested cursor FOR loop, retrieve and display:
    - the first_name
    - last_name
    - and salary of each employee in that department

followed by a blank line at the end of each department. 

Order the departments by department_id, and the employees in each department by last_name.
You will need to declare two cursors, one to fetch and display the departments, the second to fetch 
and display the employees in that department, passing the department_id as a parameter.



Your output should look something like this (only the first few departments are shown):

10 Administration

-----------------------------

Jennifer Whalen 4400


20 Marketing

-----------------------------

Pat Fay 6000

Michael Hartstein 13000


50 Shipping

-----------------------------

Curtis Davies 3400

Randall Matos 2600

Kevin Mourgos 5800

Trenna Rajs 3500

Peter Vargas 2500


    DECLARE
        CURSOR c_deps IS SELECT department_id, department_name
                         FROM departments;

        CURSOR c_emps(v_dep_id NUMBER) IS SELECT first_name, last_name, salary
                         FROM employees WHERE department_id = v_dep_id;
    BEGIN
        FOR dep IN c_deps
            LOOP
                DBMS_OUTPUT.PUT_LINE(dep.department_id||' '||dep.department_name);
                DBMS_OUTPUT.NEW_LINE;
                DBMS_OUTPUT.PUT_LINE('----------------------------');

                FOR emp IN c_emps (dep.department_id)
                    LOOP
                        DBMS_OUTPUT.NEW_LINE;
                        DBMS_OUTPUT.PUT_LINE(emp.first_name||' '||emp.last_name||' '||emp.salary);
                    END LOOP;
                DBMS_OUTPUT.NEW_LINE;
                DBMS_OUTPUT.NEW_LINE;
            END LOOP;
    END;

---

## 16.
**Identify the vocabulary word for each definition below**

Declares a record with the same fields as the cursor on which it is based.
	
    Resposta 1 --> %ROWTYPE

An attribute used to determine whether the most recent FETCH statement successfully returned a row.
	
    Resposta 2 --> %NOTFOUND

A composite data type in PL/SQL, consisting of a number of fields each with their own name and data type.
	
    Resposta 3 --> Record

Returns the status of the cursor.
	
    Resposta 4 --> %ISOPEN

An attribute that processes an exact number of rows or counts the number of rows fetched in a loop.

    Resposta 5 --> %ROWCOUNT

