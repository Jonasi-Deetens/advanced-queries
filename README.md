# SQL questionnaire

## Setup
Import this [fake database content](mysqlsampledatabase.zip) into your database.

Below you will find a list of questions.

Find out the answer to each question using the dummy data in your database.

**Copy this file** (see: copy raw content) and fill in your queries + answer on the given location in each question.

## START !

### 1) How many customers do we have?
```
SELECT COUNT(*) FROM customers;
```

solution: 122


### 2) What is the customer number of Mary Young?
```
SELECT customerNumber FROM customers 
WHERE customers.contactFirstName="Mary" && customers.contactLastName="Young";
```

solution: 219

### 3) What is the customer number of the person living at Magazinweg 7, Frankfurt 60528?
```
SELECT customerNumber FROM customers 
WHERE CONCAT(customers.addressLine1, ", ", customers.city, " ", customers.postalCode)="Magazinweg 7, Frankfurt 60528";
```

solution: 247


### 4) If you sort the employees on their last name, what is the email of the first employee?
```
SELECT email FROM employees ORDER BY lastName
LIMIT 1;
```

solution: gbondur@classicmodelcars.com

### 5) If you sort the employees on their last name, what is the email of the last employee?
```
SELECT email FROM employees ORDER BY lastName DESC
LIMIT 1;
```

solution: gvanauf@classicmodelcars.com


### 6) What is first the product code of all the products from the line 'Trucks and Buses', sorted first by productscale, then by productname.
```
SELECT productCode FROM products 
WHERE productLine="Trucks and Buses"
ORDER BY productScale, productName
LIMIT 1
```

solution: S12_4473

### 7) What is the email of the first employee, sorted on their last name that starts with a 't'?
```
SELECT email FROM employees 
WHERE LEFT(lastName, 1) = "t" 
ORDER BY lastName
LIMIT 1;
```

solution: lthompson@classicmodelcars.com


### 8) Which customer (give customer number) payed by check on 2004-01-19?
```
SELECT customerNumber FROM payments 
WHERE paymentDate="2004-01-19";
```

solution: 177

### 9) How many customers do we have living in the state Nevada or New York?
```
SELECT COUNT(*) FROM customers 
WHERE state in ("NV", "NY");
```

solution: 7


### 10) How many customers do we have living in the state Nevada or New York or outside the united states?
```
SELECT COUNT(*) FROM customers 
WHERE state in ("NV", "NY") OR country <> "USA";
```

solution: 93

### 11) How many customers do we have with the following conditions (only 1 query needed):  - Living in the state Nevada or New York OR - Living outside the USA or the customers and with a credit limit above 1000 dollar?
```
SELECT COUNT(*) FROM customers 
WHERE state in ("NV", "NY") OR (country <> "USA" AND creditLimit > 1000);
```

solution: 70


### 12) How many customers don't have an assigned sales representative?
```
SELECT COUNT(*) FROM customers 
WHERE salesRepEmployeeNumber IS NULL;
```

solution: 22

### 13) How many orders have a comment?
```
SELECT COUNT(*) FROM orders 
WHERE comments IS NOT NULL;
```

solution: 80


### 14) How many orders do we have where the comment mentions the word "caution"?
```
SELECT COUNT(*) FROM orders 
WHERE comments LIKE '%caution%'
```

solution: 4

### 15) What is the average credit limit of our customers from the USA? (rounded)
```
SELECT ROUND(AVG(creditLimit)) FROM customers
WHERE country = "USA";
```

solution: 78103


### 16) What is the most common last name from our customers?
```
SELECT contactLastName FROM customers
GROUP BY contactLastName
ORDER BY COUNT(*) DESC
LIMIT 1;
```

solution: Young

### 17) What are valid statuses of the orders?
- [X] Resolved

- [X] Cancelled

- [ ] Broken

- [X] On Hold

- [X] Disputed

- [X] In Process

- [ ] Processing

- [X] Shipped

```
SELECT status
FROM (
    SELECT 'Resolved' AS status
    UNION ALL SELECT 'Cancelled'
    UNION ALL SELECT 'Broken'
    UNION ALL SELECT 'On Hold'
    UNION ALL SELECT 'Disputed'
    UNION ALL SELECT 'In Process'
    UNION ALL SELECT 'Processing'
    UNION ALL SELECT 'Shipped'
) AS status_list
WHERE status IN (
    SELECT DISTINCT status
    FROM orders
);
```

solution: See list ^


### 18) In which countries don't we have any customers?
- [ ] Austria

- [ ] Canada

- [X] China

- [ ] Germany

- [X] Greece

- [ ] Japan

- [ ] Philippines

- [X] South Korea

```
SELECT country_name
FROM (
    SELECT 'Austria' AS country_name
    UNION ALL SELECT 'Canada'
    UNION ALL SELECT 'China'
    UNION ALL SELECT 'Germany'
    UNION ALL SELECT 'Greece'
    UNION ALL SELECT 'Japan'
    UNION ALL SELECT 'Philippines'
    UNION ALL SELECT 'South Korea'
) AS country_list
WHERE country_name NOT IN (
    SELECT DISTINCT country
    FROM customers
);
```

