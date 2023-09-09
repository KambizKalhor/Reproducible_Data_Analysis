# Base R and R Basics

HINT: Remember that you can get help on any function by typing `?`(function name). For instance, `?rnorm` gives help on the `rnorm()` function.

## Creating and naming variables

1.  Create a variable called `x` and use it to store the result of the calculation `(3*(4+2)`.

    ```{r}
    x <- (3*(4+2))
    # or 
    x = (3*(4+2))
    print(x)
    ```

2.  Calculate the product of `x` (from the above question) times π.

    ```{r}
    product_of_x = x * pi
    print(product_of_x)
    ```

3.  Use the `getwd()` function to show your current working directory. Is that a good working directory, and what program do you think set it that way?'

    ```{r}
    getwd()
    # it's in my GitHub Repository folder
    # RStudio
    ```

## Vectors

1.  Use the `c()` function to create a vector of numbers.

    ```{r}
    vector_of_numbers <- c(0, 1, 1.6180339, exp(1), pi, 299792458)
    ```

2.  Use the `c()` function to create a vector of characters.

    ```{r}
    vector_of_characters <- c('Rachel Carson', 'Edward Wilson', 'Jane Goodall', 'Sylvia Earle', 'David Attenborough')
    ```

3.  Use the `:` implicit function to create a vector of integers from 1 to 10.

    ```{r}
    vector <- 1:10
    ```

4.  Explain *why* the following code returns what it does. Also address whether you think this was a good decision on the part of the designers of R?

    ANSWER: Recycling shorter vectors gives R the ability to be more flexible when using vectors with different length and can also be useful for data science and statistics.

    but personally this kind of behavior is why i trust python more than R. when a user is not familiar with R's behavior, R does not generate errors or warnings, potentially leading to challenging and hard to detect errors that can result in disaster.

    i search internet for some advantages for this behavior and none of the reasons was convincing for me.

```{r, warning=FALSE}
v1 <- 1:3
v2 <- c(1:4)
v1 + v2
```

5.  Explain what the following code does. It may be helpful to reference the answer to the previous question:

    again it's not convincing for me when you can write it in more easy to understand form like:

    c(1, 5, 9) + c(3, 3, 3)

    My personal preference is for programming languages that have fewer surprises and fewer rules.

```{r}
c(1, 5, 9) + 3
```

6.  Remove (delete) every variable in your workspace.

    ```{r}
    rm(list = ls())
    ```

## Graphics

1.  Load the tidyverse package. **NOTE:** Be sure to use the chunk option `message=FALSE` to suppress the messages that tidyverse prints when loaded. These messages are useful in the

```{r, message=FALSE}
library(tidyverse)
```

2.  Recreate the visualization of `body_mass_g` to `flipper_length_mm`, from the penguins data set, that is shown in question 8 of section 2.2.5 of [R4DS](https://r4ds.hadley.nz/data-visualize).

    ```{r}
    # install.packages("palmerpenguins")
    library(palmerpenguins)

    # nice view of the data
    glimpse(penguins)
    ```

    ```{r}
    ggplot(data = penguins,
           mapping = aes(x = flipper_length_mm, y = body_mass_g, color = island)) +
           geom_point() + geom_smooth(method = 'lm')

    ```

3.  Explain why each aesthetic is mapped at the level that it is (i.e., at the global level, in the `ggplot()` function call, or at the geom level, in the `geom_XXX()` function call). Note: A lot of different options will work, but some options are clearly better than others.

ANSWER: you can apply it in different levels and see the diference.

when you apply aesthetic on global level it means it's gonna change the entire plot, or it's same in all geoms, but if you apply it on specific geom level, it means it's specific to a particular data layer.
