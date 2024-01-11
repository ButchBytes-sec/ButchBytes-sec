### Summary
In the context of enhancing system security, various SQL queries with filters were employed to investigate security incidents and obtain specific information about login attempts and employee machines. Two primary tables, log_in_attempts and employees, were utilized. Filters were implemented using the AND, OR, and NOT operators to cater to distinct requirements. Additionally, the LIKE operator, coupled with the percentage sign (%) wildcard, was employed for pattern-based filtering.

### Retrieve after hours failed login attempts
To address a potential security incident, a SQL query was crafted to retrieve failed login attempts after business hours (18:00). The query incorporated conditions for both time and unsuccessful logins.

### Retrieve login attempts on specific dates
In response to a suspicious event, a SQL query was designed to extract login attempts on specific dates (2022-05-09 and 2022-05-08) using the OR operator.

### Retrieve login attempts outside of Mexico
Concerns about login attempts outside of Mexico led to a SQL query utilizing the NOT operator and LIKE with the pattern MEX% to filter out non-Mexican attempts.

### Retrieve employees in Marketing
To identify employees in the Marketing department in the East building, a SQL query was constructed with conditions for department and office location.

### Retrieve employees in Finance or Sales
For updating machines in the Finance and Sales departments, a SQL query employed the OR operator to filter employees from these specific departments.

### Retrieve all employees not in IT
To facilitate a security update for employees outside the Information Technology department, a SQL query with the NOT operator was used to filter out IT employees.
