---
title: "How to Make a Scatter Plot"
output:
  html_document:
    keep_md: true
    df_print: paged
---

## Comparing Continuous Data

### Numbers are Fun
Continuous data are numbers - typically, from taking measurements, and you can find such data in many fields. In this entry we'll look at how you can **structure** a collection of such data, **visualise** it, and apply a **statistical test** to compare such data. 

This article is for beginners with R, and it doesn’t matter if you’re a student or a professional. To follow along, you'll need an installation of R (although we generally advise using RStudio as well!), and you won't need do download any external data or packages. Hopefully, you can follow along and simply run the code chunks below for yourself - and go on to perform similar analyses on your own data. 

Check our other articles for more R!

# 0. Load Some Built-In Data

* R ships with several built in data sets which we can lean on, and lots of them are great for comparing continuous data.
* We'll be using one called `airquality` - measurements from New York in 1973.
* More details can be found here: https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/airquality.html
* The `data()` function is used to load a built-in dataset.


```r
data(airquality)
```

Easy! There are lots of other datasets available with R, and they're great for practicing various data science tasks. 

# 1. Examine the Data

### The Airquality Dataset

We can take a quick look at this data by applying a couple of handy functions. 


```r
head(airquality) # look at the top 5 rows of the data
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Ozone"],"name":[1],"type":["int"],"align":["right"]},{"label":["Solar.R"],"name":[2],"type":["int"],"align":["right"]},{"label":["Wind"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Temp"],"name":[4],"type":["int"],"align":["right"]},{"label":["Month"],"name":[5],"type":["int"],"align":["right"]},{"label":["Day"],"name":[6],"type":["int"],"align":["right"]}],"data":[{"1":"41","2":"190","3":"7.4","4":"67","5":"5","6":"1","_rn_":"1"},{"1":"36","2":"118","3":"8.0","4":"72","5":"5","6":"2","_rn_":"2"},{"1":"12","2":"149","3":"12.6","4":"74","5":"5","6":"3","_rn_":"3"},{"1":"18","2":"313","3":"11.5","4":"62","5":"5","6":"4","_rn_":"4"},{"1":"NA","2":"NA","3":"14.3","4":"56","5":"5","6":"5","_rn_":"5"},{"1":"28","2":"NA","3":"14.9","4":"66","5":"5","6":"6","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
str(airquality) # how is this data structured
```

```
## 'data.frame':	153 obs. of  6 variables:
##  $ Ozone  : int  41 36 12 18 NA 28 23 19 8 NA ...
##  $ Solar.R: int  190 118 149 313 NA NA 299 99 19 194 ...
##  $ Wind   : num  7.4 8 12.6 11.5 14.3 14.9 8.6 13.8 20.1 8.6 ...
##  $ Temp   : int  67 72 74 62 56 66 65 59 61 69 ...
##  $ Month  : int  5 5 5 5 5 5 5 5 5 5 ...
##  $ Day    : int  1 2 3 4 5 6 7 8 9 10 ...
```

We can see that this is a `data.frame` object, with a number of data columns.
The measurements we are interested in are the first four columns, which describe: 

* mean ozone readings 
* solar radiation
* average wind speed
* maximum daily temperature
The final two columns detail when the measurements were collected.

In case you're interested in individual stats for each variable, the `summary()` function enables us to easily explore some summary statistics for these data.

```r
summary(airquality)
```

```
##      Ozone           Solar.R           Wind             Temp      
##  Min.   :  1.00   Min.   :  7.0   Min.   : 1.700   Min.   :56.00  
##  1st Qu.: 18.00   1st Qu.:115.8   1st Qu.: 7.400   1st Qu.:72.00  
##  Median : 31.50   Median :205.0   Median : 9.700   Median :79.00  
##  Mean   : 42.13   Mean   :185.9   Mean   : 9.958   Mean   :77.88  
##  3rd Qu.: 63.25   3rd Qu.:258.8   3rd Qu.:11.500   3rd Qu.:85.00  
##  Max.   :168.00   Max.   :334.0   Max.   :20.700   Max.   :97.00  
##  NA's   :37       NA's   :7                                       
##      Month            Day      
##  Min.   :5.000   Min.   : 1.0  
##  1st Qu.:6.000   1st Qu.: 8.0  
##  Median :7.000   Median :16.0  
##  Mean   :6.993   Mean   :15.8  
##  3rd Qu.:8.000   3rd Qu.:23.0  
##  Max.   :9.000   Max.   :31.0  
## 
```

