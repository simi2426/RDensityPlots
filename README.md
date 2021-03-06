# RDensityPlots
Density plots in R! This shows examples for both base R and the ggplot2 package :)

# Density Plots with ggplot2
For this example, I am going to use the Titanic dataset from Kaggle, which can be found here: https://www.dropbox.com/s/00j17o6xzxsdjo4/titanictest.csv?dl=0

```r
# load data and libraries
train = read.csv("~/downloads/titanictest.csv")
library(ggplot2)
```

Now, let's use `ggplot2` to create a basic density plot. Let's try to find the density of ages of passengers on the Titanic:

```r
plot = ggplot(train, aes(x = Age)) +
  geom_density()
plot
```

`ggplot()` in this function, we input everything about the data we want in our graph, such as the dataset and variables. We add another function, the `geom_density()` to tell the computer what type of graph to make.

`plot` is the name of the graph we have created. By assigning it to a variable, we can add on other things later. 

`aes(x = Age)` This is how we set the variable we want to measure, which in this case is Age. R is case sensitive, so make sure to capitalize if the variable name is!

`geom_density()` We use the + to "add" this on to tell the computer what type of graph we want to create. 

Now, let's add some color. To do this, we go in the empty `geom_density` argument:

```r
plot = ggplot(train, aes(x = Age)) +
  geom_density(color = "purple", fill = "blue")
plot
```

## Creating Multiple Density Plots in One Graph
R is all about data visualization, and comparing two separate graphs can be difficult. For this example, we are going to compare the mean fare paid by males and females on the ship by creating two density plots on a singular graph.

```r
x = ggplot(train, aes(x = Fare, color = Sex)) +
  geom_density()
x
```

`x` is the name of our graph. We assign it a name to make it easier to add other things on later.

`color = Sex` tells the computer to differentiate the graphs by sex.

The `x` assignment at the end simply plots the graph.

# Graphs with Five Number Summary

What if we want to create a density graph that includes a five number summary and a graph? We can do that in R! For this example, we are going to create a density plot of age and gender on the Titanic. First, we need to install some packages (only install if you haven't already!):

```r
# install packages
install.packages("ggpubr")

# read libraries into console
library(ggpubr)
library(ggplot2)

# load data
train = read.csv("~/downloads/traintitanic.csv", stringsAsFactors = FALSE, header = TRUE)
```

## Create a Five Number Summary
First, we can use the `desc_statby` function to create the five number summary of the variables we are interested in, age and gender.

```r
fivenumber = desc_statby(train,
                         measure.var = "Age",
                         grps = "Sex")

# run to see data
fivenumber
     Sex length  min max median     mean iqr     mad
1 female    261 0.75  63     27 27.91571  19 13.3434
2   male    453 0.42  80     29 30.72664  18 11.8608
        sd        se       ci range        cv
1 14.11015 0.8733961 1.719831 62.25 0.5054554
2 14.67820 0.6896420 1.355303 79.58 0.4777027
       var
1 199.0962
2 215.4496
```

`measure.var = "Age"` tells the computer that the variable we want to measure is Age

`grps - "Sex"` tells the computer that we are grouping this data by gender.

Now, we want to create a table of this data that can be put in the final graph:

```r
fivenumber.p = ggtexttable(fivenumber, rows = NULL)
fivenumber.p
```

## Create Plot
Finally, let's create the density plot using the `ggdensity` function:

```r
density.p = ggdensity(train, x = "Age",
                      fill = "Sex", palette = "jco")
density.p
```

`palette` is simply the color palette are using.

## Arrange Graph and Five Number Summary

To do this, we use the `ggarrange` function:

```r
ggarrange(density.p, fivenumber.p, 
          ncol = 1, nrow = 3, 
          heights = c(1, .5, .3))
```

Now, we have a graph with both a density plot and five number summary.




# Creating Density Plots in Base R
Let's use base R to create a simple density plot of ages on the Titanic.

```r
# load data
train <- read.csv("downloads/titanictrain.csv")
```

We can use `density()` to see a five number summary of the ages of the passengers on the Titanic:

```r
> density(train$Age)
Error in density.default(train$Age) : 'x' contains missing values
```

This returns an error message because there are missing values of data in the set. To resolve this, we can use the `na.omit()` function, which deletes the missing values. We have to rename the dataset if we use this function, because technically it is a new set without the missing values.

```r
> trainnew <- na.omit(train)
> density(trainnew$Age)

Call:
	density.default(x = trainnew$Age)

Data: trainnew$Age (714 obs.);	Bandwidth 'bw' = 3.226

       x                y            
 Min.   :-9.258   Min.   :1.959e-06  
 1st Qu.:15.476   1st Qu.:1.270e-03  
 Median :40.210   Median :6.791e-03  
 Mean   :40.210   Mean   :1.010e-02  
 3rd Qu.:64.944   3rd Qu.:1.608e-02  
 Max.   :89.678   Max.   :3.129e-02  
 ```
 
 Whenever we use base R, we use the `$` after the dataset to specify what variable we are specifying. Now, let's actually plot the data by simply adding `plot()` in front of our previous argument:
 
 ```r
 plot(density(trainnew$Age))
```

## Multiple Curves on one Graph
There may be instances where you want to show multiple density curves on a singular graph. Here, we are going to use the mycollegedata.csv dataset to compare the means of parent and child income. The dataset can be found at this Dropbox link: https://www.dropbox.com/home?preview=collegedata.csv First, let's assign variable names to the density we want to measure.

```r
# import dataset
college = read.csv("~/downloads/mycollegedata.csv")

# assign parent and kid density variables
par <- density(college$pmean)
kid <- density(college$kmean)
```

Now, let's plot the first density graph:

```r
plot(par, lwd = 2, col = "green")

# plot second with lines() function
lines(kid, col = "purple", lwd = 2)
```


## Adding a Histogram and Density Plot

(this graph was made using the `trainnew` omitted dataset)

In base R, we can simultaneously graph a histogram and density line using the `line()` function. First, we need to make a histogram:

```r
hist(trainnew$Age, freq = FALSE, main = "Histogram with Density")

# establish dx as our density line of Age
dx <- density(trainnew$Age)

# create the graph
lines(dx, lwd = 2, col = "red")
```

`dx` - the variable we assigned the density line to

`lwd` - the width of the density line

### Citations

I gained a lot of info used here from the website at this link:

https://r-coder.com/density-plot-r/#Kernel_density_bandwidth_selection