solution: See list ^


### 19) How many orders were never shipped?
```
SELECT COUNT(*) FROM orders
WHERE status <> "Shipped";
```

solution: 23

### 20) How many customers does Steve Patterson have with a credit limit above 100 000 EUR?
```
SELECT COUNT(*) FROM customers
JOIN employees ON customers.salesRepEmployeeNumber = employees.employeeNumber
WHERE concat(employees.firstName, " ", employees.lastName) = "Steve Patterson" AND customers.creditLimit > "100000";
```

solution: 3

### 21) How many orders have been shipped to our customers?
```
SELECT COUNT(*) FROM orders
WHERE status="Shipped";
```

solution: 303


### 22) How much products does the biggest product line have? And which product line is that?
```
SELECT COUNT(*) AS products, productLine FROM products
GROUP BY productLine
ORDER BY products DESC
LIMIT 1;
```

solution: 38, Classic Cars

### 23) How many products are low in stock? (below 100 pieces)
```
SELECT COUNT(*) FROM products
WHERE quantityInStock < 100
```

solution: 2

### 24) How many products have more the 100 pieces in stock, but are below 500 pieces.
```
SELECT COUNT(*) FROM products
WHERE quantityInStock BETWEEN 100 AND 500;
```

solution: 3


### 25) How many orders did we ship between and including June 2004 & September 2004
```
SELECT COUNT(*) FROM orders
WHERE shippedDate BETWEEN CAST('2004-06-01' AS DATE) AND CAST('2004-09-30' AS DATE) AND STATUS="shipped";
```

solution: 42

### 26) How many customers share the same last name as an employee of ours?
```
SELECT COUNT(*) FROM customers
JOIN employees
WHERE customers.contactLastName = employees.lastName
```

solution: 9

### 27) Give the product code for the most expensive product for the consumer?
```
SELECT productCode FROM products
ORDER by msrp DESC
LIMIT 1;
```

solution: S10_1949


### 28) What product (product code) offers us the largest profit margin (difference between buyPrice & MSRP).
```
SELECT productCode FROM products
ORDER BY (MSRP - buyPrice) DESC
LIMIT 1;
```

solution: S10_1949

### 29) How much profit (rounded) can the product with the largest profit margin (difference between buyPrice & MSRP) bring us.
```
SELECT ROUND(MSRP - buyPrice) AS profit FROM products
ORDER BY profit DESC
LIMIT 1;
```

solution: 116

### 30) Given the product number of the products (separated with spaces) that have never been sold?
```
SELECT CONCAT(productCode, ' ') FROM products
WHERE productCode NOT IN (SELECT DISTINCT productCode FROM orderdetails);
```

solution: S18_3233 


### 31) How many products give us a profit margin below 30 dollar?
```
SELECT COUNT(*) FROM products
WHERE (MSRP - buyPrice) < 30
```

solution: 23

### 32) What is the product code of our most popular product (in number purchased)?
```
SELECT productCode FROM orderdetails
GROUP BY productCode
ORDER BY SUM(quantityOrdered) DESC
LIMIT 1;
```

solution: S18_3232

### 33) How many of our popular product did we effectively ship?
```
SELECT SUM(quantityOrdered) AS quantity FROM orderdetails
WHERE orderNumber in (SELECT orderNumber FROM orders WHERE STATUS="shipped")
GROUP BY productCode
ORDER BY quantity DESC
LIMIT 1;
```

solution: 1720


### 34) Which check number paid for order 10210. Tip: Pay close attention to the date fields on both tables to solve this.  
```
SELECT checkNumber FROM payments a
JOIN (
	SELECT * FROM orders
	WHERE orderNumber = "10210"
) b ON b.customernumber = a.customerNumber
WHERE a.paymentDate < b.requiredDate
```

solution: CI381435

### 35) Which order was paid by check CP804873?
```
SELECT b.orderNumber FROM payments a
JOIN orders b ON b.customerNumber = a.customerNumber
WHERE checkNumber = "CP804873" AND a.paymentDate < b.requiredDate;
```

solution: 10330

### 36) How many payments do we have above 5000 EUR and with a check number with a 'D' somewhere in the check number, ending the check number with the digit 9?
```
SELECT * FROM payments
WHERE amount > 5000 AND checkNumber LIKE '%D%9'
```

solution: 3


### 38) In which country do we have the most customers that we do not have an office in?
```
SELECT country FROM customers
WHERE country NOT IN (SELECT country FROM offices)
GROUP BY country
ORDER BY COUNT(*) DESC
LIMIT 1;
```

solution: Germany

### 39) What city has our biggest office in terms of employees?
```
SELECT city FROM offices
JOIN employees
WHERE employees.officeCode = offices.officeCode
GROUP BY city
ORDER BY COUNT(*) DESC
LIMIT 1;
```

