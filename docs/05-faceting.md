# Faceting Figures {#faceting}

> "But let the mind beware, that though the flesh be bugged, the circumstances of existence are pretty glorious."
>
> --- Kurt Vonnegut, Player Piano
  
> "You miss 100% of the shots you don't take."
>
> --- Wayne Gretzky



So far we have only looked at single panel figures. But as you may have guessed by now, **`ggplot2`** is capable of creating any sort of data visualisation that a human mind could conceive. This may seem like a grandiose assertion. We'll see if we can't convince you by the end of this course. For now however, let's just take our understanding of the usability of **`ggplot2`** one step further by looking at how to create grids of one figure, as well as combine different types of figure into one grid. To do this we are going to need to learn how to create a few new types of figures.

But first, here is how we would facet a single figure:


```r
# Load libraries
library(tidyverse)

# Load data
ChickWeight <- datasets::ChickWeight

# Create faceted figure
ggplot(data = ChickWeight, aes(x = Time, y = weight, colour = Diet)) +
  geom_point() +
  geom_smooth(method = "lm") + # Note the `+` sign here
  facet_wrap(~Diet, ncol = 2) # This is the line that creates the facets
```

<img src="05-faceting_files/figure-html/basic-9-1.png" width="672" />

The linear model figure we've already created:


```r
lm_1 <- ggplot(data = ChickWeight, aes(x = Time, y = weight, colour = Diet)) +
  geom_point() +
  geom_smooth(method = "lm") +
  ggtitle("A.") # Add a title
lm_1
```

<img src="05-faceting_files/figure-html/basic-lm-1.png" width="672" />

How to create a histogram:


```r
histogram_1 <- ggplot(data = ChickWeight, aes(x = weight)) +
  geom_histogram(aes(fill = Diet)) +
  ggtitle("B.") # Add a title
histogram_1
```

<img src="05-faceting_files/figure-html/basic-hist-1.png" width="672" />

How to create a density plot:


```r
density_1 <- ggplot(data = ChickWeight, aes(x = weight)) +
  geom_density(aes(fill = Diet), alpha = 0.4) +
  ggtitle("C.") # Add a title
density_1
```

<img src="05-faceting_files/figure-html/basic-dens-1.png" width="672" />

How to create a boxplot:


```r
box_1 <- ggplot(data = ChickWeight, aes(x = Diet, y = weight)) +
  geom_boxplot(aes(fill = Diet)) +
  ggtitle("D.") # Add a title
box_1
```

<img src="05-faceting_files/figure-html/basic-box-1.png" width="672" />

With these four different figures created we may now look at how to combine them. By visualising the data in different ways they are able to tell us different parts of the same story. What do we see from the figures below that we may not have seen when looking at each figure individually?


```r
# For creating grids of figures
library(gridExtra)

# The gridded figure
grid.arrange(lm_1,histogram_1, density_1, box_1, ncol = 2)
```

<img src="05-faceting_files/figure-html/basic-grid-1.png" width="1536" />

The above figure looks great, so let's save a copy of it as a PDF to our computer. In order to do so we will need to use the `ggsave()` function.


```r
# First we must assign the code to an object name
grid_1 <- grid.arrange(lm_1,histogram_1, density_1, box_1, ncol = 2)

# Then we save the object we created
ggsave(plot = grid_1, filename = "figures/grid_1.pdf")
```