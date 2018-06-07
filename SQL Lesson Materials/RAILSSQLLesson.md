# SQL Lesson

But maybe from a different perspective.

## Here's the scene:

* You have a speadsheet you want to do some analysis on and otherwise explore.
* You want to do more than what Excel can do, or you want to do it in a way that you can document and rerun.
* You do (or will!) know some SQL, look at the data, and think "hey these questions sound like SQL questions."
* Much of the data you want to work with are not quantitative values, and is mostly categorical, ordinal, or contains labels/descriptions.
* You may be limited to online platforms or platforms you don't have a choice over (so you either need to bypass it with a web client or you need a lesson that's platform agnostic).

This lesson is for you.

Goals:

* start with a spreadsheet
* make it into a database file that you can read into a variety of platforms (even cloud ones!)
* write some SQL queries to:
	* investigate your data
	* perform some calculations
	* transform your data
	* aggregate information
* output results files

All using approaches that:

* won't risk any changes to the original data
* leverages several free and open source tools
* is compatible with several cloud based tools in case you don't have admin rights)
* is easily compatible with reproducible practices
* works nicely with version control systems, such as git, GitHub, and GitLab
* doesn't require that you use a server or set one up to perform SQL queries
* are reasonably platform agnostic so you have a variety of choices in the tools that you use


We'll be focusing on data that in not numerical.  This means that the data we'll see, even if it looks like numbers, aren't all quantitative measurments.  

# Server vs serverless

We won't be actively connecting to a remote database for this lesson.  However, the language and queries that we'll be using should be universal however you connect to the database.

The tools that we'll be using will want things to appear like a database connection, so some of functions and tools will have vocabulary of an external data source.

# Which flavor?

There are many dialects of SQL, and SQLite is just one of them. It is free, light weight, and available all over the place. So a great place to get started! Don't worry, most of the skills you learn in SQLite will work with other forms of SQL.

This also means that whenever you area searching for help online, always check to see which system that help page or answer has been written for.  

Again, what we're going to cover here will be reasonably universal for any system you move to.  SQLite happens to be one of the most commonly available for free and online tools.

# What kind of data does this want?

Most SQL clients like the one that we will be using will want to either open a connection to a server or expect to open a binary `.db` file.  Just reading in a CSV is not always the most straight forward process, as SQL usually needs to know a bit more about the data before it can make anything happen. 

I've gone ahead and made a `.db` file for us to work with here so we have something to explore some of the base queries.

Later on in the lesson we'll talk about reading in a datafile and adding that data into a table.

# Databases are rectanges with lagniappe 

You may be used to working with rectangular data that you read in and transform.  Maybe you're used to working with speadsheet files that are really workbooks with many sheets of data within them.  

Databases can be seen as collections of rectangular data.  Instead of having a single CSV file to work with, you can have many tables.  Depending on your data model and design, these tables may be strongly interconnected, somewhat, or not connected at all.

Right now we're only going to work with a single table.  

# Loading a `db` file

This will look a little different depending on the platform that you are working inside.

We're going to load this into Python for now.  Python has a lovely sqlite3 package that comes with the standard library. The package name is called `sqlite3`.  

``` python
import sqlite3
conn = sqlite3.connect('pettigrew_test.db')
```
When we import the package without an alias, this means that we need to 