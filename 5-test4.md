## 1.Write a PL/SQL block to find the population of a given country in the countries table. 
---

Display a message indicating whether the population is greater than or less than 1 billion (1,000,000,000). 
Test your block twice using India (country_id = 91) and United Kingdom (country_id = 44). 
India’s population should be greater than 1 billion, while United Kingdom’s should be less than 1 billion.

Use bind variables to inform country_id.

DECLARE

    v_country_id NUMBER(3):= 91; --ID of Rep. of India
    v_country_population NUMBER(12);
    v_country_name VARCHAR2(25);
    v_result VARCHAR2(120);

BEGIN
    -- Query the name and num. of population of the given country.
    SELECT country_name,population INTO v_country_name, v_country_population
    FROM countries WHERE country_id = v_country_id;
    
    -- Assigned condition: Whether the population is more or less than a billion.
    CASE
        WHEN v_country_population > 1000000000 THEN v_result := 'The population of '||v_country_name||' is more than a billion.';
        WHEN v_country_population < 1000000000 THEN v_result := 'The population of '||v_country_name||' is less than a billion.';
    END CASE;
    
    DBMS_OUTPUT.PUT_LINE(v_result); --Display result.
    
END;

DECLARE

    v_country_id NUMBER(3):= 44; --ID of United Kingdom and Ireland
    v_country_population NUMBER(20);
    v_country_name VARCHAR2(60);
    v_result VARCHAR2(170);

BEGIN
    -- Query the name and num. of population of the given country.
    SELECT country_name,population INTO v_country_name, v_country_population
    FROM countries WHERE country_id = v_country_id;
    
    -- Assigned condition: Whether the population is more or less than a billion.
    CASE
        WHEN v_country_population > 1000000000 THEN v_result := 'The population of '||v_country_name||' is more than a billion.';
        WHEN v_country_population < 1000000000 THEN v_result := 'The population of '||v_country_name||' is less than a billion.';
    END CASE;
    
    DBMS_OUTPUT.PUT_LINE(v_result); --Display result.
    
END;


## 2. Modify the code from the previous exercise so that it handles all the following cases:
---

A. Population is greater than 1 billion.

B. Population is greater than 0.

C. Population is 0.

D. Population is null. (Display: No data for this country.)

Run your code using the following country ids. Confirm the indicated results.

• China (country_id = 86): Population is greater than 1 billion.

• United Kingdom (country_id = 44): Population is greater than 0.

• Antarctica (country_id = 672): Population is 0.

• Europa Island (country_id = 15): No data for this country.

DECLARE

    v_country_id NUMBER(3):= 86; --ID of China
    v_country_population NUMBER(20);
    v_country_name VARCHAR2(60);
    v_result VARCHAR2(170);

BEGIN
    -- Query the name and num. of population of the given country.
    SELECT country_name,population INTO v_country_name, v_country_population
    FROM countries WHERE country_id = v_country_id;
    
    -- Assigned condition: Whether the population is more or less than a billion, is 0 or is null.
    CASE
        WHEN v_country_population > 1000000000 THEN v_result := 'The population of '||v_country_name||' is more than a billion.';
        WHEN v_country_population < 1000000000 AND v_country_population > 0 THEN v_result := 'The population of '||v_country_name||' is less than a billion.';
        WHEN v_country_population = 0 THEN v_result:= 'The population of '||v_country_name||' is 0.';
        WHEN v_country_population IS NULL THEN v_result:= 'The result is null.';
    END CASE;
    
    DBMS_OUTPUT.PUT_LINE(v_result); --Display result.
    
END;


DECLARE

    v_country_id NUMBER(3):= 44; --ID of United Kingdom and Ireland
    v_country_population NUMBER(20);
    v_country_name VARCHAR2(60);
    v_result VARCHAR2(170);

BEGIN
    -- Query the name and num. of population of the given country.
    SELECT country_name,population INTO v_country_name, v_country_population
    FROM countries WHERE country_id = v_country_id;
    
    -- Assigned condition: Whether the population is more or less than a billion, is 0 or is null.
    CASE
        WHEN v_country_population > 1000000000 THEN v_result := 'The population of '||v_country_name||' is more than a billion.';
        WHEN v_country_population < 1000000000 AND v_country_population > 0 THEN v_result := 'The population of '||v_country_name||' is less than a billion.';
        WHEN v_country_population = 0 THEN v_result:= 'The population of '||v_country_name||' is 0.';
        WHEN v_country_population IS NULL THEN v_result:= 'The result is null.';
    END CASE;
    
    DBMS_OUTPUT.PUT_LINE(v_result); --Display result.
    