solution: San Francisco

### 40) How many employees does our largest office have, including leadership?

```
SELECT COUNT(*) AS employeeCount FROM offices
JOIN employees
WHERE employees.officeCode = offices.officeCode
GROUP BY city
ORDER BY employeeCount DESC
LIMIT 1;
```

solution: 6


### 41) How many employees do we have on average per country (rounded)?
```
SELECT ROUND(COUNT(*) / b.numberOfOffices) FROM employees
JOIN (
	SELECT COUNT(DISTINCT country) AS numberOfOffices FROM offices
) b

```

solution: 5

### 42) What is the total value of all shipped & resolved sales ever combined?
```
SELECT SUM(totalPrice) FROM orders a
join (
	SELECT orderNumber, SUM(quantityOrdered * priceEach) AS totalPrice FROM orderdetails
	GROUP BY orderNumber
) b ON a.orderNumber = b.orderNumber
WHERE a.status IN ("Shipped", "Resolved");
```

solution: 8999330.52

### 43) What is the total value of all shipped & resolved sales in the year 2005 combined? (based on shipping date)
```
SELECT SUM(totalPrice) FROM orders a
join (
SELECT orderNumber, SUM(quantityOrdered * priceEach) AS totalPrice FROM orderdetails
GROUP BY orderNumber
) b ON a.orderNumber = b.orderNumber
WHERE a.status IN ("Shipped", "Resolved") AND YEAR(a.shippedDate) = "2005";
```

solution: 1427944.97


### 44) What was our most profitable year ever (based on shipping date), considering all shipped & resolved orders?
```
SELECT YEAR(a.shippedDate) FROM orders a
join (
    SELECT orderNumber, SUM(quantityOrdered * priceEach) AS totalPrice FROM orderdetails
    GROUP BY orderNumber
) b ON a.orderNumber = b.orderNumber
WHERE a.status IN ("Shipped", "Resolved") 
group by YEAR(a.shippedDate)
ORDER BY totalPrice DESC
LIMIT 1;
```

solution: 2004

### 45) How much revenue did we make on in our most profitable year ever (based on shipping date), considering all shipped & resolved orders?
```
SELECT SUM(totalPrice) FROM orders a
join (
    SELECT orderNumber, SUM(quantityOrdered * priceEach) AS totalPrice FROM orderdetails
    GROUP BY orderNumber
) b ON a.orderNumber = b.orderNumber
WHERE a.status IN ("Shipped", "Resolved") 
group by YEAR(a.shippedDate)
ORDER BY totalPrice DESC
LIMIT 1;
```

solution: 4321167.85

### 46) What is the name of our biggest customer in the USA of terms of revenue?
```
SELECT a.customerName FROM customers a
JOIN payments b ON a.customerNumber = b.customerNumber
WHERE a.country = "USA"
GROUP BY a.customerNumber
ORDER BY SUM(amount) DESC
LIMIT 1;
```

solution: Mini Gifts Distributors Ltd.

### 47) How much has our largest customer inside the USA ordered with us (total value)?
```
SELECT round(SUM(quantityOrdered * priceEach)) AS totalRev FROM orders a
join orderdetails b ON a.orderNumber = b.orderNumber
JOIN customers c ON c.customerNumber = a.customerNumber
WHERE c.country = "USA"
GROUP BY a.customerNumber
ORDER BY totalRev DESC
LIMIT 1;
```

solution: 591827

### 48) How many customers do we have that never ordered anything?
```
SELECT COUNT(*) FROM customers 
WHERE customerNumber NOT IN (SELECT customerNumber FROM orders)
```

solution: 24

### 49) What is the last name of our best employee in terms of revenue?
```
SELECT c.lastName FROM customers a
JOIN (
	SELECT customerNumber, SUM(amount) AS total FROM payments a
	GROUP BY customerNumber
) b ON a.customerNumber = b.customerNumber
JOIN (
	SELECT employeeNumber, lastName FROM employees
) c ON c.employeeNumber = a.salesRepEmployeeNumber
GROUP BY c.employeeNumber
ORDER BY  SUM(total) DESC
LIMIT 1;
```

solution: Hernandez


### 50) What is the office name of the least profitable office in the year 2004?
```
SELECT d.officeCode FROM customers a
JOIN (
	SELECT customerNumber,  paymentDate, SUM(amount) AS total FROM payments
	GROUP BY customerNumber
) b ON a.customerNumber = b.customerNumber
JOIN employees c ON c.employeeNumber = a.salesRepEmployeeNumber
JOIN offices d ON d.officeCode = c.officeCode
WHERE YEAR(paymentDate) = "2004"
GROUP BY c.officeCode
ORDER BY SUM(total)
LIMIT 1;
```

solution: 5


## Are you done? Amazing!
![](../_assets/clap-clap-clap.gif)
