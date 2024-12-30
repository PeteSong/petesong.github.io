---
layout: default
title:  "SQL Tips"
date:   2024-12-30 14:00:00 +0800
tags: sql
---


## Statements

There are different versions of the SQL language.

* `SELECT`
    * `SELECT INTO`
* `UPDATE`
* `DELETE`
* `INSERT INTO`
    * `INSERT INTO SELECT`
* 
* `CREATE DATABASE`
* `ALTER DATABASE`
* 
* `CREATE TABLE`
* `ALTER TABLE`
* `DROP TABLE`
* 
* `CREATE INDEX`
* `DROP INDEX`

```sql
SELECT * 
FROM Customers
ORDER BY Country ASC, CustomerName DESC;

SELECT Country FROM Customers;
SELECT DISTINCT Country FROM Customers;
SELECT COUNT(DISTINCT Country) FROM Customers;

SELECT * 
FROM Customers
WHERE Country='Mexico' AND CustomerName LIKE 'G%';

SELECT *
FROM Customers
WHERE CustomerID=1;

SELECT *
FROM Products
ORDER BY Price ASC;

SELECT *
FROM Products
ORDER BY Price DESC;

SELECT MIN(Price) as SmallestPrice, MAX(Price), AVG(Price)
FROM Products;

SELECT *
FROM Products
WHERE Price BETWEEN 10 AND 20;

SELECT *
FROM Products
WHERE Price > (SELECT AVG(Price) FROM Products);

SELECT MIN(Price) as SmallestPrice, MAX(Price), AVG(Price), CategoryID
FROM Products
GROUP BY CategoryID;

SELECT COUNT(*)
FROM Products;

SELECT CategoryID, COUNT(*)
FROM Products
GROUP BY CategoryID;

SELECT COUNT(Price)
FROM Products;

SELECT COUNT(DISTINCT Price)
FROM Products;

SELECT *
FROM Customers
WHERE Country IN ('Germany', 'France', 'UK');

SELECT *
FROM Customers
WHERE CustomerID in (SELECT CustomerID FROM Orders);

SELECT o.OrderID, o.OrderDate, c.CustomerName  
FROM Customers AS c, Orders AS o  
WHERE c.CustomerName='Around the Horn' AND c.CustomerID=o.CustomerID;

SELECT ProductID, ProductName, CategoryName
FROM 
    Products JOIN Categories ON Products.CategoryID = Categories.CategoryID;

SELECT Customers.CustomerName, Orders.OrderID  
FROM Customers  
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID  
ORDER BY Customers.CustomerName;

SELECT Orders.OrderID, Employees.LastName, Employees.FirstName  
FROM Orders  
RIGHT JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID  
ORDER BY Orders.OrderID;

SELECT Customers.CustomerName, Orders.OrderID  
FROM Customers  
FULL OUTER JOIN Orders ON Customers.CustomerID=Orders.CustomerID  
ORDER BY Customers.CustomerName;

SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City  
FROM Customers A, Customers B  
WHERE A.CustomerID <> B.CustomerID  
AND A.City = B.City  
ORDER BY A.City;

SELECT COUNT(CustomerID), Country  
FROM Customers  
GROUP BY Country  
ORDER BY COUNT(CustomerID) DESC;

SELECT COUNT(CustomerID), Country
FROM customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5;

SELECT OrderID, Quantity,  
CASE  
    WHEN Quantity > 30 THEN 'The quantity is greater than 30'  
    WHEN Quantity = 30 THEN 'The quantity is 30'  
    ELSE 'The quantity is under 30'  
END AS QuantityText  
FROM OrderDetails;


SELECT * INTO CustomersBackup2017  
FROM Customers;

INSERT INTO Customers (CustomerName, City, Country)  
SELECT SupplierName, City, Country FROM Suppliers;

INSERT INTO Customers(CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway')

INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)  
VALUES  
('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway'),  
('Greasy Burger', 'Per Olsen', 'Gateveien 15', 'Sandnes', '4306', 'Norway'),  
('Tasty Tee', 'Finn Egan', 'Streetroad 19B', 'Liverpool', 'L1 0AA', 'UK');


UPDATE Customers
SET ContactName='Alfred Schmidt', City='Frankfurt'
WHERE CustomerID = 1;


DELETE FROM Customers WHERE CustomerName='Alfreds Futterkiste';

DROP TABLE Customers;

```


- `WHERE` clause
    - `=` , `>` , `<` , `>=` , `<=` , `<>` ,  `IN`, `NOT IN`
    - `BETWEEN`
        - begin and end values are included.
    - `LIKE`
        - `%` represents zero, one, or multiple characters
        - `_` represents one, single character
    - `AND` , `OR` , `NOT` operators
    - `IS NULL` , `IS NOT NULL` operators
    - `EXISTS`
    - `ANY` , `ALL` 
- `HAVING` clause
    - since `WHERE` keyword cannot be used with aggregate functions

- aggregate functions
    - `MIN()`
    - `MAX()`
    - `COUNT()`
    - `SUM()`
    - `AVG()`

- `ORDER BY` clause

- `JOIN` clause
    - `INNER JOIN` or `JOIN` keyword selects records that have matching values in both tables
    - `LEFT JOIN` keyword selects all records from the left table, and the matching records from the right table.
    - `RIGHT JOIN` keyword returns all records from the right table and the matching records from the left table.
    - `FULL OUTER JOIN` or `FULL JOIN` keyword returns all records when there is a match in left or right table records.

- `UNION` or `UNION ALL`

- `CASE` clause

- `NULL` functions
    - `IFNULL()`
    - `COALESCE()`
    - `NVL()`

- `CREATE PROCEDURE`

- Comments
    - `--` single line comments
    - `/* */` multi-line comments


## Abbreviation

ANSI = American National Standards Institute

ISO = International Organization for Standardization

SQL = Structured Query Language

RDBMS = Relational Database Management System


## Links
1. [https://www.w3schools.com/sql/sql_intro.asp](https://www.w3schools.com/sql/sql_intro.asp)
2. [SQLite](https://www.sqlite.org/index.html)
3. [Database Examples/Northwind/SQLite](https://en.wikiversity.org/wiki/Database_Examples/Northwind/SQLite)