END;


DECLARE

    v_country_id NUMBER(3):= 672; --ID of Antarctica
    v_country_population NUMBER(20);
    v_country_name VARCHAR2(60);
    v_result VARCHAR2(170);

BEGIN
    -- Query the name and num. of population of the given country.
    SELECT country_name,population INTO v_country_name, v_country_population
    FROM countries WHERE country_id = v_country_id;
    
    -- Assigned condition: Whether the population is more or less than a billion, is 0 or is null.
    CASE
        WHEN v_country_population > 1000000000 THEN v_result := 'The population of '||v_country_name||' is more than a billion.';
        WHEN v_country_population < 1000000000 AND v_country_population > 0 THEN v_result := 'The population of '||v_country_name||' is less than a billion.';
        WHEN v_country_population = 0 THEN v_result:= 'The population of '||v_country_name||' is 0.';
        WHEN v_country_population IS NULL THEN v_result:= 'The result is null.';
    END CASE;
    
    DBMS_OUTPUT.PUT_LINE(v_result); --Display result.
    
END;


DECLARE

    v_country_id NUMBER(3):= 15; --ID of Europa Island
    v_country_population NUMBER(20);
    v_country_name VARCHAR2(60);
    v_result VARCHAR2(170);

BEGIN
    -- Query the name and num. of population of the given country.
    SELECT country_name,population INTO v_country_name, v_country_population
    FROM countries WHERE country_id = v_country_id;
    
    -- Assigned condition: Whether the population is more or less than a billion, is 0 or is null.
    CASE
        WHEN v_country_population > 1000000000 THEN v_result := 'The population of '||v_country_name||' is more than a billion.';
        WHEN v_country_population < 1000000000 AND v_country_population > 0 THEN v_result := 'The population of '||v_country_name||' is less than a billion.';
        WHEN v_country_population = 0 THEN v_result:= 'The population of '||v_country_name||' is 0.';
        WHEN v_country_population IS NULL THEN v_result:= 'The result is null.';
    END CASE;
    
    DBMS_OUTPUT.PUT_LINE(v_result); --Display result.
    
END;

## 3. Examine the following code. What output do you think it will produce?
---

DECLARE

v_num1 NUMBER(3) := 123;

v_num2 NUMBER;

BEGIN

IF v_num1 <> v_num2 THEN

DBMS_OUTPUT.PUT_LINE('The two numbers are not equal');

ELSE

DBMS_OUTPUT.PUT_LINE('The two numbers are equal');

END IF;

END;

b.
The two numbers are equal


## 4. Write a PL/SQL block to accept a year and check whether it is a leap year. For example, if the year entered is 1990, the output should be “1990 is not a leap year.”
---

Hint: A leap year should be exactly divisible by 4, but not exactly divisible by 100. However, any year exactly divisible by 400 is a leap year.

Test your solution with the following years:

    1990 Not a leap year
    2000 Leap year
    1996 Leap year
    1900 Not a leap year
    2016 Leap year
    1884 Leap year
    2020 Leap year

DECLARE
    v_selected_year NUMBER(4) := :year; --Get input
    v_div_hundred NUMBER(5);
    v_div_four NUMBER(5);    
    v_div_four_hundred NUMBER(5);
BEGIN

    v_div_hundred := REMAINDER(v_selected_year,100); --Remainder of the division of 100.
    v_div_four  := REMAINDER(v_selected_year,4);  --Remainder of the division of 4.    
    v_div_four_hundred := REMAINDER(v_selected_year,400);  --Remainder of the division of 400.

    IF v_div_hundred > 0 OR v_div_four_hundred = 0 AND v_div_four = 0 THEN
        DBMS_OUTPUT.PUT_LINE('Is leap.');

    ELSE
        DBMS_OUTPUT.PUT_LINE('Is not leap.');

    END IF;
    
END;


## 5. Write a PL/SQL block to find the number of airports from the countries table for a supplied country_name. Based on this number, display a customized message as follows:
---

Airports 	Message
0–100 	There are 100 or fewer airports.
101–1,000 	There are between 101 and 1,000 airports.
1001–1,0000 	There are between 1,001 and 10,000 airports.
> 10,000 	There are more than 10,000 airports.
No value in database 	The number of airports is not available for this country

Use a CASE statement to process your comparisons.
Use a binding variable to inform country_name value.
Test your code for the following countries and confirm the results.

	No value 	< 101 	101-1,000 	1,001-10,000 	> 10,000
