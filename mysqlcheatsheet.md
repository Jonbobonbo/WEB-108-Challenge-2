# MySQL Cheat Sheet
This is a cheatsheet detailing the basics of MySQL. Remember, MySQL is case sensitive and caps are required for statements.

## 1. Accessing MySQL from Terminal
To log in to the MySQL monitor through the terminal, after using SSH to access your server, you will need to run this code:
```console
user@server:~$ mysql -u root -p
```
Now enter the MySQL password you set up for the root user when installing MySQL.  

## 2. Show Users
To display all of the users in MySQL use this line of code:
```sql
mysql> SELECT user FROM mysql. user;
```
This will display a table with all of MySQL's existing users.  

## 3. Create a User in MySQL
To create a new user in MySQL, run this code:
```sql
mysql> CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
```
Replace *username* and *password* with your own user name and password.

## 4. Grant MySQL Privileges
Your users will need privileges to be able to access your databases. Privileges can be both granted and viewed.
### Grant Privileges:
```sql
mysql> GRANT ALL PRIVILEGES ON database.* TO 'username'@'localhost' IDENTIFIED BY 'password';
```
Replace *database* with the name of your database, *username* with your username, and *password* with your password.
### View Privileges:
```sql
-- This shows the grants for the current user:
mysql> SHOW GRANTS FOR CURRENT_USER;

-- This shows the grants for a specific user:
mysql> SHOW GRANTS FOR 'username'@'localhost';

```
### Remove Granted Privileges:
```sql
mysql> REVOKE ALL PRIVILEGES,
    -> GRANT OPTION FROM 'username'@'localhost';
```
## 5. Show, Delete, Create, and Select Databases
### Show Databases:
```sql
mysql> SHOW DATABASES;
```
This will print out a list of all the databases that have been created.  

### Delete Databases:
```sql
mysql> DROP database_name; 

    --OR--

mysql> DELETE database_name;
```
### Create a Database:
```sql
mysql> CREATE DATABASE database_name; 
```
### Select Database:
```sql
mysql> SELECT * FROM database_name
```
The * character means *all*. You can replace this with any column name, if you want to be more specific. You can also add multiple parameters, separated by commas.  

## 6. Create a Table With Columns
When creating columns you must define their data-types. This is how you would create a table called snacks with the columns salty, sweet, and healthy:
```sql
mysql> CREATE TABLE snacks (
    -> salty VARCHAR(100),
    -> sweet VARCHAR(100),
    -> healthy VARCHAR(100)
    -> );
```  

## 7. Show, Delete, and Drop Tables
### How to show tables:
```sql
mysql> SHOW TABLES;
```  
### How to delete all data from a table:
```sql
mysql> DELETE FROM snacks;
```

### How to drop a table (this completely deletes the table and all of its data):
```sql
mysql> DROP TABLE snacks;
```  

