# HMK 5: Reading and tidying data

# Reading for this lesson

- [R4DS Chapters 6-9](https://r4ds.hadley.nz/data-import)

# install and call packages

# Question 1:

- Create a directory, within your main class directory, called `data`.
  - Note: in general, you should store raw data in a directory called
    `data`
- Download the example file for Ch 9
  [here](https://pos.it/r4ds-students-csv). Save it inside the `ddata`
  directory.

## download data using terminal (bash code)

- Create a directory using bash command `mkdir data`
- go to that directory using bash command `cd data`
- download data using bash command
  `wget https://r4ds.hadley.nz/data-import`
- change the name of data file `mv data-import student_raw_data.csv`

<!-- -->


    -   Use `read_csv()` to read the file to an R data frame. Follow the instructions in Ch 9 to format it properly. Follow the directions in Ch 9 to make sure the following are true:
        -   Column names should be *syntactic*, meaning they don't contain spaces.
        -   N/A values should be represented with the R value `NA`, not the character "N/A".
        -   Data types (character vs factor vs numeric) should be appropriate.

    ## cleaning data   
    ::: {.cell}

    ```{.r .cell-code}
    student_data <- read_csv("data/student_raw_data.csv", show_col_types = FALSE)



    clean_student_data <- student_data |>
      rename(Full_Name = `Full Name`,                                     # Column names should be *syntactic*, meaning they don't contain spaces
             Student_ID = `Student ID`) |>                                
      mutate(AGE = if_else(AGE == "five", "5", AGE)) |>                   # change five to "5", no worries we will change it to integer class later
      mutate_all(~ ifelse(. == "N/A", NA , .)) |>                         # find character N/A and change it to NA
      mutate(                                                             # Data types (character vs factor vs numeric) should be appropriate.
        Student_ID = as.factor(Student_ID),
        AGE = as.integer(AGE)
      )
    # is.na(clean_data)
      
    print(clean_student_data)

<div class="cell-output cell-output-stdout">

    # A tibble: 6 × 5
      Student_ID Full_Name        favourite.food     mealPlan              AGE
      <fct>      <chr>            <chr>              <chr>               <int>
    1 1          Sunil Huffmann   Strawberry yoghurt Lunch only              4
    2 2          Barclay Lynn     French fries       Lunch only              5
    3 3          Jayendra Lyne    <NA>               Breakfast and lunch     7
    4 4          Leon Rossini     Anchovies          Lunch only             NA
    5 5          Chidiegwu Dunkel Pizza              Breakfast and lunch     5
    6 6          Güvenç Attila    Ice cream          Lunch only              6

</div>

:::

# Qustion 2

Find (or make) a data file of your own, in text format. Read it into a
well-formatted data frame.

## Tidying

Download the data set available at
<https://tiny.utk.edu/MICR_575_hmk_05>, and save it to your `data`
folder. **This is a real data set:** it is the output from the
evaluation forms for student colloquium speakers in the Microbiology
department. I have eliminated a few columns, changed some of the scores,
and edited comments, to protect student privacy, but the output is real.

First, open this .csv file with Microsoft Excel or a text reading app,
to get a sense of the structure of the document. It is weird.

Why is the file formatted so inconveniently? I have no idea, but I do
know that this is about an average level of inconvenient formatting for
real data sets you will find in the wild.

*Note: In theory, you can pass a URL to `read_csv()` and read the file
directly from the internet. In practice, that doesn’t seem to work for
this file. So you’ll want to download this file to your hard drive.*

## Question 3-A

Next, use `read_csv()` to read the data into a data frame. Note that
you’ll need to make use of some of the optional arguments. Use
`?read_csv` to see what they are.

``` r
data <- read_csv("data/raw_data.csv", show_col_types = FALSE,na ="NA",  skip_empty_rows = TRUE)
# the argument `skip_empty_rows = TRUE` does not work here
# so we need to do it in explicit way
```

## method that works

``` r
data <- read_csv("data/raw_data.csv", show_col_types = FALSE)
clean_data <- data |>
  filter(!is.na(StartDate)) |>                             # filter the raws with all NA values
  slice(-1,-2) |>                                          # remove the first and second row which is redundent
  rename(Duration_in_seconds = `Duration (in seconds)`) |> # Column names should be *syntactic*, meaning they don't contain spaces
  separate(StartDate, into = c("Start_Date", "start_time"), sep = " ") |>  # in next three columns we seperate time and date which was written in same column into two seperate columns
  separate(EndDate, into = c("End_Date", "End_time"), sep = " ") |>
  separate(RecordedDate, into = c("Recorded_Date", "Recorded_time"), sep = " ") |>
  mutate(                                                  # Data types (character vs factor vs numeric) should be appropriate.
    Status = as.factor(Status),
    Progress = as.integer(Progress),
    Duration_in_seconds = as.integer(Duration_in_seconds),
    Finished = as.factor(Finished),
    ResponseId = as.character(ResponseId),
    LocationLatitude = as.double(LocationLatitude),
    LocationLongitude = as.double(LocationLongitude),
    DistributionChannel = as.character(DistributionChannel),
    Q4 = as.integer(Q4),
    Q5 = as.integer(Q5),
    Q6 = as.integer(Q6),
    Q7 = as.integer(Q7),
    Q8 = as.integer(Q8),
    Q9 = as.integer(Q9),
    Q10 = as.integer(Q10),
    Q11 = as.integer(Q11),
    Start_Date = as.Date(Start_Date),
    End_Date = as.Date(End_Date),
    Recorded_Date = as.Date(Recorded_Date)
    
  )
  
# is.na(clean_data)
  
glimpse(clean_data)
```

    Rows: 16
    Columns: 28
    $ Start_Date          <date> 0011-11-22, 0011-11-22, 0011-11-22, 0011-11-22, 0…
    $ start_time          <chr> "9:01", "9:02", "9:03", "9:04", "9:02", "9:14", "9…
    $ End_Date            <date> 0011-11-22, 0011-11-22, 0011-11-22, 0011-11-22, 0…
    $ End_time            <chr> "9:02", "9:02", "9:04", "9:05", "9:06", "9:14", "9…
    $ Status              <fct> 0, 0, 0, 0, 0, 8, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
    $ Progress            <int> 100, 100, 100, 100, 100, 100, 100, 100, 100, 100, …
    $ Duration_in_seconds <int> 18, 31, 109, 40, 243, 28, 15, 33, 103, 21, 22, 81,…
    $ Finished            <fct> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1
    $ Recorded_Date       <date> 0011-11-22, 0011-11-22, 0011-11-22, 0011-11-22, 00…
    $ Recorded_time       <chr> "9:02", "9:02", "9:04", "9:05", "9:06", "9:14", "9…
    $ ResponseId          <chr> "R_2b14qToYhcmCVFD", "R_2eVcizz1gWXK4gd", "R_2an9…
    $ RecipientLastName   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
    $ RecipientFirstName  <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
    $ RecipientEmail      <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
    $ ExternalReference   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
    $ LocationLatitude    <dbl> 35.9539, 35.8974, 35.9452, 35.9452, 35.8697, 35.95…
    $ LocationLongitude   <dbl> -83.9357, -83.9425, -83.9435, -83.9435, -83.7462, …
    $ DistributionChannel <chr> "anonymous", "anonymous", "anonymous", "anonymous"…
    $ UserLanguage        <chr> "EN", "EN", "EN", "EN", "EN", "EN", "EN", "EN", "E…
    $ Q4                  <int> 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 2, 1, 1, 2, 1
    $ Q5                  <int> 2, 2, 2, 2, 2, 1, 2, 2, 1, 2, 3, 2, 2, 3, 1, 2
    $ Q6                  <int> 4, 3, 2, 2, 2, 2, 2, 2, 2, 3, 2, 2, 2, 2, 1, 2
    $ Q7                  <int> 5, 5, 4, 4, 5, 4, 5, 2, 5, 5, 5, 4, 5, 5, 4, 5
    $ Q8                  <int> 5, 5, 3, 5, 5, 5, 5, 3, 4, 5, 5, 5, 4, 5, 5, 5
    $ Q9                  <int> 4, 5, 3, 4, 4, 5, 4, 2, 5, 5, 5, 5, 3, 5, 5, 5
    $ Q10                 <int> 4, 5, 3, 4, 5, 5, 5, 5, 3, 5, 5, 5, 4, 4, 4, 5
    $ Q11                 <int> 4, 5, 5, 5, 5, 5, 5, 4, 4, 5, 5, 5, 5, 5, 4, 5
    $ Q12                 <chr> NA, "Great presentation", "Be very careful when ma…

As we discussed in class, the correct shape depends on what you want to
do with the data. Use `pivot_longer()` to make the data frame longer, in
a way that makes sense. ANSWER: we want to make a long dataframe of all
questions and answers here.

``` r
long_format_data <- clean_data |>
  select("ResponseId","Q4", "Q5", "Q6","Q7", "Q8", "Q9", "Q10", "Q11") |>
  pivot_longer(cols = c("Q4", "Q5", "Q6","Q7", "Q8", "Q9", "Q10", "Q11"), names_to = "Question_number", values_to = "grade")

print(long_format_data)
```

    # A tibble: 128 × 3
       ResponseId        Question_number grade
       <chr>             <chr>           <int>
     1 R_2b14qToYhcmCVFD Q4                  1
     2 R_2b14qToYhcmCVFD Q5                  2
     3 R_2b14qToYhcmCVFD Q6                  4
     4 R_2b14qToYhcmCVFD Q7                  5
     5 R_2b14qToYhcmCVFD Q8                  5
     6 R_2b14qToYhcmCVFD Q9                  4
     7 R_2b14qToYhcmCVFD Q10                 4
     8 R_2b14qToYhcmCVFD Q11                 4
     9 R_2eVcizz1gWXK4gd Q4                  1
    10 R_2eVcizz1gWXK4gd Q5                  2
    # ℹ 118 more rows

## Question 3-B

Finally, calculate this student’s average score for each of questions
7-10.

``` r
average_answers <- long_format_data |>
  group_by(Question_number) |>
  summarise(average_grade = mean(grade))

print(average_answers)
```

    # A tibble: 8 × 2
      Question_number average_grade
      <chr>                   <dbl>
    1 Q10                      4.44
    2 Q11                      4.75
    3 Q4                       1.19
    4 Q5                       1.94
    5 Q6                       2.19
    6 Q7                       4.5 
    7 Q8                       4.62
    8 Q9                       4.31

``` r
# this student has the highest score in Question 11 which shows he/she was good in that area(i don't know the question)
```

## Important note about file paths in Quarto documents

When you render a Quarto document, RStudio spins up a new instance of R,
which is separate from the instance of R that you cna interact with. The
working directory for this instance of R is whatever directory your
Quarto document is saved in.

If your quarto document is saved in the same directory as your RStudio
project (e.g., `MICR_475`), then there is no difference between your
interactive working directory and the working directory for your Quarto
document.

However, if your homeworks are saved in a `HMK` directory, then the
Quarto working directory will be `HMK`. To access the saved `.csv` file,
`read_csv()` will need to look *up* one directory and then go back
*down* into `HMK`. `..` means “up one directory”, so you would need to
use `read_csv("../colloquium_assessment.csv")` instead of
`read_csv("colloquium_assessment.csv")`.
