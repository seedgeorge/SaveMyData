---
title: "Beginner Boxplots in R"
output:
  html_document:
    keep_md: true
    df_print: paged
---

### How to: make the boxiest of plots.

A common task in data science is to explore a continuous variable (for example, measurements or test results), in the context of a grouping variable (or variables). In this entry we'll look at some simple ways to **structure** the data, **visualise** it, and apply a **statistical test** to help us draw conclusions from it. 

This article is for beginners with R, and it doesn't matter if you're a student or a professional, although we aren't covering the real basics here. Check our other articles if you're interested in a similar guide for Python, and for more articles about R!

# 1. Structure of the Data


```r
# we need some data! 
# luckily, R ships with some built in data
# alternatively, you could read in some data - see our other articles with tips for this!

data(sleep) # use the data() function to access a built-in dataset
str(sleep) # use the str() function to look at the structure of this data
```

```
## 'data.frame':	20 obs. of  3 variables:
##  $ extra: num  0.7 -1.6 -0.2 -1.2 -0.1 3.4 3.7 0.8 0 2 ...
##  $ group: Factor w/ 2 levels "1","2": 1 1 1 1 1 1 1 1 1 1 ...
##  $ ID   : Factor w/ 10 levels "1","2","3","4",..: 1 2 3 4 5 6 7 8 9 10 ...
```

This data object is a `data.frame` - a flexible table-like format, similar to a spreadsheet in Excel. The sleep data represents 20 results - comparing two treatments and observing the difference in sleep time of each individual (compared to control). There are two key variables in the data: `extra` and `group`. These are the *continuous* and *categorical* variables respectively. There is also another variable `ID`, indicating which individual gave which result.  


```r
# we can access specific columns by using the dollar sign (among other ways)

sleep$extra # this is a numeric vector
```

```
##  [1]  0.7 -1.6 -0.2 -1.2 -0.1  3.4  3.7  0.8  0.0  2.0  1.9  0.8  1.1  0.1 -0.1
## [16]  4.4  5.5  1.6  4.6  3.4
```

```r
sleep$group # this is a factor vector of group labels
```

```
##  [1] 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2
## Levels: 1 2
```

```r
sleep$ID # another factor vector of individual labels
```

```
##  [1] 1  2  3  4  5  6  7  8  9  10 1  2  3  4  5  6  7  8  9  10
## Levels: 1 2 3 4 5 6 7 8 9 10
```

Remember, if you can structure your own data like the `sleep` data, you can do the following analyses. 

# 2. Plot the Data

A straightforward way to look at this data is a 'box plot' - sometimes referred to as a 'box and whisker plot'. R has a built-in function we can use to plot it, which we can customise to our liking. Check our other articles on how to make more complicated (and perhaps more beautiful) versions of this plot.


```r
# we'll use the boxplot() function
# it features the interesting use of the squiggly line character '~' 

# don't be afraid!

boxplot(extra~group, data = sleep) # we tell the function what columns to use, and where to find them
```

<img src="Article1_handyboxplots_files/figure-html/Starter Plot-1.png" style="display: block; margin: auto;" />

This usage (`extra~group`) is called 'formula interface', and is used in some functions to indicate doing something by groups. You might see it again in our articles that include regressions.

A boxplot has several elements, which the function `boxplot` has computed on our behalf, for each group we specified. The line in the middle indicates the **median** value of the data, the grey shaded box indicates the **1st and 3rd quartiles**, and the dotted lines indicate the **minimum and maximum** values... after removal of '**suspected outliers**'. The threshold for a suspected outlier is: 

- values greater than 1.5*IQR + 3rd quartile 
- values less than 1.5*IQR - 1st quartile

The thing to remember here is that: *boxplots help you visualise summaries of the data.* We are plotting descriptive, statistical measurements. 

We can also plot the actual raw data values, using `stripchart`. 

```r
# we'll use the stripchart() function 
# to make it readable we'll put it on multiple lines - a new line after each comma

stripchart(extra~group, 
           data = sleep, # similar to boxplot
           vertical = TRUE, # the default is horizontal, but we'll use vertical
           method = "jitter") # this 'jitters' the points a little to improve readability
```

<img src="Article1_handyboxplots_files/figure-html/Actual Data Plot-1.png" style="display: block; margin: auto;" />

And we can plot them both simultaneously, as the `stripchart` function allows us to add it over the top of an existing plot. 


```r
boxplot(extra~group, data = sleep) # first use boxplot

stripchart(extra~group, # and then stripchart
           data = sleep,
           vertical = TRUE,
           method = "jitter",
           add=TRUE) # add the stripchart to the existing plot
```

<img src="Article1_handyboxplots_files/figure-html/Combined Plot-1.png" style="display: block; margin: auto;" />

# 3. Tweak the Plot

