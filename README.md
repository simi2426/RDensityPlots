# RDensityPlots
density plots in r!

## Basic Density Plot
For this example, I am going to use the Titanic dataset from Kaggle, which can be found here: https://www.dropbox.com/s/00j17o6xzxsdjo4/titanictest.csv?dl=0

```r
# load data and libraries
train = read.csv("~/downloads/titanictrain.csv")
library(ggplot2)
```

Now, let's use `ggplot2` to create a basic density plot. Let's try to find the density of ages of passengers on the Titanic:

```r
plot = ggplot(train, aes(x = Age)) +
  geom_density()
```

`plot` is the name of the graph we have created. By assigning it to a variable, we can add on other things later. 

`aes(x = Age)` This is how we set the variable we want to measure, which is the age.

`geom_density()` We "add" this on to tell the computer what type of graph we want to create.

Now, let's add some color. To do this, we go in the empty `geom_density` argument:

```r
plot = ggplot(train, aes(x = Age)) +
  geom_density(color = "purple", fill = "blue")
plot
```

# Creating Multiple Density Plots in One Graph
Let's say we want to create two density plots, but on the same graph. For this example, we are going to find the mean fare paid by men and women by creating two density graphs on a singular graph. We need to install a new library `plyr`

```r
install.packages("plyr")
library(plyr)
library(ggplot2)
```
## Find Means
To do this, we need to create a dataset that holds the means of the fares paid by men and women. We use the `ddply` function to do this:

```r
mean = ddply(train, "Sex", summarise, grp.mean=mean(Fare))
head(mean)
```

`head` allows you to see the first six lines of data

Variables are case sensitive, so if you are encountering error messages that say something does not exist, make sure you are using capitals or lowercase!

## Create Density Graph

```r
x = ggplot(train, aes(x = Fare, color = Sex)) +
  geom_density()
x
```

`x` is the name of our graph. We assign it a name to make it easier to add other things on later.

`color = Sex` tells the computer to differentiate the graphs by sex.

The `x` assignment at the end simply plots the graph.

# Graphs with Five Number Summary

What if we want to create a density graph that includes a five number summary and a graph? We can do that in R! For this example, we are going to create a density plot of age and gender on the Titanic. First, we need to install some packages:

```r
# install packages
install.packages("ggpubr")
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

`measure.var = "Age` tells the computer that the variable we want to measure is Age

`grps` tells the computer that we are grouping this data by gender.

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