Of note here is that we can see how the `Ozone` and `Solar.R` variables in the data have some missing values, which we will need to pay attention to later on.

# 2. Make a Basic Plot

It's often very helpful to make a quick graph to get a feel for your data. Here we can use the `plot()` function to quickly compare two variables.

```r
plot(x = airquality$Wind, # the data for the x-axis
     y = airquality$Temp, # the data for the y-axis
     main = "A Quick Comparison") # the title of the plot
```

<img src="Article4_scatter_plot_files/figure-html/unnamed-chunk-1-1.png" style="display: block; margin: auto;" />
From a quick eyeball, it looks like as wind speed increases, temperature decreases. What about the other variables in this dataset?


```r
plot(airquality[,1:4]) # we can give multiple columns (1 to 4) 
```

<img src="Article4_scatter_plot_files/figure-html/unnamed-chunk-2-1.png" style="display: block; margin: auto;" />

```r
# it will try to plot them all as well 
```

This should be fairly intuitive. 

# 3. Do a Quick Statistical Test

These variables can be easily compared to each other with a correlation test, using the `cor()` function.

```r
cor(x = airquality$Wind, y = airquality$Temp)
```

```
## [1] -0.4579879
```
By default, this function calculates the 'pearson' *r* value, which measures a linear association between two variables, and ranges between -1 and 1.

* A 1 would be a perfect 'positive' correlation, indicating that as the x-axis variable increases, so does the y-axis variable
* Values closer to 0 indicate a poor or non-existant correlation.
* A value of -1 indicates a perfect 'negative' correlation, with the x-axis variable increasing and the y-axis variable decreasing. 

Values in between, like the `-0.4579879` above, suggest some degree of dispersion from a perfect line. We can conclude here that in the dataset we're assessing, as wind speed increases, recorded temperature decreases - although the correlation is not perfect. 



<!-- data(airquality) -->

<!-- plotme = airquality[,c("Temp","Ozone")] -->
<!-- plotme = plotme[complete.cases(plotme),] -->
<!-- plotme = plotme[order(plotme$Temp),] -->
<!-- plot(x = plotme$Temp,y = plotme$Ozone,pch=20,col="slategrey") -->
<!-- #abline(lm(Ozone ~ Temp,data=plotme)) -->
<!-- l1 = loess(Ozone ~ Temp,data = plotme,span = 1) -->
<!-- l2 = loess(Ozone ~ Temp,data = plotme,span = 0.5) -->
<!-- l3 = loess(Ozone ~ Temp,data = plotme,span = 0.25) -->

<!-- p1 = predict(l1) -->
<!-- p2 = predict(l2) -->
<!-- p3 = predict(l3) -->

<!-- lines(p1,x=plotme$Temp,col="red",lwd=2) -->
<!-- #lines(p2,x=plotme$Temp,col="red",lwd=2) -->
<!-- #lines(p3,x=plotme$Temp,col="blue",lwd=2) -->