## 8. Insert Rows and Records
### Adding a single row with records.
To add a row with records you must first confirm which columns you want to insert data into and then define the data that you will insert by using the *VALUES* statement. Remember if you have any constraints that prevent a column from having a null value, you MUST include them in your declaration:
```sql
mysql> INSERT INTO snacks 
    -> (salty, sweet, healthy)
    -> VALUES
    -> ('pretzels', 'fudge', 'broccoli');
```
### Now we'll add multiple rows at once:
```sql
mysql> INSERT INTO snacks
    -> (salty, sweet, healthy)
    -> VALUES
    -> ('chips', 'licorice', 'carrot sticks'),
    -> ('beef jerky',' ice cream', 'kimchi'),
    -> ('cheese', 'candy', 'apple');
```  
## 9. Using Select with the Where Clause
The where clause allows your to be more specific in your Select statements, selecting only the rows and columns that meet those specifcations. The operators that are supported by the where clause are: = , > , < , >= , <= , LIKE , IN , BETWEEN.
```sql
mysql> SELECT healthy 
    -> FROM snacks 
    -> WHERE sweet = 'licorice';
```
## 10. Select With the Where Clause Using a Range
BETWEEN is the operator that allows you to select within a range. Let's say you wanted to find all cities that have a specific range of population in a database. This is how that is done:
```sql
mysql> SELECT Name, Population
    -> FROM city
    -> WHERE Population BETWEEN 670000 AND 700000; 
```
## 11. Delete an Individual Row
The following code will delete the row that has the value of 6 in the ID column:
```sql
mysql> DELETE FROM city
    -> WHERE ID = 6;
```
## 12. Update a Row
This will update the population of the row that has 17 in the ID column:
```sql
mysql> UPDATE city
    -> SET Population = 10887
    -> WHERE ID = 17;
```
## 13. Add a New Column and Modify it
By using *ALTER TABLE*, a new column can be added to the table. You can also modify the column and change the order. In this example, I modify the column to appear first, then add a second column and modify it to appear after the *sweet* column:
```sql
mysql>> ALTER TABLE snacks
    -> ADD COLUMN id INT NOT NULL [FIRST],
    -> ADD COLUMN spicy VARCHAR(50) NOT NULL [AFTER sweet];
```
## 14. Order By and Use Distinct
Selecting with *DISTINCT* removes duplicate outputs of data:
```sql
mysql> SELECT DISTINCT CountryCode
    -> FROM city;
```
Using the *GROUP BY* clause. This will order the results in ascending order:
```sql
mysql> SELECT *
    -> FROM city
    -> GROUP BY ID ASC;
```
## 15. Concatenate Columns
```sql
mysql> SELECT CONCAT('salty', ' ', 'healthy')
    -> FROM snacks;
```
## 16. Select Distinct Rows
```sql
mysql> SELECT DISTINCT CountryCode
    -> FROM city
    -> WHERE CountryCode = 'ARG';
```
## 17. Use LIKE to Search
This will output all of the rows where the *CountryCode* begins with the letter *U*:
```sql
mysql> SELECT *
    -> FROM city
    -> WHERE CountryCode LIKE 'U%';
```
## 18. Select using IN
This will output all of the rows that have a *CountryCode* of *CA* and *ARG*:
```sql
mysql> SELECT *
    -> FROM city
    -> WHERE CountryCode IN ('CA', 'ARG');
```
## 19. Create and Remove an Index
Creating an index:
```sql
mysql> CREATE INDEX id_numbers
    -> ON city (ID);
```
Removing an index:
```sql
mysql> DROP INDEX id_numbers
    -> ON city;
```
## 20. Create Two Tables Demonstrating a One to Many Relationship That Shows a PK and an FK
```sql
mysql> CREATE TABLE Customer (
    -> CustomerID int NOT NULL AUTO_INCREMENT,
    -> Name VARCHAR(50) NOT NULL,
    -> Email VARCHAR(50) NOT NULL,
    -> PRIMARY KEY (CustomerID)
    -> );

mysql> CREATE TABLE Order (
	-> OrderID int NOT NULL AUTO_INCREMENT,
    -> Date DATE NOT NULL,
    -> Location VARCHAR(50) NOT NULL,
    -> CustomerID int NOT NULL,
    -> PRIMARY KEY (OrderID),
    -> FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
    -> );
```
## 21. Use INNER JOIN
This will output data from the selected table and the table that it is joined with:
```sql
myslq> SELECT Orders.OrderID, Name
    -> FROM Orders
    -> INNER JOIN Customer
    -> ON Orders.CustomerID = Customer.CustomerID;
```
## 22. JOIN Multiple Tables 
Let's try it with three tables instead:
```sql
mysql> CREATE TABLE Customer (
    -> CustomerID int NOT NULL AUTO_INCREMENT,
    -> Name VARCHAR(50) NOT NULL,
    -> Email VARCHAR(50) NOT NULL,
    -> PRIMARY KEY (CustomerID)
    -> );

mysql> CREATE TABLE Product (
    -> ProductID int NOT NULL,
    -> ProductName VARCHAR(50) NOT NULL,
    -> Price int NOT NULL,
    -> PRIMARY KEY (ProductID)
    -> );

mysql> CREATE TABLE Order (
	-> OrderID int NOT NULL AUTO_INCREMENT,
    -> Date DATE NOT NULL,
    -> Location VARCHAR(50) NOT NULL,
    -> CustomerID int NOT NULL,
    -> ProductID int NOT NULL,
    -> PRIMARY KEY (OrderID),
    -> FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID),
    -> FOREIGN KEY (ProductID) REFERENCES Product(ProductID)
    -> );
```
You can join multiple tables in the same *SELECT* statement by using *INNER JOIN* multiple times:
```sql
myslq> SELECT Orders.OrderID, Name, ProductName
    -> FROM Orders
    -> INNER JOIN Customer
    -> ON Orders.CustomerID = Customer.CustomerID
    -> INNER JOIN Product
    -> ON Orders.ProductID = Product.ProductID;
```