Canada 				X 	
Japan 			X 		
Mongolia 		X 			
Navassa Island 	X 				
United States of America 					X


DECLARE
    v_result VARCHAR2(100);
    v_number_airports NUMBER(5);
    v_country_name VARCHAR2(100) := :Country; -- Get input.
BEGIN
    v_country_name := lower(v_country_name); -- Convert the input for lower case and in the assignment WHERE as well convert to lower case
                                             -- Like that it's case insensitive the query.

    SELECT airports INTO v_number_airports FROM countries
    WHERE lower(country_name) = v_country_name;
    
    CASE
        WHEN v_number_airports <= 100 THEN v_result := 'There are 100 or fewer airports.';
        WHEN v_number_airports > 100 AND v_number_airports <= 1000 THEN v_result := 'There are between 101 and 1000 airports.';
        WHEN v_number_airports > 1000 AND v_number_airports <= 10000 THEN v_result := 'There are 1001 and 10000 airports.';
        WHEN v_number_airports > 10000 THEN v_result := 'There are more than 10000 airports.';
        WHEN v_number_airports IS NULL THEN v_result := 'The number of airports is not available for this country.';
    END CASE;

    DBMS_OUTPUT.PUT_LINE(v_result);
    
END;

## 6. Write a PL/SQL block to find the amount of coastline for a supplied country name. Use the countries table. Based on the amount of coastline for the country, display a customized message as stated in following table. Use a CASE expression.
---

As usual, use a vbinding variable to input country name.

Length of Coastline 	Message
0 	no coastline
< 1,000 	a small coastline
< 10,000 	a mid-range coastline
All other values 	a large coastline

Test your code for the following countries and confirm the results.

	No
coastline 	Small
coastline 	Mid-range
coastline 	Large
coastline
Canada 				X
Grenada 		X 		
Jamaica 			X 	
Mongolia 	X 			


DECLARE

    v_country_name VARCHAR2(100) := :Country; --Get input.
    v_result VARCHAR2(50);
    v_coastline_number NUMBER(8);

BEGIN

    v_country_name := lower(v_country_name); -- Make case insensitive the input.

    SELECT coastline INTO v_coastline_number FROM countries
    WHERE lower(country_name) = v_country_name;  -- Make case insensitive the input.
    
    v_result :=
        CASE
            WHEN v_coastline_number = 0 THEN 'no coastline'
            WHEN v_coastline_number < 1000 THEN 'a small coastline'
            WHEN v_coastline_number < 10000 and v_coastline_number >= 1000 THEN 'a mid-range coastline'
            ELSE 'a large coastline'
        END;

    DBMS_OUTPUT.PUT_LINE(v_result); -- Display v_result.

END;


## 7. Select the answer.
---

TRUE AND NULL --> Null
	
TRUE OR NULL --> TRUE
	
TRUE AND TRUE --> TRUE

NULL OR NULL --> NULL

NOT FALSE --> TRUE

NOT NULL --> NULL

FALSE OR NULL --> NULL

TRUE AND FALSE --> FALSE

FALSE AND NULL --> FALSE
	

## 8.  Write a PL/SQL block to display the country_id and country_name values from the COUNTRIES table for country_id whose values range from 1 through 3. Use a basic loop. Increment a variable from 1 through 3. Use an IF statement to test your variable and EXIT the loop after you have displayed the first 3 countries
---

DECLARE
    v_country_id NUMBER(3);
    v_country_name VARCHAR2(50);
    v_counter NUMBER(1) := 1;
BEGIN
    LOOP
        /* Quering the id and name for each iteration where country_id equal
           to the value of v_counter.*/
        
        SELECT country_id, country_name INTO v_country_id,v_country_name
        FROM countries WHERE country_id = v_counter;

        DBMS_OUTPUT.PUT_LINE(v_country_id||' '||v_country_name);
        
        v_counter := v_counter + 1;

        -- Increase the iteration counter variable if condition is met.
        IF v_counter > 3 THEN EXIT;
        END IF;

    END LOOP;
END;

## 9. Write a PL/SQL block to display the country_id and country_name values from the COUNTRIES table for country_id whose values range from 1 through 3. Use a basic loop. Increment a variable from 1 through 3. Use an EXIT....WHEN statement to finalize the loop.
---

DECLARE
    v_country_id NUMBER(3);
    v_country_name VARCHAR2(50);
    v_counter NUMBER(1) := 1;
