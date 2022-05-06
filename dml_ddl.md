#    ***// INSERT // UPDATE // ALTER***
---

##      ***INSERT***


###     **Implicit and Explicit inserts**

Examples:

    Implicit:
    insert into copy_d_songs values(52,'Surfing Summer','Not known','',12);
    insert into copy_d_songs values(53,'Victory Victory','5 min','',12);


    Explicit:
    insert into copy_d_clients(client_number,first_name,last_name,phone,email)
    values(6655,'Ayako','Dahish',3608859030,'dahisha@harbor.net');

    insert into copy_d_clients(client_number,first_name,last_name,phone,email)
    values(6689,'Nick','Neuville',9048953049,'nnicky@harbor.net');

---

###    **Multitable inserts**

    INSERT first
    WHEN salary > 20000
    THEN INTO special_sal values(employee_id,salary)
    ELSE
    INTO sal_history values(employee_id, hire_date,salary)
    INTO mgr_history values(employee_id,manager_id,salary)
    SELECT employee_id,hire_date,salary,manager_id
    FROM employees;

--- 

###     **Insert specific columns from one table to another**

Example:

    This is the created table:

    create table rep_email (
    id number(3) constraint rel_id_pk primary key,
    first_name varchar2(15),
    last_name varchar2(15),
    email_address varchar2(15)
    );

    insert into rep_email(id,first_name,last_name,email_address)
    select employee_id,first_name,last_name,email from employees 
    where job_id like '%_REP%';

---

##      ***ALTER***
---

###     **Alter/Modify table column types,constraints,defaults**

Set a new datatype for the column:

    alter table rep_email
      modify email_address varchar2(15);

Set a default value for the column:

    alter table copy_f_promotional_menus
    modify(start_date date default sysdate);

Drop/Delete an entire column:

    alter table rep_mail drop column last_name;

Set and drop "unused" columns:

    alter table rep_mail set unused last_name;
    (Does not restore disk space. Columns are treated as if 
     they were dropped.)

    alter table rep_mail drop unused columns;
    (All the columns which were marked as "unused" are
     going to be dropped permanently, no rollback!)

---

##    ***UPDATE***
---

###     **Change a specific column in a table**

Normal:

    update copy_f_food_items
    set price = 3.75
    where description='Strawberry Shake';

Examples:

    If the field value is null and You want to change it:

    update copy_f_staffs
    set overtime_rate = nvl(overtime_rate,0)+0.85
    where first_name ='Sue' and last_name='Doe';

If we want to update a field with the same value as another register:

    update copy_f_staffs
    set manager_id=(select manager_id from copy_f_staffs where first_name = 'Sue' and last_name='Doe')
    where id=25;

---

#     ***// TABLE // VIEW // SEQUENCE //***
##     ***The 3 kinds of database objects***
---

#       ***TABLES***
---

##      ***CREATE TABLE***
---

###     **Basic syntax**
    
    
    CREATE TABLE copies
    (copy_id NUMBER(10) CONSTRAINT copy_id_pk PRIMARY KEY,
    format VARCHAR2(3) NOT NULL,
    title_id NUMBER(10) NOT NULL CONSTRAINT copy_titleid_fk REFERENCES movies(title_id));

---

###    **Constraints**

    
    **NOT NULL** - Ensures that a column cannot have a NULL value
    **UNIQUE** - Ensures that all values in a column are different
    **PRIMARY KEY** - A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table
    **FOREIGN KEY** - Prevents actions that would destroy links between tables
    **CHECK** - Ensures that the values in a column satisfies a specific condition
    **DEFAULT** - Sets a default value for a column if no value is specified

Examples for NOT NULL / CHECK / SINGLE PRIMARY KEY:

    CREATE TABLE movies
    (title_id NUMBER(10) CONSTRAINT title_id_pk PRIMARY KEY,
    title VARCHAR2(60) NOT NULL,
    description VARCHAR2(400) NOT NULL,
    rating VARCHAR2(4) CONSTRAINT movies_rating CHECK (rating IN ('G', 'PG','R','PG13')),
    category VARCHAR2(20) CHECK (category IN ('DRAMA', 'COMEDY', 'ACTION', 'CHILD', 'SCIFI', 'DOCUMENTARY')),
    release_date date NOT NULL);


