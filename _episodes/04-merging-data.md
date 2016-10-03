---
title: "Combining DataFrames with pandas"
teaching: ??
exercises: ??
questions:
- "How do we combine data from multiple sources?"
objectives:
- "Learn how to concatenate two DataFrames together (append one dataFrame to a second dataFrame)."
- "Learn how to join two DataFrames together using a uniqueID found in both DataFrames."
- "Learn how to write out a DataFrame to csv using Pandas."

keypoints:
- "Concatenating data"
- "Data output"
- "Joining DataFrames"

# training: http://swcarpentry.github.io/instructor-training
training: do-we-have-a-repo-of-python-training-resources ?
---

In many "real world" situations, the data that we want to use come in multiple
files. We often need to combine these files into a single DataFrame to analyze
the data. The pandas package provides [various methods for combining
DataFrames](http://pandas.pydata.org/pandas-docs/stable/merging.html) including
*merge* and *concat*.

In these examples we will be using the same data set, but divided into different
tables, which you can download from [figshare](https://figshare.com/articles/LC-articles/3409471)

To work through the examples below, we first need to load the species and
surveys files into pandas DataFrames. In iPython:

~~~
import pandas as pd
articles_df = pd.read_csv('articles.csv',
                         keep_default_na=False, na_values=[""])
articles_df
~~~
{: .source}
~~~
        id                                              Title  \
0        0   The Fisher Thermodynamics of Quasi-Probabilities   
1        1  Aflatoxin Contamination of the Milk Supply: A ...   
2        2  Metagenomic Analysis of Upwelling-Affected Bra...   
...
999       1  2015  
1000     11  2015  

[1001 rows x 16 columns]
~~~
{: .output}

~~~
journals_df = pd.read_csv('journals.csv')
journals_df
~~~
{: .source}

~~~
    id     ISSN-L                ISSNs  PublisherId  \
0    0  2056-9890            2056-9890            1   
1    1  2077-0472            2077-0472            2   
2    2  2073-4395            2073-4395            2   
...
49  49  1999-4915            1999-4915            2   
50  50  2073-4441            2073-4441            2   

                                        Journal_Title  
0   Acta Crystallographica Section E Crystallograp...  
1                                         Agriculture  
2                                            Agronomy  
...
49                                            Viruses  
50                                              Water
~~~
{: .output}

Take note that the *read_csv* method we used can take some additional options which
we didn't use previously. Many functions in python have a set of options that
can be set by the user if needed. In this case, we have told Pandas to assign
empty values in our CSV to NaN *keep_default_na=False, na_values=[""]*.
[http://pandas.pydata.org/pandas-docs/dev/generated/pandas.io.parsers.read_csv.html](More
about all of the read_csv options here.)


# Concatenating DataFrames

We can use the *concat* function in Pandas to append either columns or rows from
one DataFrame to another.  Let's grab two subsets of our data to see how this
works.

~~~
# read in first 10 lines of surveys table
articles_sub = articles_df.head(10)
# grab the last 10 rows (minus the last one)
articles_sub_last10 = articles_df[-11:-1]
#reset the index values to the second dataframe appends properly
articles_sub_last10 = articles_sub_last10.reset_index(drop=True)
# drop=True option avoids adding new index column with old index values
~~~
{: .source}

When we concatenate DataFrames, we need to specify the axis. *axis=0* tells
Pandas to stack the second DataFrame under the first one. It will automatically
detect whether the column names are the same and will stack accordingly.
*axis=1* will stack the columns in the second DataFrame to the RIGHT of the
first DataFrame. To stack the data vertically, we need to make sure we have the
same columns and associated column format in both datasets. When we stack
horizonally, we want to make sure what we are doing makes sense (ie the data are
related in some way).

~~~
# stack the DataFrames on top of each other
vertical_stack = pd.concat([articles_sub, articles_sub_last10], axis=0)

# place the DataFrames side by side
horizontal_stack = pd.concat([articles_sub, articles_sub_last10], axis=1)
~~~
{: .source}

### Row Index Values and Concat
Have a look at the *vertical_stack* dataframe? Notice anything unusual?
The row indexes for the two data frames *articles_sub* and *articles_sub_last10*
have been repeated. We can reindex the new dataframe using the *reset_index()* method.

## Writing Out Data to CSV

We can use the *to_csv* command to do export a DataFrame in CSV format. Note that the code
below will by default save the data into the current working directory. We can
save it to a different folder by adding the foldername and a slash to the file
*vertical_stack.to_csv('foldername/out.csv')*.

~~~
# Write DataFrame to CSV
vertical_stack.to_csv('out.csv')
~~~
{: .source}

Check out your working directory to make sure the CSV wrote out properly, and
that you can open it! If you want, try to bring it back into python to make sure
it imports properly.

~~~
# for kicks read our output back into python and make sure all looks good
newOutput = pd.read_csv('out.csv', keep_default_na=False, na_values=[""])
~~~
{: .source}

> ## Challenge
>
> Split the articles_df DataFrame by month, and save as a separate CSV file.
> Afterwards read in some of your files, to make sure they can be loaded
> back to Python.
{: .challenge}

# Joining DataFrames

When we concatenated our DataFrames we simply added them to each other -
stacking them either vertically or side by side. Another way to combine
DataFrames is to use columns in each dataset that contain common values (a
common unique id). Combining DataFrames using a common field is called
"joining". The columns containing the common values are called "join key(s)".
Joining DataFrames in this way is often useful when one DataFrame is a "lookup
table" containing additional data that we want to include in the other.

NOTE: This process of joining tables is similar to what we do with tables in an
SQL database.

For example, the *journals.csv* file that we've been working with is a lookup
table. This table contains the name of the different journals and an journal ID.
The journal ID is unique for each line. These journals are identified in our articles
table as well using the unique journal id. Rather than adding the full name of
the journal to the articles table, we can maintain the shorter table with the
journal information. When we want to access that information, we can create a
query that joins the additional columns of information to the articles data.

Storing data in this way has many benefits including:

1. It ensures consistency in the spelling of journal information (Journal title,
  ISSN, Publisher ID).
2. It also makes it easy for us to make changes to the journal information once
   without having to find each instance of it in the larger article table.
3. It optimizes the size of our data.


## Joining Two DataFrames

To better understand joins, let's grab the first 10 lines of our data as a
subset to work with. We'll use the *.head* method to do this. We'll also read
in a subset of the species table.

~~~
# read in first 10 lines of articles table
articles_sub = articles_df.head(10)

# read in first 15 lines of journals table
journals_sub = journals_df.head(15)
~~~
{: .source}

In this example, we want to join with the data in *articles_sub* with the data
from both *journals_sub*.


## Identifying join keys

To identify appropriate join keys we first need to know which field(s) are
shared between the files (DataFrames). We might inspect both DataFrames to
identify these columns. If we are lucky, both DataFrames will have columns with
the same name that also contain the same data. If we are less lucky, we need to
identify a (differently-named) column in each DataFrame that contains the same
information.

~~~
journals_sub.columns
~~~
{: .source}

~~~
Index([u'id', u'ISSN-L', u'ISSNs', u'PublisherId', u'Journal_Title'], dtype='object')
~~~
{: .output}

~~~
articles_sub.columns
~~~
{: .source}

~~~
Index([u'id', u'Title', u'Authors', u'DOI', u'URL', u'Subjects', u'ISSNs',
       u'Citation', u'LanguageId', u'LicenceId', u'Author_Count',
       u'First_Author', u'Citation_Count', u'Day', u'Month', u'Year'],
      dtype='object')
~~~
{: .output}

In our example, the join key is the column containing the ISSNs code, called
*ISSNs*.

Now that we know the fields which links the two data frames, we are almost ready
to join our data. However, since there are [different types of joins](http://blog.codinghorror.com/a-visual-explanation-of-sql-joins/),
we also need to decide which type of join makes sense for our analysis.

## Inner joins

The most common type of join is called an _inner join_. An inner join combines
two DataFrames based on a join key and returns a new DataFrame that contains
**only** those rows that have matching values in *both* of the original
DataFrames.

Inner joins yield a DataFrame that contains only rows where the value being
joins exists in BOTH tables. An example of an inner join, adapted from [this
page](http://blog.codinghorror.com/a-visual-explanation-of-sql-joins/) is below:

![Inner join -- courtesy of codinghorror.com](http://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702708970c-pi.png)

The pandas function for performing joins is called *merge* and an Inner join is
the default option:  

~~~
merged_inner = pd.merge(left=articles_sub,right=journals_sub, left_on='ISSNs', right_on='ISSNs')
# in this case *ISSNs* is the only column name in  both dataframes, so if we skip *left_on*
# and *right_on* arguments we would still get the same result

# what's the size of the output data?
merged_inner.shape
~~~
{: .source}

~~~
(4, 20)
~~~
{: .output}

~~~
merged_inner
~~~
{: .source}

~~~
   id_x                                              Title  \
0     0   The Fisher Thermodynamics of Quasi-Probabilities   
1     1  Aflatoxin Contamination of the Milk Supply: A ...   
2     6  Ionic Liquids as Carbene Catalyst Precursors i...   
3     9            Imaging of HCC—Current State of the Art   
...
      ISSN-L  PublisherId Journal_Title  
0  1099-4300            2       Entropy  
1  2077-0472            2   Agriculture  
2  2073-4344            2     Catalysts  
3  2075-4418            2   Diagnostics  
~~~
{: .output}

The result of an inner join of *articles_sub* and *journals_sub* is a new DataFrame
that contains the combined set of columns from *articles_sub* and *journals_sub*. It
*only* contains rows that have two-letter species codes that are the same in
both the *articles_sub* and *journals_sub* DataFrames. In other words, if a row in
*articles_sub* has a value of *ISSNs* that does *not* appear in the *journals_sub*,
it will not be included in the DataFrame returned by an
inner join.  Similarly, if a row in *journals_sub* has a value of *ISSNs*
that does *not* appear in *articles_sub*, that row will not
be included in the DataFrame returned by an inner join.

The two DataFrames that we want to join are passed to the *merge* function using
the *left* and *right* argument. The *left_on='ISSNs'* argument tells *merge*
to use the *ISSNs* column as the join key from *articles_sub* (the *left*
DataFrame). Similarly , the *right_on='ISSNs'* argument tells *merge* to
use the *ISSNs* column as the join key from *journals_sub* (the *right*
DataFrame). For inner joins, the order of the *left* and *right* arguments does
not matter.

The result *merged_inner* DataFrame contains all of the columns from *articles_sub*
(record id, month, day, etc.) as well as all the columns from *journals_sub*
(ISSNs, PublisherId, Journal_Title).

Notice that *merged_inner* has fewer rows than *articles_sub*. This is an
indication that there were rows in *articles_sub* with value(s) for *ISSNs* that
do not exist as value(s) for *ISSNs* in *journals_sub*.

## Left joins

What if we want to add information from *journals_sub* to *articles_sub* without
losing any of the information from *articles_sub*? In this case, we use a different
type of join called a "left outer join", or a "left join".

Like an inner join, a left join uses join keys to combine two DataFrames. Unlike
an inner join, a left join will return *all* of the rows from the *left*
DataFrame, even those rows whose join key(s) do not have values in the *right*
DataFrame.  Rows in the *left* DataFrame that are missing values for the join
key(s) in the *right* DataFrame will simply have null (i.e., NaN or None) values
for those columns in the resulting joined DataFrame.

Note: a left join will still discard rows from the *right* DataFrame that do not
have values for the join key(s) in the *left* DataFrame.

![Left Join](http://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b01287770273e970c-pi.png)

A left join is performed in pandas by calling the same *merge* function used for
inner join, but using the *how='left'* argument:

~~~
merged_left = pd.merge(left=articles_sub,right=journals_sub, how='left', left_on='ISSNs', right_on='ISSNs')
merged_left
~~~
{: .source}

~~~
   id_x                                              Title  \
0     0   The Fisher Thermodynamics of Quasi-Probabilities   
1     1  Aflatoxin Contamination of the Milk Supply: A ...   
2     2  Metagenomic Analysis of Upwelling-Affected Bra...   
3     3  Synthesis and Reactivity of a Cerium(III) Scor...   
4     4  Performance and Uncertainty Evaluation of Snow...   
5     5  Dihydrochalcone Compounds Isolated from Crabap...   
6     6  Ionic Liquids as Carbene Catalyst Precursors i...   
7     7  Characterization of Aspartate Kinase from Cory...   
8     8  Quaternifications and Extensions of Current Al...   
9     9            Imaging of HCC—Current State of the Art   

...

      ISSN-L  PublisherId Journal_Title  
0  1099-4300          2.0       Entropy  
1  2077-0472          2.0   Agriculture  
2        NaN          NaN           NaN  
3        NaN          NaN           NaN  
4        NaN          NaN           NaN  
5        NaN          NaN           NaN  
6  2073-4344          2.0     Catalysts  
7        NaN          NaN           NaN  
8        NaN          NaN           NaN  
9  2075-4418          2.0   Diagnostics  
~~~
{: .output}

The result DataFrame from a left join (*merged_left*) looks very much like the
result DataFrame from an inner join (*merged_inner*) in terms of the columns it
contains. However, unlike *merged_inner*, *merged_left* contains the **same
number of rows** as the original *articles_sub* DataFrame. When we inspect
*merged_left*, we find there are rows where the information that should have
come from *journals_sub* (i.e., *ISSN-L*, *PublisherId*, *Journal_Title*) is
missing (they contain NaN values):

~~~
merged_left[ pd.isnull(merged_left.PublisherId) ]
~~~
{: .source}
~~~
   id_x                                              Title  \
2     2  Metagenomic Analysis of Upwelling-Affected Bra...   
3     3  Synthesis and Reactivity of a Cerium(III) Scor...   
4     4  Performance and Uncertainty Evaluation of Snow...   
5     5  Dihydrochalcone Compounds Isolated from Crabap...   
7     7  Characterization of Aspartate Kinase from Cory...   
8     8  Quaternifications and Extensions of Current Al...   

...

  ISSN-L  PublisherId Journal_Title  
2    NaN          NaN           NaN  
3    NaN          NaN           NaN  
4    NaN          NaN           NaN  
5    NaN          NaN           NaN  
7    NaN          NaN           NaN  
8    NaN          NaN           NaN  
~~~
{: .output}

These rows are the ones where the value of *ISSNs* from *articles_sub* does not
exist in *journals_sub*.


## Other join types

The pandas *merge* function supports two other join types:

* Right (outer) join: Invoked by passing *how='right'* as an argument. Similar
  to a left join, except *all* rows from the *right* DataFrame are kept, while
  rows from the *left* DataFrame without matching join key(s) values are
  discarded.
* Full (outer) join: Invoked by passing *how='outer'* as an argument. This join
  type returns the all pairwise combinations of rows from both DataFrames; i.e.,
  the result DataFrame will *NaN* where data is missing in one of the dataframes.
  This join type is very rarely used.

> ## Challenge 1
> Create a new DataFrame by joining the contents of the *articles.csv* and
> *journals.csv* tables. Are there any records with do not have ISSNs code?
{: .challenge}

> ## Challenge 2
>
> The *publishers.csv* contains data the names of the publishers for each
> journal. Create a DataFrame which also joins this data.
{: .challenge}
