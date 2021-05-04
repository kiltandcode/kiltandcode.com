---
title: "Auto-Generate Insert Statements With Data Using SQL Server Management Studio"
date: 2021-04-30 19:00:00:00 +00:00
author: steven
layout: post
image: /assets/img/2021/04/ssms-generate-scripts.png
icon: sql-server
tags: sql-server ssms
---

If you are working with a database in SQL Server Management Studio *(SSMS)*, you may find you need a 
way to auto-generate INSERT statements with the data from a table. This might be for inserting the data 
into a different database for testing purposes, or simply as a backup.

The **Generate Scripts** feature of SQL Server Management Studio will let you choose an area of the 
database *(e.g. a table)* for generating Transact-SQL scripts. It will let you choose one or more 
database objects and you can generate scripts for the **schema**, the **data** or **both**.

You can find the Generate Scripts feature in the context menu of the database. **Right-click on the database**, 
navigate to **Tasks** and then **Generate Scripts**:

{%
    include image.html
    year='2021'
    month='04'
    file='ssms-context-menu.png'
    alt='Generate Scripts feature in the context menu'
%}

Click **Next** in the **Introduction**:

{%
    include image.html
    year='2021'
    month='04'
    file='ssms-generate-scripts.png'
    alt='Introduction for Generate Scripts'
%}

In **Choose Objects**, choose a database object, or multiple objects:

{%
    include image.html
    year='2021'
    month='04'
    file='ssms-choose-objects.png'
    alt='Choose Objects for Generate Scripts'
%}

In **Set Scripting Options**, click **Advanced**:

{%
    include image.html
    year='2021'
    month='04'
    file='ssms-set-scripting-options.png'
    alt='Set Scripting Options for Generate Scripts'
%}

This is important as you can now tell the *Generate Scripts* feature to generate scripts 
for the **schema**, the **data** or **both**:

{%
    include image.html
    year='2021'
    month='04'
    file='ssms-advanced.png'
    alt='Advanced menu for Set Scripting Options'
%}

If you chose to generate scripts for both the schema and the data, you should see the 
following:

{%
    include image.html
    year='2021'
    month='04'
    file='ssms-insert-statements.png'
    alt='INSERT statements with data'
%}