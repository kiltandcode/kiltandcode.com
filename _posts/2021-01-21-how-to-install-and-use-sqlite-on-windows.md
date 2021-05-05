---
title: "How to Install and Use SQLite on Windows"
date: 2021-01-21 21:45:00 +00:00
author: steven
layout: post
image: /assets/img/2021/01/sqlite-on-windows.png
icon: sqlite
tags: sqlite windows
---

Unlike modern versions of macOS, SQLite isn't installed by default on Windows. If you're using Windows 
like I am, you need to complete a few extra steps before you can start using it.

Let's begin to install SQLite on Windows by first downloading the executables from the 
[SQLite Download Page](https://www.sqlite.org/download.html).

You'll want to find the section **Precompiled Binaries for Windows** and download one of the zip files. In this 
guide I'll be using the bundle of command-line tools, simply because it's always useful to have the 
additional utilities on hand.

The bundle includes the executables **sqlite3.exe** for managing SQLite databases, **sqldiff.exe** for 
displaying the differences between SQLite databases and **sqlite3_analyzer.exe** that provides 
information on the space utilization of SQLite databases.

{%
    include image.html
    year='2021'
    month='01'
    file='precompiled-binaries-for-windows.png'
    alt='Precompiled Binaries for Windows'
%}

Once you've downloaded the zip file, extract the executables into the local folder **C:\sqlite**

{%
    include image.html
    year='2021'
    month='01'
    file='sqlite-executables-on-pc.png'
    alt='SQLite Executables on PC'
%}

## Edit PATH Environment Variable

You've now *downloaded* SQLite to your local Windows environment, but you haven't *installed* it yet. We could simply use the command prompt to navigate to the folder containing the executables each time we run SQLite, but who wants to do that?! Let's actually *install* SQLite by editing the PATH environment variable on Windows.

Let's begin by using the Windows Start Menu to search for *"environment variables"* to open the **Environment Variables** window located in **Settings**. You'll want to **locate the *'Path'* variable** and click **Edit**:

{%
    include image.html
    year='2021'
    month='01'
    file='environment-variables.png'
    alt='Environment Variables on Windows'
%}

Within *'Edit environment variable'* for the *'Path'* variable, **add a new entry with a value of *"C:\sqlite"***. This is telling Windows that if you type the name of an executable into the command prompt, it will include this path when looking for it's location.

{%
    include image.html
    year='2021'
    month='01'
    file='edit-environment-variables.png'
    alt='Add New Environment Variable'
%}

Once you've added the new variable, open a new command prompt *(or Windows Terminal)* and type the name of one of the executables you downloaded. In this screenshot you can see that running the command **sqlite3** is able to run the executable **sqlite3.exe** within the folder **C:\sqlite**!

{%
    include image.html
    year='2021'
    month='01'
    file='sqlite3-in-command-prompt.png'
    alt='SQLite in Command Prompt'
%}

You've now got SQLite installed on Windows and are ready to manage SQLite databases!

## Useful SQLite Commands

If you're looking to create a new database or list the databases and tables already on your local Windows environment, here are a few commands that you may find useful.

### Create a new *(or opening an existing)* database
```terminal
.open <FILENAME>
```

### List all databases
```terminal
.databases
```

### List all tables in the current database
```terminal
.tables
```

### Read SQL in a file
```terminal
.read <FILENAME>
```

### View the result set in a table structure
```terminal
.mode column
.headers on
```

## Execute SQL Statements in SQLite

If you'd like to experiment with executing SQL statements in SQLite, why not create a new database and use the SQL statements in the following code snippets to return a result set in the command prompt?

### Create tables and insert data
```sql
BEGIN;
    CREATE TABLE IF NOT EXISTS artist (
        id INTEGER PRIMARY KEY NOT NULL,
        title TEXT NOT NULL
    );

    INSERT INTO artist (title) VALUES ('Arcade Fire');
    INSERT INTO artist (title) VALUES ('The Chicks');
    INSERT INTO artist (title) VALUES ('Oasis');
    INSERT INTO artist (title) VALUES ('U2');

    CREATE TABLE IF NOT EXISTS album (
        id INTEGER PRIMARY KEY NOT NULL,
        artist INTEGER,
        title TEXT NOT NULL,
        year INTEGER NOT NULL,
        label TEXT NOT NULL,
        FOREIGN KEY (artist) REFERENCES artist (id)
    );

    INSERT INTO album (artist, title, year, label) VALUES (1, 'Funeral', 2004, 'Rough Trade Records');
    INSERT INTO album (artist, title, year, label) VALUES (1, 'The Suburbs', 2010, 'Merge Records');
    INSERT INTO album (artist, title, year, label) VALUES (2, 'Taking the Long Way', 2006, 'Sony Music Nashville');
    INSERT INTO album (artist, title, year, label) VALUES (3, '(What''s the Story) Morning Glory?', 1995, 'Creation Records');
    INSERT INTO album (artist, title, year, label) VALUES (3, 'Definitely Maybe', 1994, 'Creation Records');
    INSERT INTO album (artist, title, year, label) VALUES (4, 'The Joshua Tree', 1987, 'Island Records');
COMMIT;
```

### SELECT statement
```sql
SELECT
    artist.title AS [artist], 
    album.title AS [album],
    album.year AS [released],
    album.label AS [record label]
FROM
    album
JOIN artist ON artist.id = album.artist
ORDER BY album.year DESC;
```

You should now be able to manage SQLite databases on Windows!

{%
    include image.html
    year='2021'
    month='01'
    file='sqlite3-example.png'
    alt='Example of using SQLite on Windows'
%}