<!-- p1e = predict(l1,se=T) -->
<!-- lines(plotme$Temp,y=p1e$fit-1.96*p1e$se.fit,lty=2,col="red") -->
<!-- lines(plotme$Temp,y=p1e$fit+1.96*p1e$se.fit,lty=2,col= -->


<!-- # 2. Calculate Survival -->

<!-- We can use a couple of handy functions from `survival` to take a look at the data and make some summaries. An important function to use will be `Surv()`. -->

<!-- ```{r} -->
<!-- lungsurvival = Surv(lung$time,lung$status) # make a survival time object -->
<!-- head(lungsurvival) # take a look at the first 5 elements -->
<!-- ``` -->

<!-- The object we made here is essentially a vector of survival times, with `+` symbols appended to show when censoring has taken place. We will use this going forward.  -->

<!-- ### Fitting the Data -->

<!-- The `survfit()` function is a key element of the survival package, and it fits survival curves to our data. We can have a quick go as follows. -->
<!-- ```{r} -->
<!-- survfit(formula = lungsurvival ~ 1) # make the simplest survival curve object -->
<!-- ``` -->
<!-- This is another formula interface function - see our other articles. We're telling `survfit()` to fit a survival curve (more on these later) to the data (the `lungsurvival` object we made), and we can tell it to use the whole dataset by simply including a `1` in the formula. This call outputs: -->

<!-- * The number of entries and the number of events in the dataset. -->
<!-- * Summary statistics - the **median survival**, along with the **95% confidence interval**. -->

<!-- For the record, we can also do this: `survfit(formula = Surv(time,status) ~ 1,data=lung)` - note the modified syntax with the data parameter. -->

<!-- We can look more closely at how the survival proportion over time by using the `summary()` function. For convenience we can save the fit to a variable called `lungfit`. -->

<!-- ```{r} -->
<!-- lungfit = survfit(formula = lungsurvival ~ 1) # save the fit  -->
<!-- summary(lungfit, times = c(0,100,200,500,1000)) # we can specify whatever times we like -->
<!-- ``` -->
<!-- Nice. This sort of information is frequently used in **risk tables**. What we are calculating using these functions is called the *'Kaplan-Meier estimator'*, a non-parametric way to estimate survival probabilities over time.  -->

<!-- We can also convert these data into a simple plot: the `survival` package contains instructions to tell the basic R `plot()` function how to plot it. -->

<!-- ```{r fig.height=4, fig.width=5,fig.align='center'} -->
<!-- plot(survfit(formula = lungsurvival ~ 1, data=lung),  -->
<!--      ylab = "Survival Proportion",  -->
<!--      xlab = "Time", -->
<!--      main = "Overall Lung Dataset Survival") -->
<!-- ``` -->

<!-- Ok, looking plausible. This is often called a **Kaplan-Meier Curve** (or Kaplan-Meier Plot).  -->

<!-- ### Stratify by a Variable -->

<!-- There are multiple variables in the `lung` data set. We can explore them with a modified call to `survfit`, substituting the `1` with the name of the variable, and including a *data* argument to tell the function where to find it.  -->

<!-- ```{r} -->
<!-- survfit(formula = lungsurvival ~ sex, data=lung) # performance status of individuals -->
<!-- ``` -->

<!-- This shows a breakdown of survival statistics (ie. median survival and confidence intervals) for **each level of the variable** in the data. Do these values look different? Can we observe a trend? -->

<!-- ```{r fig.height=4, fig.width=5,fig.align='center'} -->
<!-- plot(survfit(formula = lungsurvival ~ sex, data=lung),  -->
<!--      ylab = "Survival Proportion",  -->
<!--      xlab = "Time",  -->
<!--      main = "Lung Dataset Split by Variable") -->
<!-- ``` -->

<!-- This approach works best for *categorical* variables (the `ph.ecog` variable would work well too); if we tried it with a *continuous* one it looks rather odd (`meal.cal`, for example). Try it out? -->

<!-- ### Stratify by Multiple Variables -->

<!-- The `survfit()` function is clever - we can give it multiple variables at the same time and it calculates survival proportions for each combination of the variables.  -->

<!-- ```{r} -->
<!-- survfit(formula = lungsurvival ~ sex + ph.ecog, data=lung) # performance status of individuals -->
<!-- ``` -->

<!-- We can see that the different levels of these variables are shown in the output - and that the usage of `summary()` (like above) will show a detailed output. -->

<!-- ```{r fig.height=4, fig.width=5,fig.align='center'} -->
<!-- plot(survfit(formula = lungsurvival ~ sex + ph.ecog, data=lung),  -->
<!--      ylab = "Survival Proportion",  -->
<!--      xlab = "Time",  -->
<!--      main = "Lung Dataset Split by Two Variables") -->
<!-- ``` -->
<!-- These plots are a bit bland. We can improve them a little. -->

<!-- # 3. Tweaking the Plots -->

<!-- We can add colours and a legend to the lines in these plots. Colours are assigned by the levels of the variable, as can be seen in the `survfit()` call. Here we'll look at the **ECOG performance status** variable - which has four levels.  -->

<!-- ```{r} -->
<!-- survfit(formula = lungsurvival ~ ph.ecog, data=lung) -->
<!-- lungfit = survfit(formula = lungsurvival ~ ph.ecog, data=lung) # make an object for the fit -->
<!-- ``` -->

<!-- We can use this to plot with. -->

<!-- ```{r fig.height=4, fig.width=5,fig.align='center'} -->
<!-- oldpar = par(no.readonly = TRUE) # make a copy of the default graphical parameters -->
<!-- par(bg = "ivory") # this lets us specify an overall background colour -->


<!-- plot(lungfit, # plot this using 'plot'  -->
<!--      ylab = "Survival Proportion",  -->
<!--      xlab = "Time",  -->
<!--      main = "Lung Dataset With Colours", -->
<!--      mark.time = T, # include little tick marks to indicate when censoring happens -->
<!--      lwd = 2, # increase the line thickness -->
<!--      col = c("darkorange","darkorange2","darkorange3","darkorange4"), # add colours in the order we want -->
<!--      bty = "n", # remove the border -->
<!--      las = 1) # adjust the angle of the axis numbers -->

<!-- legend("topright", # you can feed in x-y coordinates, or just a keyword -->
<!--        legend = names(lungfit$strata), # give it the unique levels -->
<!--        col=c("darkorange","darkorange2","darkorange3","darkorange4"), # colours in the same order -->
<!--        bg = NA, # remove the background colour -->
<!--        lty=1, # add a line to the legend -->
<!--        lwd=2, # thicker line -->
<!--        box.lty=0) # remove legend border -->

<!-- par(oldpar) -->
<!-- ``` -->
<!-- Changing various elements is pretty straightforward - and in this case we can see that there's a reduction in survival as the value for `ph.ecog` increases. As this value relates to an overall measurement of how healthy a patient is, this makes sense.  -->

<!-- # 4. Do a Statistical Test -->

<!-- To continue exploring these data, we can perform an appropriate statistical test to compare the survival distributions of these curves, with a **null hypothesis** that the the groups have no difference. We aren't diving deep into this topic here, and there are many other resources to learn more about this.  -->

<!-- The `survdiff()` function is straightforward to use, with the same syntax as the `survfit()` function we used earlier. This performs a *log-rank test*. -->

<!-- ```{r} -->
<!-- survdiff(formula = lungsurvival ~ sex, data=lung) # survdiff! -->
<!-- ``` -->
<!-- We can also apply this test to a variable that has more than two groups - like `ph.ecog` (which has four levels), and it generates a p-value, but it doesn't explicitly tell use which groups are different to each other, just that *overall* there is a difference. It's a starting point, but for complicated survival data with multiple variables we may need more than a simple log-rank test... -->

<!-- ```{r} -->
<!-- survdiff(formula = lungsurvival ~ ph.ecog, data=lung) # different variable to above -->
<!-- ecog_p = survdiff(formula = lungsurvival ~ ph.ecog, data=lung) -->
<!-- ``` -->

<!-- # 5. Final Thoughts -->

<!-- This article is really just a start to performing survival analysis, and there's certainly more to cover. -->
<!-- The key elements to remember are as follows: -->

<!-- - To estimate **survival probabilities** over time, you need *time* and *censoring status* data. -->
<!-- - The `survival` package is the core library for this type of analysis in R. -->
<!-- - It allows you to collate **summary statistics** about your overall population, and about subgroups.  -->
<!-- - You can plot **survival curves** (Kaplan-Meier plots) to visualise these. -->
<!-- - A **statistical test** like the log-rank test can help us draw conclusions from the data. -->