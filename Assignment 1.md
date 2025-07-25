Create an empty database named a
%sql sqlite:///csssy. Db

Create a table with id, first name, last name, age, gender,  gpa, id
```
%%sql
CREATE TABlE students (
	id INTEGER PRIMARY KEY,
	firstname CHAR(15),
	lastname CHAR (15),
	age INTEGER,
	gender CHAR(1),
	gpa double
)
```

Modify the table replace the age attribute with a DOB attribute in the table students. 

ALTER table students drop column age
Alter table students add column dob date;

Insert into students (id, firstname,  lastname, dob, gender, gpa)
VALUES 
(),
(),
();


Write a query to compute lettergrade of each row in the transcript table, and show student id, course id, and lettergrade of all rows in the transcript table. 

%%sql
SELECT studentid, course id,
	CASE 
		WHEN mark ≥ 3.5 THEN ‘A’
		 ELSE ‘F’
	END AS lettergrade
FROM transcript;
%%


WRITE A query to show the first name and last name of the customers with such first name that their name starts with ‘M’, and finished with an ‘A’
%%sql
SELECT firstname, lastname
FROM Customer
WHERE firstname LIKE ‘M%r%a’
%%

Write a query to show names of the branches and first name and last name of their managers. 
%%sql
Select B.branchName, E.firstName, E.lastName
FRom Branch as B
JOIN Employee as E on b.managerSIN = E.sin;
%%

Write a query to find the SIN, first name, and last name fo employees who share the same name with one or more customers.
%%sql
SELECT DIStinct E.sin, E.firstName, E.lastName
From employee as E
JOIN customer as c on e.firstname = c.firstname and e.lastname = c.lastname
%%

Write a query to show account number, account type, account balance, and transaction amount of the accounts with balance higher than 100,000 and transaction amounts higher than 15000, starting with accounts with the highest transaction amount and highest account balance.

First, join the account with transaction 
%%%sql
SELECT A.accountnumber, A.type, A.balance, T.amount
FROM Account as A
Join transactions as T on t.accnumber = a.accnumber 
Where A.balance > 100000 and T.amount ≥ 15000
Orderby T.amount, A.balance
%%

Write a SQL query to find the customer ID, first name, and last name of customers who own accounts at London and Berlin branches, order by last name and first name
%%sql
Select C.customerID, c.firstName, c.lastName
From customer as C
Join owns as O on O.customerId = c.customerID
Join Account as A on A.accnumber = O.accnumber
Join Branch as B on B.branchnumber = A.branchnumber
Where B.branchName in (‘London’, ‘Berlin’)
GROUP by C.customerid, c.firstnae, c.lastname
Having count (DIstinct B.branchName) = 2
Order by c.lastname, c.firstname
%%

Sin, branch name, salary and manager’s salary - salary of all employees in new york, longon, or berlin order by ascednignd

%%sql
Select sin, branch name, E.salary, m.salary - e.salary as salarydiff
From employee E
Join branch on E.branchnumber = b.branchnumber
Join employee M on B.manager. Sin = M.sin
Where B.branchname in London, newyork, berlin
Order by salarydiff
%%

Firstname, lastname, and income of customers whose income is at least twice the income of any customer whose lastName is butler, orderby lastname then first name
%%sql
Select c.firstname, c.lastname,  c.income
From customer c
Where c.income ≥ (select MAX B income from customer B where B.lastname = ‘butler’)
Order by c.lastname, c.firstname
%%

Customer id, income, account numbers and branch numbers of customers with income greater than 90000 who own an account at both london an dlatveria branches, order by customer id then account number. The result should ocntain all the account numbers of customers who meet the criteria, even if the acount itself is not held at lonodon or latveria
%%sql
Select c.customer id, c.income, O.accnumber, A.branchnumber
From customer c
Join owns O on c.customerid = o.customer id
Join account a on o.accnumber = a.accnumber
Where c.income > 90000
And c.customer id in 
(select o 1. Customer id from owns o 1 join account a 1 on o 1. Accnumber = a 1. Accnumber
Join branch b 1 on a 1. Branchnumber = b 1. Branchnumber where b 1. Branchnumber = london)

Order by c.customer id, o.accnumber
%%
Customer id, types, account numbers and balances of business and savings accounts owned by customers who own at least one business account or at least one sacings account, order by suctomer id, then type then account number. 

%%sql
Select 
From owns a
Join account a on O.accnumber = a.accnumber
Where A.type in bys, sav
And o.customer id in (select distinct 01. Customer id )
%%

