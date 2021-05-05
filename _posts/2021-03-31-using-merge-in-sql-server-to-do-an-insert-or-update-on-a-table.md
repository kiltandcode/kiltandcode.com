---
title: "Using MERGE in SQL Server to do an INSERT or UPDATE on a Table"
date: 2021-03-31 21:00:00:00 +00:00
author: steven
layout: post
image: /assets/img/2021/03/sql-server-logo.png
icon: sql-server
tags: sql-server
---

I've never really made much use of the *MERGE* statement in SQL Server, but the other day I wanted to do 
either an *INSERT* or an *UPDATE* on a table, depending on if there's a match on a particular column.

For example, if I had the table *'Contacts'* containing names, addresses, phone numbers and email 
addresses, I don't want the table to include duplicate email addresses:

{%
    include image.html
    year='2021'
    month='03'
    file='sql-server-contact-table.png'
    alt='Table in SQL Server'
    caption='Table in SQL Server'
%}

If I try to add a new contact to the table, the *MERGE* statement in SQL Server will either insert a 
new row, or alternatively update an existing row that contains the same email address:

```sql
MERGE [dbo].[Contacts] AS TARGET
USING
	(VALUES (@EmailAddress))
	AS SOURCE (EmailAddress)
ON
	TARGET.EmailAddress = SOURCE.EmailAddress
WHEN NOT MATCHED BY TARGET THEN
INSERT (
	[FirstName],
	[LastName],
	[Address1],
	[Address2],
	[City],
	[Province],
	[PostalCode],
	[EmailAddress],
	[PhoneNumber]
) VALUES (
	@FirstName,
	@LastName,
	@Address1,
	@Address2,
	@City,
	@Province,
	@PostalCode,
	@EmailAddress,
	@PhoneNumber
)
WHEN MATCHED THEN 
UPDATE SET 
    [FirstName] = @FirstName,
    [LastName] = @LastName,
    [Address1] = @Address1,
    [Address2] = @Address2,
    [City] = @City,
    [Province] = @Province,
    [PostalCode] = @PostalCode,
    [PhoneNumber] = @PhoneNumber;
```

If I need to use *SCOPE_IDENTITY()*, I can do the following to either retrieve the identity of the 
inserted row if it's an *INSERT*, or the identity of the affected row if it's an *UPDATE*:

```sql
DECLARE @ContactId INT

MERGE [dbo].[Contacts] AS TARGET
USING
	(VALUES (@EmailAddress))
	AS SOURCE (EmailAddress)
ON
	TARGET.EmailAddress = SOURCE.EmailAddress
WHEN NOT MATCHED BY TARGET THEN
INSERT (
	[FirstName],
	[LastName],
	[Address1],
	[Address2],
	[City],
	[Province],
	[PostalCode],
	[EmailAddress],
	[PhoneNumber]
) VALUES (
	@FirstName,
	@LastName,
	@Address1,
	@Address2,
	@City,
	@Province,
	@PostalCode,
	@EmailAddress,
	@PhoneNumber
)
WHEN MATCHED THEN 
UPDATE SET 
    [FirstName] = @FirstName,
    [LastName] = @LastName,
    [Address1] = @Address1,
    [Address2] = @Address2,
    [City] = @City,
    [Province] = @Province,
    [PostalCode] = @PostalCode,
    [PhoneNumber] = @PhoneNumber,
    @ContactId = TARGET.[ContactId];

SELECT ISNULL(@ContactId, SCOPE_IDENTITY())
```
