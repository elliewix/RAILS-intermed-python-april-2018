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
When we import the package without an alias, this means that we need to use dot notation and have `sqlite3.` before any function from that package that we want to use.

In this case, we want to call `sqlite3.connect()` to open up a connection to our database file.  Again, the terminology here is really geared toward non-local files.  

We pass the name of our file into the function, and save it as a variable called `conn`.  This variable name is a pretty standard one that you'll see across a variety of SQL packages in Python.

This variable represents Python's knowledge of the database file and what can be done with it. You'll use this variable as an object to interact with the database's content.  

But we aren't ready to execute commands just yet.  We need to create a cursor connection to the database so we can pass it some text to parse as SQL commands.  This cursor object will have a special `execute()` function that will let us pass it a SQL command (that is stored as text in a string).

``` python
c = conn.cursor()
```

There's nothing to pass into this function, and we're going to save the resulting object to a variable called `c`.  We need to save it as a variable so it will be persistantly available in memory to act on whenever we want.

We only have one table, so we're going to do the simplest SQL command there is:  select everything from the table and show it all to us.   Let's take a look at the query and results, then we'll talk through the syntax.

This will be a two step process, breaking it down into both: 1) execute the query to create a results variable, and 2) use another command to view the contents of those results.

There are three functions that you can run on your results variable:

* `results.fetchone()` will return a single tuple that represents the first row of your results.
* `results.fetchmany(a_number)` will return a list of tuples that represents the first number of results that you requested.
* `results.fetchall()` will return a list of tuples that represents all the rows in your returned table.

For memory and space reasons, you may only want to look at the 'top' of the results to check what you were given back.  You may use this process as a check that what you got back was correct before going on to request or write out all of the data.

``` python 
results = c.execute("SELECT * FROM letters;") # executes your query and saves you results

print(results.fetchone()) # shows you the contents of your results
```
And our output to view one row:

``` text
('1', '1', '[Provenance documents and biographical sources]', 'n.d.')
```

While we have been given results back for our query, the actual contents aren't immediately viewable.  This has to do with memory usage, and for large data results there are benefits for the structure that this has been created in.  It just means that we have an extra step to do before we can bring the contents to our eyes.

