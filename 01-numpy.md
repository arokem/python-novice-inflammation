---
layout: page
title: Programming with Python
subtitle: Analyzing Patient Data
minutes: 30
---
> ## Learning Objectives {.objectives}
>
> *   Explain what a library is, and what libraries are used for.
> *   Load a Python library and use the things it contains.
> *   Read tabular data from a file into a program.
> *   Assign values to variables.
> *   Select individual values and subsections from data.
> *   Perform operations on arrays of data.
> *   Display simple graphs.

Words are useful,
but what's more useful are the sentences and stories we build with them.
Similarly,
while a lot of powerful tools are built into languages like Python,
even more live in the [libraries](reference.html#library) they are used to build.

In order to load our inflammation data,
we need to [import](reference.html#import) a library called NumPy.
In general you should use this library if you want to do fancy things with numbers,
especially if you have matrices or arrays.
We can load NumPy using:

~~~ {.python}
import numpy
~~~

Importing a library is like getting a piece of lab equipment out of a storage
locker and setting it up on the bench. Libraries provide additional
functionality to the basic Python package, much like a new piece of equipment
adds functionality to a lab space. Once you've loaded the library, we can ask
the library to read our data file for us:

~~~ {.python}
numpy.load('slice1.npy')
~~~
~~~ {.output}
array([[0, 0, 0, ..., 0, 0, 0],
       [0, 0, 0, ..., 1, 0, 0],
       [0, 0, 0, ..., 0, 1, 0],
       ...,
       [0, 0, 0, ..., 2, 1, 1],
       [0, 0, 0, ..., 2, 0, 1],
       [0, 0, 0, ..., 1, 1, 0]], dtype=int16)
~~~

The expression `numpy.load(...)` is a [function
call](reference.html#function-call) that asks Python to run the function
`load` that belongs to the `numpy` library. This [dotted
notation](reference.html#dotted-notation) is used everywhere in Python to refer
to the parts of things as `thing.component`.

`numpy.load` has a single [parameter](reference.html#parameter): the name of the
file we want to read. This needs to be a character string (or
[strings](reference.html#string) for short),and we put it in quotes.

When we are finished typing and press Shift+Enter, the notebook runs our
command. Since we haven't told it to do anything else with the function's
output, the notebook displays it. In this case, that output is the data we just
loaded. By default, only a few rows and columns are shown (with `...` to omit
elements when displaying big arrays). To save space, Python displays numbers as
`1.` instead of `1.0` when there's nothing interesting after the decimal point.

Our call to `numpy.load` read our file, but didn't save the data in memory. To
do that, we need to [assign](reference.html#assignment) the array to a
[variable](reference.html#variable). A variable is a name for a value, such as
`x`, `current_temperature`, or `subject_id`. Python's variables must begin with
a letter and are [case sensitive](reference.html#case-sensitive). We can create
a new variable by assigning a value to it using `=`. As an illustration, let's
step back and instead of considering a table of data, consider the simplest
"collection" of data, a single value. The line below assigns the value `55` to a
variable `weight_kg`:

~~~ {.python}
weight_kg = 55
~~~

Once a variable has a value, we can print it to the screen:

~~~ {.python}
print(weight_kg)
~~~
~~~ {.output}
55
~~~

and do arithmetic with it:

~~~ {.python}
print('weight in pounds:', 2.2 * weight_kg)
~~~
~~~ {.output}
weight in pounds: 121.0
~~~

We can also change a variable's value by assigning it a new one:

~~~ {.python}
weight_kg = 57.5
print('weight in kilograms is now:', weight_kg)
~~~
~~~ {.output}
weight in kilograms is now: 57.5
~~~

As the example above shows,
we can print several things at once by separating them with commas.

If we imagine the variable as a sticky note with a name written on it,
assignment is like putting the sticky note on a particular value:

![Variables as Sticky Notes](fig/python-sticky-note-variables-01.svg)

This means that assigning a value to one variable does *not* change the values
of other variables. For example, let's store the subject's weight in pounds in a
variable:

~~~ {.python}
weight_lb = 2.2 * weight_kg
print('weight in kilograms:', weight_kg, 'and in pounds:', weight_lb)
~~~
~~~ {.output}
weight in kilograms: 57.5 and in pounds: 126.5
~~~

![Creating Another Variable](fig/python-sticky-note-variables-02.svg)

and then change `weight_kg`:

~~~ {.python}
weight_kg = 100.0
print('weight in kilograms is now:', weight_kg, 'and weight in pounds is still:', weight_lb)
~~~
~~~ {.output}
weight in kilograms is now: 100.0 and weight in pounds is still: 126.5
~~~

![Updating a Variable](fig/python-sticky-note-variables-03.svg)

Since `weight_lb` doesn't "remember" where its value came from,
it isn't automatically updated when `weight_kg` changes.
This is different from the way spreadsheets work.

> ## Who's who in the memory {.callout}
>
>You can use the `%whos` command at any time to see what variables you have created and what modules you have loaded into the computers memory. As this is an IPython command, it will only work if you are in an IPython terminal or the Jupyter Notebook.
>
>~~~ {.python}
>%whos
>~~~
>~~~ {.output}
>Variable    Type       Data/Info
>--------------------------------
>numpy       module     <module 'numpy' from '/Us<...>kages/numpy/__init__.py'>
>weight_kg   float      100.0
>weight_lb   float      126.5
>~~~

Just as we can assign a single value to a variable, we can also assign an array
of values to a variable using the same syntax.  Let's re-run `numpy.load` and
save its result:

~~~ {.python}
data = numpy.load('slice1.npy')
~~~

This statement doesn't produce any output because assignment doesn't display
anything. If we want to check that our data has been loaded, we can print the
variable's value:

~~~ {.python}
print(data)
~~~
~~~ {.output}
[[0 0 0 ..., 0 0 0]
 [0 0 0 ..., 1 0 0]
 [0 0 0 ..., 0 1 0]
 ...,
 [0 0 0 ..., 2 1 1]
 [0 0 0 ..., 2 0 1]
 [0 0 0 ..., 1 1 0]]
 ~~~

Now that our data is in memory, we can start doing things with it. First, let's
ask what [type](reference.html#type) of thing `data` refers to:

~~~ {.python}
print(type(data))
~~~
~~~ {.output}
<class 'numpy.ndarray'>
~~~

The output tells us that `data` currently refers to an N-dimensional array
created by the NumPy library. These data correspond to an image of a brain taken
from an MRI experiment. The rows are one dimension of the image and the columns
are the other dimension . The values are the image intensity in each of the
pixels. We can see what the image dimensions are by referring to its
[shape](reference.html#shape), like this:

~~~ {.python}
print(data.shape)
~~~
~~~ {.output}
(81, 106)
~~~

This tells us that `data` has 81 rows and 106 columns. When we created the
variable `data` to store our brain MRI data, we didn't just create the array, we
also created information about the array, called
[members](reference.html#member) or attributes. This extra information describes
`data` in the same way an adjective describes a noun. `data.shape` is an
attribute  of `data` which described the dimensions of `data`. We use the same
dotted notation for the attributes of variables that we use for the functions in
libraries because they have the same part-and-whole relationship.

If we want to get a single number from the array, we must provide an
[index](reference.html#index) in square brackets, just as we do in math:

~~~ {.python}
print('first value in data:', data[0, 0])
~~~
~~~ {.output}
first value in data: 0.0
~~~

~~~ {.python}
print('middle value in data:', data[40, 53])
~~~
~~~ {.output}
middle value in data: 662
~~~

Referring to a specific element of the array is called "indexing". If you've
programmed before, you might not be too surprised by the expression `data[40,
53]`, but `data[0, 0]` might.

Depending on what you've used in the past! Programming languages like Fortran
and MATLAB start counting at 1, because that's what human beings have done for
thousands of years. Languages in the C family (including C++, Java, Perl, and
Python) count from 0 because that's simpler for computers to do. As a result, if
we have an M&times;N array in Python, its indices go from 0 to M-1 on the first
axis and 0 to N-1 on the second. It takes a bit of getting used to, but one way
to remember the rule is that the index is how many steps we have to take from
the start to get the item we want.

> ## In the Corner {.callout}
>
> What may also surprise you is that when Python displays an array,
> it shows the element with index `[0, 0]` in the upper left corner
> rather than the lower left.
> This is consistent with the way mathematicians draw matrices,
> but different from the Cartesian coordinates.
> The indices are (row, column) instead of (column, row) for the same reason,
> which can be confusing when plotting data.

An index like `[40, 53]` selects a single element of an array, but we can select
whole sections as well. For example, we can select the first ten columns
of values for the first four rows like this:

~~~ {.python}
print(data[0:4, 0:10])
~~~
~~~ {.output}
[[ 0  0  0  0  0  0  1  2  7  2]
 [ 0  0  0  0  0  0  3  5  6  8]
 [ 0  0  0  0  0  0  6  6 12  9]
 [ 0  0  0  0  0  0  7  2  5  6]]
~~~

The [slice](reference.html#slice) `0:4` means, "Start at index 0 and go up to,
but not including, index 4." Again, the up-to-but-not-including takes a bit of
getting used to, but the rule is that the difference between the upper and lower
bounds is the number of values in the slice.

We don't have to start slices at 0:

~~~ {.python}
print(data[5:10, 0:10])
~~~
~~~ {.output}
[[ 0  0  0  0  0  0  6  4  6 20]
 [ 0  0  0  0  0  0  2  2 11 31]
 [ 0  0  0  0  0  0  5 24 22  4]
 [ 0  0  0  0  0  0 20 25 26 13]
 [ 0  0  0  0  0  1 31 29 20 38]]

~~~

We also don't have to include the upper and lower bound on the slice. If we
don't include the lower bound, Python uses 0 by default; if we don't include the
upper, the slice runs to the end of the axis, and if we don't include either
(i.e., if we just use ':' on its own), the slice includes everything:

~~~ {.python}
small = data[:3, 100:]
print('small is:')
print(small)
~~~
~~~ {.output}
small is:
[[0 0 0 0 0 0]
 [1 0 1 1 0 0]
 [2 0 0 0 1 0]]
~~~

Arrays also know how to perform common mathematical operations on their values.
The simplest operations with data are arithmetic: add, subtract, multiply, and
divide. When you do such operations on arrays, the operation is done on each
individual element of the array. Thus:

~~~ {.python}
doubledata = data * 2
~~~

will create a new array `doubledata`
whose elements have the value of two times the value of the corresponding elements in `data`:

~~~ {.python}
print('original:')
print(data[:3, 100:])
print('doubledata:')
print(doubledata[:3, 100:])
~~~
~~~ {.output}
original:
[[0 0 0 0 0 0]
 [1 0 1 1 0 0]
 [2 0 0 0 1 0]]
doubledata:
[[0 0 0 0 0 0]
 [2 0 2 2 0 0]
 [4 0 0 0 2 0]]
 ~~~

If, instead of taking an array and doing arithmetic with a single value (as
above) you did the arithmetic operation with another array of the same shape,
the operation will be done on corresponding elements of the two arrays. Thus:

~~~ {.python}
tripledata = doubledata + data
~~~

will give you an array where `tripledata[0,0]` will equal `doubledata[0,0]` plus
`data[0,0]`, and so on for all other elements of the arrays.

~~~ {.python}
print('tripledata:')
print(tripledata[:3, 36:])
~~~
~~~ {.output}
tripledata:
tripledata:
[[0 0 0 0 0 0]
 [3 0 3 3 0 0]
 [6 0 0 0 3 0]]
~~~

Often, we want to do more than add, subtract, multiply, and divide values of
data. Arrays also know how to do more complex operations on their values. If we
want to find the average inflammation for all patients on all days, for example,
we can just ask the array for its mean value

~~~ {.python}
print(data.mean())
~~~
~~~ {.output}
6.14875
~~~

`mean` is a [method](reference.html#method) of the array, i.e., a function that
belongs to it in the same way that the member `shape` does. If variables are
nouns, methods are verbs: they are what the thing in question knows how to do.
We need empty parentheses for `data.mean()`, even when we're not passing in any
parameters, to tell Python to go and do something for us. `data.shape` doesn't
need `()` because it is just a description but `data.mean()` requires the `()`
because it is an action.

NumPy arrays have lots of useful methods:

~~~ {.python}
print('maximum pixel value:', data.max())
print('minimum pixel value:', data.min())
print('standard deviation:', data.std())
~~~
~~~ {.output}
maximum pixel value: 4711
minimum pixel value: 0
standard deviation: 629.59237567
~~~

When analyzing data, though, we often want to look at partial statistics, such
as the maximum value per row or the average value per column. One way to do
this is to create a new temporary array of the data we want, then ask it to do
the calculation:

~~~ {.python}
row_0 = data[0, :] # 0 on the first axis, everything on the second
print('maximum pixel value for row 0:', row_0.max())
~~~
~~~ {.output}
maximum pixel value for row 0: 53
~~~

We don't actually need to store the row in a variable of its own. Instead, we
can combine the selection and the method call:

~~~ {.python}
print('maximum pixel value for row 2:', data[2, :].max())
~~~
~~~ {.output}
maximum pixel value for row 2: 204
~~~

What if we need the maximum pixel value for *each* row (as in the
next diagram on the left), or the average for each column (as in the
diagram on the right)? As the diagram below shows, we want to perform the
operation across an axis:

![Operations Across Axes](fig/python-operations-across-axes.svg)

To support this, most array methods allow us to specify the axis we want to work
on. If we ask for the average across axis 0 (rows in our 2D example), we get:

~~~ {.python}
print(data.mean(axis=0))
~~~
~~~ {.output}
[  0.00000000e+00   0.00000000e+00   6.17283951e-02   7.49382716e+00
   1.10987654e+01   1.60864198e+01   2.44444444e+01   2.73827160e+01
   8.29259259e+01   1.20617284e+02   1.11518519e+02   1.88086420e+02
   3.09691358e+02   3.92876543e+02   4.05308642e+02   4.58753086e+02
   5.00567901e+02   4.92407407e+02   4.90000000e+02   5.53197531e+02
   5.66592593e+02   5.50679012e+02   5.36876543e+02   5.35259259e+02
   5.68419753e+02   5.68938272e+02   5.68419753e+02   5.76185185e+02
   5.94666667e+02   5.77691358e+02   5.45197531e+02   5.32185185e+02
   5.46790123e+02   6.05135802e+02   6.20506173e+02   5.78222222e+02
   5.92950617e+02   6.58716049e+02   7.19012346e+02   7.35666667e+02
   6.87074074e+02   6.33740741e+02   5.97493827e+02   6.20345679e+02
   6.26135802e+02   5.73765432e+02   5.14160494e+02   5.32197531e+02
   5.85567901e+02   6.10407407e+02   5.67851852e+02   5.17185185e+02
   5.20185185e+02   6.29864198e+02   6.91061728e+02   6.43197531e+02
   5.98185185e+02   5.79148148e+02   6.13876543e+02   6.11123457e+02
   6.01666667e+02   6.50716049e+02   6.89950617e+02   6.97851852e+02
   7.91790123e+02   8.36481481e+02   7.95395062e+02   7.94444444e+02
   9.20333333e+02   9.68419753e+02   9.29716049e+02   8.02333333e+02
   7.10765432e+02   6.86246914e+02   6.41950617e+02   5.57456790e+02
   5.80716049e+02   6.37345679e+02   6.17765432e+02   6.33641975e+02
   6.84469136e+02   7.03333333e+02   6.93308642e+02   7.91135802e+02
   7.33395062e+02   6.87370370e+02   7.59654321e+02   8.76135802e+02
   1.11124691e+03   1.18049383e+03   1.00862963e+03   9.19987654e+02
   9.18135802e+02   8.60975309e+02   7.34975309e+02   6.36753086e+02
   3.84222222e+02   1.97493827e+02   7.88888889e+01   8.14567901e+01
   1.12111111e+02   1.42987654e+02   1.38604938e+02   4.38888889e+01
   2.23456790e+01   1.50864198e+01]
~~~

As a quick check, we can ask this array what its shape is:

~~~ {.python}
print(data.mean(axis=0).shape)
~~~
~~~ {.output}
(106,)
~~~

The expression `(106,)` tells us we have an N&times;1 vector, so this is the
average pixel value per row for all columns. If we average across axis 1
(columns in our 2D example), we get:

~~~ {.python}
print(data.mean(axis=1))
~~~
~~~ {.output}
[  18.17924528   17.9245283    34.59433962   85.22641509  103.24528302
   92.60377358  130.90566038  267.76415094  339.31132075  410.83962264
  438.54716981  564.41509434  698.61320755  664.51886792  611.94339623
  603.02830189  580.          573.41509434  612.93396226  643.83018868
  686.16981132  679.38679245  650.41509434  635.38679245  693.30188679
  748.97169811  689.85849057  657.80188679  661.27358491  654.71698113
  621.72641509  593.08490566  597.93396226  586.21698113  583.26415094
  647.5         717.9245283   829.          844.94339623  838.83962264
  846.82075472  784.67924528  818.58490566  792.80188679  733.9245283
  654.71698113  637.54716981  626.19811321  607.63207547  595.88679245
  565.36792453  593.01886792  672.38679245  725.63207547  699.93396226
  661.41509434  657.87735849  645.          649.98113208  624.37735849
  602.33018868  611.03773585  637.53773585  596.43396226  582.40566038
  628.30188679  646.25471698  617.72641509  592.49056604  636.63207547
  564.06603774  553.95283019  497.33962264  294.22641509  137.39622642
  136.90566038  162.75471698  141.33018868  138.1509434    48.03773585
   16.69811321]
~~~

which is the average pixel value per column across all rows.

The mathematician Richard Hamming once said, "The purpose of computing is
insight, not numbers," and the best way to develop insight is often to visualize
data. Visualization deserves an entire lecture (or course) of its own, but we
can explore a few features of Python's `matplotlib` library here. While there is
no "official" plotting library, this package is the de facto standard. First, we
will import the `pyplot` module from `matplotlib` and use two of its functions
to create and display a heat map of our data:

~~~ {.python}
import matplotlib.pyplot
image  = matplotlib.pyplot.imshow(data)
matplotlib.pyplot.show()
~~~

![Heatmap of the Data](fig/01-numpy_71_0.png)

Blue regions in this heat map are low values, while red shows high values. It's
a brain! The background is dark, and the image intensity varies systematically
in different parts of the image

> ## Some IPython magic {.callout}
>
> If you're using an IPython / Jupyter notebook,
> you'll need to execute the following command
> in order for your matplotlib images to appear
> in the notebook when `show()` is called:
>
> ~~~ {.python}
> % matplotlib inline
> ~~~
>
> The `%` indicates an IPython magic function -
> a function that is only valid within the notebook environment.
> Note that you only have to execute this function once per notebook.

Let's take a look at the mean intensity in every column of the image:

~~~ {.python}
ave_inflammation = data.mean(axis=0)
ave_plot = matplotlib.pyplot.plot(ave_inflammation)
matplotlib.pyplot.show()
~~~

![Average Inflammation Over Time](fig/01-numpy_73_0.png)

Here, we have put the average per columns across all rows in the variable
`ave_inflammation`, then asked `matplotlib.pyplot` to create and display a line
graph of those values. The result is darkness around the edges and then
variations across the part that contains the brain.

Similarly, we can plot the maximum and minimum of the data:

~~~ {.python}
max_plot = matplotlib.pyplot.plot(data.max(axis=0))
matplotlib.pyplot.show()
~~~

![Maximum Value Along The First Axis](fig/01-numpy_75_1.png)

~~~ {.python}
min_plot = matplotlib.pyplot.plot(data.min(axis=0))
matplotlib.pyplot.show()
~~~

![Minimum Value Along The First Axis](fig/01-numpy_75_3.png)


You can group similar plots in a single figure using subplots. This script below
uses a number of new commands. The function `matplotlib.pyplot.figure()` creates
a space into which we will place all of our plots. The parameter `figsize` tells
Python how big to make this space. Each subplot is placed into the figure using
its `add_subplot` method. The `add_subplot` method takes 3 parameters. The first
denotes how many total rows of subplots there are, the second parameter refers
to the total number of subplot columns, and the final parameters denotes which
subplot your variable is referencing (left-to-right, top-to-bottom). Each
subplot is stored in a different variable (`axes1`, `axes2`, `axes3`). Once a
subplot is created, the axes can be titled using the `set_xlabel()` command (or
`set_ylabel()`). Here are our three plots side by side:

~~~ {.python}
import numpy
import matplotlib.pyplot

data = numpy.load('data/slice1.npy')

fig = matplotlib.pyplot.figure(figsize=(10.0, 3.0))

axes1 = fig.add_subplot(1, 3, 1)
axes2 = fig.add_subplot(1, 3, 2)
axes3 = fig.add_subplot(1, 3, 3)

axes1.set_ylabel('average')
axes1.plot(data.mean(axis=0))

axes2.set_ylabel('max')
axes2.plot(data.max(axis=0))

axes3.set_ylabel('min')
axes3.plot(data.min(axis=0))

fig.tight_layout()

matplotlib.pyplot.show()
~~~

![The Previous Plots as Subplots](fig/01-numpy_80_0.png)

The [call](reference.html#function-call) to `load` reads our data, and the
rest of the program tells the plotting library how large we want the figure to
be, that we're creating three subplots, what to draw for each one, and that we
want a tight layout. (Perversely, if we leave out that call to
`fig.tight_layout()`, the graphs will actually be squeezed together more
closely.)

> ## Scientists dislike typing {.callout}
>
> We will always use the syntax `import numpy` to import NumPy.
> However, in order to save typing, it is
> [often suggested](http://www.scipy.org/getting-started.html#an-example-script)
> to make a shortcut like so: `import numpy as np`.
> If you ever see Python code online using a NumPy function with `np`
> (for example, `np.loadtxt(...)`), it's because they've used this shortcut.

> ## Check your understanding {.challenge}
>
> Draw diagrams showing what variables refer to what values after each statement in the following program:
>
> ~~~ {.python}
> mass = 47.5
> age = 122
> mass = mass * 2.0
> age = age - 20
> ~~~

> ## Sorting out references {.challenge}
>
> What does the following program print out?
>
> ~~~ {.python}
> first, second = 'Grace', 'Hopper'
> third, fourth = second, first
> print(third, fourth)
> ~~~

> ## Slicing strings {.challenge}
>
> A section of an array is called a [slice](reference.html#slice).
> We can take slices of character strings as well:
>
> ~~~ {.python}
> element = 'oxygen'
> print('first three characters:', element[0:3])
> print('last three characters:', element[3:6])
> ~~~
>
> ~~~ {.output}
> first three characters: oxy
> last three characters: gen
> ~~~
>
> What is the value of `element[:4]`?
> What about `element[4:]`?
> Or `element[:]`?
>
> What is `element[-1]`?
> What is `element[-2]`?
> Given those answers,
> explain what `element[1:-1]` does.

> ## Thin slices {.challenge}
>
> The expression `element[3:3]` produces an [empty string](reference.html#empty-string),
> i.e., a string that contains no characters.
> If `data` holds our array of patient data,
> what does `data[3:3, 4:4]` produce?
> What about `data[3:3, :]`?

> ## Check your understanding: plot scaling {.challenge}
>
> Why do all of our plots stop just short of the upper end of our graph?

> ## Check your understanding: drawing straight lines {.challenge}
>
> Why are the vertical lines in our plot of the minimum inflammation per day not perfectly vertical?

> ## Make your own plot {.challenge}
>
> Create a plot showing the standard deviation (`numpy.std`) of the inflammation data for each day across all patients.

> ## Moving plots around {.challenge}
>
> Modify the program to display the three plots on top of one another instead of side by side.
