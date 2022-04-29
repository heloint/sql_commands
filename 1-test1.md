# 1.
Which of the following PL/SQL blocks executes successfully?

1)

BEGIN
END;

Answer: FAIL.

2)

DECLARE

amount INTEGER(10);

END;

Answer: FAIL.

3)

DECLARE BEGIN END;

Answer: FAIL.
	
4)

DECLARE

    amount NUMBER(10);

BEGIN

    DBMS_OUTPUT.PUT_LINE(amount);

END;

ANSWER: SUCCEEDS.

---

# 2.
Create and execute a simple anonymous block that does the following:

- Declares a variable of datatype DATE and populates it with the date that is six months from today
- Outputs “In six months, the date will be: .” 

Use the dual table.

Use the ADD_MONTHS function to perform the calculation

DECLARE
    future_date date;
BEGIN
    SELECT ADD_MONTHS(sysdate,6) INTO future_date
    FROM dual;
    dbms_output.put_line('In six months, the date will be: ' || add_months(sysdate,6) ||'.');
END;

---

# 3.
Examine the following anonymous block and choose the appropriate statement

DECLARE
    fname VARCHAR2(25);
    lname VARCHAR2(25) DEFAULT 'fernandez';
BEGIN
    DBMS_OUTPUT.PUT_LINE(fname || ' ' || lname);
END;


[] The block will execute successfully and print ‘ fernandez’.

[] The block will give an error because the fname variable is used without initializing.

[X] The block will execute successfully and print ‘null fernandez’

[]  The block will give an error because you cannot use the DEFAULT keyword to initialize a variable of the VARCHAR2 type.

---

# 4.
Write an anonymous block that uses a country name as input and prints the highest and lowest elevations for that country.

Use the COUNTRIES table. 

Execute your block three times using United States of America, French Republic, and Japan.


DECLARE
    target_name VARCHAR2(50) := 'United States of America';
    low_elev number;
    high_elev number;
BEGIN
    select lowest_elevation, highest_elevation into
    low_elev, high_elev
    from countries
    where country_name = target_name;
    dbms_output.put_line('Lowest: '||low_elev||'; Highest: '|| high_elev);
END;


DECLARE
    target_name VARCHAR2(50) := 'French Republic';
    low_elev number;
    high_elev number;
BEGIN
    select lowest_elevation, highest_elevation into
    low_elev, high_elev
    from countries
    where country_name = target_name;
    dbms_output.put_line('Lowest: '||low_elev||'; Highest: '|| high_elev);
END;


DECLARE
    target_name VARCHAR2(50) := 'Japan';
    low_elev number;
    high_elev number;
BEGIN
    select lowest_elevation, highest_elevation into
    low_elev, high_elev
    from countries
    where country_name = target_name;
    dbms_output.put_line('Lowest: '||low_elev||'; Highest: '|| high_elev);
END;

---

# 5.
Following code blocks...

1)

BEGIN
END;

ANSWER: FAILS.

2)

DECLARE

    amount INTEGER(10);

END;

ANSWER: FAILS.

3)

DECLARE BEGIN END;


ANSWER: FAILS.

4)
DECLARE

    amount NUMBER(10);

BEGIN

    DBMS_OUTPUT.PUT_LINE(amount);

END;

ANSWER: SUCCEEDS.

---

# 6.
Identify reserved words from list:

- create
- make
- table
- seat
- alter
- rename
- row
- number
- web

ANSWER: create, table, alter, number.

---

# 7.
Evaluate the variables in the following code. Answer the following questions about each variable.
Is it named well? Why or why not? If it is not named well, what would be a better name and why?

DECLARE
    country_name VARCHAR2(50);
    median_age NUMBER(6, 2);
BEGIN
    SELECT country_name, median_age INTO country_name, median_age
    FROM countries
    WHERE country_name = 'Japan';
    DBMS_OUTPUT.PUT_LINE('The median age in '|| country_name || ' is '
    || median_age || '.');
END;

As Oracle syntax it's alright, but should be better not to use the same variable names as the table column names.

---

# 8.
Create a PL/SQL block to select ans show the maximum department id from departments table.

Use as variable to store this value: v_max_deptn.

DECLARE
    v_max_deptn number;
BEGIN
    select max(department_id) into v_max_deptn
    from departments;
    dbms_output.put_line(v_max_deptn);
END;

---

# 9.
The following code is supposed to display the lowest and highest elevations for a country name
entered by the user. However, the code does not work. Fix the code by following the guidelines for
retrieving data that you learned in this lesson.

DECLARE
    v_country_name countries.country_name%TYPE := Federative Republic of Brazil;
    v_lowest_elevation countries.lowest_elevation%TYPE;
    v_highest_elevation countries.highest_elevation%TYPE;
BEGIN
    SELECT lowest_elevation, highest_elevation
    FROM countries;
    DBMS_OUTPUT.PUT_LINE('The lowest elevation in '
    || v_country_name || ' is ' || v_lowest_elevation
    || ' and the highest elevation is ' || v_highest_elevation || '.');
END;

DECLARE
    v_country_name VARCHAR(30) := 'Federative Republic of Brazil';
    v_lowest_elevation NUMBER;
    v_highest_elevation NUMBER;
BEGIN
    SELECT country_name, lowest_elevation, highest_elevation
    INTO v_country_name, v_lowest_elevation, v_highest_elevation
    FROM countries
    WHERE country_name = v_country_name;
    DBMS_OUTPUT.PUT_LINE('The lowest elevation in '|| v_country_name || ' is ' || v_lowest_elevation || ' and the highest elevation is ' || v_highest_elevation || '.');
END;

---

# 10.
Use (but don't execute) the following code to answer this question:

DECLARE
    last_name VARCHAR2(25) := 'Fay';
BEGIN
    UPDATE emp_dup
    SET first_name = 'Jennifer'
    WHERE last_name = last_name;
END;

What do you think would happen if you ran the above code? Write your answer here and then
follow the steps below to test your theory.


ANSWER: 
The code is confusing, because the freshly declared variable name is exactly the same as one of the column name of the table.

Besides that, would say: It's gonna execute.

But if it won't, then the cause is the variable name, which has been referenced before.

---
