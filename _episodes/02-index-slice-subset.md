---
title: "Indexing, Slicing and Subsetting DataFrames in Python"
teaching: ??
exercises: 0
questions:
- "How do we access different parts of a DataFrame?"
objectives:
- "Learn about 0-based indexing in Python."
- "Learn about numeric vs. label based indexes."
- "Learn how to select subsets of data from a DataFrame using Slicing and
   Indexing methods."
- "Understand what a boolean object is and how it can be used to 'mask' or
   identify particular sets of values within another object."

keypoints:
- "Indexing & Slicing."

# training: http://swcarpentry.github.io/instructor-training
training: do-we-have-a-repo-of-python-training-resources ?
---

## Making Sure Our Data Are Loaded

We will continue to use the surveys dataset that we worked with in the last
exercise. Let's reopen it:

~~~
# first make sure pandas is loaded
import pandas as pd
# read in the survey csv
articles_df = pd.read_csv("articles.csv")
~~~
{: .source}

# Indexing & Slicing in Python

We often want to work with subsets of a **DataFrame** object. There are
different ways to accomplish this including: using labels (column headings),
numeric ranges or specific x,y index locations.


## Selecting Data Using Labels (Column Headings)

We use square brackets *[]* to select a subset of an Python object. For example,
we can select all of data from a column named *Authors* from the *articles_df*
DataFrame by name:

~~~
articles_df['Authors']
# this syntax, calling the column as an attribute, gives you the same output
articles_df.Authors
~~~
{: .source}

We can also create an new object that contains the data within the *Authors*
column as follows:

~~~
# create an object named authors that only contains the *Authors* column
authors = articles_df['Authors']
~~~
{: .source}

We can pass a list of column names too, as an index to select columns in that
order. This is useful when we need to reorganize our data.

**NOTE:** If a column name is not contained in the DataFrame, an exception
(error) will be raised.

~~~
# select the species and plot columns from the DataFrame
articles_df[['Authors', 'ISSNs']]
# what happens when you flip the order?
articles_df[['ISSNs', 'Authors']]
#what happens if you ask for a column that doesn't exist?
articles_df['column_that_does_not_exist']
~~~
{: .source}


## Extracting Range based Subsets: Slicing

**REMINDER**: Python Uses 0-based Indexing

Let's remind ourselves that Python uses 0-based
indexing. This means that the first element in an object is located at position
0. This is different from other tools like R and Matlab that index elements
within objects starting at 1.

~~~
# Create a list of numbers:
a = [1,2,3,4,5]
~~~
{: .source}

![indexing diagram]({{ page.root }}/fig/slicing-indexing.svg)
![slicing diagram]({{ page.root }}/fig/slicing-slicing.svg)

> ## Challenge
>
> 1. What value does the code below return?
>
> ~~~
> a[0]
> ~~~
> {: .source}
> 2. How about this:
>
> ~~~
> a[5]
> ~~~
> {: .source}
> 3. Or this?
>
> ~~~
> a[len(a)]
> ~~~
> {: .source}
> 4. In the example above, calling *a[5]* returns an error. Why is that?
{: .challenge}

## Slicing Subsets of Rows in Python

Slicing using the *[]* operator selects a set of rows and/or columns from a
DataFrame. To slice out a set of rows, you use the following syntax:
*data[start:stop]*. When slicing in pandas the start bound is included in the
output. The stop bound is one step BEYOND the row you want to select. So if you
want to select rows 0, 1 and 2 your code would look like this:

~~~
# select rows 0,1,2 (but not 3)
articles_df[0:3]
~~~
{: .source}

The stop bound in Python is different from what you might be used to in
languages like Matlab and R.

~~~
# select the first, second and third rows from the surveys variable
articles_df[0:3]
# select the first 5 rows (rows 0,1,2,3,4)
articles_df[:5]
# select the last element in the list
articles_df[-1:]
~~~
{: .source}

We can also reassign values within subsets of our DataFrame. But before we do that, let's make a
copy of our DataFrame so as not to modify our original imported data.

~~~
# copy the surveys dataframe so we don't modify the original DataFrame
articles_copy = articles_df

