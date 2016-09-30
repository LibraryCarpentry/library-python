---
title: "Starting With Data"
teaching: ??
exercises: ??
questions:
- "How does Python deal with data tables?"
objectives:
- "Explain what a library is, and what libraries are used for."
- "Load a Python/Pandas library."
- "Read tabular data from a file into Python using Pandas using *read_csv*."
- "Learn about the Pandas DataFrame object."
- "Learn about data slicing and indexing."
- "Perform mathematical operations on numeric data."
- "Create simple plots of data."

keypoints:
- "Core concepts in python: Python libraries, Pandas DataFrames, working with data."

# training: http://swcarpentry.github.io/instructor-training
training: do-we-have-a-repo-of-python-training-resources ?
---

# Working With Pandas DataFrames in Python

## Presentation of the survey data

For this lesson, we will be using the DOAJ article sample data, available at [doaj-article-sample.csv](https://github.com/data-lessons/library-openrefine/raw/gh-pages/data/doaj-article-sample.csv).

This data set is a list of published articles. The dataset is stored as a *.csv*
file: each row holds information for a single article, and the columns represent:

| Column           | Description                        |
|------------------|------------------------------------|
| Title            | Title of the article               |
| Authors          | Author (or authors)                |
| DOI              | DOI                                |
| URL              | URL                                |
| Date             | Publication date                   |
| Language         | Language in which it is written    |
| Subjects         | List of subject key words          |
| ISSNs            | ISSNs code                         |
| Publisher        | Name of the publisher              |
| Citation         | Citation information               |
| Licence          | Copyright licence                  |


## About (Software) Libraries
A library in Python contains a set of tools (called functions) that perform
tasks on our data. Importing a library is like getting a piece of lab equipment
out of a storage locker and setting it up on the bench for use in a project.
Once a library is set up, it can be used or called to perform many tasks.

## Pandas in Python
One of the best options for working with tabular data in Python is to use the
[Python Data Analysis Library](http://pandas.pydata.org/) (a.k.a. Pandas). The
Pandas library provides data structures, produces high quality plots with
[matplotlib](http://matplotlib.org/) and integrates nicely with other libraries
that use [NumPy](http://www.numpy.org/) (which is another Python library) arrays.

Python doesn't load all of the libraries available to it by default. We have to
add an *import* statement to our code in order to use library functions. To import
a library, we use the syntax *import libraryName*. If we want to give the
library a nickname to shorten the command, we can add *as nickNameHere*.  An
example of importing the pandas library using the common nickname *pd* is below.


~~~
import pandas as pd
~~~
{: .source}

Each time we call a function that's in a library, we use the syntax
*LibraryName.FunctionName*. Adding the library name with a *.* before the
function name tells Python where to find the function. In the example above, we
have imported Pandas as *pd*. This means we don't have to type out *pandas* each
time we call a Pandas function.

## Lesson Overview

For this lesson we will be using the DOAJ article data.

We are analyzing the articles published in a particular field of study. The
data set is stored in *.csv* (comma separated values) format. Within
the *.csv* files, each row holds information for a single article, and the
columns represent: Title, Authors, DOI, URL, Date, Language, Subjects, ISSNs,
Publisher, Citation and Licence.

The first few rows of our first file look like this:

~~~
Title	Authors	DOI	URL	Date	Language	Subjects	ISSNs	Publisher	Citation	Licence
The Fish...	Flavia P...	10.33...	https://...	01/11/2015	English	Fisher i...	1099-4300	MDPI AG	Entropy, V...	CC BY...
Aflatoxi...	Naveed A...	10.33...	https://...	01/11/2015	English	aflatoxi...	2077-0472	MDPI AG	Agricultur...	CC BY...
Metageno...	Rafael R...	10.33...	https://...	01/11/2015	English	PKS|NRPS...	1422-0067	MDPI AG	Internatio...	CC BY...
Synthesi...	Fabrizio...	10.33...	https://...	01/11/2015	EN	lanthani...	2304-6740	MDPI AG	Inorganics...	CC BY...
Performa...	Magali T...	10.33...	https://...	01/11/2015	EN	snow mod...	2306-5338	MDPI AG	Hydrology,...	CC BY...
Dihydroc...	Xiaoxiao...	10.33...	https://...	01/11/2015	English	Malus cr...	1420-3049	MDPI AG	Molecules,...	CC BY...
~~~
{: .output}
(quite difficult to read and interpret as it is...)

### We want to:

1. Load that data into memory using Python.
2. Calculate the average number of authors per article, for each publisher.
3. Plot this information.

We can automate the process above using Python. It's efficient to spend time
building the code to perform these tasks because once it's built, we can use it
over and over on different datasets that use a similar format. This makes our
methods easily reproducible. We can also easily share our code with colleagues
and they can replicate the same analysis.

# Reading CSV Data Using Pandas

We will begin by locating and reading our survey data which are in CSV format.
We can use Pandas' *read_csv* function to pull the file directly into a
[DataFrame](http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dataframe).

## So What's a DataFrame?

A DataFrame is a 2-dimensional data structure that can store data of different
types (including characters, integers, floating point values, factors and more)
in columns. It is similar to a spreadsheet or an SQL table or the *data.frame* in
R. A DataFrame always has an index (0-based). An index refers to the position of
an element in the data structure.

First, let's make sure the Python Pandas library is loaded. We will import
Pandas using the nickname *pd*.  This is a common convention on the internet,
so if you look up Pandas usage, you will often see it this way.

~~~
import pandas as pd
~~~
{: .source}

Let's also import the [OS Library](https://docs.python.org/3/library/os.html).
This library allows us to make sure we are in the correct working directory. If
you are working in IPython Notebook, be sure to start the notebook in the
workshop repository.  If you didn't do that you can always set the working
directory using the code below.

~~~
import os
os.getcwd()
# if this directory isn't right, use the command below to set the working directory
os.chdir("YOURPathHere")

# note that pd.read_csv is used because we imported pandas as pd
pd.read_csv("doaj-article-sample.csv")
~~~
{: .source}

The above command yields the **output** below:

~~~
                                                  Title  \
0      The Fisher Thermodynamics of Quasi-Probabilities   
1     Aflatoxin Contamination of the Milk Supply: A ...   
2     Metagenomic Analysis of Upwelling-Affected Bra...   
3     Synthesis and Reactivity of a Cerium(III) Scor...   
4     Performance and Uncertainty Evaluation of Snow...   
5     Dihydrochalcone Compounds Isolated from Crabap...   

....

997   Acta Crystallographica Section E: Crystallogra...   CC BY  
998   Acta Crystallographica Section E: Crystallogra...   CC BY  
999   Acta Crystallographica Section E: Crystallogra...   CC BY  
1000  International Journal of Molecular Sciences, V...   CC BY  

[1001 rows x 11 columns]
~~~
{: .output}

We can see that there were 1,001 rows parsed. Each row has 11
columns. The first column is the index of the DataFrame. The index is used to
identify the position of the data, but it is not an actual column of the DataFrame.
It looks like  the *read_csv* function in Pandas  read our file properly. However,
we haven't saved any data to memory so we can work with it.We need to assign the
DataFrame to a variable. Remember that a variable is a name for a value, such as *x*,
or  *data*. We can create a new  object with a variable name by assigning a value to it using *=*.

Let's call the imported survey data *articles_df*:

~~~
articles_df = pd.read_csv("doaj-article-sample.csv")
~~~
{: .source}

Notice when you assign the imported DataFrame to a variable, Python does not
produce any output on the screen. We can print the value of the *articles_df*
object by typing its name into the Python command prompt.

~~~
articles_df
~~~
{: .source}

which prints contents like above

## Manipulating Our Species Survey Data

Now we can start manipulating our data. First, let's check the data type of the
data stored in *articles_df* using the *type* method. The *type* method and
*__class__* attribute tell us that *articles_df* is *<class 'pandas.core.frame.DataFrame'>*.

~~~
type(articles_df)
# this does the same thing as the above!
articles_df.__class__
~~~
{: .source}

We can also enter *articles_df.dtypes* at our prompt to view the data type for each
column in our DataFrame. *int64* represents numeric integer values - *int64* cells
can not store decimals. *object* represents strings (letters and numbers). *float64*
represents numbers with decimals.

~~~
articles_df.dtypes
~~~
{: .source}

which returns:

~~~
Title        object
Authors      object
DOI          object
URL          object
Date         object
Language     object
Subjects     object
ISSNs        object
Publisher    object
Citation     object
Licence      object
dtype: object
~~~
{: .output}

We'll talk a bit more about what the different formats mean in a different lesson.

### Useful Ways to View DataFrame objects in Python

There are multiple methods that can be used to summarize and access the data
stored in DataFrames. Let's try out a few. Note that we call the method by using
the object name *articles_df.method*. So *articles_df.columns* provides an index
of all of the column names in our DataFrame.

> ## Try out the methods below to see what they return.
>
> 1. *articles_df.columns*.
> 2. *articles_df.head()*. Also, what does *articles_df.head(15)* do?
> 3. *articles_df.tail()*.
> 4. *articles_df.shape*. Take note of the output of the shape method. What format does it return the shape of the DataFrame in?
{: .challenge}

HINT: [More on tuples, here](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences).

## Calculating Statistics From Data In A Pandas DataFrame

We've read our data into Python. Next, let's perform some quick summary
statistics to learn more about the data that we're working with. We might want
to know how many animals were collected in each plot, or how many of each
species were caught. We can perform summary stats quickly using groups. But
first we need to figure out what we want to group by.

Let's begin by exploring our data:

~~~
# Look at the column names
articles_df.columns.values
~~~
{: .source}

which **returns**:

~~~
array(['Title', 'Authors', 'DOI', 'URL', 'Date', 'Language', 'Subjects',
       'ISSNs', 'Publisher', 'Citation', 'Licence'], dtype=object)
~~~
{: .output}

Let's get a list of all the publishers. The *pd.unique* function tells us all of
the unique values in the *Publisher* column.

~~~
pd.unique(articles_df['Publisher'])
~~~
{: .source}

which returns:

~~~
array(['MDPI AG', 'MDPI AG ', 'Aurel Vlaicu University Editing House',
       'Society of Pharmaceutical Technocrats',
       'Consejo Superior de Investigaciones Cient\xc3\xadficas',
       'Akshantala Enterprises', 'International Union of Crystallography'], dtype=object)
~~~
{: .source}

> ## Challenge
>
> Create a list of unique Licences found in the surveys data.
> Call it *licence_types*. How many unique licences are there in the data?
{: .challenge}

# Groups in Pandas
At this point, our DataFrame has only String types. Some of the grouping
operations work different for numeric types (e.g. calculating averages). So we
will create a new column on our data frame, which includes the author count.

We notice that our *Author* column contains the name of the authors of an article
separates by the *|* character. So we can *split* each cell on the *Author* column
by the *|* character, and *count* how many parts we have. We can do this for the
whole column at once using the *pd.apply* method. Don't worry too much if this
seems like a bit of magic at the moment, it will become clearer later on.
~~~
articles_df['AuthorCount'] = articles_df.Authors.apply(lambda authors: len(authors.split('|')))
~~~
{: .source}

We often want to calculate summary statistics grouped by subsets or attributes
within fields of our data. For example, we might want to know the number of
articles published by each Publisher.

We can calculate basic statistics for all records in a single column using the
syntax below:

~~~
articles_df['Publisher'].describe()
~~~
{: .source}
gives:

~~~
count                                       1001
unique                                         7
top       International Union of Crystallography
freq                                         858
Name: Publisher, dtype: object
~~~
{: .source}

We can also extract one specific metric if we wish:
~~~
articles_df['Publisher'].unique()
articles_df['Publisher'].count()

articles_df['AuthorCount'].min()
articles_df['AuthorCount'].max()
articles_df['AuthorCount'].mean()
articles_df['AuthorCount'].std()
~~~
{: .source}

But if we want to summarize by one or more variables, for example Language, we can
use Pandas' *.groupby* method. Once we've created a groupby DataFrame, we
can quickly calculate summary statistics by a group of our choice.

~~~
# Group data by sex
sorted = articles_df.groupby('Language')
~~~
{: .source}

The Pandas function *describe* will return descriptive stats including: mean,
median, max, min, std and count for a particular column in the data. Pandas'
*describe* function will only return summary values for columns containing
numeric data.

~~~
# summary statistics for all numeric columns by Language
sorted.describe()
# provide the mean for each numeric column by Language
sorted.mean()
~~~
{: .source}

**OUTPUT:**

~~~
          AuthorCount
Language             
EN           3.977038
ES           4.142857
English      4.308411
FR           5.000000
~~~
{: .output}

The *groupby* command is powerful in that it allows us to quickly generate
summary stats.

> ## Challenge
>
> 1. How many articles are written in which language?
> 2. What happens when you group by two columns using the following syntax and
>     then grab mean values:
>
> ~~~
> sorted2 = articles_df.groupby(['Language', 'Publisher'])
> sorted2.mean()
> ~~~
> {: .source}
>
> 3. Summarize author counts for each publisher in your data. HINT: you can use the
>    following syntax to only create summary statistics for one column in your data
>    *by_publisher['AuthorCount'].describe()*
{: .challenge}

> ## Did you get #3 right?
> A Snippet of the Output from challenge 3 looks like:
> ~~~
> Publisher                                             
> Akshantala Enterprises                           count     13.000000
>                                                  mean       3.384615
>                                                  std        1.850156
>                                                  min        1.000000
>                                                  25%        2.000000
>                                                  50%        3.000000
>                                                  75%        4.000000
>                                                  max        8.000000
> ...
> ~~~
> {: .output}
{: .solution}

## Quickly Creating Summary Counts in Pandas

Let's next count the number of articles for each publisher. We can do this in a few
ways, but we'll use *groupby* combined with a *count()* method.

~~~
# count the number of samples by publisher
article_counts = articles_df.groupby('Publisher')['Title'].count()
~~~
{: .source}

Or, we can also count just the rows that have the publisher "MDPI AG":

~~~
articles_df.groupby('Publisher')['Title'].count()['MDPI AG']
~~~
{: .source}

## Basic Math Functions

If we wanted to, we could perform math on an entire column of our data. For
example let's multiply all author count values by 2. A more practical use of this might
be to normalize the data according to a mean, area, or some other value
calculated from our data.

~~~
# multiply all author count values by 2
articles_df['AuthorCount'] * 2
~~~
{: .source}


> ## Another Challenge
>
> 1. What's another way to create a list of licences and associated *count* of the
>    records in the data? Hint: you can perform *count*, *min*, etc functions on
>    groupby DataFrames in the same way you can perform them on regular
>    DataFrames.
{:.challenge}


# Quick & Easy Plotting Data Using Pandas

We can plot our summary stats using Pandas, too.

~~~
# make sure figures appear inline in Ipython Notebook
%matplotlib inline
# create a quick bar chart
article_counts.plot(kind='bar')
~~~
{: .source}

![Articles by Publisher Plot]({{ page.root }}/fig/articlesByPublisher.png)
Weight by species plot

We can also look at how many articles were published in each language:

~~~
language_count=articles_df.groupby('Language')['Title'].count()
# let's plot that too
language_count.plot(kind='bar');
~~~
{: .source}

> ## Activities
> 1. Create a plot of average number of authors across all publishers.
> 1. Create a plot of average number of authors across all publishers per language.
{:.challenge}


> ## Summary Plotting Challenge
>
> Create a stacked bar plot, with AuthorCount on the Y axis, and the stacked variable
> being Language. The plot should show total authors by Language for each Publisher. Some
> tips are below to help you solve this challenge:
>
> * [For more on Pandas plots, visit this link.](http://pandas.pydata.org/pandas-docs/dev/generated/pandas.core.groupby.DataFrameGroupBy.plot.html)
> * You can use the code that follows to create a stacked bar plot but the data to stack
>   need to be in individual columns.  Here's a simple example with some data where
>   'a', 'b', and 'c' are the groups, and 'one' and 'two' are the subgroups.
>
> ~~~
> d = {'one' : pd.Series([1., 2., 3.], index=['a', 'b', 'c']),'two' : pd.Series([1., 2., 3., 4.], index=['a', 'b', 'c', 'd'])}
> pd.DataFrame(d)
> ~~~
> {: .source}
>
> shows the following data
>
> ~~~
>        one  two
>    a    1    1
>    b    2    2
>    c    3    3
>    d  NaN    4
> ~~~
> {: .output}
>
> We can plot the above with
>
> ~~~
> # plot stacked data so columns 'one' and 'two' are stacked
> my_df = pd.DataFrame(d)
> my_df.plot(kind='bar',stacked=True,title="The title of my graph")
> ~~~
> {: .source}
>
> ![Stacked Bar Plot]({{ page.root }}/fig//stackedBar1.png)
>
> * You can use the *.unstack()* method to transform grouped data into columns
> for each plotting.  Try running *.unstack()* on some DataFrames above and see
> what it yields.
>
> Start by transforming the grouped data (by publisher and language) into an
> unstacked layout, then create a stacked plot.
>
{:.challenge}

> ## Solution to Summary Challenge
>
> First we group data by Publisher and by Language, and then calculate a total for each Publisher.
>
> ~~~
> by_pub_lang = articles_df.groupby(['Publisher','Language'])
> pub_lang_count = by_pub_lang['AuthorCount'].sum()
> ~~~
> {: .source}
>
> This calculates the sums of weights for each language within each publisher as a table
>
> ~~~
> Publisher                                        Language
> Akshantala Enterprises                           English       44
> Consejo Superior de Investigaciones Científicas  EN            10
>                                                  ES            29
>                                                  FR             5
> International Union of Crystallography           EN          3412
> ...
> ~~~
> {: .output}
>
> Below we'll use *.unstack()* on our grouped data to figure out the total weight
> that each language contributed to each publisher.
>
> ~~~
> by_pub_lang = articles_df.groupby(['Publisher','Language'])
> pub_lang_count = by_pub_lang['AuthorCount'].sum()
> pub_lang_count.unstack()
> ~~~
> {: .source}
>
> The *unstack* function above will display the following output:
>
> ~~~
> Language                                             EN    ES  English   FR
> Publisher                                                                  
> Akshantala Enterprises                              NaN   NaN     44.0  NaN
> Aurel Vlaicu University Editing House               NaN   NaN     36.0  NaN
> Consejo Superior de Investigaciones Científicas    10.0  29.0      NaN  5.0
> International Union of Crystallography           3412.0   NaN      NaN  NaN
> MDPI AG                                            42.0   NaN    348.0  NaN
> MDPI AG                                             NaN   NaN     12.0  NaN
> Society of Pharmaceutical Technocrats               NaN   NaN     21.0  NaN
> ~~~
> {: .output}
>
> Now, create a stacked bar plot with that data where the weights for each Publisher
> are stacked by Language.
>
> Rather than display it as a table, we can plot the above data by stacking the
> values of each Publisher as follows:
>
> ~~~
> by_pub_lang = articles_df.groupby(['Publisher','Language'])
> pub_lang_count = by_pub_lang['AuthorCount'].sum()
> spc = pub_lang_count.unstack()
>
> s_plot = spc.plot(kind='bar',stacked=True,title="Total number of authors by Publisher and Language")
> s_plot.set_ylabel("Author Count")
> s_plot.set_xlabel("Publisher");
> ~~~
> {: .source}
>
> ![Stacked Bar Plot]({{ page.root }}/fig/stackedBar.png)
{:.solution}
