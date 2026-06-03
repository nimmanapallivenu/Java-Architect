# 38 15 SQL interview Q&As   Java Success.com

## Table of Contents

- [Q1: Can you explain DDL statements in regards to SQL?](#q1)
- [Q2: Can you explain DML statements in regards to SQL?](#q2)
- [Q3: Explain inner and outer joins?](#q3)
- [Q4: What is a self join?](#q4)
- [Q5: What is a sub-query? How does a sub-query impact on performance? What are the ad](#q5)
- [Q6: What is a correlated sub-query?](#q6)
- [Q7: Can you explain aggregate SQL functions?](#q7)
- [Q8: How can you compare a part of the name rather than the entire name?](#q8)
- [Q9: How do you get distinct entries from a table?](#q9)
- [Q10: How can you find the total number of records in a table?](#q10)
- [Q11: In your experience, what are some of the common mistakes developers make related](#q11)
- [Q12: What is the dif ference between “T runcate” and “Delete” commands?](#q12)
- [Q13: How will you go about deleting a few records from parent and child tables where ](#q13)
- [Q14: What do you do with the PURGE command?](#q14)

---

## Q1: Can you explain DDL statements in regards to SQL?

**Answer:**

DDL stands for Data Definition Language, which are statements used to define the database structure or schema. CREA TE – to create objects in the
database, ALTER – alters the structure of the database, DROP – delete objects from the database, TRUNCA TE – remove all records from a table, including all
spaces allocated for the records are removed, COMMENT – add comments to the data dictionary , and RENAME – rename an object.
Create a table named “EMPLOYEES” using a DDL statement
Create a table named “EXECUTIVES” using a DDL statementCREA TE TABLE "EMPLOYEES"
 ( "ID" NUMBER (*,0) NOT NULL ENABLE ,
 "CODE" NUMBER (*,0) NOT NULL ENABLE ,
 "NAME" VARCHAR2 (1024 BYTE ),
 CONSTRAINT "EMPLOYEES_PK" PRIMAR Y KEY ("ID"),
 CONSTRAINT "EMPLOYEES_UQ" UNIQUE ("CODE" )); 
CREA TE TABLE "EXECUTIVES"
 ( "ID" NUMBER (*,0) NOT NULL ENABLE ,
 "CODE" NUMBER (*,0) NOT NULL ENABLE ,
 "AREA" VARCHAR2 (1024 BYTE ),
 CONSTRAINT "EXECUTIVES_PK" PRIMAR Y KEY ("ID"),
 CONSTRAINT "EXECUTIVES_FK" FOREIGN KEY ("CODE" ) REFERENCES EMPLOYEES (CODE ),
 CONSTRAINT "EXECUTIVES_UQ" UNIQUE ("CODE" ));

---

## Q2: Can you explain DML statements in regards to SQL?

**Answer:**

DML stands for Data Manipulation Language, which are used for managing data within schema objects. SELECT – retrieve data from the a database,
INSER T – insert data into a table, UPDA TE – updates existing data within a table, DELETE – deletes all records from a table, the space for the records
remain, CALL – call a PL/SQL or Java subprogram, EXPLAIN PLAN – explain access path to data, and LOCK T ABLE – control concurrency are DML
statements.
To insert data into abbe tables:
Also, note that DCL stands for Data Control Language (DCL) statements with GRANT – gives user ’s access privileges to database and REV OKE – withdraw
access privileges given with the GRANT command, and TCL statements stand for T ransaction Control (TCL) with statements are COMMIT – save work done,
SAVEPOINT – identify a point in a transaction to which you can later roll back, ROLLBACK – restore database to original since the last COMMIT , and SET
TRANSACTION – Change transaction options like isolation level and what rollback segment to use.

---

## Q3: Explain inner and outer joins?

**Answer:**

Joins allow database users to combine data from one table with data from one or more other tables (or views, or synonyms). T ables are joined two at a timeCREA TE SEQUENCE "EMPLOYEES_SEQ" MINV ALUE 1 MAXV ALUE 999999 INCREMENT BY 1 START WITH 1 CACHE 20
NOORDER NOCYCLE ;
CREA TE SEQUENCE "EXECUTIVES_SEQ" MINV ALUE 1 MAXV ALUE 999999 INCREMENT BY 1 START WITH 1 CACHE 20
NOORDER NOCYCLE ;
NSER T INTO EMPLOYEES (ID, CODE , NAME ) VALUES (EMPLOYEES_SEQ .NEXTV AL, 10, 'JOHN' );
NSER T INTO EMPLOYEES (ID, CODE , NAME ) VALUES (EMPLOYEES_SEQ .NEXTV AL,20, 'Sam' );
NSER T INTO EMPLOYEES (ID, CODE , NAME ) VALUES (EMPLOYEES_SEQ .NEXTV AL,30, 'Peter' );
NSER T INTO EMPLOYEES (ID, CODE , NAME ) VALUES (EMPLOYEES_SEQ .NEXTV AL,40, 'Dan' );
NSER T INTO EMPLOYEES (ID, CODE , NAME ) VALUES (EMPLOYEES_SEQ .NEXTV AL, 50, 'Emma' );
NSER T INTO "EXECUTIVES" ("ID","CODE" , "AREA" ) VALUES (EXECUTIVES_SEQ .NEXTV AL, 20, 'REGION1' );
NSER T INTO "EXECUTIVES" ("ID","CODE" , "AREA" ) VALUES (EXECUTIVES_SEQ .NEXTV AL, 50, 'REGION2' );

making a new table containing all possible combinations of rows from the original two tables.
Joins with venn diagram
Employees T able
Executives T able
Inner join
select em.*, ex.id as exec_id , ex.code as exec_code , ex.area from Employees em, Executives ex
where em.code = ex.code ;
or

SQL inner join
Left outer join
SQL left outer join
Right outer joinselect em.*, ex.id as exec_id , ex.code as exec_code , ex.area from Employees em
inner join Executives ex on em.code = ex.code ;
select em.*, ex.id as exec_id , ex.code as exec_code , ex.area from Employees em
eft outer join Executives ex on em.code = ex.code ;
select em.*, ex.id as exec_id , ex.code as exec_code , ex.area from Employees em

SQL right outer join
Full outer join
SQL full outer join

---

## Q4: What is a self join?

**Answer:**

A self-join is a join of a table to itself. In certain scenarios, a self join is a better alternative to a sub-query .
Step 1: If you want to add a new column called “Manager” to “Employees” table, you cal use the AL TER DDL. right outer join Executives ex on em.code = ex.code ;
 select em.*, ex.id as exec_id , ex.code as exec_code , ex.area from Employees em
 full outer join Executives ex on em.code = ex.code ;

Step 2 : To update who the manager is with the following DML
SQL all employees with manager_code
Step 3 : SQL self join can be used to print employee name and manager name side by side. Basically , you are joining the same table to each other .
SQL self join

---

## Q5: What is a sub-query? How does a sub-query impact on performance? What are the advantages and disadvantages of sub-queries?

**Answer:**

It is possible to embed a SQL statement within another . When this is done on the WHERE or the HA VING statements, we have a subquery construct. What
is subquery useful for? It is used to join tables and there are cases where the only way to correlate two tables is through a sub-query .ALTER table EMPLOYEES add (MANAGER_CODE NUMBER (*,0));
UPDA TE EMPLOYEES set MANAGER_CODE =20 where CODE IN (10,40);
UPDA TE EMPLOYEES set MANAGER_CODE =50 where CODE IN (30);

There can be performance problems with sub-queries.The above query can be re-written as a left outer join for a faster performance as shown below:
Similarly , the following sub query
can be rewritten with a “ right outer join ” as shown belowSELECT emp.name FROM employees emp WHERE
emp.code NOT IN (SELECT code FROM executives );
SELECT emp.name FROM employees emp
eft join executives exec on emp.code = exec.code
where exec.code is null;
SELECT exec.name FROM executives exec WHERE
exec.code NOT IN (SELECT code FROM enployees );
SELECT exec.name FROM employees emp

Replacing sub queries with left/right joins with “is null” in where clause
Advantages:
1) Sub-queries allow you to use the results of another query in the outer query .
2) Sub-queries in some complex SQL queries can simplify coding and improve maintainability by breaking down the complex query into a series of logical
steps.
3) In some cases, subqueries are easier to understand than complex joins and unions.
Disadvantages:
When a sub-query is used, the query optimizer of the database server may have to perform additional steps like sorting the results, etc. Hence, in some cases
sub-queries can be less ef ficient than using joins.right join executives exec on emp.code = exec.code
where emp.code is null;

---

## Q6: What is a correlated sub-query?

**Answer:**

A query is called correlated sub-query when both the inner query and the outer query are interdependent. For every row processed by the inner query , the
outer query is processed as well. The inner query depends on the outer query before it can be processed.
If a subquery is not dependent on the outer query it is called a non-corr elated subquery .

---

## Q7: Can you explain aggregate SQL functions?

**Answer:**

SQL provides aggregate functions to assist with the summarization of lar ge volumes of data.
SQL aggregate
SELECT outer .product_name FROM product outer   
WHERE outer .product_id = (SELECT inner .product_id FROM order_items inner   
 WHERE outer .product_id = inner .product_id );
SELECT SUM (QTY ) AS Total FROM Orders ; -- Total = 75
SELECT AVG(UnitPrice * QTY ) As AveragePrice FROM Orders ; -- Average 262.50

SQL aggregate

---

## Q8: How can you compare a part of the name rather than the entire name?

**Answer:**

You can use wild card characters like:
* ( % in oracle) : Matches any number of characters.
? ( _ in oracle) : Matches a single character .
To find all the employees who have “au” in their names

---

## Q9: How do you get distinct entries from a table?

**Answer:**

The SELECT statement in conjunction with DISTINCT lets you select a set of distinct values from a table.SELECT FIRSTNAME ,SUM (QTY ) FROM orders
 GROUP BY FIRSTNAME
 HAVING SUM (QTY ) > 25; -- John 45
SELECT * FROM employees emp
 WHERE emp.name LIKE ‘%au%’;
SELECT DISTINCT name FROM employees ;

---

## Q10: How can you find the total number of records in a table?

**Answer:**

Use the COUNT key word:

---

## Q11: In your experience, what are some of the common mistakes developers make related to SQL?

**Answer:**

1. Cartesian joins
SQL Joins are used to relate information in dif ferent tables. A Join condition is a part of the sql query that retrieves rows from two or more tables. A SQL Join
condition is used in the SQL WHERE Clause of select, update, delete statements. If a sql join condition is omitted as shown below:
or if the condition is invalid, then the join operation will result in a Cartesian product. The Cartesian product returns a number of rows equal to the product of all
rows in all the tables being joined. For example, if the first table has 20 rows and the second table has 10 rows, the result will be 20 * 10, or 200 rows. This
query will take a long time to execute.
2. Use of SELECT *
For example, a common misuse of SELECT * is to extract a set of all employees and to insert them into another table called Contractors with the same structureSELECT COUNT (*) FROM employees WHERE emp.name LIKE ‘P%’;
SELECT col1, col2, col3
FROM table_name1 , table_name2

The above query does the job, however , one day business requirements change and two new columns are added to the Employees table:
All of sudden the query that extracts from the Employees table and insert records into the Contractor table results in error .
“Insert Error: Column name or number of supplied values does not match table definition.”
The fix is to explicitly list the column names in the query:
3. Not using Pr epar ed statements . Prepared statements are more secured and ef ficient than the ordinary statements. Prepared statements prevent SQL injection
attacks.
4. Using the pr edicate “LIKE” in indexed columns . The “LIKE” predicate typically performs a search without the normal performance benefit of indexes.
Using ‘=’, ‘<>‘, etc instead of “LIKE” will increase performance. Also should be aware of that case sensitivity (e.g., ‘A ’ versus ‘a’) may be dif ferent based upon
database Server or configuration.
5. Over use of cursors in stor ed pr ocedur es. If possible, avoid using SQL stored proc cursors. They generally use a lot of Server resources and reduce the
performance and scalability of your applications. If you need to perform row-by-row operations, try to find another method to perform the task.NSER T INTO Contractors
SELECT * FROM Employees WHERE emp_type = 'C';
ALTER TABLE Products
ADD effective_start_date DATETIME , effective_end_date DATETIME ;
NSER T INTO Contractors (emp_id , emp_name )
SELECT emp_id , emp_name FROM Employees WHERE emp_type = 'C';

---

## Q12: What is the dif ference between “T runcate” and “Delete” commands?

**Answer:**

TRUNCA TE is useful for pur ging a table with huge amount of data. Alternatively , you can drop the table and recreate it that makes sense. Firing a delete
command instead of a truncate command to empty a table with millions of records can result in locking the whole table and also can take longer time to
complete, and at times cause the machine to hang.
a) TRUNCA TE T ABLE_NAME always locks the table and page but not each row , whereas DELETE statement is executed using a row lock, each row in the
table is locked for deletion.
b) Truncate removes all the records in the table whereas delete can be used with WHERE clause to remove records conditionally . That is remove only a handful
number of records.
c) Truncate performance is much faster than Delete, as its logging is minimal wheres the Delete command logs every record.
d) Truncate does not retain the identity , whereas DELETE command retains the identity . When you use T runcate, If the table contains an identity column, the
counter for that column is reset to the seed value that is defined for the column.
e) Truncate cleans up the object statistics and clears the allocated space whereas Delete retains the object statistics and allocated space.
f) TRUNCA TE is a DDL (Data Definition Language) and DELETE is a DML (Data Manipulation Language).
g) Data removed by TRUNCA TE command cannot be generally rolled back unless the database server specifically supports it. The DELETE command can
rollback a transaction.
h) The TRUNCA TE command does not fire any triggers, whereas the DELETE command fires any triggers defined on the table. For example, to keep an audit
trail of records that have been deleted by inserting the deleted records into an audit table via the DELETE triggers.
Q. Which command will you use to periodically pur ge data from your tables as part of a house keeping job?
A. Use a DELETE command within a transaction with a WHERE clause to remove data that are older than 7 years. Remove large amount of data in batches as
opposed to in a single transaction.

---

## Q13: How will you go about deleting a few records from parent and child tables where the parent table with parent_name = ‘Peter ’?

**Answer:**

Firstly , you need to delete the child records because the integrity constraint won’ t let you delete the parent record when there are child records.TRUNCA TE TABLE table_name

Now , the parent table can be deleted as shown below

---

## Q14: What do you do with the PURGE command?

**Answer:**

The purge command is used to clear the recycle bin. It is generally used with the DROP command. For example,
the above command will clear away the table from database as well as from the recycle bin. After executing the pur ge command, you cannot retrieve the table
using a flashback query .

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
