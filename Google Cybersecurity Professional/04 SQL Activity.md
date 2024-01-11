### Summary
In the context of enhancing system security, various SQL queries with filters were employed to investigate security incidents and obtain specific information about login attempts and employee machines. Two primary tables, log_in_attempts and employees, were utilized. Filters were implemented using the AND, OR, and NOT operators to cater to distinct requirements. Additionally, the LIKE operator, coupled with the percentage sign (%) wildcard, was employed for pattern-based filtering.

### Retrieve after hours failed login attempts
To address a potential security incident, a SQL query was crafted to retrieve failed login attempts after business hours (18:00). The query incorporated conditions for both time and unsuccessful logins.

![001](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/458cc550-61b7-4990-a14e-424a6db30050)


### Retrieve login attempts on specific dates
In response to a suspicious event, a SQL query was designed to extract login attempts on specific dates (2022-05-09 and 2022-05-08) using the OR operator.

![002](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/51e8d2c6-29f0-438a-8b72-e2790bc9b62a)


### Retrieve login attempts outside of Mexico
Concerns about login attempts outside of Mexico led to a SQL query utilizing the NOT operator and LIKE with the pattern MEX% to filter out non-Mexican attempts.

![003](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/81b4252d-70b4-4498-865e-12a5cf7220ad)


### Retrieve employees in Marketing
To identify employees in the Marketing department in the East building, a SQL query was constructed with conditions for department and office location.

![004](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/8b06a1ea-7b0c-4717-854e-40c21e3a9cfe)


### Retrieve employees in Finance or Sales
For updating machines in the Finance and Sales departments, a SQL query employed the OR operator to filter employees from these specific departments.

![005](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/5f6b977a-3617-4b47-b5e5-606e0dcc475b)


### Retrieve all employees not in IT
To facilitate a security update for employees outside the Information Technology department, a SQL query with the NOT operator was used to filter out IT employees.

![006](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/32bbf29c-c41f-4017-9bee-3d7d4133cbe6)


