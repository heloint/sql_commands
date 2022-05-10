# 1.
Create, save, and execute a procedure which updates the salary of employees in employees_dup according to the following rules: 

 - if the employee is in department 80, the new salary = 1000 

- if the employee is in department 50, the new salary = 2000

- if the employee is in any other department, the new salary = 3000. 

You will need to include three UPDATE statements, one for each of the above rules. In a later lesson you will learn how to avoid this. Execute your procedure from an anonymous block and verify that the updates have been performed correctly.

    CREATE TABLE employees_dup AS (SELECT * FROM employees);


    CREATE OR REPLACE PROCEDURE update_salary IS
    BEGIN
        UPDATE employees_dup
        SET salary = 1000
        WHERE department_id = 80;
        
        UPDATE employees_dup
        SET salary = 2000
        WHERE department_id = 50;
        
        UPDATE employees_dup
        SET salary = 3000
        WHERE department_id NOT IN (80, 50);
    END;
---

# 2.
Create a procedure that accepts a country_id as a parameter and displays the name of the country and its capitol city. 
Name your procedure get_country_info. Save your procedure definition for later use.

    CREATE OR REPLACE PROCEDURE get_country (p_country_id IN countries.country_id%TYPE)
    IS
        v_c_name countries.country_name%TYPE;
        v_capitol countries.capitol%TYPE;
    BEGIN
        Select country_name, capitol
        Into v_c_name, v_capitol
        From countries
        Where country_id = p_country_id;
        DBMS_OUTPUT.PUT_LINE('The country name is '||v_c_name||' and the capital city is '||v_capitol);
    END;
---

# 3.
Retrieve your procedure get_country_info and modify it to trap the NO_DATA_FOUND exception in an exception handler. 
Execute the modified procedure using country_id 95. 
What happens?


    CREATE OR REPLACE PROCEDURE get_country_exception (p_country_id IN countries.country_id%TYPE)
    IS
        v_c_name countries.country_name%TYPE;
        v_capitol countries.capitol%TYPE;
    BEGIN
        Select country_name, capitol
        Into v_c_name, v_capitol
        From countries
        Where country_id = p_country_id;
        DBMS_OUTPUT.PUT_LINE('The country name is '||v_c_name||' and the capital city is '||v_capitol);

        EXCEPTION
            WHEN NO_DATA_FOUND THEN
            DBMS_OUTPUT.PUT_LINE('Country does not exist.');
    END;

---

# 4.
Write a procedure named get_country_count that displays the number of countries in a given region whose highest elevations exceed a given value. 
The procedure should accept two formal parameters, one for a region_id and the other for an elevation value for comparison. 
Use DBMS_OUTPUT.PUT_LINE to display the results in a message.


    CREATE OR REPLACE PROCEDURE get_country_count (p_reg_id countries.region_id%TYPE, p_elev countries.highest_elevation%TYPE)
    IS
        v_count NUMBER(4);
    BEGIN
        SELECT COUNT(*) INTO v_count
        FROM countries
        WHERE region_id = p_reg_id
        AND highest_elevation > p_elev;
        DBMS_OUTPUT.PUT_LINE(p_reg_id||' has'||v_count||' countries higher than '||p_elev);
    End;

---

# 5.
Retrieve your procedure get_country_count and modify it to accept a third formal parameter of datatype CHAR. 
The procedure should display a count of the number of countries in a given region whose highest elevations exceed a given value and whose country name starts with a given alphabetic character. 
Your SELECT statement should include a WHERE condition to compare the first character of each country’s name with the third parameter value (Hint: use SUBSTR). 

    CREATE OR REPLACE PROCEDURE get_country_count_char (p_reg_id countries.region_id%TYPE, p_elev countries.highest_elevation%TYPE, p_character CHAR)
    IS
        v_count NUMBER(4);
    BEGIN
        SELECT COUNT(*) INTO v_count
        FROM countries
        WHERE region_id = p_reg_id
        AND highest_elevation > p_elev
        AND SUBSTR(country_name,1,1) = p_character;
        DBMS_OUTPUT.PUT_LINE(p_region_id||' starts with '||p_character||'character and has'||v_count||' countries higher than '||p_elevation);
    End;

---

# 6.

Test get_country_count procedure in an an anonymous block 
(declare three variables to store actual parameter values and then executes the procedure passing these values).

Execute the block using values 5, 2000, and ‘B’.

    DECLARE
        v_reg_id NUMBER(3) := 5;
        v_elev NUMBER(6) := 2000;
        v_char CHAR := 'B';
    BEGIN
        get_country_count_char(v_reg_id, v_elev, v_char);
    END;

