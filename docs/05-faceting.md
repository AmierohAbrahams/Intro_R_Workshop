# Faceting Figures {#faceting}

> "But let the mind beware, that though the flesh be bugged, the circumstances of existence are pretty glorious."
>
> --- Kurt Vonnegut, Player Piano
  
> "You miss 100% of the shots you don't take."
>
> --- Wayne Gretzky



So far we have only looked at single panel figures. But as you may have guessed by now, **`ggplot2`** is capable of creating any sort of data visualisation that a human mind could conceive. This may seem like a grandiose assertion, but we'll see if we can't convince you of it by the end of this course. For now however, let's just take our understanding of the usability of **`ggplot2`** two steps further by first learning how to facet a single figure, and then stitch different types of figures together into a grid. In order to aid us in this process we will make use of an additional package, **`ggpubr`**. The purpose of this package is to provide a bevy of additional tools that researchers commonly make use of in order to produce publication quality figures. Note that `library(ggpubr)` will not work on your computer if you have not yet installed the package.


```r
# Load libraries
library(tidyverse)
library(ggpubr)
```

## Faceting one figure

Faceting a single figure is built into **`ggplot2`** from the ground up and will work with virtually anything that could be passed to the `aes()` function. Here we see how to create an individual facet for each `Diet` within the `ChickWeight` dataset.


```r
# Load data
ChickWeight <- datasets::ChickWeight

# Create faceted figure
ggplot(data = ChickWeight, aes(x = Time, y = weight, colour = Diet)) +
  geom_point() +
  geom_smooth(method = "lm") + # Note the `+` sign here
  facet_wrap(~Diet, ncol = 2) + # This is the line that creates the facets
  labs(x = "Days", y = "Mass (g)")
```

<div class="figure">
<img src="05-faceting_files/figure-html/facet-1-1.png" alt="Simple faceted figure showing a linear model applied to each diet." width="672" />
<p class="caption">(\#fig:facet-1)Simple faceted figure showing a linear model applied to each diet.</p>
</div>

## New figure types

Before we can create a gridded figure of several smaller figures, we need to learn how to create a few new types of figures first. The code for these different types is shown below. Some of the figures types we will learn how to use now do not work well with the full `ChickWeight` dataset. Rather we will want only the weights from the final day of collection. To filter only these data we will need to use a bit of "tidy" code. Don't worry too much about this now as we will go into this in depth on Day 4.


```r
ChickLast <- ChickWeight %>% 
  filter(Time == 21)
```


### Linear model


```r
lm_1 <- ggplot(data = ChickWeight, aes(x = Time, y = weight, colour = Diet)) +
  geom_point() +
  geom_smooth(method = "lm") +
  labs(x = "Days", y = "Mass (g)")
lm_1
```

<div class="figure">
<img src="05-faceting_files/figure-html/facet-lm-1.png" alt="Linear models for the progression of chicken weights (g) over time (days) based on four different diets." width="672" />
<p class="caption">(\#fig:facet-lm)Linear models for the progression of chicken weights (g) over time (days) based on four different diets.</p>
</div>

### Histogram


```r
# Note that we are using 'ChickLast', not 'ChickWeight'
histogram_1 <- ggplot(data = ChickLast, aes(x = weight)) +
  geom_histogram(aes(fill = Diet), position = "dodge") +
  labs(x = "Final Mass (g)", y = "Count")
histogram_1
```

<div class="figure">
<img src="05-faceting_files/figure-html/facet-hist-1.png" alt="Histogram showing final chicken weights (g) by diet." width="672" />
<p class="caption">(\#fig:facet-hist)Histogram showing final chicken weights (g) by diet.</p>
</div>

### Density plot


```r
# Note that we are using 'ChickLast', not 'ChickWeight'
density_1 <- ggplot(data = ChickLast, aes(x = weight)) +
  geom_density(aes(fill = Diet), alpha = 0.4) +
  labs(x = "Final Mass (g)", y = "Density")
density_1
```

<div class="figure">
<img src="05-faceting_files/figure-html/facet-dens-1.png" alt="Density plot showing the distribution of final chicken weights (g) by diet." width="672" />
<p class="caption">(\#fig:facet-dens)Density plot showing the distribution of final chicken weights (g) by diet.</p>
</div>

### Boxplot


```r
# Note that we are using 'ChickLast', not 'ChickWeight'
violin_1 <- ggplot(data = ChickLast, aes(x = Diet, y = weight)) +
  geom_violin(aes(fill = Diet)) +
  labs(x = "Diet", y = "Final Mass (g)")
violin_1
```

<div class="figure">
<img src="05-faceting_files/figure-html/facet-violin-1.png" alt="Violin plot showing the distribution of final chicken weights (g) by diet." width="672" />
<p class="caption">(\#fig:facet-violin)Violin plot showing the distribution of final chicken weights (g) by diet.</p>
</div>

## Gridding figures

With these four different figures created we may now look at how to combine them. By visualising the data in different ways they are able to tell us different parts of the same story. What do we see from the figures below that we may not have seen when looking at each figure individually?


```r
ggarrange(lm_1, histogram_1, density_1, violin_1, 
          ncol = 2, nrow = 2, # Set number of rows and columns
          labels = c("A", "B", "C", "D"), # Label each figure
          common.legend = TRUE) # Create common legend
```

<div class="figure">
<img src="05-faceting_files/figure-html/facet-grid-1.png" alt="All four of our figures gridded together with an automagically created common legend." width="672" />
<p class="caption">(\#fig:facet-grid)All four of our figures gridded together with an automagically created common legend.</p>
</div>

The above figure looks great, so let's save a copy of it as a PDF to our computer. In order to do so we will need to use the `ggsave()` function.


```r
# First we must assign the code to an object name
grid_1 <- ggarrange(lm_1, histogram_1, density_1, violin_1, 
                    ncol = 2, nrow = 2, 
                    labels = c("A", "B", "C", "D"),
                    common.legend = TRUE)

# Then we save the object we created
ggsave(plot = grid_1, filename = "figures/grid_1.pdf")
```
