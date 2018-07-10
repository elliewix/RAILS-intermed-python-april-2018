# Pandas

## Who is this for?

There are many pandas and python tutorials out there.  Many are great, but most of the time they don't specify the kind of audience they are looking for. Pandas is a wonderful tool to know about and use in python, but it holds many expectations that are somewhat abnormal to what a python novice is used to.

This is also a brain dump document right now that I'm using to structure my thoughts.

The primary audicence for these thoughts are:

* python novices who have completed some python and are resonably comfortable (if not strongly experienced) with working in python
* python novices where Python is their only language or their mother tounge, so no exposure to Java or C++.
* people who have not had much exposure to math beyond college algebra or stats, and are not comfortable or experienced with the application of linear algebra and statistical vocabulary words to code/programming.
* instructors who may be teaching students who come from some or all of the above

The first section of this dump will be general commentary on the technical perspective, historical context, and linguistic commentary about pandas.  I've found that all these things are necessary for students learning pandas, particularly those who are very new to python and cannot immidiately tell where the normal python and pandas python are in a single block of code.

Learning pandas as a python and programming novice will require you to use your pattern adaptation skills more than normal. You will gradually start filling in any gaps you end up with, but surviving pandas is all about pattern matching to examples.

## Technically

From a purely technical perspective, we can describe pandas as a free and open source third party library for Python.  What the heck does that mean?

### Free and open source

* free: no cost to install and use
* open source: all the code and development of pandas is done in an open and public way.  Not only can you see all the source code behind the library, you can also contribute to it and communicate directly to the maintainer community.
* Together this means that it is released under a permissive license, such that you can use it for free, but you can also take the code and adapt all or part of it for your own needs, even commercial.  More info on the license is here: http://pandas.pydata.org/pandas-docs/stable/overview.html#license and help understanding it is here:  https://tldrlegal.com/license/bsd-3-clause-license-(revised).

### Third party library for python

* third party: this is not part of the Python standard library.  This means that it is outside of the central control and support of core python, and you must install it separately of your normal Python. Many installation resources (like anaconda) will include it, so you may not need to do much more to install it.  This also means that the syntax and other tools in the pandas ecosystem are not tested or discussed within the core python community, so it may violate or do odd things compared to standard python. This isn't always a bad thing, but something to remain aware of.
* library: roughly, this means that it brings in a set of functions, data types, and classes for use as needed. There are many of each of those, and you import them in whole or part as you need them.


## Canonically

Pandas is considered an industry standard package for data science related activities and for reading in complex data formats.  It has connections to a variety of other data platforms and can also be seen used solely for loading and exporting data.  

There are common names used for both loading the library into memory and certain variables that are commonly used.  The two most common to see will be:

* `pd` which is the most common way to import the pandas library
	* `import pandas as pd` in the line of code to do this
* `df` is the common variable name used for pandas dataframes.  These will often be numbered later on as the data is manipulated, so you'll see `df2` etc. Which isn't to say that this is the best practice, but it is common practice.

Pandas is also often described as a sort of glue within Python to help people make use of tools written in other langauages or other data systems.  This is particularly nice for business or enterprise systems where tools must work across humans using a variety of tools and a mix of technological systems.

## History of pandas

### Python and not python at the same time

The development of pandas was strongly inspired by R and that language's vocabulary and syntax.  Pandas has a syntax all its own, trying to make specific kinds of queries and commands work in a programming language that often doesn't share the same design perspective.  This is a nearly independent language that is attempting to be expressed within Python, and there are some places where this goal gets a little strange or is incomplete and thus the code falls back onto core python.

Operationally, it means that there are some very specific and very different syntaxes within pandas that:

* look plausibly like python operate very differently
* look completely abnormal for Python
* look like R syntax ripped out and pasted into normal looking python
* be actual real core python, just in a more advance syntactical structure that you don't recognize
* some combination of all of the above

### Understand evolution

This package is about 10 years old at this time, and was bugun by a small group of people aiming to fill in a gap they saw for the Python ecosystem at that time. Things were devoloped with a very small scale of people and for very specific tasks.  This means a few things.  All of these things are pretty normal when across all socio technical systems, and is not a dig specifically about pandas. 

