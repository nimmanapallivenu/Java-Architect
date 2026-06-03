# 14 SQL interview questions and answers you must know

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

---

## Q1: Can you explain DDL statements in regards to SQL?

**Answer:**

DDL stands for Data Definition Language, which are statements used to define the database structure or schema. CREA TE – to create objects in the database, ALTER – alters the structure of the database, DROP – delete objects from
the database, TRUNCA TE – remove all records from a table, including all spaces allocated for the records are removed, COMMENT – add comments to the data dictionary , and RENAME – rename an object.
Create a table named “EMPLOYEES” using a DDL statement
Create a table named “EXECUTIVES” using a DDL statement

---

## Q2: Can you explain DML statements in regards to SQL?

**Answer:**

DML stands for Data Manipulation Language, which are used for managing data within schema objects. SELECT – retrieve data from the a database, INSER T – insert data into a table, UPDA TE – updates existing data within a table,
DELETE – deletes all records from a table, the space for the records remain, CALL – call a PL/SQL or Java subprogram, EXPLAIN PLAN – explain access path to data, and LOCK T ABLE – control concurrency are DML statements.
To insert data into abbe tables:CREA TE TABLE IF NOT EXISTS EMPLOYEES
 (ID INT AUT O_INCREMENT ,
 CODE INT NOT NULL ,
 NAME VARCHAR (1024 ),
 PRIMAR Y KEY (ID),
 UNIQUE (CODE )
; 
CREA TE TABLE IF NOT EXISTS EXECUTIVES
 (ID INT AUT O_INCREMENT ,
 EMPLOYEE_ID INT NOT NULL ,
 CODE INT NOT NULL ,
 AREA VARCHAR (256),
 PRIMAR Y KEY (ID),
 FOREIGN KEY (EMPLOYEE_ID ) REFERENCES EMPLOYEES (ID),
 UNIQUE (CODE )
; 
NSER T INTO EMPLOYEES (CODE , NAME ) VALUES (10, 'JOHN' );
NSER T INTO EMPLOYEES (CODE , NAME ) VALUES (20, 'Sam' );
NSER T INTO EMPLOYEES (CODE , NAME ) VALUES (30, 'Peter' );
NSER T INTO EMPLOYEES (CODE , NAME ) VALUES (40, 'Dan' );
NSER T INTO EMPLOYEES (CODE , NAME ) VALUES (50, 'Emma' );

Also, note that DCL stands for Data Control Language (DCL) statements with GRANT – gives user ’s access privileges to database and REV OKE – withdraw access privileges given with the GRANT command, and TCL statements stand
for T ransaction Control (TCL) with statements are COMMIT – save work done, SAVEPOINT – identify a point in a transaction to which you can later roll back, ROLLBACK – restore database to original since the last COMMIT , and SET
TRANSACTION – Change transaction options like isolation level and what rollback segment to use.

---

## Q3: Explain inner and outer joins?

**Answer:**

Joins allow database users to combine data from one table with data from one or more other tables (or views, or synonyms). T ables are joined two at a time making a new table containing all possible combinations of rows from the
original two tables.
Joins with venn diagram
MySQL 8 – Example
Inner joinNSER T INTO EXECUTIVES (EMPLOYEE_ID , CODE , AREA ) VALUES (2, 20, 'REGION1' );
NSER T INTO EXECUTIVES (EMPLOYEE_ID , CODE , AREA ) VALUES (5, 50, 'REGION2' );
SELECT em.*, ex.code , ex.area

MySQL – INNER JOIN
Left outer joinFROM EMPLOYEES em
NNER JOIN EXECUTIVES ex
ON em.ID = ex.EMPLOYEE_ID ;
SELECT em.*, ex.code , ex.area
FROM EMPLOYEES em
LEFT JOIN EXECUTIVES ex
ON em.ID = ex.EMPLOYEE_ID
ORDER BY ID;

MySQL – LEFT JOIN
Right outer join
SELECT em.*, ex.code , ex.area
FROM EMPLOYEES em
RIGHT JOIN EXECUTIVES ex
ON em.ID = ex.EMPLOYEE_ID
ORDER BY ID;

MySQL – RIGHT JOINx
Full outer join
You don’ t have FULL JOINS on MySQL, but you can UNION. FULL OUTER JOIN operation would not produce any duplicate rows.
MySQL – UNION

---

## Q4: What is a self join?

**Answer:**

A self-join is a join of a table to itself. In certain scenarios, a self join is a better alternative to a sub-query .
Step 1: If you want to add a new column called “Manager” to “Employees” table, you cal use the AL TER DDL.SELECT em.*, ex.code , ex.area
FROM EMPLOYEES em
LEFT JOIN EXECUTIVES ex
ON em.ID = ex.EMPLOYEE_ID
UNION
SELECT em.*, ex.code , ex.area
FROM EMPLOYEES em
RIGHT JOIN EXECUTIVES ex
ON em.ID = ex.EMPLOYEE_ID
ORDER BY ID;

Step 2 : To update who the manager is with the following DML
MYSQL – SELF JOIN
Step 3 : SQL self join can be used to print employee name and manager name side by side. Basically , you are joining the same table to each other .ALTER table EMPLOYEES add (MANAGER_CODE INT);
UPDA TE EMPLOYEES set MANAGER_CODE =20 where CODE IN (10,40);
UPDA TE EMPLOYEES set MANAGER_CODE =50 where CODE IN (30);
SELECT e1.CODE , e1.NAME , e2.CODE , e2.NAME
FROM EMPLOYEES e1
NNER JOIN EMPLOYEES e2
ON e1.MANAGER_CODE = e2.CODE

MySQL – SELF JOIN RESUL T
Q. How will you count the total no. of employees and total no. of executives from the “EMPLOYEES” table?
A. SELF-JOIN
OUTPUT :

---

## Q5: What is a sub-query? How does a sub-query impact on performance? What are the advantages and disadvantages of sub-queries?

**Answer:**

It is possible to embed a SQL statement within another . When this is done on the WHERE or the HA VING statements, we have a subquery construct. What is subquery useful for? It is used to join tables and there are cases where the only
way to correlate two tables is through a sub-query .
Firstly , the inner SELECT query is executed and its result is sent to the outer SELECT query . Inner query knows nothing about the outer query .SELECT count (e1.code ) as tot_employess ,
count (e2.CODE ) as tot_executives
FROM EMPLOYEES e1
LEFT JOIN EMPLOYEES e2
ON e1.CODE = e2.CODE
AND e2.MANAGER_CODE is null
ot_employess tot_executives
--------------------------------
 5 2

OUTPUT :
There can be performance problems with sub-queries. The above query can be re-written as an ANTI-JOIN for a faster performance as shown below:
Similarly , the following sub query
OUTPUT :SELECT emp.name
FROM EMPLOYEES emp
WHERE emp.code NOT IN (SELECT code FROM EXECUTIVES );
name
JOHN
Peter
Dan
SELECT emp.name
FROM EMPLOYEES emp
LEFT JOIN EXECUTIVES exec
ON emp.code = exec.code
WHERE exec.CODE IS NULL
SELECT exec.name FROM executives exec WHERE
exec.code NOT IN (SELECT code FROM enployees );
name
JOHN
Peter
Dan

Replacing sub queries with left/right joins with “is null” in where clause
can be rewritten with a “ ANTI-JOIN ” as shown below
Returns nothing.
Q. What is an anti-join?
A. An “anti-join” between two tables returns rows from the first table where no matches are found in the second table. An anti-join is the opposite of a semi-join where it returns one copy of each row in the first table for which at least one
match is found. An anti-join returns one copy of each row in the first table for which no match is found.
Q. What are the advantages & disadvantages of sub-queries?
A.
Advantages:
1) Sub-queries allow you to use the results of another query in the outer query .
2) Sub-queries in some complex SQL queries can simplify coding and improve maintainability by breaking down the complex query into a series of logical steps.SELECT exec.code
FROM EXECUTIVES exec
WHERE exec.code NOT IN (SELECT code FROM EMPLOYEES );
SELECT emp.name
FROM EMPLOYEES emp
RIGHT JOIN EXECUTIVES exec
ON emp.code = exec.code
WHERE emp.CODE IS NULL ;

