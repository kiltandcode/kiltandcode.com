---
title: "How to Install and Use SQLite on Windows"
date: 2021-01-18 19:00:00 +00:00
author: "Steven McLintock"
layout: post
image: /assets/img/2021/01/sqlite-on-windows.png
category: sql
---

{%
    include image-lead.html
    year='2021'
    month='01'
    file='sqlite-on-windows.png'
    alt='SQLite Download Page'
%}

Unlike modern versions of macOS, **SQLite isn't installed by default on Windows**. If you're using Windows 
like I am, you need to do a few extra steps before you can start using it.

Let's begin to install SQLite on Windows by first downloading the executables from the 
[SQLite Download Page](https://www.sqlite.org/download.html).

You'll want to find the section **Precompiled Binaries for Windows** and download one of the ZIPs. In this 
guide I'll be using the bundle of command-line tools, simply because it's always useful to have the 
additional utilities available if you ever need them.

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

Once you've downloaded the ZIP onto your PC, extract the files into the local directory **C:\sqlite**

{%
    include image.html
    year='2021'
    month='01'
    file='sqlite-executables-on-pc.png'
    alt='SQLite Executables on PC'
%}

## Add PATH Environment Variable

{%
    include image.html
    year='2021'
    month='01'
    file='environment-variables.png'
    alt='Environment Variables on Windows'
%}

{%
    include image.html
    year='2021'
    month='01'
    file='sqlite3-in-command-prompt.png'
    alt='SQLite in Command Prompt'
%}

## Create a New SQLite Database

```terminal
.open albums.db
```

creates DB in the C:\sqllite directory

## Executing SQLite Commands in the Command Prompt

.databases to list all of the databases
.tables to list all of the tables
.read to execute a SQL file (saves typing in a lot of SQL into the terminal)
".mode column" to set the output to display left-aligned columns in the terminal
".headers on" to turn the headers on or off in the terminal

link to the full list of SQLite commands in official documentation

## Executing SQL Commands Using SQLite

show how to use ".read create-table.sql" to create an entire table in the current database

quick note (in italic): use transactions to boost performance (BEGIN/COMMIT)
- show SQL script here to create tables and insert some data

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

run ".tables" to actually check if the table was created in the database

then show how to do "SELECT * FROM table INNER JOIN ..... WHERE ...." to return the row

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

{%
    include image.html
    year='2021'
    month='01'
    file='sqlite3-example.png'
    alt='Example of using SQLite on Windows'
%}