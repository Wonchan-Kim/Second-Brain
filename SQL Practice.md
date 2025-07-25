Check constraints

Syntax - check (conditional expressions)

Create table products (
	Pcode Integer Primary Key
	Pname varchar (10)
	Pdesc varchar (20)
	Ptype varchar (20)
	Price numeric (6,2) CHECK (price > 0 )
	Check (Ptype IN (‘Book’, ‘Movie’)
);

Create table invoices (
	Invid integer Primary Key,
	Ordid Interger not null unique,
	Amount numeric (8,2) CHECK (amount > 0),
	Issued data,
	Due DATE,
	CHECK Ordid in SELECT ordid from Orders,
	Check (due ≥ issued)
);

Similar to the foreign key, but not the same

Domain Constraints
Is essentially a data type with optional constraints

Create Domain name datatype 