# set the first three rows of data in the DataFrame to 0
articles_copy[0:3] = 0
~~~
{: .source}

Next, try the following code:

~~~
articles_copy.head()
articles_df.head()
~~~
{: .source}
What is the difference between the two data frames?

## Referencing Objects vs Copying Objects in Python
We might have thought that we were creating a fresh copy of the *articles_df* objects when we
used the code *articles_copy = articles_df*. However the statement  y = x doesn’t create a copy of our DataFrame.
It creates a new variable y that refers to the **same** object x refers to. This means that there is only one object
(the DataFrame), and both x and y refer to it. So when we assign the first 3 columns the value of 0 using the
*articles_copy* DataFrame, the *articles_df* DataFrame is modified too. To create a fresh copy of the *articles_df*
DataFrame we use the syntax y=x.copy(). But before we have to read the articles_df again because the current version contains the unintentional changes made to the first 3 columns.

~~~
articles_df = pd.read_csv("articles.csv")
articles_copy = articles_df.copy()
~~~
{: .source}

## Slicing Subsets of Rows and Columns in Python

We can select specific ranges of our data in both the row and column directions
using either label or integer-based indexing.

- *loc*: indexing via *labels* or *integers*
- *iloc*: indexing via *integers*

To select a subset of rows AND columns from our DataFrame, we can use the *iloc*
method. For example, we can select month, day and year (columns 2, 3 and 4 if we
start counting at 1), like this:

~~~
articles_df.iloc[0:3, 1:4]
~~~
{: .source}

which gives:

~~~
                                               Title  \
0   The Fisher Thermodynamics of Quasi-Probabilities   
1  Aflatoxin Contamination of the Milk Supply: A ...   
2  Metagenomic Analysis of Upwelling-Affected Bra...   

                                             Authors  \
0                     Flavia Pennini|Angelo Plastino   
1                         Naveed Aslam|Peter C. Wynn   
2  Rafael R. C. Cuadrat|Juliano C. Cury|Alberto M...   

                          DOI  
0           10.3390/e17127853  
1  10.3390/agriculture5041172  
2       10.3390/ijms161226101  
~~~
{: .output}

Notice that we asked for a slice from 0:3. This yielded 3 rows of data. When you
ask for 0:3, you are actually telling python to start at index 0 and select rows
0, 1, 2 **up to but not including 3**.

Let's next explore some other ways to index and select subsets of data:

~~~
# select all columns for rows of index values 0 and 10
articles_df.loc[[0, 10], :]
# what does this do?
articles_df.loc[0, ['Authors', 'ISSNs', 'Title']]

# What happens when you type the code below?
articles_df.loc[[0, 10, 35549], :]
~~~
{: .source}

NOTE: Labels must be found in the DataFrame or you will get a *KeyError*. The
start bound and the stop bound are **included**.  When using *loc*, integers
*can* also be used, but they refer to the index label and not the position. Thus
when you use *loc*, and select 1:4, you will get a different result than using
*iloc* to select rows 1:4.

We can also select a specific data value according to the specific row and
column location within the data frame using the *iloc* function:
*dat.iloc[row,column]*.


~~~
articles_df.iloc[2,1]
~~~
{: .source}

which gives:

~~~
'Metagenomic Analysis of Upwelling-Affected Brazilian Coastal Seawater Reveals Sequence Domains of Type I PKS and Modular NRPS'
~~~
{: .output}

Remember that Python indexing begins at 0. So, the index location [2, 0] selects
the element that is 3 rows down and first column in the DataFrame.

> ## Challenge Activities
>
> 1. What happens when you type:
>
> ~~~
> articles_df[0:3]
> articles_df[:5]
> articles_df[-1:]
> ~~~
> {: .source}
>
> 2. What happens when you call:
>     - *dat.iloc[0:4, 1:4]*
>     - *dat.loc[0:4, 1:4]*
>     - How are the two commands different?
{: .challenge}

## Subsetting Data Using Criteria

We can also select a subset of our data using criteria. For example, we can
select all rows that have a single author.

