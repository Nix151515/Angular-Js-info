SQL functionalities:

- DISTINCT
SELECT DISTINCT Country FROM Customers;

Select all the different countries from the "Customers" table:

- LIKE
SELECT * FROM Customers
WHERE Country = 'Spain' AND CustomerName LIKE 'G%';

Select all customers from Spain that starts with the letter 'G':

- ORDER BY
SELECT * FROM Products
ORDER BY Price DESC;

Sort the products by price (descending order):

- INSERT INTO
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');

The INSERT INTO statement is used to insert new records in a table.

- JOIN
A JOIN clause is used to combine rows from two or more tables, based on a related column between them.

Here are the different types of the JOINs in SQL:

 (INNER) JOIN: Returns records that have matching values in both tables
 LEFT (OUTER) JOIN: Returns all records from the left table, and the matched records from the right table
 RIGHT (OUTER) JOIN: Returns all records from the right table, and the matched records from the left table
 FULL (OUTER) JOIN: Returns all records when there is a match in either left or right table

Note: The FULL OUTER JOIN keyword returns all matching records from both tables whether the other table matches or not. So, if there are rows in "Customers" that do not have matches in "Orders", or if there are rows in "Orders" that do not have matches in "Customers", those rows will be listed as well.

To be continued ... add pictures and operations