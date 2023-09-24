Homework 03
================

# Base R and R Basics

HINT: Remember that you can get help on any function by typing
`?`(function name). For instance, `?rnorm` gives help on the `rnorm()`
function.

## Creating and naming variables

1.  Create a variable called `x` and use it to store the result of the
    calculation `(3*(4+2)`.

    ``` r
    x <- (3*(4+2))
    # or 
    x = (3*(4+2))
    print(x)
    ```

        [1] 18

2.  Calculate the product of `x` (from the above question) times π.

    ``` r
    product_of_x = x * pi
    print(product_of_x)
    ```

        [1] 56.54867

3.  Use the `getwd()` function to show your current working directory.
    Is that a good working directory, and what program do you think set
    it that way?’

    ``` r
    getwd()
    ```

        [1] "/Users/kami/my_GitHub_Repository/Reproducible_Data_Science/homework2"

    ``` r
    # it's in my GitHub Repository folder
    # RStudio
    ```

## Vectors

1.  Use the `c()` function to create a vector of numbers.

    ``` r
    vector_of_numbers <- c(0, 1, 1.6180339, exp(1), pi, 299792458)
    ```

2.  Use the `c()` function to create a vector of characters.

    ``` r
    vector_of_characters <- c('Rachel Carson', 'Edward Wilson', 'Jane Goodall', 'Sylvia Earle', 'David Attenborough')
    ```

3.  Use the `:` implicit function to create a vector of integers from 1
    to 10.

    ``` r
    vector <- 1:10
    ```

4.  Explain *why* the following code returns what it does. Also address
    whether you think this was a good decision on the part of the
    designers of R?

    ANSWER: Recycling shorter vectors gives R the ability to be more
    flexible when using vectors with different length and can also be
    useful for data science and statistics.

    but personally this kind of behavior is why i trust python more
    than R. when a user is not familiar with R’s behavior, R does not
    generate errors or warnings, potentially leading to challenging and
    hard to detect errors that can result in disaster.

    i search internet for some advantages for this behavior and none of
    the reasons was convincing for me.

``` r
v1 <- 1:3
v2 <- c(1:4)
v1 + v2
```

    [1] 2 4 6 5

5.  Explain what the following code does. It may be helpful to reference
    the answer to the previous question:

    We can see vector recycling, when we perform some kind of operations
    like addition, subtraction, etc. on two vectors of unequal length.
    The vector with a small length will be repeated as long as the
    operation completes on the longer vector. again it’s not convincing
    for me when you can write it in more easy to understand form like:

    c(1, 5, 9) + c(3, 3, 3)

``` r
c(1, 5, 9) + 3
```

    [1]  4  8 12

6.  Remove (delete) every variable in your workspace.

    ``` r
    rm(list = ls())
    ```

## Graphics

1.  Load the tidyverse package. **NOTE:** Be sure to use the chunk
    option `message=FALSE` to suppress the messages that tidyverse
    prints when loaded. These messages are useful in the

``` r
library(tidyverse)
```

2.  Recreate the visualization of `body_mass_g` to `flipper_length_mm`,
    from the penguins data set, that is shown in question 8 of section
    2.2.5 of [R4DS](https://r4ds.hadley.nz/data-visualize).

    ``` r
    # install.packages("palmerpenguins")
    library(palmerpenguins)

    # nice view of the data
    glimpse(penguins)
    ```

        Rows: 344
        Columns: 8
        $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
        $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
        $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
        $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
        $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
        $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
        $ sex               <fct> male, female, female, NA, female, male, female, male…
        $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…

    ``` r
    ggplot(data = penguins,
           mapping = aes(x = flipper_length_mm, y = body_mass_g, color = bill_depth_mm)) +
           geom_point() + geom_smooth()
    ```

        `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

        Warning: Removed 2 rows containing non-finite values (`stat_smooth()`).

        Warning: The following aesthetics were dropped during statistical transformation: colour
        ℹ This can happen when ggplot fails to infer the correct grouping structure in
          the data.
        ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
          variable into a factor?

        Warning: Removed 2 rows containing missing values (`geom_point()`).

    ![](hmk_03_files/figure-commonmark/unnamed-chunk-12-1.png)

3.  Explain why each aesthetic is mapped at the level that it is (i.e.,
    at the global level, in the `ggplot()` function call, or at the geom
    level, in the `geom_XXX()` function call). Note: A lot of different
    options will work, but some options are clearly better than others.

    ANSWER: you can apply it in different levels and see the diference.

    when you apply aesthetic on global level it means it’s gonna change
    the entire plot, or it’s same in all geoms, but if you apply it on
    specific geom level, it means it’s specific to a particular data
    layer.
