## Stored Procedure in SQL

#### Table of Contents

- [SQL Stored Procedures](#sql-stored-procedures)
- [Creating a Procedure](#creating-a-procedure)
- [Stored Procedure Parameter Types](#stored-procedure-parameter-types)
- [Procedure with IN parameter](#procedure-with-in-parameter)
- [Procedure with OUT parameter](#procedure-with-out-parameter)
- [Procedure with INOUT parameter](#procedure-with-inout-parameter)
- [Advantages of Stored Procedures](#advantages-of-stored-procedures)
- [Drawbacks of Stored Procedures](#drawbacks-of-stored-procedures)

### SQL Stored Procedures

An SQL stored procedure is a group of pre-compiled SQL statements (prepared SQL
code) that can be reused by simply calling it whenever needed.

It can be used to perform a wide range of database operations such as inserting,
updating, or deleting data, generating reports, and performing complex
calculations. Stored procedures are very useful because they allow you to
encapsulate (bundle) a set of SQL statements as a single unit and execute them
repeatedly with different parameters, making it easy to manage and reuse the
code.

_Procedures have similar structure as functions: they accept parameters and
perform operations when we call them. But, the difference between them is that
SQL stored procedures are simpler to write or create, whereas functions have a
more rigid structure and support fewer clauses._

#### Syntax

The basic syntax to create an SQL stored procedure is as follows −

```sql
DELIMITER //
CREATE PROCEDURE procedure_name(parameter1 datatype, parameter2 datatype, ...)
BEGIN
   -- SQL statements to be executed
END
DELIMITER ;
```

- The `CREATE PROCEDURE` statement is used to create the procedure. We can
  define any number of input parameters as per the requirement.
- The SQL statements that make up the procedure are placed between the `BEGIN`
  and `END` keywords.

### Creating a Procedure

We can create a stored procedure using the `CREATE PROCEDURE` statement in SQL.
Following are the simple steps for creating a stored procedure −

- Choose a name for the procedure.
- Write the SQL code for the procedure.
- We can then test the stored procedure by executing it with different input
  parameters.

#### Example

To understand it better let us consider the `CUSTOMERS` table which contains the
personal details of customers including their name, age, address and salary etc.
as shown below −

```sql
CREATE TABLE CUSTOMERS (
   ID INT NOT NULL,
   NAME VARCHAR (20) NOT NULL,
   AGE INT NOT NULL,
   ADDRESS CHAR (25),
   SALARY DECIMAL (18, 2),
   PRIMARY KEY (ID)
);
```

Now, insert values into this table using the `INSERT` statement as follows −

```sql
INSERT INTO CUSTOMERS VALUES
(1, 'Ramesh', 32, 'Ahmedabad', 2000.00),
(2, 'Khilan', 25, 'Delhi', 1500.00),
(3, 'Kaushik', 23, 'Kota', 2000.00),
(4, 'Chaitali', 25, 'Mumbai', 6500.00),
(5, 'Hardik', 27, 'Bhopal', 8500.00),
(6, 'Komal', 22, 'Hyderabad', 4500.00),
(7, 'Muffy', 24, 'Indore', 10000.00);
```

Let us look at a simple example of creating a stored procedure that takes an
input parameter and returns a result set. In the following query, we are
creating the stored procedure with the name `GetCustomerInfo`. then we provide
it with a single input parameter called `@CutomerAge`. The stored procedure then
selects all records from the `CUSTOMERS` table where the value of the
`CutomerAge` matches the input parameter.

```sql
DELIMITER //
CREATE PROCEDURE GetCustomerInfo(IN CustomerAge INT)
   BEGIN
      SELECT * FROM CUSTOMERS WHERE AGE = CustomerAge;
   END //
DELIMITER ;
```

#### Output

This would produce the following result −

```sql
Query OK, 0 rows affected (0.01 sec)
```

#### Verification

We can test the stored procedure by executing it using the CALL statement as
shown below −

```sql
CALL GetCustomerInfo(25);
```

This will return all columns from the `CUSTOMERS` table where the customers age
is 25.

| ID  | NAME     | AGE | ADDRESS | SALARY  |
| --- | -------- | --- | ------- | ------- |
| 2   | Khlan    | 25  | Delhi   | 1500.00 |
| 4   | Chaitali | 25  | Mumbai  | 6500.00 |

### Stored Procedure Parameter Types

Stored procedures in a database system can have different types of parameters,
which are placeholders for values that will be passed to the stored procedure
when it is executed. Following are the different types of stored procedure
parameters in SQL −

| S.No. | <center>Parameter</center> | <center>Description</center>                                             |
| ----- | -------------------------- | ------------------------------------------------------------------------ |
| 1     | Input parameters           | Pass values from the calling statement to the stored procedure           |
| 2     | Output parameters          | Return values from the stored procedure                                  |
| 3     | Input/Output parameters    | Allow a stored procedure to accept input values and return output values |

### Procedure with IN parameter

`IN` is the default parameter of the procedure that will receive input values.
We can pass the values as arguments when the stored procedure is being called.

These values are read-only, so they cannot be modified by the stored procedure.

#### Example

In the following query, we are creating a stored procedure that takes a customer
`ID` as an input parameter and returns the corresponding customer salary.

The procedure body simply performs a `SELECT` statement to retrieve the "Salary"
column from the `CUSTOMERS` table, where the `CustomerID` matches the input
parameter.

```sql
DELIMITER //
CREATE PROCEDURE GetCustomerSalary(IN CustomerID Int)
   BEGIN
      SELECT SALARY FROM CUSTOMERS WHERE ID = CustomerID;
   END //
DELIMITER ;
```

#### Output

This would produce the following result −

```sql
Query OK, 0 rows affected (0.01 sec)
```

#### Verification

We can test it by executing it with different ID as an input parameter as shown
in the query below −

```sql
CALL GetCustomerSalary(6);
```

This will return the salary for the customer with an ID of 6, assuming there is
a corresponding row in the `CUSTOMERS` table −

| SALARY  |
| ------- |
| 4500.00 |

### Procedure with OUT parameter

The `OUT` parameter is used to return the output value from the procedure.

Note that when using an `OUT` parameter, we must specify the keyword `OUT`
before the parameter name when passing it to the stored procedure. This tells
the SQL database that the parameter is an output parameter and should be
assigned with a value in the stored procedure.

#### Example

In the following query we are creating a stored procedure that used to count the
number of records of customer having same age and assign this count to the
'total' variable which holds the number of records.

The procedure body performs a `SELECT` statement to get the count of records
having same age from the `CUSTOMERS` table

```sql
DELIMITER //
CREATE PROCEDURE GetDetail(OUT total INT)
   BEGIN
      SELECT COUNT(AGE) INTO total FROM CUSTOMERS
      WHERE AGE = 25;
   END //
DELIMITER ;
```

Calling the created procedure and passing the 'total' parameter

```sql
CALL GetDetail(@total);
```

Here, we are using the `SELECT` statement and getting the count −

```sql
SELECT @total;
```

#### Output

This would produce the following result −

| @total |
| ------ |
| 2      |

#### Verification

To verify weather the procedure is created, we can use the following query −

```sql
SHOW CREATE PROCEDURE GetDetails;
```

### Procedure with INOUT parameter

The `INOUT` parameter is a combination of an IN parameter and an OUT parameter.
You can pass data into the stored procedure and receive data from the stored
procedure using the same parameter.

To declare an `INOUT` parameter in a stored procedure, we need to specify the
`INOUT` keyword before the parameter name.

#### Example

In the following query, we provide two `INOUT` parameters to the stored
procedure: `cust_id` and `curr_Salary`. These two are used as both an input and
output parameters.

The stored procedure first retrieves the current salary of the customer from the
database using the `cust_id` parameter. It then increases the salary by 10% and
updates the customers salary in the database using the same parameter.

```sql
DELIMITER //
CREATE PROCEDURE increaseSalary(INOUT Cust_Id Int,  INOUT curr_Salary Int)
   BEGIN
      SELECT SALARY INTO curr_Salary From CUSTOMERS Where ID = Cust_Id;
      SET curr_Salary = curr_Salary * 1.1;
      Update CUSTOMERS SET SALARY = curr_Salary Where ID = Cust_Id;
   END //
DELIMITER ;
```

#### Output

This would produce the following result −

```sql
Query OK, 0 rows affected (0.01 sec)
```

####Verification We can test it by executing it with different ID or input
parameters as shown in the query below −

```sql
SET @customerID = 1;
SET @salary = 0.0;
CALL increaseSalary(@customerID, @salary);
```

Following is Query to select the updated salary from the stored procedure

```sql
SELECT @salary AS updated_salary;
```

The result-set is obtained as −

| updated_salary |
| -------------- |
| 2200           |

### Advantages of Stored Procedures

- **Improved Performance**: Stored procedures are pre-compiled and stored on the
  server, so they can be executed more quickly than SQL statements that are sent
  from client applications.
- **Code Reuse**: Stored procedures can be called from different client
  applications, which means that the same code can be reused across different
  applications. This reduces development time and maintenance costs.
- **Reduced Network Traffic**: Because stored procedures are executed on the
  server, only the results are returned to the client, which reduces network
  traffic and improves application performance.
- **Better Security**: Stored procedures can be used to enforce security rules
  and prevent unauthorized access to sensitive data. They can also limit the
  actions that can be performed by users, making it easier to maintain data
  integrity and consistency.
- **Simplified Maintenance**: By storing SQL code in a single location, it
  becomes easier to maintain and update the code. This makes it easier to fix
  bugs, add new functionality, and optimize performance.

### Drawbacks of Stored Procedures

- **Increased Overhead:** Stored procedures can consume more server resources
  than simple SQL statements, particularly when they are used frequently or for
  complex operations.
- **Limited Portability:** Stored procedures are often specific to a particular
  database management system (DBMS), which means they may not be easily portable
  to other DBMSs.
- **Debugging Challenges:** Debugging stored procedures can be more challenging
  than debugging simple SQL statements, particularly when there are multiple
  layers of code involved.
- **Security Risks:** If stored procedures are not written correctly, they can
  pose a security risk, particularly if they are used to access sensitive data
  or to perform actions that could compromise the integrity of the database.

[Content Source](https://www.tutorialspoint.com/sql/sql-stored-procedures.htm)
