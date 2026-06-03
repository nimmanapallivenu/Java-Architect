# SQL Subqueries

> **Module**: Database and SQL  
> **Topic**: SQL Subqueries

---

## 📋 Table of Contents



- [Q1: What is a subquery?](#q1)
- [Q2: What is a correlated subquery ?](#q2)
- [Q3: What are the advantages and disadvantages of using a subquery?](#q3)

---

## Q1: What is a subquery?

**Answer:**

Subquery or Inner query or Nested query is a query in a query. A subquery is usually added in the WHERE clause of the sql statement. A subquery can be
nested inside a SELECT, INSER T, UPDA TE, or DELETE statement or inside another subquery. Subqueries are an alternate way of returning data from
multiple tables.
Q. Can you create a subquery in a From clause?
A. Yes. Subqueries can be used in From, Wher e and Having clauses. For example, in Sybase
Returns :
Joining virtual tables is one of the most powerful feature of subqueries. V irtual in this context means the result set you are joining is built on the fly. Here is a
more advanced example :select *
from
 select 'A' as colVal
 union
 select 'B' as colVal
 data
colVal
-----
A
B

---

## Q2: What is a correlated subquery ?

**Answer:**

A query is called correlated subquery when both the inner query and the outer query are interdependent. For every row processed by the inner query, the
outer query is processed as well. The inner query depends on the outer query before it can be processed.declare @clientId varchar (30),
 @reportDate date,
 
set nocount on
select reportId from 
 Report_Status s, 
 ReportKey k, 
 ReportGroupKey gk, 
 
 --subquery in from clause
 (select max(s.createddttm ) as maxdate, k1.clientId from 
 Report_Status s, 
 ReportKey k1, 
 ReportGroupKey gk 
 where k1.InactiveFlag ='N' 
 and gk.InactiveFlag ='N' 
 and gk.KeyId = k1.Id 
 and gk.Id = s.GroupKeyId 
 group by k1.clientId 
 ) maxdates 
 
 where k.InactiveFlag ='N' 
 and gk.InactiveFlag ='N' 
 and gk.KeyId = k.Id 
 and gk.Id = s.GroupKeyId 
 and s.CreatedDtTm = maxdates .maxdate 
 and k.ClientId = @clientId
 and maxdates .ClientId = k.ClientId
 and k.reportDate = @reportDate

If a subquery is not dependent on the outer query it is called a non-corr elated subquery .

---

## Q3: What are the advantages and disadvantages of using a subquery?

**Answer:**

Advantages :
Subqueries allow you to use the results of another query in the outer query .
Subqueries in some complex SQL queries can simplify coding and improve maintainability by breaking down the complex query into a series of logical
steps.
In some cases, subqueries are easier to understand than complex joins and unions.
Disadvantages :
 When a subquery is used, the query optimizer of the database server may have to perform additional steps like sorting the results, etc. Hence, in some
cases subqueries can be less ef ficient than using joins. So, favor joins to subqueries.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »SELECT outer .product_name FROM product outer
WHERE outer .product_id = (SELECT inner .product_id FROM order_items inner 
 WHERE outer .product_id = inner .product_id );

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03