BEGIN
    LOOP
        /* Quering the id and name for each iteration where country_id equal
           to the value of v_counter.*/

        SELECT country_id, country_name INTO v_country_id,v_country_name
        FROM countries WHERE country_id = v_counter;

        DBMS_OUTPUT.PUT_LINE(v_country_id||' '||v_country_name);
        
        -- Increase the iteration counter variable if condition is met.
        v_counter := v_counter + 1;

        -- When v_counter hits 3, loop breaks.
        EXIT WHEN v_counter > 3;

    END LOOP;
END;

## 10. Create a MESSAGES table and insert several rows into it. 
---

To create the messages table execute:

DROP TABLE messages;

CREATE TABLE messages (results NUMBER(2));

Following, write a PL/SQL block to insert numbers into the MESSAGES table. Insert the numbers 1 through 10, excluding 6 and 8.

BEGIN
    FOR num in 1..10
    LOOP
        IF num != 6 AND num != 8 THEN
            DBMS_OUTPUT.PUT_LINE(num);
        END IF;
    END LOOP;
END;

## 11. Write a PL/SQL block to display the country_id and country_name values from the COUNTRIES table for country_id whose values range from 51 through 55. Use a WHILE loop. Increment a variable from 51 through 55. Test your variable to see when it reaches 55. EXIT the loop after you have displayed the 5 countries
---

DECLARE
    v_country_id NUMBER(3);
    v_country_name VARCHAR2(50);
    v_counter NUMBER(3) := 51;
BEGIN
    WHILE v_counter <= 55 LOOP

        SELECT country_id, country_name INTO v_country_id, v_country_name
        FROM countries WHERE country_id = v_counter;
        
        DBMS_OUTPUT.PUT_LINE(v_country_id||' '||v_country_name);

        v_counter := v_counter + 1;
        
    END LOOP;
END;

## 12. Write a PL/SQL block to display the country_id and country_name values from the COUNTRIES table for country_id whose values range from 51 through 55 in the reverse order. Use a FOR loop.
---

DECLARE
    v_country_id NUMBER(3);
    v_country_name VARCHAR2(50);
BEGIN
    FOR num IN REVERSE 51..55
    LOOP
        SELECT country_id, country_name INTO v_country_id, v_country_name
        FROM countries WHERE country_id = num;

        DBMS_OUTPUT.PUT_LINE(v_country_id||' '||v_country_name);

    END LOOP;
END;

## 13. Execute the following statements to build a new_emps table.
---

DROP TABLE new_emps;

CREATE TABLE new_emps AS SELECT * FROM employees;

ALTER TABLE new_emps ADD stars VARCHAR2(50);


Create a PL/SQL block that inserts an asterisk in the stars column for every whole $1,000 of an employee’s salary. For example, if an employee has salary of $7,800, the string “*******” would be inserted, and, if an employee has salary of $3,100, the string “***” would be inserted. 

DECLARE
    CURSOR c_new_emps IS SELECT * FROM new_emps;
    v_result VARCHAR2(50);
    v_counter NUMBER(5);
BEGIN
    
    FOR emp IN c_new_emps
    LOOP
    
        v_counter := ROUND((emp.salary / 1000),0) + 1;
        
        v_result := LPAD(' ',v_counter,'*');

        UPDATE new_emps
        SET stars = v_result
        WHERE employee_id=emp.employee_id;

    END LOOP;
END;

## 14. Write a code block to print put the prime numbers between 2 and 100. Remember that a prime number is a number that is divisible only by itself and 1. 
---

The program also should show how many prime numbers have been found.

DECLARE
    v_check number(3) := 0;
    v_counter NUMBER(3) := 0;
BEGIN
    DBMS_OUTPUT.PUT_LINE('The prime number from 1 to 100 are: ');
    --Start looping trough the numbers from 1 to 100.
    FOR num IN 1..100

        LOOP

        /*Starts a nested loop for each "num"
          from 2 to num - 1.*/
            FOR divider in 2..(num-1)           
                LOOP

                    /* If "num" divided by "divider" has
                       no remainer, then keep accumulating the "v_check".*/
                    IF mod(num,divider) = 0 then
                        v_check := v_check + 1;
                    END IF;

                END LOOP;

            /*After iterating trough the "num" with all of
              its dividers, if the v_check remains 0,
              that means it's a primer.*/
            IF v_check = 0 then
                dbms_output.put_line('--> '||num);
                v_counter := v_counter + 1;
            END IF;
            -- Restart the v_check for the next "num".
            v_check := 0;

        END LOOP;

    -- Print out the total of prime numbers.
    DBMS_OUTPUT.PUT_LINE('Total number of prime numbers: '||v_counter||'.');
END;

