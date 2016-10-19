---
title: "Plotting Your Data - Matplotlib"
teaching: ??
exercises: ??
questions:
- "How can you visualize your data?"
objectives:
- "Visualizing data using *matplotlib*."
keypoints:
- "Line graphs, scatter plots."

# training: http://swcarpentry.github.io/instructor-training
training: do-we-have-a-repo-of-python-training-resources ?
---

## About Matplotlib

[Matplotlib](http://matplotlib.org/) is a library that can be used to visualize
data that has been loaded with a library like Pandas, Numpy, or Scipy.
For this tutorial, we'll use Pandas.

## Loading Data

For a more detailed tutorial on loading data, see
[the lesson on beginning with Pandas]({{ page.root }}/01-starting-with-data/)

For now, we'll just use a simple statement to load the articles data.

~~~
import pandas as pd
articles_df = pd.read_csv('articles.csv')
~~~
{: .source}

Matplotlib has a wide variety of plots that it can produce. First, we'll introduce
the simplest of plots: the 2 dimensional line plot.

First, we'll import matplotlib.

~~~
import matplotlib.pyplot as plt
~~~
{: .source}

Matplotlib can easily plot a set of data even larger than `articles.csv`, but for
this example, we'll take the first 50 of the ~1000 entries that are in
`articles.csv`. For a more detailed tutorial on slicing data, see
[this lesson on masking and grouping]({{ page.root }}/05-masking-and-groups/).

~~~
small_dataset = articles_df[:50]
~~~
{: .source}

There's a column in `articles.csv` named `Author_Count` which would make an excellent
value to plot.

~~~
plot_data = small_dataset['Author_Count']
~~~
{: .source}

## Simple Plotting

Now, we have an array of plot data indexed by the `record_id` value. Let's plot it
and give it a label.

~~~
plt.plot(plot_data, label='My Data')
~~~
{: .source}

The data has now been plotted, to see it we can do 2 things:

1. We can interact with the plot by using `plt.show()` like so:

~~~
plt.show()
~~~
{: .source}

2. Or we can save the plot to a file using `plt.savefig` like so:

~~~
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
plt.savefig('myplot.pdf')
~~~
{: .source}

## Plot Essentials

What's a plot without a title, axis labels, and a legend? These can be easily
set like so:

~~~
plt.xlabel('Index')
plt.ylabel('Author count')
plt.title('Author counts From articles.csv')
~~~
{: .source}

## Clearing the Plot

The plot is created using some default settings, eg. default line color. You
may have noticed, the first line plotted is blue.  We can change this by
replotting the figure. But first, the blue line is still on the plot, so we
must clear it. Like so:

~~~
plt.clf()
~~~
{: .source}

## Managing figures

It is important to note, that subsequent plots we may have created with
`plt.plot`
are (by default) superimposed on the same figure that is created implicitly
upon first `plt.plot` call. Figures are numbered from 1 and one can switch
between them by calling `plt.figure(number)`. When creating a new figures
one can give a number of options, for example one can fine tune the size and
default resolution by using `figsize` and `dpi` parameters:

~~~
plt.figure(figsize=(10, 8), dpi=200)
~~~
{: .source}

which will create a figure 8 inches high and 10 inches wide with resolution of
200 dots per inch.

It is also possible to control the margin between the edge of the axes box and
the edge of the image:

~~~
plt.subplots_adjust(left=0.1, bottom=0.2, right=0.99, top=0.99)
~~~
{: .source}

Values are fractions of the image size and denote the position of the
respective edge of the axes bounding box. Lower left corner of the image is at
(0,0). The invocation above leaves 10% margin on the left side, 20% margin on
the bottom and 1% for top and right edges.

## Managing styles

### Colors

To use a different color, like red, we would plot our data like so:

~~~
plt.plot(plot_data, color='r')
~~~
{: .source}

Note that creating more than one plot on the same figure will make them use
different colors (like in gnuplot) unless specified otherwise. Here's a list of
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
plt.plot(plot_data, color='#aa5599')
~~~
{: .source}

or use RGB coefficients of range 0-1 (which makes it easy to create multiple
color-encoded plots in a loop):

~~~
plt.plot(plot_data, color=(0.1, 0.9, 0.6))
~~~
{: .source}

### Line style

The default line style is a solid line. We can make it thinner or thicker by
specifying `linewidth` or `lw`:

~~~
plt.plot(plot_data, linewidth=3)
~~~
{: .source}

The default `linewidth` is 1. A `linewidth` of 3 would be 3 times as thick as the
default. Likewise, a `linewidth` of .75 would be 3/4 of the thickness of the
default.

## Other types of plots

Matplotlib can do many types of plots. For example, a dot plot can be
constructed like so:

~~~
plt.plot(plot_data, 'o')
~~~
{: .source}

The `o` means a dot. There are a variety of markers you can use. Here's a
complete list: [Matplotlib line markers](http://matplotlib.org/api/markers_api.html#module-matplotlib.markers)

A simple dashed line:

~~~
plt.plot(plot_data, linestyle='--')
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
plt.plot(plot_data, marker='s')
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

So far we have used a simple, default, uniform axis. The user has, however, a
complete control over the way axes are organized.

## Plot range

One can adjust the range of axes using set `plt.xlim` for horizontal and
`plt.ylim` for vertical axis. For instance to set `X` limit to [-10; 15] one can
use:

~~~
plt.xlim(-10, 15)
~~~
{: .source}

### Plot scale

In many cases it is useful to use logarithmic scale on one or both axes. One
can use dedicated plot methods:

Method | Result
------ | ------
`plt.semilogx`|logarithmic scaling on X-axis
`plt.semilogy`|logarithmic scaling on Y-axis
`plt.loglog`|logarithmic scaling on both axes (log-log plot)

~~~
plt.loglog(plot_data)
~~~
{: .source}

### Two independent X or Y axes

To create a plot with two X or two Y axes having different scales, units,
ranges one can use `plt.twinx` and `plt.twiny`:


~~~
plt.bar(plot_data.index, plot_data.values)
plt.twinx()
plt.plot(1/ plot_data, color='k')
~~~
{: .source}

This will create a plot with two independent Y axes, one for barplot and one
for line plot of inverse values. Both plots will share the same X-axis.

## Describing the plot

In the examples above the plot is not ready to be published. We would like to
add titles, axes labels, tick markers, maybe some grid or legend.

### Adding legend

All plots can be labelled upon creation:

~~~
plt.plot(..., label='some description')
~~~
{: .source}

and a legend can be automatically generated in the automatically chosen _best_
location:

~~~
plt.legend(loc='best')
~~~
{: .source}

### Modifying ticks

One can change the location and labels of the axes ticks using `plt.xticks` and
`plt.yticks` methods:

~~~
plt.xticks([1,2,3,4])* # put ticks in given locations of X-axis

plt.yticks([1,2,3], ['A', 'B', 'C'])* # put ticks in given locations on Y-axis, denote them with letters
~~~
{: .source}

Labels can be rotated by adding parameter `rotation=angle_in_degrees`. To draw
a grid with grid lines at the ticks use `plt.grid()`.

### Labelling

Axes can be labelled using:

~~~
plt.xlabel('X-axis label')
plt.ylabel('Y-axis label')
~~~
{: .source}

To set a title use

~~~
plt.title('Plot title')
~~~
{: .source}

## Plot variations

Matplotlib supports a number of different plot variations including;
* bar plot (`plt.bar`),
* contour plots (`plt.contour`),
* pie chart (`plt.pie`),
* error bars (`plt.errorbar`) and
* polar plot (`plt.polar`).

To use a bar plot:

~~~
plt.bar(plot_data.index, plot_data.values)
~~~
{: .source}

# Go exploring

There are excellent examples on [Matplotlib](http://matplotlib.org/) website,
especially:

* [Matplotlib Gallery](http://matplotlib.org/gallery.html)
* [Scipy Cookbook](http://wiki.scipy.org/Cookbook/Matplotlib)

A box and whisker plot:

~~~
plt.boxplot(plot_data.values)
~~~
{: .source}

# A Realistic Example -- TODO

You may have noticed there's some more data beyond just the plot value in
`articles.csv`. Let's plot the number of authors and group them by month.
A dot plot would be ideal for this.

Pandas has some built-in tools that make it easy to group your data.

~~~
grouped_plot_data = articles_df.groupby('Month')
~~~
{: .source}

This returns our data in an iterable object. Each entry in `grouped_plot_data`
is formatted like so:

~~~
('group_name', pandas data pertaining to the group)
~~~
{: .output}

We can check the size of each data group using the `len()` function. So we can
plot our data like so:

~~~
for group_label, group_data in grouped_plot_data:
    plt.plot(group_label, len(group_data), 'o')
plt.xlim(0, 13)
plt.xlabel('Month')
plt.ylabel('Number of articles')
plt.title('Article count per month')
~~~
{: .source}

## More Information

This is a basic tutorial to get you started using Python to make your graphs.
For more information on Matplotlib, visit the official site: http://matplotlib.org/
