# Cannot find jvm.dll #
Essentially, you don't have yet a working rJava package.  Try to add
your jvm.dll to your path.  For example, on windows you might have
something like "C:/Program Files/Java/jdk1.7.0\_11/jre/bin/server".

# Cannot install package xlsx on Mac OS X Lion #

Please install the package directly from sources.  Download the tar.gz
file from cran in your working directory, and then
```
install.packages("xlsx_0.X.X.tar.gz", type="source")
```

# Package doesn't install properly from within R-Studio #

Please install the package directly from R.
```
install.packages(c("xlsxjars", "xlsx"))
```


# I get an error when I try to read an xlsx file #

If you just installed the package, make sure you have the dependent
package **rJava** installed properly.  To check that, do

```
require(rJava)
.jcall('java.lang.System','S','getProperty','java.version')
```

If you don't get back a string with your java version, it means your
**rJava** package is not installed correctly.  You will need to fix this
before you can use the **xlsx** package.

On Windows, you may need to add to your PATH variable the
location of your JVM (e.g.`C:/Program Files/Java/jre6/bin/client`) to
get package **rJava** working.


# I'm running out of java heap memory when using write.xlsx #

This is probably the complaint I get most from users.  It happens when
you try to write a large `data.frame` to file.  What large means
depends on your machine.  Unfortunately, I don't understand why so
much memory is used when writing.  If you run into problems, you can
try to increase the amount of java heap space that `R` allocates
before you load the package.

```
options(java.parameters="-Xmx1024m")
require(xlsx)
```

The default is `Xmx512m`, 512 MB.  If your computer has more RAM,
increase it more and it may solve your problem.

# My write.xslx takes a long time #

For every element of the `data.frame` you intend to write, a
Java cell object will get created, and then serialized back into the
XML file.  There is a lot of marshaling between R and Java and that
slows things down.

The function `write.xlsx2` is a version that uses Java internally for
looping over the rows and the result is a significant improvement in
performance.  However there are some limitations, so do read the
documentation.

Also, if the data is really large and intended primarily for storage,
you should seriously evaluate if you need it in an `xlsx` format.
Perhaps a compressed csv file will do just as well and be faster to
read/write from `R`.

# I want to read an xls file and output an xlsx file #

Sorry, you can't do that in an easy way. You need to construct an
instance of HSSF and an instance of XSSF and then read from one and
copy to the other, cell by cell, preserving the formats, etc.
Can be done, but gruesome work.

# Formulas don't recalculate automatically after save #

You modify an input cell in your spreadsheet with R, and you save your
workbook.  The cells that depend on the input cell don't get
recalculated once you open the workbook again.  To force this
recalculation, do `wb$setForceFormulaRecalculation(TRUE)` before saving
the workbook.


# Formulas don't get read properly by read.xlsx2 #

I have a spreadsheet with formulas.  Function read.xlsx reads the
values of the formulas correctly, but read.xlsx2 or readColumns return
NA.  Solution: specify the colClasses argument, because the code
cannot guess the type of the column when the cell type is a formula.
See [Issue 35](https://code.google.com/p/rexcel/issues/detail?id=35).

