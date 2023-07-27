# SQL Injection Login Bypass

- This lab presents a SQL injection vulnerability in the application's login function.

- The objective is to bypass the login process and gain access to the application as the administrator user.

## Solution
- Use Burp Suite to intercept the login request sent by the application.

- Modify the "username" parameter, providing the value: administrator'--. 
This modification injects an SQL comment after the username, causing the rest of the SQL query to be ignored.

Example of the modified request:
~~~
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=administrator'--&password=mypassword
~~~

- Send the modified request to the server.

- The server processes the request, and the SQL query is interpreted as:

~~~
SELECT * FROM users WHERE username='administrator'--' AND password='mypassword'
~~~

- The SQL query is manipulated to bypass the password check, allowing the user to be authenticated as an administrator.

- The application grants access to the user with the username "administrator" without requiring a valid password, effectively bypassing the login.
