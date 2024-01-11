### Summary
In the context of enhancing system security, various SQL queries with filters were employed to investigate security incidents and obtain specific information about login attempts and employee machines. Two primary tables, log_in_attempts and employees, were utilized. Filters were implemented using the AND, OR, and NOT operators to cater to distinct requirements. Additionally, the LIKE operator, coupled with the percentage sign (%) wildcard, was employed for pattern-based filtering.

### Retrieve after hours failed login attempts
To address a potential security incident, a SQL query was crafted to retrieve failed login attempts after business hours (18:00). The query incorporated conditions for both time and unsuccessful logins.

![001](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/458cc550-61b7-4990-a14e-424a6db30050)

The first part of the screenshot is my query, and the second part is a portion of the output.
This query filters for failed login attempts that occurred after 18:00. First, I started by selecting
all data from the log_in_attempts table. Then, I used a WHERE clause with an AND operator
to filter my results to output only login attempts that occurred after 18:00 and were
unsuccessful. The first condition is login_time > '18:00', which filters for the login
attempts that occurred after 18:00. The second condition is success = FALSE, which filters
for the failed login attempts.


### Retrieve login attempts on specific dates
In response to a suspicious event, a SQL query was designed to extract login attempts on specific dates (2022-05-09 and 2022-05-08) using the OR operator.

![002](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/51e8d2c6-29f0-438a-8b72-e2790bc9b62a)

The first part of the screenshot is my query, and the second part is a portion of the output.
This query returns all login attempts that occurred on 2022-05-09 or 2022-05-08. First, I
started by selecting all data from the log_in_attempts table. Then, I used a WHERE clause
with an OR operator to filter my results to output only login attempts that occurred on either
2022-05-09 or 2022-05-08. The first condition is login_date = '2022-05-09', which
filters for logins on 2022-05-09. The second condition is login_date = '2022-05-08',
which filters for logins on 2022-05-08.


### Retrieve login attempts outside of Mexico
Concerns about login attempts outside of Mexico led to a SQL query utilizing the NOT operator and LIKE with the pattern MEX% to filter out non-Mexican attempts.

![003](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/81b4252d-70b4-4498-865e-12a5cf7220ad)

The first part of the screenshot is my query, and the second part is a portion of the output.
This query returns all login attempts that occurred in countries other than Mexico. First, I
started by selecting all data from the log_in_attempts table. Then, I used a WHERE clause
with NOT to filter for countries other than Mexico. I used LIKE with MEX% as the pattern to
match because the dataset represents Mexico as MEX and MEXICO. The percentage sign (%)
represents any number of unspecified characters when used with LIKE.


### Retrieve employees in Marketing
To identify employees in the Marketing department in the East building, a SQL query was constructed with conditions for department and office location.

![004](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/8b06a1ea-7b0c-4717-854e-40c21e3a9cfe)

The first part of the screenshot is my query, and the second part is a portion of the output.
This query returns all employees in the Marketing department in the East building. First, I
started by selecting all data from the employees table. Then, I used a WHERE clause with AND
to filter for employees who work in the Marketing department and in the East building. I used
LIKE with East% as the pattern to match because the data in the office column represents
the East building with the specific office number. The first condition is the department =
'Marketing' portion, which filters for employees in the Marketing department. The second
condition is the office LIKE 'East%' portion, which filters for employees in the East
building.


### Retrieve employees in Finance or Sales
For updating machines in the Finance and Sales departments, a SQL query employed the OR operator to filter employees from these specific departments.

![005](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/5f6b977a-3617-4b47-b5e5-606e0dcc475b)

The first part of the screenshot is my query, and the second part is a portion of the output.
This query returns all employees in the Finance and Sales departments. First, I started by
selecting all data from the employees table. Then, I used a WHERE clause with OR to filter for
employees who are in the Finance and Sales departments. I used the OR operator instead of
AND because I want all employees who are in either department. The first condition is
department = 'Finance', which filters for employees from the Finance department. The
second condition is department = 'Sales', which filters for employees from the Sales
department.


### Retrieve all employees not in IT
To facilitate a security update for employees outside the Information Technology department, a SQL query with the NOT operator was used to filter out IT employees.

![006](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/32bbf29c-c41f-4017-9bee-3d7d4133cbe6)

The first part of the screenshot is my query, and the second part is a portion of the output. The
query returns all employees not in the Information Technology department. First, I started by
selecting all data from the employees table. Then, I used a WHERE clause with NOT to filter for
employees not in this department.


