# SQL gFg

| English        | SQL tems
| ------------- |:-------------:|
| table      | relation
|  header | attributes
|  rows | tuples
| no of rows | cardinality
| no of cols | degree

-	Data Definition Language: It is used to define the structure of the database. e.g; CREATE TABLE, ADD COLUMN, DROP COLUMN and so on.

-	Data Manipulation Language: It is used to manipulate data in the relations. e.g.; INSERT, DELETE, UPDATE and so on.

-	Data Query Language: It is used to extract the data from the relations. e.g.; SELECT

## Data Query

1. SELECT [DISTINCT] Attribute_List FROM R1,R2….RM
2. [WHERE condition]
3. [GROUP BY (Attributes)[HAVING condition]]
4. [ORDER BY(Attributes)[DESC]];

1 is compulasory. [] are optional.

AGGRATION FUNCTIONS: Aggregation functions are used to perform mathematical operations on data values of a relation. Some of the common aggregation functions used in SQL are: COUNT, SUM,MIN, MAX and AVG. They return only one row.

SELECT COUNT (PHONE) FROM STUDENT;

SELECT SUM (AGE) FROM STUDENT;

## Constraints
They are the rules that we can apply on the type of data in a table.

NOT NULL
UNIQUE
PRIMARY KEY (column name)
FOREIGN 
CHECK(condition)
DEFAULT <value>

[SQL | DDL, DML, TCL and DCL - GeeksforGeeks](https://www.geeksforgeeks.org/sql-ddl-dml-tcl-dcl/)

![sql-commands.jpg](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/sql-commands.jpg)

```sql
create table department
(dept_name  char(20),
  building   char(15),
  budget     numeric(12,2));
  ```
  
 ## Transactions
 It group a set of tasks into a single execution unit. Each transaction begins with a specific task and ends when all the tasks in the group successfully complete. If any of the tasks fail, the transaction fails. Therefore, a transaction has only two results: success or failure
  
 #### ACID
  
Transactions have the following four standard properties, usually referred to by the acronym ACID −

**Atomicity** − Ensures that all operations within the work unit are completed successfully; otherwise, the transaction is aborted at the point of failure, and previous operations are rolled back to their former state.

**Consistency** − Ensures that the database properly changes state upon a successfully committed transaction.

**Isolation** − Enables transactions to operate independently of and transparent to each other.

**Durability** − Ensures that the result or effect of a committed transaction persists in case of a system failure.

#### Transaction Control

There are following commands used to control transactions −

**COMMIT** − To save the changes.

**ROLLBACK** − To roll back the changes.

**SAVEPOINT** − Creates points within groups of transactions in which to ROLLBACK.

**SET TRANSACTION** − Places a name on a transaction.

```sql
Begin Tran 
DELETE FROM CUSTOMERS 
   WHERE AGE = 25 
COMMIT 

Begin Tran 
DELETE FROM CUSTOMERS 
   WHERE AGE = 25; 
ROLLBACK

Begin Tran 
SAVE Transaction SP1 
Savepoint created. 
DELETE FROM CUSTOMERS WHERE ID = 1  
1 row deleted. 
SAVE Transaction SP2 
--Savepoint created. 

ROLLBACK Transaction SP2 
-- Rollback complete. 
```
 ## Roles
 
 
|SYSTEM ROLES |	PRIVILEGES GRANTED TO THE ROLE
|---|---|
|Connect|	Create table, Create view, Create synonym, Create sequence, Create session etc.
|Resource|	Create Procedure, Create Sequence, Create Table, Create Trigger etc. The primary usage of the Resource role is to restrict access to database objects.
|DBA|	All system privileges
 
```sql
-- Create ROLE
CREATE ROLE manager;
Role created.

--Grant privileges to a role 
GRANT create table, create view TO manager;.

--Grant a role to users
GRANT manager TO SAM, STARK;


--Revoke privilege from a Role
REVOKE create table FROM manager;

--Drop a Role
DROP ROLE manager;
```


## Views
Views in SQL are kind of virtual tables

```sql
CREATE VIEW DetailsView AS
SELECT NAME, ADDRESS
FROM StudentDetails
WHERE S_ID < 5;

-- To create a View from **multiple tables**  
CREATE VIEW MarksView AS
SELECT StudentDetails.NAME, StudentDetails.ADDRESS, StudentMarks.MARKS
FROM StudentDetails, StudentMarks
WHERE StudentDetails.NAME = StudentMarks.NAME;

-- Delete View
DROP VIEW MarksView;
```


## Stored Procedures

[Sql Server - How To Write a Stored Procedure in SQL Server - CodeProject](https://www.codeproject.com/Articles/126898/Sql-Server-How-To-Write-a-Stored-Procedure-in-SQL) 

```sql
 /* 
GetstudentnameInOutputVariable is the name of the stored procedure which
uses output variable @Studentname to collect the student name returns by the
stored procedure
*/

Create  PROCEDURE GetstudentnameInOutputVariable
(

@studentid INT,                       --Input parameter ,  Studentid of the student
@studentname VARCHAR(200)  OUT        -- Out parameter declared with the help of OUT keyword
)
AS
BEGIN
SELECT @studentname= Firstname+' '+Lastname FROM tbl_Students WHERE studentid=@studentid
END
```

```sql
/*
This Stored procedure is used to Insert value into the table tbl_students. 
Following ex is also for insert values
*/
Create Procedure InsertStudentrecord
(
 @StudentFirstName Varchar(200),
 @StudentLastName  Varchar(200),
 @StudentEmail     Varchar(50)
) 
As
 Begin
   Insert into tbl_Students (Firstname, lastname, Email)
   Values(@StudentFirstName, @StudentLastName,@StudentEmail)
 End
```


```sql
Execute Getstudentname 1
Exec Getstudentname 1
```

Another Example

```sql
CREATE PROCEDURE dbo.uspGetAddressCount @City nvarchar(30), @AddressCount int OUT
AS
SELECT @AddressCount = count(*) 
FROM AdventureWorks.Person.Address 
WHERE City = @City

--- To call this stored procedure we would execute it as follows.  First we are going to declare a variable, execute the stored procedure and then select the returned valued.
DECLARE @AddressCount int
EXEC dbo.uspGetAddressCount @City = 'Calgary', @AddressCount = @AddressCount OUTPUT
SELECT @AddressCount
```