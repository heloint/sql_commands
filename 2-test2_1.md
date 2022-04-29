## Pregunta 1
**S'han de fer eleccions sindicals i s'ha de constituir la mesa electoral. Per aquest motiu s'han de seleccionar de tots els empleats, els vocals d'edat que son aquell empleat més veterà i aquell més novell. S'ha de considerar que l'empleat que cobra més i el que cobra menys de l'empresa, tampoc poden ser membre de la mesa electoral.**

**Els empleats estan registrats a la taula employees.**

**Fes un script que obtingui els dos vocals seleccionats i mostri per pantalla:**

- La persona més veterana de l'empresa és:  <nom_cognoms>

- La persona més novella de l'empresa és:  <nom_cognoms>
---

DECLARE
CURSOR c_employees IS SELECT * FROM EMPLOYEES; -- Create the cursor of table of subject
v_max_hdate DATE;
v_min_hdate DATE;

BEGIN
SELECT MAX(hire_date) INTO v_max_hdate FROM EMPLOYEES; -- Import the most recent hire date.
SELECT MIN(hire_date) INTO v_min_hdate FROM EMPLOYEES; -- Import the oldest hire date.
FOR c_emp IN c_employees -- Start parsing through the registers in emp table.
LOOP

IF c_emp.hire_date = v_max_hdate THEN -- Display if the "youngest" condition statement matched
DBMS_OUTPUT.PUT_LINE('The most recent employee in the company is: '||c_emp.first_name||' '||c_emp.last_name);

ELSIF c_emp.hire_date = v_min_hdate THEN -- Display most veteran emp if the condition statement matched
DBMS_OUTPUT.PUT_LINE('The most veteran employee in the company is: '||c_emp.first_name||' '||c_emp.last_name);
END IF;
END LOOP;
END;

---

## Pregunta 2

**Enguany s'han de celebrar diverses eleccions a l'empresa i s'opta per modificar el criteri de selecció de les eprsones que han d'estar a la mesa electoral. Per evitar que sempre hi siguin els mateixos es vol que cada vegada que es selecciona algú per a una mesa es registri aquesta assignació.**

**Modifica l'script anterior per a què l'script de selecció de membres de la mesa en registri la seva selecció.**

CREATE TABLE electoral_register -- Create the register table to save the results
( employee_id NUMBER(4) CONSTRAINT elect_reg_pk PRIMARY KEY,
first_name VARCHAR2(20) NOT NULL,
last_name VARCHAR2(25) NOT NULL,
hire_date DATE ,
department_id NUMBER(4),
electoral_year NUMBER DEFAULT to_number(to_char(sysdate,'yyyy')),
CONSTRAINT elect_reg_fk FOREIGN KEY (employee_id) REFERENCES employees(employee_id)
);
---

DECLARE
CURSOR c_employees IS SELECT * FROM EMPLOYEES; -- Create the cursor of table of subject
v_max_hdate DATE;
v_min_hdate DATE;

BEGIN
SELECT MAX(hire_date) INTO v_max_hdate FROM EMPLOYEES; -- Import the most recent hire date.
SELECT MIN(hire_date) INTO v_min_hdate FROM EMPLOYEES; -- Import the oldest hire date.

FOR c_emp IN c_employees -- Start parsing through the registers in emp table.
LOOP
IF c_emp.hire_date = v_max_hdate THEN -- Display if the "youngest" condition statement matched
DBMS_OUTPUT.PUT_LINE('The most recent employee in the company is: '||c_emp.first_name||' '||c_emp.last_name);

INSERT INTO electoral_register(employee_id,first_name,last_name,hire_date,department_id)
VALUES(c_emp.employee_id, c_emp.first_name, c_emp.last_name, c_emp.hire_date, c_emp.department_id); --Inserts into the electoral_register table the datas of the result emp.

ELSIF c_emp.hire_date = v_min_hdate THEN -- Display most veteran emp if the condition statement matched
DBMS_OUTPUT.PUT_LINE('The most veteran employee in the company is: '||c_emp.first_name||' '||c_emp.last_name);

INSERT INTO electoral_register(employee_id,first_name,last_name,hire_date,department_id)
VALUES(c_emp.employee_id, c_emp.first_name, c_emp.last_name, c_emp.hire_date, c_emp.department_id); --Inserts into the electoral_register table the datas of the result emp.

END IF;
END LOOP;
END;

---

## Pregunta 3

**Modifica l'script de selecció de membres de la mesa per a què descarti aquelles persones que ja han participat en alguna elecció sent membres de la mesa.**

DECLARE
CURSOR c_employees IS SELECT * FROM EMPLOYEES WHERE employee_id NOT IN (SELECT employee_id FROM electoral_register);
v_max_hdate DATE;
v_min_hdate DATE;

BEGIN
SELECT MAX(hire_date) INTO v_max_hdate -- Import the most recent hire date.
FROM employees
WHERE employee_id NOT IN (SELECT employee_id FROM electoral_register);

SELECT MIN(hire_date) INTO v_min_hdate  -- Import the oldest hire date.
FROM employees
WHERE employee_id NOT IN (SELECT employee_id FROM electoral_register);

FOR c_emp IN c_employees -- Start parsing through the registers in emp table.
LOOP

IF c_emp.hire_date = v_max_hdate THEN -- Display if the "youngest" condition statement matched
DBMS_OUTPUT.PUT_LINE('The most recent employee in the company is: '||c_emp.first_name||' '||c_emp.last_name);

ELSIF c_emp.hire_date = v_min_hdate THEN -- Display most veteran emp if the condition statement matched
DBMS_OUTPUT.PUT_LINE('The most veteran employee in the company is: '||c_emp.first_name||' '||c_emp.last_name);

END IF;
END LOOP;
END;

---

## Pregunta 4

**Fes un script que mostri el número de registres de les següents taules, així com el percentatge en relació al total de registres. Intenta mostrar la informació de manera que sigui fàcilment intel·ligible.**

Les taules son:

- employees

- departments

- locations

---

DECLARE
 v_total_registers NUMBER(5);
 v_employees_registers NUMBER(5);
 v_emp_percentage NUMBER(5);
 v_dep_percentage NUMBER(5);
 v_loc_percentage NUMBER(5);
 v_departments_registers NUMBER(5);
 v_locations_registers NUMBER(5);
BEGIN
-- Count regnums.
 SELECT COUNT(employee_id) into v_employees_registers FROM employees;
 SELECT COUNT(department_id) into v_departments_registers FROM DEPARTMENTS;
 SELECT COUNT(location_id) INTO v_locations_registers FROM LOCATIONS;
 v_total_registers := v_employees_registers + v_departments_registers + v_locations_registers;

--Calc. percanteges
 v_emp_percentage := (v_employees_registers/v_total_registers) * 100;
 v_dep_percentage := (v_departments_registers/v_total_registers) * 100;
 v_loc_percentage := (v_locations_registers/v_total_registers) * 100;

--Display results
 DBMS_OUTPUT.PUT_LINE('Total employees:'||v_employees_registers||'%. Which is '||v_emp_percentage||' of the total.');
 DBMS_OUTPUT.PUT_LINE('Total departments: '||v_departments_registers||'%. Which is '||v_dep_percentage||' of the total.');
 DBMS_OUTPUT.PUT_LINE('Total locations: '||v_locations_registers||'%. Which is '||v_loc_percentage||' of the total.');
 DBMS_OUTPUT.PUT_LINE('Total registers: '||v_total_registers);
END;
