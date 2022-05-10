## 1. 
Write a SQL statement to create the above table.

    CREATE TABLE grocery_items(
        product_id NUMBER(5) PRIMARY KEY,
        brand VARCHAR2(10),
        description VARCHAR2(10)
    );

    CREATE TABLE grocery_items

        (product_id NUMBER(3) PRIMARY KEY,

        brand VARCHAR2(20),

        description VARCHAR2(20));

## 2. 
Write and execute three SQL statements to explicitly add the above data to the table.

    INSERT INTO grocery_items(product_id,brand,description) VALUES(110,'Colgate','Toothpaste');
    INSERT INTO grocery_items(product_id,brand,description) VALUES(111,'Ivory','Soap');
    INSERT INTO grocery_items(product_id,brand,description) VALUES(112,'Heinz','Ketchup');

---

## 3.
Write and execute a SQL statement that will explicitly add your favorite beverage to the table.

    INSERT INTO grocery_items(product_id,brand,description) VALUES(113,'Guinness','Dark beer');

---

## 4.
According grocery_items table, write and execute a SQL statement that modifies the description for Heinz ketchup to “tomato catsup”.

    --Change the permited length of the "description" column

    ALTER TABLE grocery_items
    MODIFY(description VARCHAR2(20));

    --Update field
    UPDATE grocery_items SET description='tomato catsup' WHERE brand='Heinz';

---

## 5.
According grocery_items table, write and execute a SQL statement that will implicitly add your favorite candy to the table.


    INSERT INTO grocery_items VALUES (114,'Haribo','gummy bear');
    Retroacció

---

## 6.
Write and execute a SQL statement that changes the soap brand from “Ivory” to “Dove", according grocery_items table.

    UPDATE grocery_items
    SET brand='Dove'
    WHERE brand='Ivory';

---

## 7.
Write and execute SQL statements to create the new_items table and populate it with the data in the table.

    --Create the table
    CREATE TABLE new_items(
        product_id NUMBER(5) PRIMARY KEY,
        brand VARCHAR2(20),
        description VARCHAR2(20)
    );

    --Insert new rows
    INSERT INTO new_items VALUES(110,'Colgate','Dental paste');
    INSERT INTO new_items VALUES(175,'Dew','Soda');
    INSERT INTO new_items VALUES(275,'Palmolive','Dish detergent');

---

## 8.
Write a SQL statement that will update the grocery_items table with the brand and description from the new_items table when the product ID values match. If they don’t match, add a new row to the grocery_items table. DO NOT EXECUTE YOUR STATEMENT YET.

- How many rows will be updated by the SQL statement?

     1 row will be updated. The 110.

- How many rows will be inserted by the SQL statement?

    2 rows will be inserted.

    MERGE INTO grocery_items groc USING new_items new
    ON (groc.product_id=new.product_id)
    WHEN MATCHED THEN UPDATE
    SET groc.brand=new.brand , groc.description=new.description
    WHEN NOT MATCHED THEN INSERT
    VALUES(new.product_id, new.brand,new.description);