You will find that there are leftover bits from when things seemed clever and cool, but ended up not scaling well as more people adopted it. (who hasn't been here?)



### Linguistically

This package was developed with strong connections to economists and related data analytics areas.  The developers are also strongly within the scientific python community, so you will see strong connections to many other packages within the scientific python stack of packages.  This is an informal name for the many tools and packages that were created for use within the scientific programming community.  These tools tend to be very focused on performing computations and optimizing the speed of those computations.  

Within the pandas universe you'll see a strong bias towards certain data types and data models of the universe. It can handle many data types, but the dataframe framework that is introduced comes with its own rules on how data must be organized. You'll also see that there are several strong specializations within this ecosystem, such as timeseries data.  Pandas also builds frameworks on top of many other packages, which you will see pop up from time to time.  The strong connection to Python is also not lost, and you will usually see some core python tools and data structures appear, but this pattern is gradually going away as the pandas framework begins to support more activites.  However, this can also be a sign of someone newer to pandas who isn't yet intimately femiliar with many of the tools available in the library.

From a style standpoint, you'll find that many of the core python tools adopted into the cononical pandas framework live in the more advanced domain, so there is a near guarenteed possibility that novices will see a steady barrage of new syntax and tools.  There are many reasons for this, as those who are developing this tool are more advanced or professional python developers, and they are trying to adapt a general purpose programming language into a very specialized language for data at scale. That scale component often means that they will make the choice to sacrefice readability for speed, running with the presumption that their users will also have similar scale needs and accept that trade off.  Unfortunately, this means that novices to programming and python will struggle in trying to understand the syntax and purpose behind some of the common patterns used within this library.

There will also be a stronger visible presence of more traditional computing vocabulary words and upper level maths vocabulary.  Both of these facts stem from the origins of the devolopers of the package.  Many have programmed in lower level languages before python, and are deliberately writing tools in pandas to help Python communicate with those languages because they tend to be computationally faster (this is the glue, as mentioned before). Many of the functions and services being created in pandas originate from linear algebra and other statistical areas, so it would only be natural for those items to retaisn that vocabulary. 

None of these things are bad necessarily, but they do represent a presumption of the community that users of this package have a working comfort in more traditional programming lanugages, linear algebra, and statistics.  Operationally, this means that novices who have only been exposed to Python's data structures (which are named and used in strongly different ways than many other languages) will face an additional vocabulary learning curve, because they will not always know the mapping of the traditional to the python ecosystem.  

### As a project

While this is a free and volunteer developed package, many of the core maintainers have devolopment for it built into their jobs.  Pandas is also a fiscally sponsored project of NumFOCUS, a non-profit dedicated to supporting open souce projects and communities.  This means that they help pay for community building, some development, and computing resources (such as hosting and websites).  


## What kind of data?

While there are ways to make it work, at the core pandas expects that you have two dimensional data. This means that you have rows and columns of data. You'll also see this referred to as tabular data or tables of data.

The other advantage of using pandas is that you can refer to things by name rather than position of a natural number ID. What's even better is that these items are also stored in memory and treated like a primary access point. So much of the language and syntax of pandas allows for these sorts of queries. Multiple IDs, or IDs that are combinations of multiple columns are also directly supported, giving even more power to accurately represent your data.

The premise of the dataframe does impose several restrictions on how you organize things.  First, the rows must be your entities or observations, and the columns carry properties of all those rows.  Missing data is supported, but it is presumed that every row will be able to answer every column, and that missing values are exceptions.  Some data naturally stored as a tree or triples will not easily be molded into this shape, but this is a very common data structure.

## What kind of platform?

While pandas can be imported and used in any Python script so long as the library is correctly installed, the interactive nature of data analysis means that there are many special perks for those using it within the Jupyter environment.  The rendering of dataframes for viewing data is the primary example here.  So while you can print a dataframe, this library is really meant for use within a Jupyter notebook.  Notable exceptions to this would be for anyone using pandas strictly for data import/export convenienece, and interactive manipulation of data frames is not desired.

## Make it work time

Let's go ahead and try to make some things work.  First step is to launch a Jupyter notebook window.  