Creating tables

Basic syntax 

Create table <name> (
<column 1> <type 1>,
<col 2><type 2>
);

Ex)
Create table customer (
Custid varchar (10),
Name varchar (20),
City varchar (30),
Address varchar (30))
);

Varchar is variable lenth, at most n characters

Numbers smallint, int, bigint, numeric (p, s)

Default values
Syntax
Create table <>(
	<colname> <col 1 type> Default <value>

)

Populating table

Insert into <name> Values ( , , ,)

Alter table <name>
	Rename to <>
	 Rename <col> to <new col>
	 Add <col> <type>
	 Drop <colum>
	 Alter <col>
		 Type<type>
		 Set default <value>
		 Drop default

Basic queries in SQL

Select <list of attributes>
From <list of tables>
Where <condition>

Loop over all rows of the tabels listed in FROM
Take those that sitisfy the where condition
Output the values of attributes listed in select
