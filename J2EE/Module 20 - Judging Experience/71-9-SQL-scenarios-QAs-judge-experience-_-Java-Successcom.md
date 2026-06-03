# 71 9 SQL scenarios Q&As judge experience   Java Success.com

## Table of Contents

Q1 How will you go about identifying duplicate records in a table?
A1 The following SQL query will do the trick.
Note: Interviewer is testing if you understand that the aggregate queries need to have a “group by” clause on columns that are not aggregated. E.g. code,
user_name. Finding duplicate records is a real life scenario.
Q2 How would you go about deleting the duplicate records?
A2 You could do it in a number of steps as shown below .
Create a temporary table.
Insert the unique records into the temporary table.
Delete the records from the original table.
Insert the saved single records from the temporary table back to the original table.
Q3 How will you go about searching for table and column names that you don’ t know where they really are? For example, search for a column name to find out
in which tables they do exist.
A3 You need to query the database system tables. For example, in Sybase, you can query it as shown below .SELECT code , user_name , COUNT (user_name ) AS NumOccurrences
FROM tbl_user
GROUP BY code , user_name
HAVING ( COUNT (user_name ) > 1 )
select a.name , b.name
from sysobjects a, syscolumns b

Q4 How will you go about writing an SQL query for the following scenario?
database table structure
Calculation table with the following columns PortfolioId , AccountId , Balance , InActiveFlag , CalculationDate , and CalcT ypeCd . The Portfolio table
has columns PortfolioId , and PortfolioCd . The Account table has columns AccountId and AccountCd .
Write an SQL query to extract out the Accountcd and the corresponding Balance for a given Portfoliocd and CalculationDate . Please note that there will be
multiple balance records for each account, and your query must only extract out a single balance record per account based on the rule ‘extract the record with
minimum value for CalcT ypeCd ‘.where a.id = b.id
and b.name like '%split_income%'

A4 As you can see in the sample answer below , inner joins are used to join with the relevant tables. A sub query is used to calculate the min(CalcT ypeCd) to
extract the record with minimum value for CalcT ypeCd .
Q5 If you need to map actual values retrieved from the database to some other value and then sort by these translated values as well, how will you go about
accomplishing this in your SQL code?
For example , StatusCd is the column in the Portfolio table, and it can have the values of New , and Processed. But the SQL query should return a status of
‘Excluded’ if the ExcludedFlag column is set yes, and ‘Sent’ if the SentDateT ime is not null. iIf none of the above conditions are met, then return the StatusCd
as in the database. The sorting needs to be carried out in the order of ‘ New ‘, ‘Processed ‘, ‘Sent ‘, and then ‘ Excluded ‘.
A5 This can be achieved with a switch or case statement. The syntax of switch/case statement can vary among databases. Here is a sample SQL based on
Sybase database server .
case-when|else-end statementselect acc.AccountCd , calc.Balance
from Calculation calc
 inner join Portfolio pf on pf.PortfolioId = calc.PortfolioId
 inner join Account acc on acc.AccountId = calc.AccountId
where pf.PortfolioCd = 'P456'
and calc.CalculationDate = '25 Feb 2015'
and calc.InActiveFlag = 'N'
and acc.InActiveFlag = 'N'
and calc.CalcT ypeCd = (select min(calc2 .CalcT ypeCd ) from calculation calc2
 where calc2 .CalculationDate = calc.CalculationDate
 and calc2 .InActiveFlag = 'N'
 and calc2 .AccountId = calc.AccountId
 group by AccountId )
order by acc.AccountCd

Q6 How would you retrieve a date time column converted to string and formatted as dd/mm/yy hh:mm:ss
A6 You can use specific functions provided by your database server . These functions are specific to the database server you are using, hence your code cannot
be ported to other database servers. Here is an example in Sybase.SELECT PortfolioCd , SentDateT ime, ExcludedFlag , StatusCd as ActualStatusCd ,
 case
 when p.ExcludedFlag = 'Y' then 'Excluded'
 else
 case
 when p.SentDateT ime is null then p.StatusCd
 else 'Sent'
 end
 end as EvaluatedStatusCd
 
FROM Portfolio p WHERE calculationdate > '09 Jan 2013' and InActiveFlag = 'N'
ORDER BY case
 when p.ExcludedFlag = 'Y' then '4'
 else
 case
 when p.SentDateT ime is not null then '3'
 else
 case
 when p.StatusCd = 'New' then '1'
 when p.StatusCd = 'Processed' then '2'
 end
 end
 end, PortfolioCd 
 
SELECT PortfolioCd , convert (char(11), p.SentDateT ime, 103) + convert (char(12), p.SentDateT ime, 108) as SentDateT ime

In the above example, the convert function is used to convert the date time field to char . The 103 in Sybase means dd/mm/yy format and and 108 to convert to
the time format hh:mm:ss .
Q7 How will you go about tuning your SQL and stored procedures?
A7 You can use tools like DB Artisan, T OAD, etc to analyse the query plan. The code (in Sybase) below gives you the elapsed time.
Q8 How will you go about tuning your SQL and stored procedures?
A8 You can use tools like DB Artisan, T OAD, etc to analyse the query plan. The code below gives you the elapsed time.
Proper indexing is key to get good performance e out of your SQL queries.
Q9 What are all the dif ferent types of indexes?
A9 There are three types of indexes
Unique Index: does not allow the field to have duplicate values if the column is unique indexed. Unique index can be applied automatically when primary key
is defined.FROM Portfolio p
WHERE calculationdate > '09 Jan 2013' 
AND InActiveFlag = 'N'
DECLARE @start datetime , @stop datetime
SET @start = GETDA TE()
exec MY_PROC 'AC345' , '02 Jan 2013' , null, 'N'
SET @stop = GETDA TE()
elect datedif f(ms, @start, @stop)

Cluster ed Index: reorders the physical order of the table and search based on the key values. Each table can have only one clustered index.
NonCluster ed Index: does not alter the physical order of the table and maintains logical order of data. Each table can have 999 non-clustered indexes.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
