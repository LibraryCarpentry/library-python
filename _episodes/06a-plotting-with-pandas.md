---
title: "Plotting Your Data - Pandas"
teaching: ??
exercises: ??
questions:
- "How can you visualize your data?"
objectives:
- "Visualizing data using *pandas*."
keypoints:
- "Line graphs, box plots, bar graphs."

# training: http://swcarpentry.github.io/instructor-training
training: do-we-have-a-repo-of-python-training-resources ?
---

## About Matplotlib

[Matplotlib](http://matplotlib.org/) is a library that can be used to visualize
data that has been loaded with a library like Pandas, Numpy, or Scipy.

For this tutorial, we'll use Pandas for both data loading and as a easy
front end to Matplotlib.

Pandas can use Matplotlib to create a wide variety of plots as shown
in the [Pandas documentation]( http://pandas.pydata.org/pandas-docs/stable/visualization.html).
To be able to display the plots in the Jupyter Notebook we have to turn on the
support for inline graphs by using the "magic" command `%pylab inline`.
The "magic" commands are special instructions for Jupyter Notebook
that start with `%` and are not part of standard Python.

It is best to do this at the top of the notebook that you want to plot
because it loads the Python libraries for plotting in a particular order,
and it can sometimes cause problems if you have already loaded them separately.

~~~
%pylab inline
~~~
{: .source}

If you are using the Python Interactive Shell (with the `>>>` prompt), then you
need to import matplotlib using a normal `import` command instead of the `%pylab inline`.

~~~
>>> import matplotlib.pyplot as plt
~~~
{: .source}

## Loading Data

For a more detailed tutorial on loading data, see
[the lesson on beginning with Pandas]({{ page.root }}/01-starting-with-data/)

For now, we'll just use a simple statement to load the articles data.

~~~
import pandas as pd
articles_df = pd.read_csv('articles.csv')
~~~
{: .source}

Pandas can easily plot a set of data even larger than `articles.csv`, but for
this example, we'll take the first 50 of the ~1000 entries that are in
`articles.csv`. For a more detailed tutorial on slicing data, see
[this lesson on masking and grouping]({{ page.root }}/05-masking-and-groups/).

~~~
small_dataset = articles_df[:50]
~~~
{: .source}

There's a column in `articles.csv` named `Author_Count` which would make an excellent
value to plot.

## Simple Plotting

Now, we have an array of plot data indexed by the `record_id` value. Let's plot it
and give it a label.

~~~
small_dataset.Author_Count.plot(title='My Data')
~~~
{: .source}

This will plot the graph in your Jupyter notebook. If you are using the Python
shell you will need to call `plt.show()` to make the graph visible.

## Saving the Plot

Note: the `plt.savefig()` must be in the same Notebook cell (see below for
how to access the plot in subsequent cells)

In Jupyter notebook we can save the plot to a file like so:

~~~
small_dataset.Author_Count.plot(title='My Data')
plt.savefig('myplot.png')
~~~
{: .source}

This would save the file as a rasterized PNG image. The format is deduced from
the file name or can be given explicitly using `format` parameter,
eg. `format="png"`. For raster images, in order to enhance quality one can also
request particular resolution in dots per inch using `dpi` parameter. This may
be useful when creating quality images for printing/publication. Vectorized
images are supported as well, we just need to save the file as a SVG, EPS or
PDF which is as simple as:

~~~
small_dataset.Author_Count.plot(title='My Data')
fig.savefig('myplot.pdf')
~~~
{: .source}

## Labels

What's a plot without a title, axis labels?
Depending on the type of plot, Pandas will usually create a graph with the
labels and legends set, but you can set them by accessing Matplotlib `plt` commands.

~~~
small_dataset.Author_Count.plot(title='Author counts From articles.csv')
plt.xlabel('Index')
plt.ylabel('Author count')
~~~
{: .source}

## Accessing Plot in later Notebook cells

When you use Pandas to plot graphs, the pd.plot() function returns the Matplotlib
axis object which can be used to make changes to the graph and to save it in
later cells in the Jupyter notebook.

Repeat the plot but saving the result to the variable `ax`:

~~~
ax = small_dataset.Author_Count.plot(label='My Data')
~~~
{: .source}

~~~
fig = ax.get_figure()
fig.savefig('myplot.png')
~~~
{: .source}


## Size and DPI

Another option when creating figures with Pandas is to fine tune the size and
default resolution by using `figsize`, `dpi` and `bbox_inches` parameters:

~~~
small_dataset.Author_Count.plot(title='Author counts From articles.csv', figsize=(10, 8))
plt.savefig('junk.pdf', dpi=200, bbox_inches='tight')
~~~
{: .source}

which will create a figure 8 inches high and 10 inches wide with resolution of
200 dots per inch with the margins of the saved figure as small as possible.

## Managing styles

### Colors

To use a different color, like red, we would plot our data like so:

~~~
small_dataset.Author_Count.plot(color='r')
~~~
{: .source}

Note that creating more than one plot on the same figure will make them use
different colors unless specified otherwise. Here's a list of
predefined colors:

Code | Color
---- | -----
b | blue
g | green
r | red
c | cyan
m | magenta
y | yellow
k | black
w | white

For more color flexibility, you can specify hexadecimal RGB values like so:

~~~
small_dataset.Author_Count.plot(color='#aa5599')
~~~
{: .source}

or use RGB coefficients of range 0-1 (which makes it easy to create multiple
color-encoded plots in a loop):

~~~
small_dataset.Author_Count.plot(color=(0.1, 0.9, 0.6))
~~~
{: .source}

### Line style

The default line style is a solid line. We can make it thinner or thicker by
specifying `linewidth` or `lw`:

~~~
small_dataset.Author_Count.plot(linewidth=3)
~~~
{: .source}

The default `linewidth` is 1. A `linewidth` of 3 would be 3 times as thick as the
default. Likewise, a `linewidth` of .75 would be 3/4 of the thickness of the
default.

## Other types of plots

Pandas can do many types of plots. For example, a dot plot can be
constructed like so:

~~~
small_dataset.Author_Count.plot(style='o')
~~~
{: .source}

The `o` means a dot. There are a variety of markers you can use. Here's a
complete list: [Matplotlib line markers](http://matplotlib.org/api/markers_api.html#module-matplotlib.markers)

A simple dashed line:

~~~
small_dataset.Author_Count.plot(style='--')
~~~
{: .source}

Value | Style
----- | -----
'-'|solid line (default)
'--'|dashed line
'-.'|dash-dot line
':' |dotted line

### Marker style

So far we have plotted only a simple line plot which is default. It is possible
to specify also the data marker style which will create scatter plot or various
connect-the-dots-like plot. For example, to use square data marker:

~~~
small_dataset.Author_Count.plot(marker='s')
~~~
{: .source}

Marker | Meaning
------ | -------
'.'|point
'o'|circle
'v'|triangle down
'^'|triangle up
's'|square
'p'|pentagon
'star'|star
'h'|hexagon
'+'|plus
'D'|diamond

## Configuration of plot axes

So far we have used a simple, default, uniform axis. But you have complete
control over the way axes are organized.

## Plot range

One can adjust the range of axes using set `xlim` for horizontal and
`ylim` for vertical axis. For instance to set `X` limit to [10; 30] one can
use:

~~~
small_dataset.Author_Count.plot(xlim=[10,30])
~~~
{: .source}

### Plot scale

In many cases it is useful to use logarithmic scale on one or both axes.

Just the y-axis:
~~~
small_dataset.Author_Count.plot(logy=True)
~~~
{: .source}

Both:
~~~
small_dataset.Author_Count.plot(loglog=True)
~~~
{: .source}

or
~~~
small_dataset.Author_Count.plot(logx=True, logy=True)
~~~
{: .source}


### Two independent X or Y axes

To create a plot with two X or two Y axes having different scales, units,
ranges one can use `plt.twinx` and `plt.twiny`:

~~~
ax = small_dataset.Author_Count.plot(color='g')
ax2 = ax.twinx()
small_dataset.Citation_Count.plot(color='r', ax=ax2)
ax.set_ylabel('Author Count')
ax2.set_ylabel('Citation Count')
~~~
{: .source}

This will create a plot with two independent Y axes, one for Author_Count and one
for Citation_Count. Both plots will share the same X-axis.

## Describing the plot

In the examples above the plot is not ready to be published. We would like to
add titles, axes labels, tick markers, maybe some grid or legend.

### Adding legend

All plots can be labelled upon creation:

~~~
small_dataset.Author_Count.plot(color='g', title="Citation Count and Author Count")
~~~
{: .source}

and a legend can be automatically generated and positioned either by keyword or
coordinates.

~~~
ax = small_dataset.Author_Count.plot(color='g', title="Citation Count and Author Count")
ax2 = ax.twinx()
small_dataset.Citation_Count.plot(color='r', ax=ax2)
ax.set_ylabel('Author Count')
ax.legend(loc=[1.2, 0.9])
ax2.set_ylabel('Citation Count')
ax2.legend(loc=[1.2, 0.7])
~~~
{: .source}

### Label rotation & Grids

Labels can be rotated by adding parameter `rotation=angle_in_degrees`. To draw
a grid with grid lines at the ticks use `plt.grid()`.

~~~
small_dataset.Citation_Count.plot(rot=45, grid=True)
~~~
{: .source}

## Plot variations

Pandas supports a number of different plot variations by setting the `kind`
parameter including;
kind :
 - 'line' : line plot (default)
 - 'bar' : vertical bar plot
 - 'barh' : horizontal bar plot
 - 'hist' : histogram
 - 'box' : boxplot
 - 'kde' : Kernel Density Estimation plot
 - 'density' : same as 'kde'
 - 'area' : area plot
 - 'pie' : pie plot

To use a bar plot:

~~~
small_dataset.Citation_Count.plot(kind='bar')
~~~
{: .source}

or

~~~
small_dataset.Citation_Count.plot.bar()
~~~
{: .source}

# Go exploring

There are many examples of plot types on these sites:

* [Pandas Visualisation](http://pandas.pydata.org/pandas-docs/stable/visualization.html)
* [Matplotlib Gallery](http://matplotlib.org/gallery.html)
* [Scipy Cookbook](http://wiki.scipy.org/Cookbook/Matplotlib)

A box and whisker plot:

~~~
small_dataset.Author_Count.plot.box()
~~~
{: .source}

# A Realistic Example

You may have noticed there's some more data beyond just the `Author_Count` value in
`articles.csv`. Let's plot the number of articles per month.

Pandas has some built-in tools that make it easy to group your data.

~~~
grouped_small_dataset = articles_df.groupby('Month')
~~~
{: .source}

A bar chart of articles per month:

~~~
ax = grouped_small_dataset.Title.count().plot(kind='bar', color='green', title='Article count per month')
ax.set_ylabel('Number of articles')
~~~
{: .source}

Using a boxplot we can look at the distribution of the number of authors and
citations per article for each language:

~~~
ax = articles_df.boxplot(column=['Author_Count', 'Citation_Count'], by='LanguageId')
~~~
{: .source}

## More Information

This is a basic tutorial to get you started using Python to make your graphs.
For more information on Pandas visualisation, visit the documentation: http://pandas.pydata.org/pandas-docs/stable/visualization.html