The `fetchone()` function is called on our results variable and will retrieve only the first line of results, and give us a tuple (which you can see are the `()` in the output.  This will always be the first one and it doesn't make a cursor that reads thorugh the results as you execute it.

Likewise, we can use `fetchall()` to get all the results or `fetchmany(num)` to request the first `num` rows.

``` python
print(results.fetchmany(3))

```
And our output is:

``` text
[ ('1', '1', '[Provenance documents and biographical sources]', 'n.d.'), 
  ('1', '2', '[Provenance documents and biographical sources]', 'n.d.'), 
  ('1', '3', '[Provenance documents and biographical sources]', 'n.d.') ]

```
Let's take a look at this structure with a little more detail. We have a list around the outside (which we can see with the `[]` hanging out on either end.  Inside our list, we see that there are three 'rows' of data that appear to match the order of our original data.  Rather than being separated by commas or rendered like a table, each value is contained in a string, and every value for that row is contained in a tuple.  You can see that there are three rows of data because there are three pairs of tuples, which are the `()` items in there.  You can also see that the order of the data stored in these tuples is consistant through all of them. 

Now, you may be asking: where are the column names here?  Our results variable holds more information about those results, including an attribute called `description`.  You can access this with the following syntax, noting the lack of `()` here because this is a class attribute that we're accessing the value of rather than a function for the class that we are calling.

Also note that we didn't overwrite our results variable when we printed out our rows.  We left the results as it stood coming back in from our cursor object.

``` python
print(results.description)
```

And the output is:

``` text
(('BoxNumber', None, None, None, None, None, None),
 ('FolderNumber', None, None, None, None, None, None),
 ('Contents', None, None, None, None, None, None),
 ('Date', None, None, None, None, None, None))

```

Yes, this structure is as intended because these tuples are meant to hold more than this information.  This structure is also used to be in compliance with another database standard.  But for us, there's such little information it looks silly.  We'll need to dig into this structure to get out the column names, which will look funny but doesn't require anything different than what we'll need to do for digging into the rows of data. 

## To sum

Let's remind ourselves what we needed to do in Python to get the query run and the results.

We:

1. Estabished a connection to the database file.
2. Created a cursor object so we can execute queries on that database.
3. Used the `execute()` function to execute a sql query.
4. Used one of the fetching methods to grab out however many results that we want.

# Doing Python things to this data

Now we have two structures:  the list of tuples with the data, and the tuple of tuples that contains the column names.  

## Grabbing all the column names in an orderly way

This will be a general review of list operations and a list accumulator pattern.  There will be two chunks that we'll need to craft: 

1. First, we need to dig into to the 2D structure and access the first item of each sublist.
2. Second, now that we have the individual strings, we need to accumulate all these into another list.

Technically speaking, we don't need to do this.  But it will make any lookups a little cleaner in our later code.

### Digging in

To access sublists in a list (in this case we have tuples, but it will be operationally the same), we need to loop through the primary list.  This will unpack the list into the individual sublists into our iterable variable, one at a time.


``` python
for col_info in columns:
    print(col_info)
```

And the output:

``` text
('BoxNumber', None, None, None, None, None, None)
('FolderNumber', None, None, None, None, None, None)
('Contents', None, None, None, None, None, None)
('Date', None, None, None, None, None, None)
```

This really doesn't look anything different from what we had before, but it is different in a very important way:  we are getting each subtuple individually.  This means that we can now treat our iterable variable as if it were a single tuple.  We use the same extraction notation on tuples as on lists, so we can use `[0]` to grab the first item.

Take a moment to think about which variable you'd add that on to before looking at the solution.  Hint:  which variable is the one holding our subtuples one at a time?

The answer is our iterable variable, which is `col_info`.  Note that I carefully named this variable and didn't call it `column_name`, which would be incorrect.  The iterable at this level contains a tuple that is meant to hold a bunch of information about the column, with the name being just a part of that.

So next we're going to use our same code as before, but add the extraction syntax onto the iterable variable.

``` python 
for col_info in columns:
    print(col_info[0])
```

And the output:

``` text
BoxNumber
FolderNumber
Contents
Date
```

See where the `[0]` went in there? It's added onto the `col_info` variable, and that whole thing is in the print statement.  This means that it's extracting out the single element that I requested, which happens to be a single string, and then passes that to the `print()` function for it to be printed to the screen.

Important to note here is that while we can see this data on our screen, we haven't stored all these values into anything in memory all at once.  The individual values don't exist in that variable at the same time.

I need them all together in a clean way, which is where my accumulator comes in.

### Bringing the values all together

We're going to add a list accumulator pattern into our previous bit of code.  This pattern sits around and inside our code.  Because we're starting this process having already isolated the value that we want, we can slide in the accumulator pieces without worrying about data extraction further.

In a nutshell, we need to accomplish the following things:

1. Establish an empty list to throw these values into.  After all, you can't sort a pile of socks into separate baskets if you don't already have the baskets.
2. Instead of printing out the individual values, we need to add them into our new list.  There are several ways to add an item into a list, but the most common in this kind of situation is to use the `.append()` method for the list.  

A quick reminder about how `append()` works for lists:

* tl;dr:  call `append()` on your list in memory, pass it what you want to add in the `()`, and don't use any assignment statements in this line of code.
* this is a list method, so we call it on our list directly.  So you'll see something like `my_list.append()`.  It isn't a function that exists independently from the list object that we are working with.
* this method takes an argument:  the item to add.  We put this item in the `()`, and after the method executes that item will now be the last item is out list.  So you'll see somethig like:  `my_list.append(thing_to_add)`.  The contents of `thing_to_add` will now be the last item in the `my_list` object.
* this is a mutator method, which means that it will alter the list you call it on directly.  Which roughly matches what we want to do, but has two big implications to be reminded of. One is that the list is immediately changed in memory after this method is called, so your original object will be changed.  Two, nothing is returned from this method, so there's no need for an assignment statement here.  You'll just have `my_list.append(thing_to_add)` hanging out alone, and it is sufficient to get the job done. In fact, if you try to reassign this back to your list object, you will have erased your list.

Let's see this in action.  

``` python
col_names = []

for col_info in columns:
    col_names.append(col_info[0])
```

Three big changes here:  

* `col_names = []` gives me an empty list to add items into.
* I removed the `print()` statement, but left `col_info[0]`
* I surroundid `col_info[0]` with `colnames.append(...)`

This means that, one at a time, the contents of `col_info[0]` will be added to the end of my `col_names` list. Because they are all being added at the end, they will be left in the order that they were seen.

Now we can print our list and see the results:

``` python
print(col_names)
```

And the output:

``` text
['BoxNumber', 'FolderNumber', 'Contents', 'Date']
```

## Digging into the data

Now that we have our column names in a nice flat list, we can turn our attention to the list of data.

What you want to do with this is up to you, but we can play around with how me might transform this into a dictionary.  This will give un a nice structure for looking at all the data.

Let's remind ourselves of the 