Branch name, account number, and balance of accounts with balances greater than 110000 helf at the branch managed by Philip edwards order by account number
%%sql
Select b.branchname, a.accnumber, a.balance
From account a
Join branch b on b.branchnumber = a.branchnumber 
Join employee e on manager sin = e.sin
Where a.balance > 110000
And e.firstname = phillip
%%

Customer id of customer who have an account at new work branch, who do not won an account at the london branch and who do not co own an account with another customer who owns an account at the london branch order by customer id. The result should not contain duplicate customer id.

Branch name, account type, and average transaction amount of each account type for each branch for branches that have at least 50 accounts combined
%%sql
Select b.branchname, a.type, acg (t.amount) as avg_transaction_anmount
From branch b
Join account A on b.branchnumber = a.branch number
Join transaction t on a.accnumber = t.accnumber
Where b.branchnumber in
Select a 1. Branchnumber
From account a 1
Group by a 1. Branchnumber
Having count (a 1. Accnumber) ≥50
Group by 

%% 
Sum of the employee salaries at the new york branch
Select sum (e.salary) as total_sararl
From Employee e
Join branch b on e.branchnumber = b. branchnumber
Where b.branchname=’newyork’

Customer id, firstname, lastname of customers who own accounts at four different banks order by last name and first name
%%sql
Select c.customer id, c.firstname, c.lastname 
From customer c
Join owns as O on owns. Id = c, customer. Id
Join account as a on o.account number
Group by c.customerid, c.firstname, c.lastname
Having count Distinct (a.branchnumber =4)
Order by

%% 
Average income of customers older than 60 on June 12, 2023, and the average income of customers younger than 2- on June 12, 2024. The resul tmust have two named columns with one row in a single result set.

%%sql
Select
	Avg (case when birthdate <‘1963-06-12’ then income end) as avg_ioncome over 60
	Avg (case wehn birthdate > ‘2003-06-12’ then income end as ave_income under 20
%%

Customer id, lastname, first name, income, and average account balance of customers who have at least three accounts and whose last names begin with S and conttain an e or whose first names begin with A and have the letter n just before the last 2 letters. Oderr by customoer id. 
%%sql
Select c.customerid, c.lastname. C.firstname, c.income, AVG (a.balance) as avg
From customer c
Join owns o on customer id = customerid'
Join account a on accnumber = o.accountnumber
Where c, customerid in 
Select (o.customer id
From own o
Group by o 1. Customerid
Having count distinct accnumber > 3)
And c lastname like 
%%

Account number, balance, sum of transaction amounts and balance - transaction sum for accounts in the london branch that have at least 15 transactions order by the sum of the transactions

%%sql
Select a.account number, a.balance, sum (t .ammount) as transaction_sum, A.balance - T.amnt as balance after transactions
From account a
Join transactions t on t.accnumber = a.accnumber
Join branch b on a.branchnumber = b.branchnumber
Where b.branchname = london
Group by a.accnumber, a.balance
Having count by t.trnasnumber ≥ 15
Order by trnasaction sum

%% 
Branch name, account type, and average transaction amoun tof each account type for each branch for branches that ahve at least 50 accounts combined order by branch name then account type. 
%%sql
Select name, a.type, AVG (t.ammount)
From branch b
Join account A on b b, branchnumber = a.branchnumber
Join transaction t on a.accnumber = t.accnumber
Where b.branchnumber in 
Select a 1. Branchnumber
From account a 1
Group by a 1. Branchnumber
Having count account. Number ≥ 50
Group by name, a.type
Order by name, a.type
%%

Branch name, account type, account number, transcatio number and amount of transactions of accounts where the average transaction amount is greater than three times the average transaction amount of accounts of that type. For exmaple, if the average transaction amount of all business accounts is 2000 then return transactions from business accounts where the average transaction amount for that account is greater than 6000. 
%%sql
Select b.name, a.type, a.number, t.number, t.amount
From branch b
Join account a on branchnumber = b.branchnumber
Join transaction t on a.accnumber = t.accnumber
Where a 1. Number in (
	Select t 1. Accnumber
	From t 1.
	Group by t 1. Accnumber
	Having avg t 1. Amount > 3 * (
		Select avg. T 2. Amount
		From t 2
		Join a 2 on t 2. Accnumber = a 2. Accnumber
		Where a 2. Type  = a.type
	)
%%

1. Find the accounts with the same type 
   