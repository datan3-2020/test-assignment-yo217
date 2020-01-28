Test statistical assignment
================
Youran Xu
27 January 2020

## Introduction

Please change the author and date fields above as appropriate. Do not
change the output format. Once you have completed the assignment you
want to knit your document into a markdown document in the
“github\_document” format and then commit both the .Rmd and .md files
(and all the associated files with graphs) to your private assignment
repository on Github.

## Reading data (40 points)

First, we need to read the data into R. For this assignment, I ask you
to use data from the youth self-completion questionnaire (completed by
children between 10 and 15 years old) from Wave 9 of the Understanding
Society. It is one of the files you have downloaded as part of SN6614
from the UK Data Service. To help you find and understand this file you
will need the following documents:

1)  The Understanding Society Waves 1-9 User Guide:
    <https://www.understandingsociety.ac.uk/sites/default/files/downloads/documentation/mainstage/user-guides/mainstage-user-guide.pdf>
2)  The youth self-completion questionnaire from Wave 9:
    <https://www.understandingsociety.ac.uk/sites/default/files/downloads/documentation/mainstage/questionnaire/wave-9/w9-gb-youth-self-completion-questionnaire.pdf>
3)  The codebook for the file:
    <https://www.understandingsociety.ac.uk/documentation/mainstage/dataset-documentation/datafile/youth/wave/9>

<!-- end list -->

``` r
library(tidyverse)
```

    ## ── Attaching packages ───────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ──────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
# This attaches the tidyverse package. If you get an error here you need to install the package first. 
Data <- read_tsv("/Users/youran/Data3-2020/data/UKDA-6614-tab/tab/ukhls_w9/i_youth.tab")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double()
    ## )

    ## See spec(...) for full column specifications.

``` r
# You need to add between the quotation marks a full path to the required file on your computer.
```

## Tabulate variables (10 points)

In the survey children were asked the following question: “Do you have a
social media profile or account on any sites or apps?”. In this
assignment we want to explore how the probability of having an account
on social media depends on children’s age and gender.

Tabulate three variables: children’s gender, age (please use derived
variables) and having an account on social media.

``` r
# add your code here
library(magrittr)
```

    ## 
    ## Attaching package: 'magrittr'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     set_names

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     extract

``` r
library(dplyr)

Data <- Data %>% 
  select(i_sex_dv, i_age_dv, i_ypsocweb)

table(Data$i_sex_dv)
```

    ## 
    ##    0    1    2 
    ##    2 1411 1408

``` r
table(Data$i_age_dv)
```

    ## 
    ##   9  10  11  12  13  14  15  16 
    ##   1 460 496 467 463 491 434   9

``` r
table(Data$i_ypsocweb)
```

    ## 
    ##   -9    1    2 
    ##   14 2277  530

## Recode variables (10 points)

We want to create a new binary variable for having an account on social
media so that 1 means “yes”, 0 means “no”, and all missing values are
coded as NA. We also want to recode gender into a new variable with the
values “male” and “female” (this can be a character vector or a factor).

``` r
# add your code here
Data <- Data %>% 
  mutate(social.account = recode(i_ypsocweb,
                                 "1" = 1,
                                 "2" = 0,
                                 .default = NA_real_))
Data <- Data %>% 
  mutate(gender = recode(i_sex_dv,
                         "1" = "male",
                         "2" = "female",
                         .default = NA_character_))
Data %>% head(5)
```

    ## # A tibble: 5 x 5
    ##   i_sex_dv i_age_dv i_ypsocweb social.account gender
    ##      <dbl>    <dbl>      <dbl>          <dbl> <chr> 
    ## 1        2       14          1              1 female
    ## 2        1       12          1              1 male  
    ## 3        1       13          1              1 male  
    ## 4        2       14          1              1 female
    ## 5        1       11          1              1 male

## Calculate means (10 points)

Produce code that calculates probabilities of having an account on
social media (i.e. the mean of your new binary variable produced in the
previous problem) by age and gender.

``` r
# add your code here
Data %>% 
  group_by(i_age_dv) %>% 
  summarise(
    mean_age = mean(social.account, na.rm = TRUE) * 100
  )
```

    ## # A tibble: 8 x 2
    ##   i_age_dv mean_age
    ##      <dbl>    <dbl>
    ## 1        9      0  
    ## 2       10     48.7
    ## 3       11     69.9
    ## 4       12     86.6
    ## 5       13     91.5
    ## 6       14     94.7
    ## 7       15     95.9
    ## 8       16    100

``` r
Data %>% 
  group_by(gender) %>% 
  summarise(
    mean_gender = mean(social.account, na.rm = TRUE) * 100
  )
```

    ## # A tibble: 3 x 2
    ##   gender mean_gender
    ##   <chr>        <dbl>
    ## 1 female        84.0
    ## 2 male          78.3
    ## 3 <NA>         100

## Write short interpretation (10 points)

Write two or three sentences interpreting your findings above. - The
majority of the participants have a social media account. - The
probability of having a social media account increases with age. -
Females are slightly more likely to have a social media account compared
to males, with a difference of 5.7 percentage points.

## Visualise results (20 points)

Create a statistical graph (only one, but it can be faceted)
illustrating your results (i.e. showing how the probability of having an
account on social media changes with age and gender). Which type of
statistical graph would be most appropriate for this?

``` r
# add your code here
```

## Conclusion

This is a test formative assignment and the mark will not count towards
your final mark. If you cannot answer any of the questions above this is
fine – we are just starting this module\! However, please do submit this
assignment in any case to make sure that you understand the procedure,
that it works correctly and you do not have any problems with summative
assignments later.
