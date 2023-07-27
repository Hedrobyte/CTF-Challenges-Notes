# Retrieving Database Version

- This lab features a SQL injection vulnerability in the product category filter. 

- The goal is to perform a UNION attack to retrieve the database version string.


## Solution

- Use Burp Suite to intercept and modify the request that sets the product category filter.

- Determine the number of columns being returned by the query and identify which columns contain text data. Verify if the query returns two columns, both containing text, using a payload like the following in the "category" parameter:
~~~
'+UNION+SELECT+'abc','def'+FROM+dual--
~~~

- Use the following payload to display the database version:
~~~
    '+UNION+SELECT+BANNER,+NULL+FROM+v$version--
~~~
    
- Submit the modified request to the server.

- The server processes the request, and the UNION SELECT attack combines the results of your injected query with the original query. The result should display the database version information along with the existing product category data.