Examples for FOREIGN KEY ( REFERENCES ) / DOUBLE PRIMARY KEY:

    CREATE TABLE rental_history
    (copy_id NUMBER(10) CONSTRAINT copy_id_fk REFERENCES copies(copy_id),
    rental_date date default SYSDATE NOT NULL,
    customer_id NUMBER(10) NOT NULL CONSTRAINT customer_id_fk REFERENCES customers(customer_id),
    return_date date,
    CONSTRAINT rental_history_pk PRIMARY KEY (copy_id, rental_date));



Examples for DEFAULT / DEFAULT SYSDATE:

    CREATE TABLE Persons (
    ID NUMBER(10) NOT NULL,
    LastName VARCHAR2(255) NOT NULL,
    FirstName VARCHAR(255),
    Age NUMBER(10),
    City VARCHAR2(255) DEFAULT 'Sandnes'); 

    ALTER TABLE copy_f_promotional_menus
    MODIFY(start_date DATE DEFAULT SYSDATE);

Examples for UNIQUE:

    CREATE TABLE Persons (
        ID NUMBER(10) NOT NULL UNIQUE,
        LastName VARCHAR22(255) NOT NULL,
        FirstName VARCHAR2(255),
        Age NUMBER(10));

---

##      ***DELETE TABLE***
---

###     **Drop tables/delete tables**

Syntax:

    DROP TABLE <tablename>;

*If a table has been dropped by mistake, then we have the option to bring them back with:*
   
 
    FLASHBACK TABLE tablename TO BEFORE DROP;


To check which tables can be restored, visit: **"USER_RECYCLEBIN"**.

*If for any security reasons we want to delete/drop a table permanently, without being able to restore it or see it in the USER_RECYCLEBIN, then we have the option with:*
    

    DROP TABLE copy_employees PURGE;


---

###     **Eliminate a complete register (row) from a table.**

    DELETE FROM copy_f_staffs WHERE id=25;

---

###     **Eliminate registers from a table which exists in another one.**


    DELETE FROM lesson7_emp 
    WHERE employee_id IN (select employee_id from job_history);

---

##     **Truncate tables**


Truncate tables means to remove every row from the given tables, which are notgoing to be able to be restored (released from the storage).

Does exactly the same as the DELETE statement, but with DELETE we don't release the storage place, so later it can be restored.

Syntax:


    TRUNCATE TABLE tablename;


*Truncate is faster than DELETE, because it doesn't generate rollback information.*

---

## ***Merge one table into another.***


    MERGE INTO manager_copy_d_cds tgt USING d_cds src
    ON (src.cd_number = tgt.cd_number)
    WHEN MATCHED THEN UPDATE
    SET  tgt.title = src.title, tgt.producer = src.producer, tgt.year = src.year
    WHEN NOT MATCHED THEN INSERT
    VALUES (src.cd_number, src.title, src.producer, src.year);

*Don't forget, always that table comes first which is being "inserted into" and the second the table which we want to insert INTO THE FIRST TABLE.*

---


##    ***Rename tables***


If we want to change the name/rename a table, we have the option with:
    
    RENAME old_name TO new_name;

---

##     **Comment on tables,columns,views**

Comment on table:

    COMMENT ON TABLE tablename IS 'your comment';

Comment on columns:

    COMMENT ON COLUMN tablename.columnname IS 'your comment';

The comments can be queried from the: **USER_TAB_COMMENTS**

    SELECT * FROM USER_TAB_COMMENTS;

---

##     **FLASHBACK QUERY / check out previous versions at INSERT,UPDATE,DELETE**
---

Example:

    SELECT employee_id, first_name ||' '|| last_name as "NAME",
        versions_operation AS "OPERATION"
        versions_starttime AS "START_DATE",
        versions_endtime AS "END_DATE", salary
    FROM employees
        VERSIONS BETWEEN SCN MINVALUE AND MAXVALUE
    WHERE employee_id = 1;

---

#     ***Sequences***

Sequencies are used to automatically generate unique numbers.
It's a shareable object, so multiple users can access it and use it for different tables.
Good for time saver, because this way no need to write the same type of code various times.

Syntax:

    CREATE SEQUENCE sequence
    INCREMENT by n
    START with n
    MAXVALUE n or NOMAXVALUE
    MINVALUE n or NOMINVALUE
    CYCLE or NOCYCLE
    CACHE n or NOCACHE;

