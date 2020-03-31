Lab 7 - Geographic and Temporal Context
================

  - [Instructions and Overview](#instructions-and-overview)
  - [Getting Started](#getting-started)
  - [Facets](#facets)
  - [The Importance of Place and
    Time](#the-importance-of-place-and-time)
  - [Other Confounding Variables](#other-confounding-variables)
  - [Continue your shiny app.](#continue-your-shiny-app.)
  - [Other Case Studies to Consider (I will continue to grow this
    list)](#other-case-studies-to-consider-i-will-continue-to-grow-this-list)

## Instructions and Overview

At this point in the quarter, we have produced a number of plots and
calculate a number of measures in regards to our data. Now, we’re going
to explore in more depth how the stories the data tell change depending
on where we look. This exploration will turn into user inputs in our
Shiny
    app.

## Getting Started

### Load the relevant libraries

``` r
library(tidyverse)
```

    ## ── Attaching packages ──────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.0     ✔ purrr   0.3.3
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   0.8.3     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following object is masked from 'package:base':
    ## 
    ##     date

``` r
library(shiny)
library(shinydashboard)
```

    ## 
    ## Attaching package: 'shinydashboard'

    ## The following object is masked from 'package:graphics':
    ## 
    ##     box

``` r
library(shinyWidgets)
```

### Import and clean example datasets

``` r
hospitals <- read.csv("https://raw.githubusercontent.com/lindsaypoirier/STS-115/master/datasets/Hospitals.csv", stringsAsFactors = FALSE)

world_health_econ <- read.csv("https://raw.githubusercontent.com/lindsaypoirier/STS-115/master/datasets/world_health_econ.csv", stringsAsFactors = FALSE)

cases <- read.csv("https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv", stringsAsFactors = FALSE)

#Do not worry about this line of code for now. Since the cases data gets appended every day with a new column representing that day's case counts, if we want the total cases per country, we need to add up all of the previous day's counts into a new column. The column below does this for us. 
cases <- 
  cases %>% 
  mutate(Total.Cases = 
           cases %>% 
           select(starts_with("X")) %>% 
           rowSums()
         ) %>%
  select(Province.State, Country.Region, Total.Cases)

hospitals$ZIP <- as.character(hospitals$ZIP)

hospitals$ZIP <- str_pad(hospitals$ZIP, 5, pad = "0") 

is.na(hospitals) <- hospitals == "NOT AVAILABLE"
is.na(hospitals) <- hospitals == -999
is.na(cases) <- cases == ""

hospitals$SOURCEDATE <- ymd_hms(hospitals$SOURCEDATE)
hospitals$VAL_DATE <- ymd_hms(hospitals$VAL_DATE)
```

### Import and clean your dataset.

``` r
#Copy and paste relevant code from Lab 4 to import your data here. 

#Copy and paste relevant code from Lab 4 to clean your data here. This includes any row binding, character removals, converions in variable type, date formatting, or NA conversions. 
```

## Facets

To get us started in putting our data into context, we are going to look
at some of the plots we produced in previous labs grouped according to a
categorical variable in our dataset.

By *faceting* plots, we split them into a series of panels each
representing the grouped data associated with a particular value in a
categorical variable. Let’s start with an easy example from our
hospitals data - faceting a bar plot of the number of hospitals that
have a helipad by state. We can facet a plot by adding: +
facet\_wrap(\~VARIABLE\_NAME) to the
call.

``` r
#df %>% ggplot(aes(x = CATEGORICAL_VARIABLE)) + geom_bar() + facet_wrap(~CATEGORICAL_VARIABLE)

#without facet_wrap()
hospitals %>% 
  ggplot(aes(x = HELIPAD)) + 
  geom_bar() +
  labs(title = "Number of Hospitals by Helipad Status", x ="Helipad", y = "Count of Hospitals" ) +
  theme_bw()
```

![](lab7_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
#with facet_wrap()
hospitals %>% 
  ggplot(aes(x = HELIPAD)) + 
  geom_bar() +
  facet_wrap(~STATE) +
  labs(title = "Number of Hospitals by Helipad Status", x ="Helipad", y = "Count of Hospitals" ) +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 45, hjust=1, size = 6), 
        axis.text.y = element_text(size = 6),
        strip.text = element_text(size = 6)) 
```

![](lab7_files/figure-gfm/unnamed-chunk-4-2.png)<!-- -->

The first plot suggests that more hospitals have helipads than those
that don’t. Grouping this by state, however, we can see that in some
states (like California and New York), more hospitals don’t have
helipads than those that do. Faceting can help us zoom into the data for
more geographic specificity.

Let’s also facet data by a temporal variable. Assuming we have already
converted a date/time variable in our dataset into a data/time format
(using lubridate in lab 4), we should be able to extract a year, month,
day (and if available hour, minute, and second) from the data using
these lubridate functions:

  - year(DATE\_VARIABLE) will extract the year from the date
  - month(DATE\_VARIABLE) will extract the month from the date
  - day(DATE\_VARIABLE) will extract the day from the date
  - … and so on

<!-- end list -->

``` r
#without facet_wrap()
hospitals %>% 
  ggplot(aes(x = STATUS)) + 
  geom_bar() +
  labs(title = "Number of Hospitals by Status", x ="Status", y = "Count of Hospitals" ) +
  theme_bw()
```

![](lab7_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
#with facet_wrap()
hospitals %>% 
  ggplot(aes(x = STATUS)) + 
  geom_bar() +
  labs(title = "Number of Hospitals by Status", x ="Status", y = "Count of Hospitals" ) +
  facet_wrap(~year(SOURCEDATE)) +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 45, hjust=1, size = 6), 
        axis.text.y = element_text(size = 6),
        strip.text = element_text(size = 6)) 
```

![](lab7_files/figure-gfm/unnamed-chunk-5-2.png)<!-- --> Faceting this
plot by the year in which the data was sourced helps put the STATUS
field into context. We can see that the vast majority of the data
represented in the plot was sourced in 2018. We know that hospitals
across the country are not open or closed in perpetuity, so we need to
take the temporal context of the data into consideration when presenting
our results.

If we have qualified observational units in our dataset, faceting allows
us to group data by one of the qualifying variables and display the data
as a set of side-by-side plots. For instance, remember how we have been
filtering to the most recent year data was reported when plotting the
world\_health\_econ data. Instead, we could facet() the data to
demonstrate how different health ecnomics indicators change over time.

``` r
world_health_econ %>%
  ggplot(aes(x = tot_health_sp_pp, y = life_exp, size = pop, col = continent)) + 
  geom_point(shape = 21, stroke = 0.5) +
  facet_wrap(~year) +
  labs(title = "Relationship between Country Total Health Spending Per Person and Life Expectancy", x = "Total Health Spending Per Person", y = "Life Expectancy", size = "Population", col = "Continent") + # To add titles and labels
  theme_bw() +
  scale_size_continuous(range = c(1, 10), labels = scales::comma)
```

    ## Warning: Removed 160 rows containing missing values (geom_point).

![](lab7_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Copy and paste a plot from one of your previous labs. Then copy it again
and facet it by a geographic or temporal
variable.

``` r
#df %>% ggplot(aes(x = CATEGORICAL_VARIABLE)) + geom_bar() + facet_wrap(~CATEGORICAL_VARIABLE)

#Copy and paste a previoulsy created plot here. 

#Copy that plot again and facet it by a geographic or temporal variable. Adjust the theme as I have above to make your plot more legible.  
```

How do the stories that the two plots tell differ? What does your first
plot tell you about the geo-political landscape of the issue you are
studying? - OR - What does the first plot tell you about the temporal
landscape of the issue you are studying?

``` r
Fill your response here. 
```

Repeat the exercise you completed above for a second plot you created in
a previous
lab.

``` r
#df %>% ggplot(aes(x = CATEGORICAL_VARIABLE)) + geom_bar() + facet_wrap(~CATEGORICAL_VARIABLE)

#Copy and paste a previoulsy created plot here. 

#Copy that plot again and facet it by a geographic or temporal variable. Adjust the theme as I have above to make your plot more legible.  
```

How do the stories that the two plots tell differ? What does your second
plot tell you about the geo-political landscape of the issue you are
studying? - OR - What does the second plot tell you about the temporal
landscape of the issue you are studying?

``` r
Fill your response here. 
```

## The Importance of Place and Time

Time and place often matter a great deal in how we interpret data. To
get us started in thinking about why, let’s walk through a few examples:

### Case Study 1: Covid-19 Case Reporting by Country

Let’s take a look at the number of Covid-19 cases reported each day by
each country. To do so, we will be reimporting the cases data as a time
series - that is, as a dataset that reports the number of cases per day.
On import, every column represents a specific date and the case totals
for that date are the values stored in that
column.

``` r
cases_time_series <- read.csv("https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv", stringsAsFactors = FALSE)

head(cases_time_series)
```

    ##   Province.State      Country.Region      Lat     Long X1.22.20 X1.23.20
    ## 1                        Afghanistan  33.0000  65.0000        0        0
    ## 2                            Albania  41.1533  20.1683        0        0
    ## 3                            Algeria  28.0339   1.6596        0        0
    ## 4                            Andorra  42.5063   1.5218        0        0
    ## 5                             Angola -11.2027  17.8739        0        0
    ## 6                Antigua and Barbuda  17.0608 -61.7964        0        0
    ##   X1.24.20 X1.25.20 X1.26.20 X1.27.20 X1.28.20 X1.29.20 X1.30.20 X1.31.20
    ## 1        0        0        0        0        0        0        0        0
    ## 2        0        0        0        0        0        0        0        0
    ## 3        0        0        0        0        0        0        0        0
    ## 4        0        0        0        0        0        0        0        0
    ## 5        0        0        0        0        0        0        0        0
    ## 6        0        0        0        0        0        0        0        0
    ##   X2.1.20 X2.2.20 X2.3.20 X2.4.20 X2.5.20 X2.6.20 X2.7.20 X2.8.20 X2.9.20
    ## 1       0       0       0       0       0       0       0       0       0
    ## 2       0       0       0       0       0       0       0       0       0
    ## 3       0       0       0       0       0       0       0       0       0
    ## 4       0       0       0       0       0       0       0       0       0
    ## 5       0       0       0       0       0       0       0       0       0
    ## 6       0       0       0       0       0       0       0       0       0
    ##   X2.10.20 X2.11.20 X2.12.20 X2.13.20 X2.14.20 X2.15.20 X2.16.20 X2.17.20
    ## 1        0        0        0        0        0        0        0        0
    ## 2        0        0        0        0        0        0        0        0
    ## 3        0        0        0        0        0        0        0        0
    ## 4        0        0        0        0        0        0        0        0
    ## 5        0        0        0        0        0        0        0        0
    ## 6        0        0        0        0        0        0        0        0
    ##   X2.18.20 X2.19.20 X2.20.20 X2.21.20 X2.22.20 X2.23.20 X2.24.20 X2.25.20
    ## 1        0        0        0        0        0        0        1        1
    ## 2        0        0        0        0        0        0        0        0
    ## 3        0        0        0        0        0        0        0        1
    ## 4        0        0        0        0        0        0        0        0
    ## 5        0        0        0        0        0        0        0        0
    ## 6        0        0        0        0        0        0        0        0
    ##   X2.26.20 X2.27.20 X2.28.20 X2.29.20 X3.1.20 X3.2.20 X3.3.20 X3.4.20
    ## 1        1        1        1        1       1       1       1       1
    ## 2        0        0        0        0       0       0       0       0
    ## 3        1        1        1        1       1       3       5      12
    ## 4        0        0        0        0       0       1       1       1
    ## 5        0        0        0        0       0       0       0       0
    ## 6        0        0        0        0       0       0       0       0
    ##   X3.5.20 X3.6.20 X3.7.20 X3.8.20 X3.9.20 X3.10.20 X3.11.20 X3.12.20
    ## 1       1       1       1       4       4        5        7        7
    ## 2       0       0       0       0       2       10       12       23
    ## 3      12      17      17      19      20       20       20       24
    ## 4       1       1       1       1       1        1        1        1
    ## 5       0       0       0       0       0        0        0        0
    ## 6       0       0       0       0       0        0        0        0
    ##   X3.13.20 X3.14.20 X3.15.20 X3.16.20 X3.17.20 X3.18.20 X3.19.20 X3.20.20
    ## 1        7       11       16       21       22       22       22       24
    ## 2       33       38       42       51       55       59       64       70
    ## 3       26       37       48       54       60       74       87       90
    ## 4        1        1        1        2       39       39       53       75
    ## 5        0        0        0        0        0        0        0        1
    ## 6        1        1        1        1        1        1        1        1
    ##   X3.21.20 X3.22.20 X3.23.20 X3.24.20 X3.25.20 X3.26.20 X3.27.20 X3.28.20
    ## 1       24       40       40       74       84       94      110      110
    ## 2       76       89      104      123      146      174      186      197
    ## 3      139      201      230      264      302      367      409      454
    ## 4       88      113      133      164      188      224      267      308
    ## 5        2        2        3        3        3        4        4        5
    ## 6        1        1        3        3        3        7        7        7
    ##   X3.29.20
    ## 1      120
    ## 2      212
    ## 3      511
    ## 4      334
    ## 5        7
    ## 6        7

Note how the column headers are formatted above. When dates are stored
as column names and not as values in and of themselves, it can be very
difficult to manipulate the data based on dates. Remember that column
names are not data in and of themselves but a reference to a vector of
values. The dates in this dataset are a reference to a vector of case
numbers. Because the date values are not represented *in* the data, but
only as a reference to case numbers, we can’t filter observations to
specific dates or to dates that happened before or after another date.
The code below reshapes the data so that such filtering is possible.
Using **gather()**, for every province/country in the dataset, a new row
will be created in the dataset for every date data was reported. The
reshaped data will have two new columns: Case.Date (the date reported)
and Cases (the number reported on that date). Basically, we will take
wide data (many columns with fewer rows), and reshape it into long data
(fewer columns with more rows).

If you’d like to know more about how to use gather() to create tidy
data, I highly recommend [this](https://r4ds.had.co.nz/tidy-data.html)
chapter in R for Data Science.

You’ll also note above that R automatically places an “X” character in
front of numeric column names. Below we remove that X character and then
we transform the values into a date-time format.

``` r
cases_time_series <- cases_time_series %>% 
  gather(key = "Case.Date", value = "Cases", -Province.State, -Country.Region, -Lat, -Long) %>%
  mutate(Case.Date = str_remove(Case.Date, pattern = "X")) %>%
  mutate(Case.Date = mdy(Case.Date))

head(cases_time_series)
```

    ##   Province.State      Country.Region      Lat     Long  Case.Date Cases
    ## 1                        Afghanistan  33.0000  65.0000 2020-01-22     0
    ## 2                            Albania  41.1533  20.1683 2020-01-22     0
    ## 3                            Algeria  28.0339   1.6596 2020-01-22     0
    ## 4                            Andorra  42.5063   1.5218 2020-01-22     0
    ## 5                             Angola -11.2027  17.8739 2020-01-22     0
    ## 6                Antigua and Barbuda  17.0608 -61.7964 2020-01-22     0

Now let’s plot the total number of cases reported each day, faceted by
country. (This may take a bit of time to load.)

``` r
cases_time_series %>% 
  ggplot(aes(x = Case.Date, y = Cases)) + 
  stat_summary(geom = "col", fun.y = "sum") + 
  facet_wrap(~Country.Region) +
  labs(title = "Time Series of Case Counts in the US by Day and Country", x = "Case Date", y = "Cases") +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 45, hjust=1, size = 6), 
        axis.text.y = element_text(size = 6),
        strip.text = element_text(size = 6)) +
  scale_y_continuous(labels = scales::comma) #Change labels from scientific to comma notation
```

![](lab7_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

One observation that might stand out in this plot is just how few cases
were reported in Russia through mid-March. As of March 24, 2020, Russia
had reported fewer than 500 cases of Covid-19. For a country covering
more than 6 million square miles, with a population of 145 million, and
in such close proximity to China where the case counts are highest,
these numbers are surprisingly low. Russian officials have claimed that
measures they took - such as shutting down the Russian border with China
and setting up isolation zones for quarantined patients - have helped to
keep the virus under control. However, several news reports have been
published casting skepticism on the numbers, suggesting that in Russia
there is a widespread culture against producing information that may
draw negative international attention. To interpret this plot then, we
need to have an understanding of global *geopolitics*, or politics
mediated by geography, demography, and cross-border relations. The
numbers that we see reported in some parts of the world will vary from
numbers reported in other parts of the world, not because the values are
actually different, but because there are different cultures of
reporting in certain countries.

Characterize the geopolitics in your own dataset. Why might it be
important to consider the variable of place when interpreting your data?

``` r
Fill your response here.
```

### Case Study 2: Covid-19 Cases over Time

Now let’s look at a time series of cases reported in the US.

``` r
cases_time_series %>% 
  filter(Country.Region == "US") %>%
  ggplot(aes(x = Case.Date, y = Cases)) + 
  stat_summary(geom = "col", fun.y = "sum") +
  labs(title = "Time Series of Case Counts in the US by Day", x = "Case Date", y = "Cases") +
  theme_bw() +
  scale_y_continuous(labels = scales::comma) #Change labels from scientific to comma notation
```

![](lab7_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

This plot indicates that few cases were reported prior to mid-March.
However, to responsibly interpret this plot, we need to pay close
attention to how case totals are produced. The numbers of confirmed
cases reported in this dataset are based on the number of Covid-19 tests
that are reported positive. However, the extent of testing in the US has
been variable since January due to a number of political, regulatory,
and material roadblocks. Up through February, very few tests were being
conducted in the US as the federal government downplayed the threat
posed by the virus. A series of follow-up events continued to shift the
testing landscape:

  - February 5: The CDC
    [shipped](https://www.technologyreview.com/s/615323/why-the-cdc-botched-its-coronavirus-testing/)
    coronavirus test kits to state labs with contaminated reagents.
    These labs ended up having to send their samples to the CDC for
    testing.
  - February 29th: Up until this point, FDA regulations prevented labs
    from developing their own coronavirus test kits. On February, 29th,
    this rule was
    [lifted](https://www.fda.gov/news-events/press-announcements/coronavirus-covid-19-update-fda-issues-new-policy-help-expedite-availability-diagnostics)
    allowing high-complexity labs to develop and run tests under what is
    called an Emergency Use Authorization.
  - March 12th: Up until this point, in order for a lab to begin testing
    patients, they needed to be authorized by the FDA to do so. On March
    12, the FDA
    [granted](https://www.fda.gov/news-events/press-announcements/coronavirus-covid-19-update-fda-gives-flexibility-new-york-state-department-health-fda-issues)
    more flexibility to states to authorize this testing.
  - While these regulatory changes quickened the pace at which labs
    could test patients, at this point, a new issue was emerging. Labs
    began reporting [critical
    shortages](https://www.newyorker.com/news/news-desk/why-widespread-coronavirus-testing-isnt-coming-anytime-soon)
    of testing supplies and reagents for conducting tests.

With every change in the testing landscape, we can expect that the
number of cases reported will also change. To interpret this plot, we
need to have an understanding of *chrono-politics*, or the politics of
time. Importantly, these regulatory timelines are playing out
differently in different countries. There is much to consider at the
intersection of geo-politics and chrono-politics.

Characterize the chrono-politics in your own dataset. Why might it be
important to consider the variable of time when interpreting your data?

``` r
Fill your response here.
```

## Other Confounding Variables

Sometimes, when we are analyzing data, it can seem as though there is a
strong relationship between two variables in our dataset. Perhaps life
expenctancy increases as spending on health increases, or perhaps the
number of staff at a hospital increases as the number of beds at the
hospital increases. We may assume that there is is a causal relationship
between these two variables. *Confounding variables*, however, refer to
a third phenomena that influences the relationship we see between two
variables in a dataset. While it may appear as though two variables are
highly correlated, a shared association with a confounding variable may
be mediating that correlation. Before we make any claims with our data
it is important that we consider such variables.

For instance, consider if were to look at data documenting the
relationship between the number of Covid-19 cases each country has
reported and the number of deaths it has
reported.

``` r
deaths <- read.csv("https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv", stringsAsFactors = FALSE)

is.na(deaths) <- deaths == ""

deaths <- 
  deaths %>% 
  mutate(Total.Deaths = 
           deaths %>% 
           select(starts_with("X")) %>% 
           rowSums()
         ) %>%
  select(Province.State, Country.Region, Total.Deaths) %>%
  right_join(cases, by = c("Province.State", "Country.Region"))
```

``` r
deaths %>% 
  group_by(Country.Region) %>%
  summarize(Total.Cases = sum(Total.Cases, na.rm = TRUE), Total.Deaths = sum(Total.Deaths, na.rm = TRUE)) %>%
  ungroup() %>%
  ggplot(aes(x = Total.Cases, y = Total.Deaths)) + 
  geom_point(shape = 21, stroke = 1) +
  labs(title = "Relationship between Total Covid-19 Deaths and Total Cases Globally", x = "Cases", y = "Deaths") + # To add titles and labels
  theme_bw() +
  scale_x_continuous(trans='log10', labels =  scales::comma) +
  scale_y_continuous(trans='log10', labels = scales::comma)
```

    ## Warning: Transformation introduced infinite values in continuous y-axis

![](lab7_files/figure-gfm/unnamed-chunk-18-1.png)<!-- --> Keep in mind
that each dot above represents a country. Before we make any assumptions
about the causal relationship between these two variables, there are
number of confounding variables to consider: \* What is the median age
of the population in the country \* How advanced is the country’s
healthcare system \* To what extent do citizens in the country have
access to healthcare \* How soon were social distancing policies put
into place in the country \* How is the country documenting a Covid-19
related case/death \* Not to mention the geopolitics and chronopolitics
we described above

Can you come up with others?

### Population

When comparing certain phenomena across places - particuarly phenomena
that are related to human activity - it is often important to consider
how population can impact the numbers we see. For instance, when
assessing the readiness of hospitals to take on an influx of Covid-19
patients across counties and/or states in the US, we need to not only
consider the number of beds available, but also the population of the
county and/or state. Population is one confounding variable, among many,
that will mediate the relationship between hospital readiness for
patient intake and the number of beds available.

Consider another example. In 2018, I was at a conference in Brooklyn,
listening to a representative from MasterCard discuss the importance of
opening up data and getting it into the hands of community members to
anaylze. A health practitioner from NYC’s Department of Mental Health
and Hygeine stood up to voice his concerns to the speaker. He said that
it worries him that untrained community members may draw poor
assumptions from the data. He described a dataset that documented the
number mental health discharges by neighborhood in New York, and how it
indicated that there were far more discharges in low income communities.
This, he went on, could potentially lead someone to conclude that poor
people are more likely to experience mental health issues or to
instigate gun violence. However, he went on to assert that many
community members might not know just from looking at the data that
there are far more mental health hospitals in low income communities in
NYC than in higher income communities. …and that this is often because
higher income communities have more power to lobby for keeping such
hospitals out of their backyards\! In this case, the practitioner was
expressing concern that community members may not consider a significant
confounding variable mediating the number of mental health discharges
across NYC communities: the number of hospitals in each community.

In what ways might the distinct sizes or populations of a place impact
how you compare issues across your data?

``` r
Fill your response here.
```

### Semantic Changes

*Semantics* refer to the meaning of words. When I say the word ‘apple’
you are likely to conjure up an image in your head of what I am
referring to. In using the word, I am attempting to signify something
that exists in the world so that we both may communicate about that
thing. Semantics refer to the meaning behind this signifier. When we
work with data, we are dealing with a number of signifiers that have
have particular meanings behind them.

We always view data in a particular moment in time. However, over time,
our understanding of particular phenomena inevitably shifts as we gain
more knowledge, as culture changes, or as new phenomena come into
existence or into our awareness. An entire sub-field of anthropology is
dedicated to studying how language changes culturally over time. For
instance, consider how gender pronouns have shifted as the politics of
gender have also shifted. While historically, the pronoun “he” was often
used as a generic pronoun in English language, the femininst movement
played a key role in shifting our language systems to reject this use
and to encourage more gender neutral language. More recently,
transgender pronouns have become more present in our langauge systems,
indicating the continuity of culture shifts.

Research in information studies has shown how even categories that have
been institutionally standardized are constantly changing. For instance,
Geoffrey Bowker and Susan Leigh Star have traced changes in the
International Classification of Diseases (or the ICD) - a
coding/classifcation system for diseases maintained by the World Health
Organization. Medical professionals reference the ICD to diagnose
patients based on symptoms and their social circumstances; in other
words, every time an individual is diagnosed with something, that
something has an associated code in the ICD. The ICD has been
revised(with codes added, removed, reorganized, and edited) ten times -
the most recent version of the classification system being ICD-11. These
revisions not only reflect new research on diseases, but also changes in
culture. For instance, how abortion gets coded in the ICD has been
politically, ethically, and religiously charged.

While ICD-11 does have a classification for Covid-19, ICD-10 does not,
and ICD-10 is considered current and in use until January 1, 2022 when
it will be replaced by the new version. This means that, in the current
system for assigning codes to diagnoses, there is no formal signifier
for referring to the disease. Because of this, the WHO had to administer
an emergency ICD-10 code for use in diagnosing patients with Covid-19.
The current language system had to adapt to the new circumstances - and
in this case very quickly. You can read more about this
[here](https://www.who.int/classifications/icd/covid19/en/).

When we’re working with data, these issues can matter a great deal. When
classifications change in our data, i.e. when new terms become
available, when categories break into two, or when signifers are
removed, it also changes how we interpret quantities represented in the
data.

For instance, when I worked for NYC’s civic data team, I would regularly
work with open 311 data. 311 is basically a customer service number for
cities - a number for residents and visitors to report non-emergency
issues, such as potholes and graffiti, to city officials. In most
cities, every call to 311 gets aggregated in a database, and
increasingly, city officials are performing data analysis on the calls
to figure out where certain issues in the city are concentrated. If they
receive a number of calls about missed garbage pick-ups in a certain
community, they may divert attention and resources there for improved
sanitation. Often this data is publicly accessible for communities to
analyze.

Every 311 call is coded with a certain category, such as Noise, No Heat
and Hot Water in Building, Rat Sighting, or Broken Tree Limb. We would
often filter the data to certain categories and create maps of the
number of complaints deriving from different neighborhoods about that
issue. However, these categories were collectively maintained by over 70
city agencies which were constantly changing how they recorded issues.
For instance, In 2015, the category HEATING (referring to a heating
issue in an apartment) changed to HEAT/HOT WATER. If we did not know
that the categories were changing, we may have looked at the data and
thought that there was a sudden drop off in heating complaints or a
sudden uptick in hot water complaints - when in fact, it was just the
language in encoded in the system changing.

In what ways have semantics, along with the words used to represent
certain meanings, changed over time in your dataset?

``` r
Fill your response here.
```

Last week you listed a number of questions that your quantitative
calculations might help you to address. Select two of those questions,
and list them below. Then consider what confounding variables you may
need to consider before assuming the value you calculated or the plot
you created could adequately address that question.

What was one question you identified last week?

``` r
Fill your response here.
```

What confounding variable might you need to consider before assuming
your calculation could answer that question?

``` r
Fill your response here.
```

What was one question you identified last week?

``` r
Fill your response here.
```

What confounding variable might you need to consider before assuming
your calculation could answer that question?

``` r
Fill your response here.
```

## Continue your shiny app.

We are now going to add the capacity for users to specify specific
geographies and timeframes when viewing the data in our app. While I
believe all of you have a geography represented in your data, some of
you do not have date and time represented in your data. If this is the
case, you may remove the commented data-time inputs from the code below.

At this point, we will begin working in the ui code block and creating
more crosstalk between the ui and the server. Users will input
information into the front end of the app, and that input will be
communicated to the server. More specifically, users will select a
geography and a data/timeframe, and the server will filter the dataset
according to the selected features and recreate the values/plots on the
filtered data.

Be sure to follow all commented instructions below to add inputs to your
Shiny app.

``` r
#INPUT VARIABLES

geo_input_choices <- 
  #REPLACE hospitals BELOW WITH YOUR OWN DATAFRAME
  hospitals %>% 
  #REPLACE STATE BELOW WITH YOUR OWN GEOGRAPHIC VARIABLE
  select(STATE) %>% 
  distinct() %>% 
  #REPLACE STATE BELOW WITH YOUR OWN GEOGRAPHIC VARIABLE
  arrange(STATE)

#COMMENT LINES BELOW IF YOU DO NOT HAVE A TEMPORAL VARIABLE IN YOUR DATAFRAME
date_input_start <- 
  #REPLACE hospitals BELOW WITH YOUR OWN DATAFRAME
  hospitals %>% 
  #REPLACE SOURCEDATE BELOW WITH YOUR OWN TEMPORAL VARIABLE
  summarize(date = min(SOURCEDATE))

date_input_end <- 
  #REPLACE hospitals BELOW WITH YOUR OWN DATAFRAME
  hospitals %>% 
  #REPLACE SOURCEDATE BELOW WITH YOUR OWN TEMPORAL VARIABLE
  summarize(date = max(SOURCEDATE))
#COMMENT LINES ABOVE IF YOU DO NOT HAVE A TEMPORAL VARIABLE IN YOUR DATAFRAME

ui <- dashboardPage(
  
  #REPLACE 'TITLE HERE' BELOW WITH YOUR OWN TITLE
  dashboardHeader(title = "TITLE HERE"),
  #REPLACE 'TITLE HERE' ABOVE WITH YOUR OWN TITLE
  
  dashboardSidebar(
      selectInput(inputId = "geo_val", label = "Select an geography:", choices = geo_input_choices, selected = geo_input_choices[1]),
      #COMMENT LINE BELOW IF YOU DO NOT HAVE A TEMPORAL VARIABLE IN YOUR DATAFRAME
      dateRangeInput(inputId = "date_val", label = "Select a date range:", start = date_input_start$date, end = date_input_end$date)
      #COMMENT LINE ABOVE IF YOU DO NOT HAVE A TEMPORAL VARIABLE IN YOUR DATAFRAME
  ),
  
  dashboardBody(
      infoBoxOutput("value1", width = 4),
      infoBoxOutput("value2", width = 4),
      infoBoxOutput("value3", width = 4),
   
      box(plotOutput("plot1")),
      box(plotOutput("plot2")),
      box(plotOutput("plot3")),
      box(plotOutput("plot4"))

  )
)
```

In each of your functions below, you are going to add a filter
statement, filtering to the user input.

``` r
server <- function(input, output) {
  
  output$value1 <- renderInfoBox({
    quant_insight1 <-
    #REPLACE BELOW WITH YOUR OWN CODE
      0
    #REPLACE ABOVE WITH YOUR OWN CODE
    
    #REPLACE 'FILL DESCRIPTION HERE' BELOW WITH YOUR OWN DESCRIPTION
    infoBox(quant_insight1,'FILL DESCRIPTION HERE', icon = icon("stats", lib='glyphicon'), color = "purple")
    #REPLACE 'FILL DESCRIPTION HERE' ABOVE WITH YOUR OWN DESCRIPTION
  })
  
  output$value2 <- renderInfoBox({
    quant_insight2 <- 
    #REPLACE BELOW WITH YOUR OWN CODE
      0
    #REPLACE ABOVE WITH YOUR OWN CODE 
    
    #REPLACE 'FILL DESCRIPTION HERE' BELOW WITH YOUR OWN DESCRIPTION
    infoBox(quant_insight2,'FILL DESCRIPTION HERE', icon = icon("stats", lib='glyphicon'), color = "purple")
    #REPLACE 'FILL DESCRIPTION HERE' ABOVE WITH YOUR OWN DESCRIPTION
  })
  
  output$value3 <- renderInfoBox({
    quant_insight3 <- 
    #REPLACE BELOW WITH YOUR OWN CODE
      0
    #REPLACE ABOVE WITH YOUR OWN CODE 
    
    #REPLACE 'FILL DESCRIPTION HERE' BELOW WITH YOUR OWN DESCRIPTION
    infoBox(quant_insight3,'FILL DESCRIPTION HERE', icon = icon("stats", lib='glyphicon'), color = "purple")
    #REPLACE 'FILL DESCRIPTION HERE' ABOVE WITH YOUR OWN DESCRIPTION
  })
  
  
  output$plot1 <- renderPlot({
    #REPLACE BELOW WITH YOUR OWN PLOT
    hospitals %>% 
      filter(
        STATE == input$geo_val & 
        SOURCEDATE > input$date_val[1] &
        SOURCEDATE < input$date_val[2]
        ) %>%
      ggplot(aes(x = TYPE)) + geom_bar()
    #REPLACE ABOVE WITH YOUR OWN PLOT 
    
  })
  
  output$plot2 <- renderPlot({
    #REPLACE BELOW WITH YOUR OWN PLOT
    hospitals %>% 
      filter(
        STATE == input$geo_val & 
        SOURCEDATE > input$date_val[1] &
        SOURCEDATE < input$date_val[2]
        ) %>%
      ggplot(aes(x = TYPE)) + geom_bar()
    #REPLACE ABOVE WITH YOUR OWN PLOT 
  })
  
  output$plot3 <- renderPlot({
    #REPLACE BELOW WITH YOUR OWN PLOT
    hospitals %>% 
      filter(
        STATE == input$geo_val & 
        SOURCEDATE > input$date_val[1] &
        SOURCEDATE < input$date_val[2]
        ) %>%
      ggplot(aes(x = TYPE)) + geom_bar()
    #REPLACE ABOVE WITH YOUR OWN PLOT  
    
  })
  
  output$plot4 <- renderPlot({
    #REPLACE BELOW WITH YOUR OWN PLOT
    hospitals %>% 
      filter(
        STATE == input$geo_val & 
        SOURCEDATE > input$date_val[1] &
        SOURCEDATE < input$date_val[2]
        ) %>%
      ggplot(aes(x = TYPE)) + geom_bar()
    #REPLACE ABOVE WITH YOUR OWN PLOT  
  })
  
}
```

``` r
shinyApp(ui, server)
```

<!--html_preserve-->

<div class="muted well" style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;">

Shiny applications not supported in static R Markdown documents

</div>

<!--/html_preserve-->

## Other Case Studies to Consider (I will continue to grow this list)

### Case Study: Chrono-politics of Calls to 311 during Hurricane Sandy

``` r
library(scales)
```

    ## 
    ## Attaching package: 'scales'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     discard

    ## The following object is masked from 'package:readr':
    ## 
    ##     col_factor

``` r
nyc311 <- read.csv("https://github.com/lindsaypoirier/STS-115/blob/master/datasets/311_before_2015.csv?raw=true", stringsAsFactors = FALSE) 
```

When Hurricane Sandy hit NYC in 2012, the City leveraged data about
calls New Yorkers had made to 311 to track where a number of different
issues were occurring. Visualizing data about 311 calls in the three
months following the disaster indeed showed a spike in complaints about
issues like damaged trees and lack of heat in apartment buildings
throughout the city. Leveraging this data, the city was able to position
the devastation of the disaster as episodic - a result of natural forces
beyond their control.

``` r
nyc311 %>% 
  filter(year(Created.Date) == 2012 & month(Created.Date) %in% c(7:11)) %>%
  ggplot(aes(x = as.Date(Created.Date), group = 1)) +
  geom_vline(xintercept=as.Date(c("2012-10-29")),linetype=4, colour="black") +
  geom_line(stat="count") + 
  facet_wrap(~Complaint.Type) + 
  labs(x="Month", y="Count") +
  scale_x_date() +
  theme_bw() +
  annotate(geom = "label", x = as.Date("2012-10-29"), y = 4000, label = "Hurricane Sandy", angle = 90)
```

![](lab7_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

However, social researchers have argued that it is not so simple to
delimit the temporal boundaries of a disaster. While a natural event may
happen on a particular day or span of days, the structural conditions of
the communities they impact have much longer timespans. Natural events
occurring in delimited periods of time often exacerbate existing social
issues like poverty, lack of affordable housing, and unequal access to
healthcare.

Let’s zoom out on the 311 data to look at these issues over the course
of years versus these three months. Complaints to 311 about damaged
trees do spike following most major natural events. However, complaints
about lack of heat in apartments spike every year in October …because in
New York, that’s when it gets cold. By only looking at the data in
months immediately after the disaster, the city could obscure the big
picture - that poor New Yorkers face apartment negligence every year.
The temporal context in which we analyze data matters a great deal for
how we interpret it.

``` r
nyc311 %>% 
  ggplot(aes(x=as.Date(Created.Date), col=as.factor(year(Created.Date)), group = year(Created.Date))) +
  geom_vline(xintercept=as.Date(c("2010-03-12","2010-09-16","2011-08-28","2011-10-29","2012-10-29")),linetype=4, colour="black") +
  geom_line(stat = "count") + 
  facet_wrap(~Complaint.Type) +
  labs(x="Month", col="Year") +
  scale_x_date(breaks = "3 months", labels=date_format("%b-%y")) +
  theme_bw() +
  annotate(geom = "label", x = as.Date("2010-03-12"), y = 4000, label = "nor'easter", angle = 90) +
  annotate(geom = "label", x = as.Date("2010-09-16"), y = 4000, label = "tornadoes", angle = 90) +
  annotate(geom = "label", x = as.Date("2011-08-28"), y = 4000, label = "Hurricane Irene", angle = 90) +
  annotate(geom = "label", x = as.Date("2011-10-29"), y = 4500, label = "Halloween nor'easter", angle = 90) +
  annotate(geom = "label", x = as.Date("2012-10-29"), y = 4000, label = "Hurricane Sandy", angle = 90)
```

![](lab7_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->

``` r
  #nor'easter, tornadoes, Irene, Halloween nor'easter, Sandy
```

> Liboiron, Max. 2015. “Disaster Data, Data Activism: Grassroots
> Responses to Representing Superstorm Sandy.” In Extreme Weather and
> Global Media, edited by Julia Leyda and Diane Negra. Taylor & Francis
> Group. <https://doi.org/10.4324/9781315756486-7>.
