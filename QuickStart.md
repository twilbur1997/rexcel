# High-level API #

To read a file use
```
res <- read.xlsx("C:/Temp/infile.xlsx", sheetName="Sheet1")
```

To write a `data.frame` `x` to a file
```
write.xlsx(x, "C:/Temp/outfile.xlsx")
```

If reading/writing large spreadsheets (more than 100,000 cells) takes
too long, take a look at functions `read.xlsx2` and `write.xlsx2`.
They provide a different implementation which is an order of magnitude
faster compared to `read.xlsx` and `write.xlsx`.  In a future version of
the package I intend to keep only the faster version.


## Control output format ##

Since version `0.4.0` the function `addDataFrame` has been added.  It
is intended primarily for users who want to customize their
spredsheet.

```
wb    <- createWorkbook()
sheet <- createSheet(wb, sheetName="Sheet1")


```