---

# 7.

Create a procedure that receives a country_id as an IN parameter and returns the name and population of that country as OUT parameters. 
Include an exception handler to trap the NO_DATA_FOUND exception if the country does not exist. 
The procedure should not display the returned values; this will be done in the next step. 
Name your procedure find_area_pop. 

    DECLARE
        v_c_id employees.employee_id%TYPE;
        v_c_name countries.country_name%TYPE;
        v_c_pop countries.population%TYPE;
    BEGIN
        v_c_id := 591;
        find_area_pop(v_c_id, v_c_name, v_c_pop);
        DBMS_OUTPUT.PUT_LINE(v_c_id||' '||v_c_name||' '||v_c_pop);
    END;

---

# 8.

Modify your procedure find_area_pop to add a third OUT parameter which is the population density of the country, using the formula: density = (population / area). 
You will need to modify your SELECT statement to fetch the area column value into a local variable.


    CREATE OR REPLACE PROCEDURE find_area_pop (p_c_id IN countries.country_id%TYPE,
                                               p_c_name OUT countries.country_name%TYPE,
                                               p_population OUT countries.population%TYPE,
                                               p_density OUT NUMBER)
    IS
    v_area countries.area%TYPE;

    BEGIN
        SELECT country_name, population, area INTO
        p_c_name, p_population, v_area
        FROM countries
        WHERE country_id = p_c_id;

        SELECT (p_population / v_area) INTO p_density FROM dual;

    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            DBMS_OUTPUT.PUT_LINE('No registers of: '||p_c_id);
    END;


    DECLARE
        v_c_id employees.employee_id%TYPE;
        v_c_name countries.country_name%TYPE;
        v_c_pop countries.population%TYPE;

        v_density NUMBER;
    BEGIN
        v_c_id := 591;
        find_area_pop(v_c_id, v_c_name, v_c_pop, v_density);
        DBMS_OUTPUT.PUT_LINE(v_c_id||' '||v_c_name||' '||v_c_pop||' with density of: '||v_density);
    END;

---

# 9.

Create a procedure which accepts an integer as an IN OUT parameter and returns the square of that integer, for example the square of 4 is 16. Save your code. 
Test your procedure from an anonymous block three times, using integer values 4, 7, and –20 (negative 20).


    CREATE OR REPLACE PROCEDURE square_number(p_number IN OUT NUMBER)  
    AS  
    BEGIN  
        p_number  := p_number * p_number;  
          
        EXCEPTION  
            WHEN OTHERS THEN  
                DBMS_OUTPUT.PUT_LINE('ERROR has been raised.');  
    END;  
 

    DECLARE  
        v_number NUMBER := 4;
    BEGIN   
        square_number(v_number);  
        DBMS_OUTPUT.PUT_LINE('Square of input number is: '||v_number);
    END;


    DECLARE  
        v_number NUMBER := -7;
    BEGIN   
        square_number(v_number);  
        DBMS_OUTPUT.PUT_LINE('Square of input number is: '||v_number);
    END;


    DECLARE  
        v_number NUMBER := -20;
    BEGIN   
        square_number(v_number);  
        DBMS_OUTPUT.PUT_LINE('Square of input number is: '||v_number);
    END; 

---

# 10.

Test find_area_pop procedure passing the four parameters using named notation. 
Test your block using country_id 2 (Canada).


    CREATE OR REPLACE PROCEDURE find_area_pop (p_c_id IN countries.country_id%TYPE,
                                               p_c_name OUT countries.country_name%TYPE,
                                               p_population OUT countries.population%TYPE,
                                               p_density OUT NUMBER)
    IS
    v_area countries.area%TYPE;

    BEGIN
        SELECT country_name, population, area INTO
        p_c_name, p_population, v_area
        FROM countries
        WHERE country_id = p_c_id;

        SELECT (p_population / v_area) INTO p_density FROM dual;

    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            DBMS_OUTPUT.PUT_LINE('No registers of: '||p_c_id);
    END;


    DECLARE
        v_c_id employees.employee_id%TYPE;
        v_c_name countries.country_name%TYPE;
        v_c_pop countries.population%TYPE;
        v_density NUMBER(10,2)
    BEGIN
        v_c_id := 2;
        find_area_pop(p_c_id => v_c_id, p_c_name  => v_c_name, p_population => v_c_pop,p_density => v_density);
        DBMS_OUTPUT.PUT_LINE(v_c_id||' '||v_c_name||' '||v_c_pop||' with density of: '||p_density);
    END;

---
