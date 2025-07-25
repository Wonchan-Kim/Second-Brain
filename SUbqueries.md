
Accountbs with a higher balance than the average of all accounts

Select A.number
From Account A
Where A.balance >  Select AVG A.balance from Account A 1

Aggregate functinos can only be used in Select and having

The where clause revisited
A <> B at least one of them are not equal to each other

<
Same as compare string in c++. Starts from the first element.


ANY term,,,, term op ANY query
True if there exists any of the reuslts of query such that term…term op is true

Ex> 3 < any 1,2,3 is false
3 < any 2,3,4 is true
What about 3 < any{} ? Its false

ALL

Term…term op ALL query
True iuf for all rows r_ in the results iof query op R is ture

3 < All{} all checks if the condition is true for all the elemtnets in the set, however, ther is no elemtn to camper in the empty set. Therefore, it is always true

Examples
ID of customers from LONDON who own an account
SELECT C.custid
From customer c
Where c.city = london and c.custid = any (select a.custid from account A);

Customers living in cities without a branch
Select *
From customer C
Where c.city <> All ( select A branch from Account A)

IN and NOT IN is same as ANY, <>ALL


For example, the first example above can be transformed into

Select c.custid
From customer C
Where c.city=london and c.custid in (select a.custid from account a)

Exists is true if the result of query is non empty


Correlated subqueries
All nested queries can refer to atttrivutes in the parent queries
Return customers who have an account in London

SELECT *
From customer C
Where exists ( select 1
			From account a
			Where branch = london
			And c.custid = A.id)
Customers living in a city without branch

Select *
From customer C
Where not exists (select * from account a, where a.branch = c.city)



Scoping 
A subquery has a local scope from clause
N outer scopes n is the level of nesting

Queries with and without having

Branches with a total balance of at least 500

Select a.branch
From account a 
Group by a. branch
Having sum (a.balance) ≥ 500

Same query without having
Select subquery. Branch
From (select a.branch, sum a.balance as total
From account a group by a.branch as subquery)
Where subquery. Total ≥ 500

Example aggregation on aggregates
Average of total balances across each customers accounts
1. Find the total balance across each customer’s accounts
2. Take the average of the total
Select avg (subquery. Total)
From select a.custid, sum a.balance as tot
From account a
Group by a.custid as subquery

 5