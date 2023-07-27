# SQL Injection - Exposing Unreleased Products
- This lab presents a SQL injection vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following:
~~~
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
~~~

- The goal is to perform a SQL injection attack that causes the application to display one or more unreleased products.

## Solution

- Use Burp Suite to intercept and modify the request responsible for setting the product category filter.

- Modify the "category" parameter, giving it the value: '+OR+1=1--.

- This modification injects SQL code that always evaluates to true, bypassing the category filter.
Example of the modified request:
~~~
GET /products?category='+OR+1=1-- HTTP/1.1
Host: example.com
~~~

- Submit the modified request to the server.

- The server processes the request and performs the SQL query as:
~~~
SELECT * FROM products WHERE category = '' OR 1=1-- AND released = 1
~~~

- The SQL query returns all products, including unreleased ones (released = 0), as the condition 1=1 always evaluates to true. The part after -- acts as an SQL comment, making the rest of the query ineffective.

- Consequently, the application now displays one or more unreleased products, successfully exposing the vulnerability in the product category filter.