~~~
articles_df[articles_df.Author_Count==1]
~~~
{: .source}

Which produces the following output:

~~~
      id                                              Title  \
15    15  Performance-Based Cognitive Screening Instrume...   
27    27  Comments on Ekino et al. Cloning and Character...   
64    64  The Ubiquity of Humanity and Textuality in Hum...   
...
     Author_Count             First_Author  Citation_Count  Day  Month  Year  
15              1         Andrew J. Larner               4    1     11  2015  
27              1           Leopoldo Palma               4    1     11  2015  
...
827             1        Natale Perchiazzi               9    1      3  2015  
932             1          Anaelle Tilborg               9    1     11  2015  
~~~
{: .output}

Or we can select all rows that do not contain the year 2002.

~~~
articles_df[articles_df.Year != 2002]
~~~
{: .source}

We can define sets of criteria too:

~~~
articles_df[(articles_df.Year >= 1980) & (articles_df.Year <= 1985)]
~~~
{: .source}

# Python Syntax Cheat Sheet

Use can use the syntax below when querying data from a DataFrame. Experiment
with selecting various subsets of the "surveys" data.

* Equals: *==*
* Not equals: *!=*
* Greater than, less than: *>* or *<*
* Greater than or equal to *>=*
* Less than or equal to *<=*


> ## Challenge Activities
>
> 1. Select a subset of rows in the *articles_df* DataFrame that contain articles
>    from at least 2 authors in Spanish (LanguageId=3). How many rows did you
>    end up with? What did your neighbor get?
> 2. You can use the *isin* command in python to query a DataFrame based upon a
>    list of values as follows:
>    *articles_df[articles_df['ISSNs'].isin([listGoesHere])]*. Use the *isin* function
>    to find all articles from particular ISSNs. How many records did you get?
> 3. Experiment with other queries. Create a query that finds all rows with
>    an *Author_Count* of 0 or less.
> 4. The *~* symbol in Python can be used to return the OPPOSITE of the
>    selection that you specify in python. It is equivalent to **is not in**.
>    Write a query that selects all rows that are NOT in English (LanguageId=1).
{: .challenge}

# Using Masks

A mask can be useful to locate where a particular subset of values exist or
don't exist - for example,  NaN, or "Not a Number" values. To understand masks,
we also need to understand *BOOLEAN* objects in python.

Boolean values include *true* or *false*. So for example

~~~
# set x to 5
x = 5
# what does the code below return?
x > 5
# how about this?
x == 5
~~~
{: .source}

When we ask python what the value of *x > 5* is, we get *False*. This is because x
is not greater than 5 it is equal to 5. To create a boolean mask, you first create the
True / False criteria (e.g. values > 5 = True). Python will then assess each
value in the object to determine whether the value meets the criteria (True) or
not (False). Python creates an output object that is the same shape as
the original object, but with a True or False value for each index location.

Let's try this out. Let's identify all locations in the survey data that have
null (missing or NaN) data values. We can use the *isnull* method to do this.
Each cell with a null value will be assigned a value of  *True* in the new
boolean object.


~~~
pd.isnull(articles_df)
~~~
{: .source}

To select the rows where there are null values,  we can use
the mask as an index to subset our data as follows:

~~~
#To select just the rows with NaN values, we can use the .any method
articles_df[pd.isnull(articles_df).any(axis=1)]
~~~
{: .source}

We can run *isnull* on a particular column too. What does the code below do?

~~~
# what does this do?
no_doi = articles_df[pd.isnull(articles_df['DOI'])]
~~~
{: .source}

Let's take a minute to look at the statement above. We are using the Boolean
object as an index. We are asking python to select rows that have a *NaN* value
for Language.


> ## Challenges
>
> 1. Create a new DataFrame that only contains observations with Language values
>    that are English or French. Assign each language value in the  new DataFrame
>    to a new value of *x*. Determine the number of null values in the subset.
> 2. Create a new DataFrame that contains only observations that are English or
>    french and where the author count is greater than 2. Create a stacked bar
>    plot of average number of authors by language with English vs French values
>    stacked for each publisher.
{: .challenge}
