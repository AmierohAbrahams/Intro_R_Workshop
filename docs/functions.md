---
output:
  pdf_document: default
  html_document: default
---
### Writing your own functions

Writing your code into functions is key for writing neat R code and prevents repitition of code (when you have copied and pasted a piece of code twice). In my experience, young biologists are reluctant to use functions in their code and often when functions are being used they are abused and created to do too much at once. Creating a function reduces mistakes from copying and pasting and also makes updating code much easier. RStudio has several built in functions, and with more packages installed, more functions are available. Throughout this course we have continuously been making use of functions. The **tidyverse** package in itself contain several functions, such as the `mean()`, `sqrt()` and **ggplot2** functions.

A typical example of a function would be `sqrt()`. Where the argument must be a number, and the return value is the square root of that number. Executing a function is called *calling* the function. Here we *call* a built in R function.

```{r, eval=FALSE, purl=FALSE}
b <- sqrt(a)
```

Here, the value of `a` is given to the `sqrt()` function, the `sqrt()` function calculates the square root, and returns the value which is then assigned to the object `b`. This function is very simple, because it takes just one argument.

Now we learn to write our own basic function. The process in which we write any function, regardless of the size, is similar and involves three key steps:

- Make use of the term `function`
- Specify the arguements in that function
- Specify the body of the function

```{r, eval=FALSE}
my_func <- function(arg1, arg2) {
  body
}
```

`my_func` is the function name. This can be any variable name, but as previously mentioned throughout this workshop, we should avoid using names that are used in built in R functions such as `mean`, `length`, *etc*., as this may often lead to confusion and ultimately result in unnessasary errors.

`arg1` and `arg2` are the *arguements* of the function. You can write a function with any number of arguments, and some arguments may have default values specified. 

Above, 'body' denotes the function's body. This is all the code found within the { ... } brackets. The size of this code may vary depending on the required outcome. 

Once we start putting things in functions so that we can re-use them, we need to start testing that those functions are working correctly. To see how to do this, lets write a function. As mentioned earlier some arguments may have default values specified and so here I specify `y` to have a default value of 1. So, in the function call, when no value is assigned to `y` the default value will be 1.

```{r}
add <- function(x , y = 1) {
  x + y
}
```

Here we created a function called `add()`. This function takes two arguments, `x` and `y`, and returns the sum of `x` and `y`. Since `y` already has a default value assigned to it, it is necessary to only specify `x` in the function call; specifying `y` is optional. Since the code was 'activated' by running it through the console, the function is now available, like any other built-in functions in R, for you to use.

Now lets put our function to test:

```{r}
add(15) # When no y value is given it is taken as 1, as specified in the function

add(15, 5)
```

For some practice write a function where we calculate the sum of the squares: 

```{r}
sum_squares <- function(x, y) {
  x^2 + y^2
}
```

Testing the function

```{r}
sum_squares(9, 20)
```

## Basic plotting using functions

Remember the `ChickWeight` dataset used when we explored the `ggplot()` function?. Now lets use this again. The `ChickWeight` dataset, as previously seen is a built in R dataset and so we will load this data in the usual way by using `datasets::ChickWeight`. With the `filter()` function we split the ChickWeight dataset into three different datasets based on the diet type.

```{r}
chicks <- datasets::ChickWeight

Diet1 <- chicks %>% 
  filter(Diet == "1")
Diet2 <- chicks %>% 
  filter(Diet == "2")
Diet3 <- chicks %>% 
  filter(Diet == "3")
```
With our three newly created datasets, we are able to create our very own function. Here we are plotting the same graph for each of the individual datasets. But why create a function to do this?. Without a function one would simply copy and paste the same code over three times. But by creating a function we are maintaining a neat coding style and avoiding repetition. 

```{r}
func_plot <- function(df) {
  plot1 <- df %>% 
    ggplot(aes(x = Time, y = weight)) +
    geom_point() +
    geom_smooth(method = "lm") +
    labs(x = "Days", y = "Weight (gm)", colour = "diet type") 
}
```
Lets put our function to test

```{r}
(diet1 <- func_plot(df = Diet1))
(diet2 <- func_plot(df = Diet2))
(diet3 <- func_plot(df = Diet3))
```

### Basic summary statistics function

Now that we have created our very own plotting function, lets try and create another. But here lets create a function where we use some built in functions within our function. 

```{r}
sum_stats <- function(df) {
  new_data <- df %>% 
    summarise(mean_wt = mean(weight),
              sd_wt = sd(weight),
              var_wt = var(weight)) 
}
```
Great! lets test this function

```{r}
(diet1 <- sum_stats(df = Diet1))
(diet2 <- sum_stats(df = Diet2))
(diet3 <- sum_stats(df = Diet3))
```

## Exercise:

1. Create two vectors:
x <- c( 1, 6, 21, 19 , NA, 73, NA)
y <- c(NA, NA, 3, NA, 13, 24, NA)

a) Count the number of elements are missing in both x and y

b) Transform the code, used above (a), into a function

c) Create three new vectors and test the function created in (b)

## Bonus


1. With all of this information in hand, make use of the `laminaria` dataset and create one plotting function and one basic statistic function.

2. Create a function that calculates at least three summary statistics and run it on two different sites in the `laminaria` dataset.