For simplicity, the above plots have been kept quite plain, but for presentation or publication use you may want to customise various parameters. There are lots of things we can improve with these plots - as in the following example. We aren't changing the data or the statistics involved, just tweaking the visuals.


```r
# this code is similar to above, but with some optional parameters used inside the function calls
oldpar = par(no.readonly = TRUE) # make a copy of the default graphical parameters
par(bg = "ivory") # this lets us specify an overall background colour

boxplot(extra~group, # same boxplot as above... but with some additions
        data = sleep,
        xlab = "Treatment Group", # add a custom x-axis label
        ylab = "Difference in Sleep", # and a custom y-axis label
        main = "Boxplot of Sleep Data",# and a main title
        lwd = 2, # thickness of box lines
        border= "slategrey", # colour of the box borders
        col = "slategray2", # colour of the inside of the boxes
        col.axis = 'grey20', # colour of the axis numbers 
        col.lab = 'grey20', # colour of the axis labels
        frame = F, # remove the outer box
        boxwex = 0.5) # overall width of the boxes

stripchart(extra~group, 
           data = sleep,
           vertical = TRUE,
           method = "jitter", 
           add = TRUE, 
           pch = 16, # specify the type of point to use
           cex = 1, # how big the points should be
           col = "darkred") # and the colour to plot them as  
```

<img src="Article1_handyboxplots_files/figure-html/Customised Plot-1.png" style="display: block; margin: auto;" />

```r
par(oldpar)
```
Here we've demonstrated a couple of concepts. Firstly, the `par` function lets us alter **graphics parameters** like plot background colour - although it works globally, so it's good practice to make a copy first and then revert back to it at the end of the code. **RStudio** has a 'Clear All Plots' button that also does this. 

The rest of the code here are simply style choices - choosing the colours for various elements, the size of points, the width of lines, and so on. 

# 4. Do a Statistical Test

To continue exploring these data, we can perform an appropriate statistical test. Here, we will ask if there is a significant difference between these two groups of values: ie. whether the results we observed were due to chance or due to the effect of the treatment. The **null hypothesis** is that both sets of results have (effectively) equal means.

We aren't diving deep into this topic here, just showing how to apply such a test to this data.

In R, we can simply use the appropriately named `t.test` function.


```r
t.test(extra~group, 
       data = sleep, # notice how similar this is to the boxplot/stripchart functions!
       paired = TRUE) # our data is paired, and the observations are ordered by the individual ID, so we should tell the function that
```

```
## 
## 	Paired t-test
## 
## data:  extra by group
## t = -4.0621, df = 9, p-value = 0.002833
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -2.4598858 -0.7001142
## sample estimates:
## mean of the differences 
##                   -1.58
```
By default, the `t.test` function outputs a chunk of text including some values of interest. As the p-value is quite small (below the commonly used threshold 0.05), we can say that the likelihood of these results happening by chance is very low, and that there is a **significant difference** between the two groups.

We can also save this test result to a variable, and do something else with it downstream - like use it in a label for a plot!


```r
t_result = t.test(extra~group, 
       data = sleep, 
       paired = TRUE) # save the result of the test to a new variable
p_value = signif(t_result$p.value,digits = 3) # round to 3 significant figures
main_title = paste("Boxplot of Sleep Data","\n","p =",p_value) # make a new title for the plot

par(bg = "ivory") # this lets us specify an overall background colour

boxplot(extra~group, 
        data = sleep,
        xlab = "Treatment Group", # add a custom x-axis label
        ylab = "Difference in Sleep", # and a custom y-axis label
        main = main_title, # use our custom title
        lwd = 2, # thickness of box lines
        border= "slategrey", # colour of the box borders
        col = "slategray2", # colour of the inside of the boxes
        col.axis = 'grey20', # colour of the axis numbers 
        col.lab = 'grey20', # colour of the axis labels
        frame = F, # remove the outer box
        boxwex = 0.5) # overall width of the boxes

stripchart(extra~group, 
           data = sleep,
           vertical = TRUE,
           method = "jitter", 
           add = TRUE, 
           pch = 16, # specify the type of point to use
           cex = 1, # how big the points should be
           col = "darkred") # and the colour to plot them as  
```

<img src="Article1_handyboxplots_files/figure-html/Save Test-1.png" style="display: block; margin: auto;" />

```r
par(oldpar)
```
Looks good.

# 5. Final Thoughts

This entry has covered a couple of topics, and the key points to remember are as follows:

- If your data is a mix of **categorical** and **continuous** data, you may be interested in using box plots to explore it.
- You can visualise both **statistical measurements** and the **raw data** on the same plot. 
- There are **lots** of ways to customise plots in R.
- A **statistical test** can helps us draw conclusions from the data, and we can combine the result with the plot labels. 
