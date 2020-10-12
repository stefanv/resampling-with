# About the technology

This version of the book uses the *Python* [^python] programming language to
implement resampling algorithms.

Python is a programming language that can be used for many tasks.  It is
a very popular language for teaching, but is also widely used in industry and
academia.

For many of the initial examples, we will also be using the *Numpy* [^numpy]
package. A *package* is a library of Python code and data.  Numpy is a Python
package that makes it easier to work with sequences of data values, such as
sequences of numbers.  These are typical in statistics.

We will also be using the *Matplotlib* [^matplotlib] package. This is the main Python library for producing plots, such as bar charts, histograms, and scatterplots.  See the rest of the book for more details on these plots.

Later on, we will use more specialized libraries for data manipulation and
analysis.  *Pandas* [^pandas] is the standard Python package for loading data
files and working with data tables.  *Scipy* [^scipy] is a package that houses
a wide range of numerical routines, including some simple statistical methods.
The *Statsmodels* [^statsmodels] package has code for many more statistical
procedures. We will often find ourselves comparing the results of our own
resampling algorithms to those in Scipy and Statsmodels.

## The environment {-}

For each chapter that has code, there is an matching *Jupyter Notebook*
[^jupyter-nb].  In the web edition of this book, you can click on the
*Download* link at the top of the page to download the chapter as a notebook.
You can click on the *Interact* link at the top of the page to open the
notebook on a cloud computer.  This allows you to run the code on the cloud
computer, and experiment by making changes.

In the print version of the book, we point you to the web version, to get the links.

## Running the code on your own computer {-}

It is reasonably easy to set up your own computer so you can run the code in this book.

You will need to install the Python language on your computer, and then install the following packages:

* Numpy
* Matplotlib
* Scipy
* Pandas
* Statsmodels
* Jupyter

One easy way to do that on Windows, Mac or Linux, is to use the *Anaconda
Python distribution* [^anaconda_distro].  Anaconda provides a single installers that installs Python and all these packages for you.

Another method is to install Python from the Python website [^python].  Then use the *Pip* [^pip] installer to install the packages you need.

To use Pip, start a terminal (Start key, "cmd" in Windows, Command key and space then "Terminal" on Mac), and then, at the prompt, type:


```bash
pip install numpy matplotlib scipy pandas statsmodels jupyter
```

Now you should be able to start the Jupyter notebook application.  Open the
notebook you downloaded for the chapter; you now be able to run the code on
your own computer, and experiment by making changes.

<!---
Links
-->

[^numpy]: <https://numpy.org>
[^matplotlib]: <https://matplotlib.org>
[^scipy]: <https://scipy.org>
[^pandas]: <https://pandas.pydata.org>
[^statsmodels]: <https://www.statsmodels.org>
[^jupyter-nb]: <https://jupyter.org>
[^anaconda_distro]: <https://www.anaconda.com/distribution>
[^pip]: <https://pip.pypa.io>
