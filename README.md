# Data-Science-Challenge

## Question 1:

 Given some sample data, write a program to answer the following: (link to data set here https://docs.google.com/spreadsheets/d/16i38oonuX1y1g7C_UAmiK9GkY7cS-64DfiDMNiR41LM/edit#gid=0)
 
 On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

1. Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.
2. What metric would you report for this dataset?
3. What is its value?

## Answers: 

1. I calculated the incorrect AOV calculation of 3145.13 by finding the quotient of sum of order_amount divided by 5,000 total orders. To find the appropriate Average Order Value you would need to find the quotient of sum of order_amount divided by the sum of total_items. 
2. To calculate the correct Average Order Value, the reporting metrics would be the quotient of 'order_amount' divided by 'total_items':
Shopify <- read.csv(‘data/2019 Winter Data Science Intern Challenge Data Set - Sheet1.csv', header=TRUE, stringsAsFactors = FALSE)
total_order_value <- sum(Shopify$order_amount)
total_items_sold<- sum(Shopify$total_items)

total_order_value/total_items_sold  #Average Order Value of Items sold 
3. The Average Order Value (AOV) is: $357.92

VIEW CODE HERE:https://github.com/GordonWillingham/Data-Science-Challenge/blob/main/Shopify%20Code%20Test.Rmd
## Question 2: 

For this question you’ll need to use SQL. Follow this link (https://www.w3schools.com/SQL/TRYSQL.ASP?FILENAME=TRYSQL_SELECT_ALL) to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

1. How many orders were shipped by Speedy Express in total?
2. What is the last name of the employee with the most orders?
3. What product was ordered the most by customers in Germany?

## Answers:

1. How many orders were shipped by Speedy Express in total? 54
SELECT * FROM [Orders] WHERE ShipperID=1;
Or

CREATE VIEW Shipper_Orders AS
SELECT Orders.OrderID, Orders.ShipperID, Shippers.ShipperName
FROM Orders
JOIN Shippers
ON Shippers.ShipperID=Orders.ShipperID;

SELECT COUNT(*) FROM [Shipper_Orders]
WHERE ShipperName = ‘Speedy Express’;

2. What is the last name of the employee with the most orders?
   Employee: Peacock
   Most orders:40
   CREATE VIEW Employee_Orders AS
SELECT Orders.EmployeeID, Employees.LastName, Orders.OrderID
FROM Orders
JOIN Employees
ON Employees.EmployeeID=Orders.EmployeeID;

SELECT LastName, COUNT(*)
FROM Employee_Orders
GROUP BY LastName
ORDER BY COUNT(*) desc;

3. What product was ordered the most by customers in Germany?
   Product: Camembert Pierrot
   Quantity: 40
   Orders: 300
   Total Ordered:12000
CREATE VIEW Products_Ordered AS
SELECT Orders.OrderID, Customers.Country, OrderDetails.Quantity, Products.ProductName
FROM Orders, OrderDetails
JOIN Customers ON Orders.CustomerID=Customers.CustomerID
JOIN Products ON OrderDetails.ProductID=Products.ProductID
WHERE Country=‘Germany’;

CREATE VIEW Product_Orders AS
SELECT ProductName, Quantity, COUNT(*) as ‘Orders’
FROM Products_Ordered
GROUP BY ProductName;

SELECT ProductName, Quantity, Orders, (Quantity * Orders) As TotalOrders
FROM Product_Orders
ORDER BY TotalOrders desc
LIMIT 1;


