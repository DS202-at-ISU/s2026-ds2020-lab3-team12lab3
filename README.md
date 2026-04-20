
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyr)
library(readr)
```

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
deaths <- av %>%
  pivot_longer(
    cols = starts_with("Death"),
    names_to = "Time",
    values_to = "Death"
  ) %>%
  mutate(Time = parse_number(Time))

head(deaths)
```

    ## # A tibble: 6 × 18
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 3 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return1 <chr>, Return2 <chr>,
    ## #   Return3 <chr>, Return4 <chr>, Return5 <chr>, Notes <chr>, Time <dbl>,
    ## #   Death <chr>

Similarly, deal with the returns of characters.

``` r
returns <- av %>%
  pivot_longer(
    cols = starts_with("Return"),
    names_to = "Time",
    values_to = "Return"
  ) %>%
  mutate(Time = parse_number(Time))

head(returns)
```

    ## # A tibble: 6 × 18
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 3 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Death1 <chr>, Death2 <chr>,
    ## #   Death3 <chr>, Death4 <chr>, Death5 <chr>, Notes <chr>, Time <dbl>,
    ## #   Return <chr>

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
avg_deaths <- deaths %>%
  filter(Death == "YES") %>%
  group_by(URL) %>%
  summarise(total_deaths = n()) %>%
  summarise(avg = mean(total_deaths))

avg_deaths
```

    ## # A tibble: 1 × 1
    ##     avg
    ##   <dbl>
    ## 1  1.29

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

## Individually - Mariana Correa

### FiveThirtyEight Statement

> “Out of 173 listed Avengers, my analysis found that 69 had died at
> least one time after they joined the team. That’s about 40 percent of
> all people who have ever signed on to the team.”

### Include the code

``` r
num_avengers <- nrow(av)
num_avengers
```

    ## [1] 173

``` r
died_at_least_once <- deaths %>%
  filter(Death == "YES") %>%
  distinct(URL) %>%
  nrow()

died_at_least_once
```

    ## [1] 69

``` r
pct_died <- died_at_least_once / num_avengers * 100
pct_died
```

    ## [1] 39.88439

### Include your answer

My calculations show that the dataset contains 173 Avengers, and 69 of
them dies at least once, totaling about 40%. Therefore, the original
statement is correct.

## Ewan Individual work

### Ewan FiveThirtyEight Statement

“There’s a 2-in-3 chance that a member of the Avengers returned from
their first stint in the afterlife

## Ewan’s work

``` r
first_deaths <- deaths %>%
  filter(Death == "YES") %>%
  group_by(URL) %>%
  arrange(Time) %>%
  slice(1) %>%
  ungroup()

first_returns <- first_deaths %>%
   left_join(returns, by = c("URL", "Time")) %>%
  summarise(
  return_rate = mean(Return == "YES", na.rm = TRUE)
  )
first_returns
```

    ## # A tibble: 1 × 1
    ##   return_rate
    ##         <dbl>
    ## 1       0.667

## Ewan’s Answer

after fact checking the statement I found that the return rate of
avengers after their first death is 66.67% which is a 2 in 3 chance, so
the statement was correct.

### Mason’s FiveThirtyEight Statement

> “Of the nine Avengers we see on screen — Iron Man, Hulk, Captain
> America, Thor, Hawkeye, Black Widow, Scarlet Witch, Quicksilver and
> The Vision — every single one of them has died at least once in the
> course of their time Avenging in the comics. In fact, Hawkeye died
> twice!”

### Mason’s code

``` r
deaths_filtered <- deaths %>% filter (Death == 'YES', Name.Alias %in% c('Anthony Edward "Tony" Stark','Robert Bruce Banner' ,'Steven Rogers', 'Thor Odinson', 'Clinton Francis Barton', 'Natalia Alianovna Romanova', 'Wanda Maximoff', 'Pietro Maximoff', 'Victor Shade (alias)'))
deaths_filtered
```

    ## # A tibble: 11 × 18
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Anthony …        3068 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Robert B…        2089 YES      MALE   ""                 
    ##  3 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ##  4 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ##  5 http://marvel.wik… "Steven R…        3458 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Clinton …        1456 YES      MALE   ""                 
    ##  7 http://marvel.wik… "Clinton …        1456 YES      MALE   ""                 
    ##  8 http://marvel.wik… "Pietro M…         769 YES      MALE   ""                 
    ##  9 http://marvel.wik… "Wanda Ma…        1214 YES      FEMALE ""                 
    ## 10 http://marvel.wik… "Victor S…        1036 YES      MALE   ""                 
    ## 11 http://marvel.wik… "Natalia …        1112 YES      FEMALE ""                 
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return1 <chr>, Return2 <chr>,
    ## #   Return3 <chr>, Return4 <chr>, Return5 <chr>, Notes <chr>, Time <dbl>,
    ## #   Death <chr>

``` r
counts_df <- deaths_filtered |> count(Name.Alias)
counts_df
```

    ## # A tibble: 9 × 2
    ##   Name.Alias                          n
    ##   <chr>                           <int>
    ## 1 "Anthony Edward \"Tony\" Stark"     1
    ## 2 "Clinton Francis Barton"            2
    ## 3 "Natalia Alianovna Romanova"        1
    ## 4 "Pietro Maximoff"                   1
    ## 5 "Robert Bruce Banner"               1
    ## 6 "Steven Rogers"                     1
    ## 7 "Thor Odinson"                      2
    ## 8 "Victor Shade (alias)"              1
    ## 9 "Wanda Maximoff"                    1

### Mason’s answer

The quote stated that all 9 of the avengers have died at least once,
with hawkeye having suffered two deaths. I filtered the deaths dataframe
to only those 9 avengers and the rows in which they have a death marked,
and summed them up in a dataframe which counts the deaths. The statement
is TRUE! However, it fails to account for the fact that Thor has also
died twice.