3) In some cases, subqueries are easier to understand than complex joins and unions.
Disadvantages:
When a sub-query is used, the query optimizer of the database server may have to perform additional steps like sorting the results, etc. Hence, in some cases sub-queries can be less ef ficient than using joins.

---

## Q6: What is a correlated sub-query?

**Answer:**

A query is called correlated sub-query when both the inner query and the outer query are interdependent. For every row processed by the outer query , the inner query is processed as well. The inner query depends on the outer query before
it can be processed.
If a subquery is not dependent on the outer query it is called a non-corr elated subquery .
A very popular interview question is:
Q. Given the following “ employee ” table, how will you calculate the second highest salary?
A. You can use the correlated subquery or the row_number() function.
Subquery approach
The output will be:SELECT outer .product_name FROM product outer   
WHERE outer .product_id = (SELECT inner .product_id FROM order_items inner   
 WHERE outer .product_id = inner .product_id );
emp_id , emp_name , emp_salary
1, Jogan , 45000.00
2, Samantha , 50000.00
3, Samuel , 55000.00
4, Smith , 75000.00
select max(emp_salary ) from employee
where emp_salary not in ( select max(emp_salary ) from employee );
max(emp_salary )
55000

Correlated subquery approach
Each salary in outer query is compared with the inner query .
Output:
row_number() function

---

## Q7: Can you explain aggregate SQL functions?

**Answer:**

SQL provides aggregate functions to assist with the summarization of lar ge volumes of data.
SQL aggregateselect emp_name from employee a
where 2 = ( select count (*)
 from employee b
 where a.emp_salary <= b.emp_salary );
emp_name
Samuel
select *
from (
 select emp_name ,
 rank() over ( order by emp_salary desc ) as sal_rank 
 from employee
 ) as e
where e.sal_rank = 2;

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

The SELECT statement in conjunction with DISTINCT lets you select a set of distinct values from a table.

---

## Q10: How can you find the total number of records in a table?

**Answer:**

Use the COUNT key word:SELECT SUM (QTY ) AS Total FROM Orders ; -- Total = 75
SELECT AVG(UnitPrice * QTY ) As AveragePrice FROM Orders ; -- Average 262.50
SELECT FIRSTNAME ,SUM (QTY ) FROM orders
 GROUP BY FIRSTNAME
 HAVING SUM (QTY ) > 25; -- John 45
SELECT * FROM employees emp
 WHERE emp.name LIKE ‘%au%’;
SELECT DISTINCT name FROM employees ;
SELECT COUNT (*) FROM employees WHERE emp.name LIKE ‘P%’;

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