SEQUENCE:

    Is the name of the sequence generator (object)
 

INCREMENT BY n:

    Specifies the interval between sequence numbers
       where n is an integer (If this caluse is omitted, the
       sequence increments by 1.)
 

START WITH n:

    Specifies the first sequence number to be generated.
       (If this caluse is omitted, the sequence starts with 1.)
 

MAXVALUE n:

    Specifies the maximum value the sequence can generate.

 

NOMAXVALUE:

    Specifies a maximum value of 10^27 for an ascending sequence and -1 for a descending
       sequence (default).

MINVALUE n:

    Specifies the minimum sequence value.


NOMINVALUE:

    Specifies a minimum value of 1 for an ascending sequence
       and -(10^26) for a descending sequence (default).

 
CYCLE OR NOCYCLE:

    Specifies whether the sequence continues to generate
       values after reaching its maximum or minimum value
       (NOCYCLE is the default option.)


Example:

    CREATE SEQUENCE runner_id_seq
        INCREMENT BY
        START WITH
        MAXVALUE
        NOCACHE
        NOCYCLE;

To check the sequences of the user, visit: USER_SEQUENCES table.

    SELECT * FROM USER_SEQUENCES;

---

##      ***// NEXTVAL // CURRVAL //***
###     **Use of already created sequences**

###     **NEXTVAL:**

->  *When we reference NEXTVAL for the first time,
    the initial value of the sequence will be returned.*

Syntax example:

    INSERT INTO employees
        (employee_id, department_id,...)
    VALUES (employees_seq.NEXTVAL, dept_deptid_seq .CURRVAL, ...);

---

###     **CURRVAL:**

->  *Before CURRVAL can be referenced in a statement, there must be
    an already referenced NEXTVAL to initialize the "count" of the 
    SEQUENCE.*

    Syntax example:

    INSERT INTO employees (employee_id, department_id,...)
    VALUES (employees_seq.NEXTVAL, dept_deptid_seq .CURRVAL, ...);

 
It is posible to view/check out the next value in a sequence by
query it, in the USER_SEQUENCE.

SELECT sequence_name, min_value, max_value, last_number AS "Next number"
FROM USER_SEQUENCES
WHERE sequence_name = 'RUNNER_ID_SEQ';

---

##     ***VIEWS***

Are used to create an imaginery "window" for other users. Basically,
a kind of restriction for other users. What they can or cannot see
from the database.

**Simple view**

syntax:
 
    CREATE VIEW view_employees
    AS SELECT employee_id, first_name, last_name, email
        FROM employees
        WHERE employee_id BETWEEN 100 and 124;
 

    SELECT *
    FROM view_employees;

**Complex View**

syntax:

    CREATE OR REPLACE VIEW view_euro_countries
    ("ID", "Country", "Capitol City", "Region")
    AS SELECT c.country_id, c.country_name, c.capitol, 
    r.region_name
    FROM wf_countries c JOIN wf_world_regions r
    USING (region_id)
    WHERE location LIKE '%Europe';
    SELECT * 
    FROM view_euro_countries;

**View with constraint/limitation:

    **WITH CHECK OPTION**
    -> Specifies that rows remain accessible to 
    the view after insert or update  operations.

    **CONSTRAINT**
    -> Is the name assigned to the CHECK OPTION constraint.

    **WITH READ ONLY**
    -> Ensures that no DML operations can be 
    performed on this view.


    CREATE OR REPLACE VIEW title_unavail AS SELECT m.title, c.copy_id, r.rental_date, r.return_date
    FROM movies m, copies c, rental_history r
    WHERE m.title_id = c.title_id
    and r.copy_id = c.copy_id
    and return_date is null
    with read only;

    *In this case with read-only*


---

#     ***Data dictionary tables of the database system of Oracle***
---

**USER_TABLES**:

    Describes the relational tables owned by the current user.
    Its columns(except for OWNER) are the same as those in ALL_TABLES.

    **USER_CATALOG**:

    Lists indexes, tables, views, clusters, synonyms and sequences owned by the current user. Its columns are the same as those in "ALL_CATALOG"

**USER_OBJECTS**:
   
     Describes all objects owned by the current user. Its columns are the sameas those in "ALL_OBJECTS".
---
