Lab 6 - Quantitative Insights
================

  - [Instructions and Overview](#instructions-and-overview)
  - [Getting Started](#getting-started)
  - [Measures of Central Tendency](#measures-of-central-tendency)
  - [Measures of Dispersion](#measures-of-dispersion)
  - [Grouped Quantitative Insights](#grouped-quantitative-insights)
  - [Continue your shiny app.](#continue-your-shiny-app.)
  - [More examples and Useful Functions (This is only for
    reference.)](#more-examples-and-useful-functions-this-is-only-for-reference.)

## Instructions and Overview

In this lab, we will practice calculating and interpreting measures of
central tendency and measures of dispersion, as well as investigating
the consequences of relying on such numbers out of context to represent
complex problems. To begin you will need to import and clean your
dataset, and then you will follow the prompts while responding to short
answer questions. Examples have been provided to support you throughout
the process. At the end of the assignment, we will integrate some of
your calculations into the Shiny application you started last
    week.

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

## Measures of Central Tendency

A measure of central tendency is a single numeric quantity describing
data by identifying a central position within it. It summarizes values
within a set into a single value. We often refer to such a value as an
average.

At times, finding this measure can be quite useful \_\_. However, we
also need to be careful when relying on measures of central tendency to
communicate information about our data. This is because measures of
central tendency can be *reductionist* - reducing a complex story told
across data to a single value. They can hide meaningful outliers and
erase the nuance of a complicated narrative. For instance, measures of
central tendency related to wealth in the US are likely to hide the
experiences of the most impoverished communities. Such a measure can be
a weapon for stakeholders combatting government policies to direct
public resources towards communities in need.

Further, there are many pitfalls we need to steer clear of when
assessing a measure of central tendency. Such a measure should only be
taken to summarize the values across *similar observations.* If you have
qualified units of observation, this will likely mean filtering your
data to a set of similar values - to one year in the world\_health\_econ
dataset for instance if you were looking to average populations across
countries.

Let’s say that we were looking to calculate the average number of beds
in hospitals across the US. To do so, would it be appropriate to
identify a central position within the BEDS variable in the dataset?
Probably not. There are many different types of hospitals in the
hospitals dataset, and we would expect these different hospitals to have
different numbers of beds. For instance, we *know* from our research
that Critical Access Hospitals are supposed to have 25 beds or fewer. In
averaging across all of them, we would be measuring the center value of
dissimilar observations. Before taking an average of BEDS, we would want
to filter our data to a set of similar observations. We will do this
below - filtering to open general acute care hospitals.

### Mean

A commomn measure of central tendency is a *mean* - the sum of a series
of values divided by the number of values summed. This measure considers
every value in a set of data and thus is a model of the entire set of
data. Remember from lab five that we calculate mean with **summarize()**
Let’s calculate the mean number of beds at open general acute care
hospitals.

``` r
#df %>% summarize(mean_value = mean(NUMERIC_VARIABLE, na.rm = TRUE))

hospitals %>% 
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  summarize(mean_value = mean(BEDS, na.rm = TRUE))
```

    ##   mean_value
    ## 1   199.3746

Similarly, if we wanted to know the mean number of Covid-19 cases per
country, as we learned last week, we would need to group\_by() the
country and summarize() the total number of cases. Remember that this is
because some cases are reported at the province level and some are
reported at the country level. To ensure that we are taking a measure of
central tendency across similar data, we need to ensure that all of the
values we are summarizing are reported at one geographic scale - in this
case, the country level.

``` r
#df %>% summarize(mean_value = mean(NUMERIC_VARIABLE, na.rm = TRUE))

cases %>% 
  group_by(Country.Region) %>%
  summarize(Total.Cases = sum(Total.Cases, na.rm = TRUE)) %>%
  ungroup() %>%
  summarize(mean_value = mean(Total.Cases, na.rm = TRUE))
```

    ## # A tibble: 1 x 1
    ##   mean_value
    ##        <dbl>
    ## 1     50647.

### Median

Another measure of central tendency considers the middle value in a
dataset; this is referred to as the *median*. Now let’s calculate the
median number of beds at open general acute care hospitals.

``` r
#df %>% summarize(median_value = median(NUMERIC_VARIABLE, na.rm = TRUE))

hospitals %>% 
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  summarize(median_value = median(BEDS, na.rm = TRUE))
```

    ##   median_value
    ## 1          139

Let’s also calculate the median number of Covid-19 cases per country.

``` r
#df %>% summarize(mean_value = mean(NUMERIC_VARIABLE, na.rm = TRUE))

cases %>% 
  group_by(Country.Region) %>%
  summarize(Total.Cases = sum(Total.Cases, na.rm = TRUE)) %>%
  ungroup() %>%
  summarize(median_value = median(Total.Cases, na.rm = TRUE))
```

    ## # A tibble: 1 x 1
    ##   median_value
    ##          <dbl>
    ## 1         1351

Notice how there’s a considerable difference between the mean and the
median value across both of these sets of calculations. So which should
we rely on to summarize the central location in our dataset?

We want to keep in mind that because the mean is a model of the entire
dataset, it is easily influenced by outliers in the dataset, as well as
a skewed distribution. When the distribution of values in a set is
normal, the mean and median of the dataset will be the same because the
values on either side of the middle value will be symmetrical. In other
words, the distribution will be balanced on either side of the median.
Frequency plots give us a good indication of the distribution of values
the dataset.

``` r
hospitals %>% 
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  ggplot(aes(x = BEDS)) +
  geom_freqpoly(binwidth = 10) + 
  labs(title = "Distribution of Beds across General Acute Care Hospitals in the US that are Open", x = "Beds", y = "Count of Hospitals") +
  theme_bw()
```

    ## Warning: Removed 181 rows containing non-finite values (stat_bin).

![](lab6_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
cases %>% 
  group_by(Country.Region) %>%
  summarize(Total.Cases = sum(Total.Cases, na.rm = TRUE)) %>%
  ungroup() %>%
  ggplot(aes(x = Total.Cases)) + 
  geom_freqpoly(binwidth = 10000) +
  labs(title = "Distribution of Cases across Countries", x = "Total Cases", y = "Count of Countries") + # To add titles and labels
  theme_bw() +
  theme(axis.text.x = element_text(angle = 90, hjust=1)) + #Turn labels 90 degrees
  scale_x_continuous(labels = scales::comma) #Change labels from scientific to comma notation
```

![](lab6_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

You’ll notice in the hospitals and the cases dataset, the values of are
significantly right-skewed. We can tell because the values represented
on the plot are far from symmetrical. Instead, there is a long tail
running to the right of the data. In such cases, the mean is going to be
significantly influenced by the larger values in the data even though
there are far fewer.

When the values in a frequency plot are skewed to the left or the right,
we often want to rely on the median rather than the mean as a measure of
central tendency, as it more clearly distinguishes the central location
in the data. In the hospitals and the cases dataset, median is a better
indicator of central tendency than the mean. However, both still
significanltly gloss over the dispersion of values in the data.

We also need to be careful when taking measures of central tendency that
we do not attempt to take an average of an average. Let me provide an
exmaple of why this is inappropriate. Let’s say I were to find the
average number of beds at open general acute hospitals per state using
group\_by() and summarize().

``` r
avg_by_state <- 
  hospitals %>% 
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  group_by(STATE) %>%
  summarize(mean_value = mean(BEDS, na.rm = TRUE)) %>%
  ungroup() 

avg_by_state
```

    ## # A tibble: 57 x 2
    ##    STATE mean_value
    ##    <chr>      <dbl>
    ##  1 AK          96.5
    ##  2 AL         181. 
    ##  3 AR         174. 
    ##  4 AS         NaN  
    ##  5 AZ         193. 
    ##  6 CA         196. 
    ##  7 CO         153. 
    ##  8 CT         298. 
    ##  9 DC         398. 
    ## 10 DE         286. 
    ## # … with 47 more rows

In taking the mean of the mean\_value column we just created (i.e. an
average of the state averages), you will notice that we get a different
value than the mean we calculated above:

``` r
#Mean of BEDS across all observations
avg_across_all_obs <-
  hospitals %>% 
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  summarize(mean_value = mean(BEDS, na.rm = TRUE))

#Mean of BEDS across all state averages
avg_across_state_avgs <-
  avg_by_state %>%
  summarize(mean_value = mean(mean_value, na.rm = TRUE))

#Check if they are equal
paste("When we check if", avg_across_all_obs, "(the mean of BEDS across all observations) is equal to", avg_across_state_avgs, "(the mean of BEDS across state averages) the result is", avg_across_all_obs == avg_across_state_avgs)
```

    ## [1] "When we check if 199.374598468001 (the mean of BEDS across all observations) is equal to 190.239127684961 (the mean of BEDS across state averages) the result is FALSE"

This is because when we take an average of averages, we are ignoring a
key confounding variable - the size of the denominator in each of the
state averages. We know that there are different numbers of hospitals in
each state. When we take the mean across all state averages, we
basically ignore this variable - producing a different mean than if we
were to calculate mean across all observations in the set. This can have
dramatic consequences on the values reported in our data.

Please watch this short video on Simpon’s Paradox to learn more about
the importance of avoiding this pitfall:
<iframe width="560" height="315" src="https://www.youtube.com/watch?v=ebEkn-BiW5k" frameborder="0" allowfullscreen></iframe>

This is an issue we would need to keep in mind when calculating measures
of central tendency in our world\_health\_econ dataset. The majority of
the variables reported in the world\_health\_econ dataset are already
averages - either averages across the population of the country or
averages across the country’s total spending on health. Because of this,
it would be inappropriate to calculate measures of central tendency
across these variables without knowing their denominators.

> You might be thinking, “Well don’t we have population as another
> variable in the dataset. Doesn’t that tell us the denominator of some
> of the averages?” That is corret. However, looking at our data
> dictionary you will notice that the population variable in the dataset
> is derived from a different source than the health economics
> indicators. While we can expect these numbers to be in proximity,
> different organizations count the number of people in a country
> differently. This value might not accurately reflect the denominator
> of many of our health economics
indicators.

### Measures of Central Tendency in your Own Dataset

#### Calculate the mean and median of a numeric variable in your dataset.

Be sure to filter your dataset to ensure that you are summarizing across
similar observations. Also be sure to avoid averaging an average.

``` r
#Fill your code here. 
```

What question might these measures of central tendency help to address?

``` r
Fill your response here. 
```

Create a frequency plot of the numeric variable you produced
calculations of above. (You may have created this last week, and you may
copy and paste it from the previous lab.)

``` r
#Create a frequency plot here. 
```

Characterize the distribution of values in the frequency plot you
created. Is it symmetrical or skewed?

``` r
Fill your response here. 
```

Which value is more appropriate as a measure of central tendency in your
data?

``` r
Fill your response here. 
```

Characterize the extent to which the value you selected as a measure of
central tendency above (mean or median) is representative of your data.
What story is told when we rely on this number as a summary of the data?

``` r
Fill your response here. 
```

If we were to use this single value to summarize this variable, what
narratives would be left out? How does the complexity of the narratives
in your data get reduced in calculating this
value.

``` r
Fill your response here. 
```

#### Select another numeric variable in your dataset and calculate both the mean and the median.

Be sure to filter your dataset to ensure that you are summarizing across
similar observations. Also be sure to avoid averaging an average.

``` r
#Fill your code here. 
```

What question might these measures of central tendency help to address?

``` r
Fill your response here. 
```

Create a frequency plot of the numeric variable you produced
calculations of above. (You may have created this last week, and you may
copy and paste it from the previous lab.)

``` r
#Create a frequency plot here. 
```

Characterize the distribution of values in the frequency plot you
created. Is it symmetrical or skewed?

``` r
Fill your response here. 
```

Which value is more appropriate as a measure of central tendency in your
data?

``` r
Fill your response here. 
```

What story do these calculations tell? Would you select the median or
the mean as a measure of central tendency? Why?

``` r
Fill your response here. 
```

Characterize the extent to which the value you selected as a measure of
central tendency above is representative of your data.

``` r
Fill your response here. 
```

If we were to use this single value to summarize this variable, what
narratives would be left out? How does the complexity of the narratives
in your data get reduced in calculating this value.

``` r
Fill your response here. 
```

As we’ve been discussing in class, any measure that we calculate is
mediated by the way the phenomena we are measuring gets defined. How
were the numeric variables that you selected defined? What was included
in these definitions, and what potentially relevant values were
excluded?

``` r
Fill your response here. 
```

## Measures of Dispersion

Measures of dispersion help us to understand how spread out the values
in our dataset are - their variations from each other and from the
data’s center. Like measures of central tendency, measures of
dispersion should be taken across a set of data with similar values.
This often means filtering our data to those that represent a similar
set of observations.

### Ranges

The range of data refers to the difference between the maximum value in
a set and the minimum value in a set. We can also use summarize() to
find the max value, min value, and range of values in a numeric
variable.

``` r
#df %>% summarize(max_value = max(NUMERIC_VARIABLE, na.rm = TRUE))
hospitals %>% 
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  summarize(max_value = max(BEDS, na.rm = TRUE))
```

    ##   max_value
    ## 1      1592

``` r
#df %>% summarize(min_value = min(NUMERIC_VARIABLE, na.rm = TRUE))
hospitals %>% 
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  summarize(min_value = min(BEDS, na.rm = TRUE))
```

    ##   min_value
    ## 1         2

``` r
#df %>% summarize(range = max(NUMERIC_VARIABLE, na.rm = TRUE) - min(NUMERIC_VARIABLE, na.rm = TRUE))
hospitals %>% 
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  summarize(range = max(BEDS, na.rm = TRUE) - min(BEDS, na.rm = TRUE))
```

    ##   range
    ## 1  1590

### Quartile Deviation

Even though we are introducing the boxplot in relation to quartile
deviation, it can be a tool for summarizing a number of quantitative
insights in relation to a numerical variable in our dataset. Boxplots
provide a visual representation of both measures of central tendency and
measures of dispersion. The center line in a boxplot indicates the
median of the dataset. The bottom of the box represents the 1st quartile
- the value in the middle of the minimum and the median (or the the
value at the 1st quarter position). The top of the box represents the
3rd quartile - the value in the middle of the median and the maximum (or
the value at the 3rd quarter position). The whiskers include almost all
of the data - indicating its range from minimum to maximum excluding
outliers. The dots represent outliers. ggplot has a calculation for
outliers that we need not go into in this course.

The further the 1st and 3rd quartile are from the median (or, in other
words, the wider the box), the greater the *quartile deviation.* In
general, a narrower box and whiskers indicates less dispersion in the
data, while a wider box and whiskers indicates greater dispersion.

Let’s use a boxplot to check out the quartile deviation of beds in open
general acute care hospitals in the US.

``` r
#df %>% ggplot(aes(y = NUMERIC_VARIABLE)) + geom_boxplot() + theme_bw()

hospitals %>%
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  ggplot(aes(y = BEDS)) +
  geom_boxplot() +
  labs(title = "Distribution of Beds across General Acute Care Hospitals in the US that are Open", y = "Beds") +
  theme_bw()
```

    ## Warning: Removed 181 rows containing non-finite values (stat_boxplot).

![](lab6_files/figure-gfm/unnamed-chunk-29-1.png)<!-- --> \> Note how we
can title a boxplot very similarly to how we title a frequency plot.
Both show distributions of values.

We can see from this plot that the quartile deviation is much smaller
than the range in our dataset. This suggests that centered values are
more concentrated than the extremes.

### Standard Deviation

*Standard deviation* calculates the extent of concentration of values
around the mean. A higher standard deviation indicates that values are
more dispersed from the mean, and a lower standard deviation indicates
that values are more concentrated around the mean.

``` r
#df %>% summarize(sd_value = sd(NUMERIC_VALUE, na.rm = TRUE))

hospitals %>% 
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  summarize(sd_value = sd(BEDS, na.rm = TRUE))
```

    ##   sd_value
    ## 1  194.511

### Measures of Dispersion in Your Own Dataset

#### Select one of the numeric variables on which you calculated a measure of central tendency above, and calculate the maximum value, the minimum value, and the range of values within that variable.

Be sure to filter your dataset to ensure that you are summarizing across
similar observations.

``` r
#Fill your code to calculate max here. 

#Fill your code to calculate min here. 

#Fill your code to calculate range here. 
```

What question might this measure of dispersion help to address?

``` r
Fill your response here. 
```

Do the numbers surprise you? What insight can you draw from this
calculation?

``` r
Fill your response here. 
```

#### Create a boxplot for one of the numeric variables on which you calculated a measure of central tendency above.

Be sure to filter your dataset to ensure that you are plotting across
similar observations.

``` r
#Fill your plot here. Be sure to add a title and labels to your plot. 
```

What question might this measure of dispersion help to address?

``` r
Fill your response here. 
```

What insight can you draw from the plot you
created?

``` r
Fill your response here. 
```

### Calculate the standard deviation for one of the numeric variables on which you calculated a measure of central tendency above.

Be sure to filter your dataset to ensure that you are summarizing across
similar observations.

``` r
#Fill your code here.
```

What question might this measure of dispersion help to address?

``` r
Fill your response here. 
```

What insight can you draw from this calculation?

``` r
Fill your response here. 
```

Now that you have a sense of the dispersion of some of the values in
your data, why do you believe the values are as concentrated or
dispersed as they are? What do the measures of dispersion you calculated
say about social, political, or economic life in the area the data is
representing? You may need to do some research to answer this question.
For instance, noting the dispersion of beds in the hospitals data, I may
do a Web search for “Why do some hospitals in the US have more beds than
others?” and find
[this](https://www.nytimes.com/interactive/2020/03/17/upshot/hospital-bed-shortages-coronavirus.html)
article. Be sure to assess the reputability of your source and cite it
in your response below.

``` r
Fill your response here. 
```

## Grouped Quantitative Insights

Like we learned in regards to co-variation, some of the most interesting
information we can gain from our data is how values change depending on
where in the data we are looking.

### Boxplots

ggplot’s geom\_boxplot feature is particularly good at comparing
measures of dispersion across grouped values in a categorical variable.
It is set up to visualize several boxplots side-by-side. When we do have
dissimilar observations in our dataset, this is a way to compare the
dispersion of values across them. Let’s take a look at how we could
compare dispersions in the number of hospital beds available by hospital
ownership.

``` r
#df %>% ggplot(aes(x = CATEGORICAL_VARIABLE, y = NUMERIC_VARIABLE)) + geom_boxplot()

hospitals %>%
  filter(STATUS == "OPEN") %>%
  ggplot(aes(x = TYPE, y = BEDS)) +
  geom_boxplot() +
  labs(title = "Distribution of Beds across Hospitals in the US that are Open by Type", x = "Type", Y = "Beds") +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 90, hjust=1)) + #Changes x-axis tick labels 90 degrees
  coord_flip() #Flips the x and y axis to make the data easier to read and compare
```

    ## Warning: Removed 570 rows containing non-finite values (stat_boxplot).

![](lab6_files/figure-gfm/unnamed-chunk-41-1.png)<!-- --> We can also
filter our data to similar values and the compare dispersions across
another
category.

``` r
#df %>% ggplot(aes(x = CATEGORICAL_VARIABLE, y = NUMERIC_VARIABLE)) + geom_boxplot()

hospitals %>%
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  ggplot(aes(x = OWNER, y = BEDS)) +
  geom_boxplot() +
  labs(title = "Distribution of Beds across Hospitals in the US that are Open by Type", x = "OWNER", Y = "Beds") +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 90, hjust=1)) + #Changes x-axis tick labels 90 degrees
  coord_flip() #Flips the x and y axis to make the data easier to read and compare
```

    ## Warning: Removed 181 rows containing non-finite values (stat_boxplot).

![](lab6_files/figure-gfm/unnamed-chunk-42-1.png)<!-- -->

We see above that there is a greater dispersion in the number of beds
available at state hospitals and non-profit hospitals. We also see that,
in local hospitals, the median is closer to the first quartile than in
other plots, and there are a number of outliers, indicating that the
data is going to be more right skewed than in other plots right-skewed.
Compare this to federal hospitals, where the median is more centered in
the box (and there are few outliers). We are likely to see a less skewed
distribution in this plot. Let’s confirm this.

``` r
hospitals %>%
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE" & OWNER == "GOVERNMENT - LOCAL") %>%
  ggplot(aes(x = BEDS)) +
  geom_freqpoly(binwidth = 25) +
  labs(title = "Distribution of Beds across Open General Acute Care Local Hospitals in the US", x = "Beds", y = "Count of Hospitals") + # To add titles and labels
  theme_bw()
```

![](lab6_files/figure-gfm/unnamed-chunk-43-1.png)<!-- -->

``` r
hospitals %>%
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE" & OWNER == "GOVERNMENT - FEDERAL") %>%
  ggplot(aes(x = BEDS)) +
  geom_freqpoly(binwidth = 25) +
  labs(title = "Distribution of Beds across Open General Acute Care Federal Hospitals in the US", x = "Beds", y = "Count of Hospitals") + # To add titles and labels
  theme_bw()
```

    ## Warning: Removed 12 rows containing non-finite values (stat_bin).

![](lab6_files/figure-gfm/unnamed-chunk-43-2.png)<!-- --> We can see
above how the distribution in the second plot is a bit more normal than
the distribution in the first, confirming what we concluded above.

### Create a grouped boxplot for your dataset below.

Be sure to filter your dataset to ensure that you are summarizing across
similar observations **or** ensure that each group is made up of similar
observations.

``` r
#Fill your code here. Add a title and labels to your plot. 
```

Summarize what you learn from the plot.

``` r
Fill your response here. 
```

How does the story that this grouped boxplot tells differ from the story
told by the single boxplot you created above? Interpret why the story
differs.

``` r
Fill your response here. 
```

## Continue your shiny app.

Now we will incorporate some of the values we calculated and some of the
plots we produced above into the Shiny App. In the server function,
follow the instructions in the comments to create three value boxes in
your app. You will also place two more plots in the app. Don’t worry
about styling the app. That will be the focus of a later lab.

``` r
ui <- dashboardPage(
  
  dashboardHeader(title = "TITLE HERE"),
  
  dashboardSidebar(
    #EVENTUALLY INPUTS WILL GO HERE. 
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

``` r
server <- function(input, output, session) {
  
  output$value1 <- renderInfoBox({
    quant_insight1 <- 0
    #Replace '0' above with the code for one of the values you calculated above. Replace 'FILL DESCRIPTION HERE' with a brief description of this number.  
    infoBox(quant_insight1,'FILL DESCRIPTION HERE', icon = icon("stats", lib='glyphicon'), color = "purple")
  })
  
  output$value2 <- renderInfoBox({
    quant_insight2 <- 0
    #Replace '0' above with the code for one of the values you calculated above. Replace 'FILL DESCRIPTION HERE' with a brief description of this number.  
    infoBox(quant_insight2,'FILL DESCRIPTION HERE', icon = icon("stats", lib='glyphicon'), color = "purple")
  })
  
  output$value3 <- renderInfoBox({
    quant_insight3 <- 0
    #Replace '0' above with the code for one of the values you calculated above. Replace 'FILL DESCRIPTION HERE' with a brief description of this number.  
    infoBox(quant_insight3,'FILL DESCRIPTION HERE', icon = icon("stats", lib='glyphicon'), color = "purple")
  })
  
  
  output$plot1 <- renderPlot({
    hospitals %>% ggplot(aes(x = TYPE)) + geom_bar()
    #Replace plot above with your own plot. 
    
  })
  
  output$plot2 <- renderPlot({
    hospitals %>% ggplot(aes(x = TYPE)) + geom_bar()
    #Replace plot above with your own plot. 
  })
  
  output$plot3 <- renderPlot({
    hospitals %>% ggplot(aes(x = TYPE)) + geom_bar()
    #Replace plot above with your own plot. 
    
  })
  
  output$plot4 <- renderPlot({
    hospitals %>% ggplot(aes(x = TYPE)) + geom_bar()
    #Replace plot above with your own plot. 
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

## More examples and Useful Functions (This is only for reference.)

### stat\_summary()

Sometimes, we also want to graphically represent a calculation that we
perform for a grouped categorical variable. For instance, let’s say I
wanted to show the median number of beds available in open general acute
care hospitals grouped by the hospital owner. To do this, I need to
calculate the median of the BEDS variable for each OWNER. One way to do
this is to group\_by(OWNER), summarize the median of BEDS, and then plot
the resulting data frame. We discussed how to group\_by() and
summarize() in lab 4. A ggplot call can be appended to these functions
with a pipe (%\>%) to plot them. However, here we can also use
stat\_summary(), in conjunction with a function. We will plot the median
as a column plot by setting the geom = “col”. Again, to ensure that we
are calculating across similar observations, we will filter our data to
open general acute care
hospitals.

``` r
#df %>% ggplot(aes(x = CATEGORICAL_VARIABLE, y = NUMERIC_VARIABLE)) + stat_summary(geom = "col", fun.y = "median")

hospitals %>%
  filter(STATUS == "OPEN" & TYPE == "GENERAL ACUTE CARE") %>%
  ggplot(aes(x = OWNER, y = BEDS)) +
  stat_summary(geom = "col", fun.y = "median") +
  theme_bw() + # To change the plot theme
  theme(axis.text.x = element_text(angle = 90, hjust=1)) + # To turn x-axis ticks 90 degrees
  scale_y_continuous(labels = scales::comma)  # To set x-axis ticks to comma notation
```

    ## Warning: Removed 181 rows containing non-finite values (stat_summary).

![](lab6_files/figure-gfm/unnamed-chunk-50-1.png)<!-- -->

We can apply functions other than median in stat\_summary(). Let’s
graphically represent the total number of beds for each owner.

``` r
hospitals %>%
  ggplot(aes(x = OWNER, y = BEDS)) +
  stat_summary(geom = "col", fun.y = "sum") + #fun.y tells ggplot to perform this function on the y-variable
  theme_bw() + # To change the plot theme
  theme(axis.text.x = element_text(angle = 90, hjust=1)) + # To turn x-axis ticks 90 degrees
  scale_y_continuous(labels = scales::comma)  # To set x-axis ticks to comma notation
```

    ## Warning: Removed 662 rows containing non-finite values (stat_summary).

![](lab6_files/figure-gfm/unnamed-chunk-51-1.png)<!-- -->

I can group these calculations further using the *fill* aesthetic. Below
we create three plots by setting the fill aesthetic to the hospitals’
TYPE. Depending on the *position* attribute, we can create either a
stacked column plot, a dodged column plot, or a filled column plot
(which shows proportions of totals versus actual totals. )

``` r
#Stacked column plot of the total hospital beds by owner and type
hospitals %>%
  ggplot(aes(x = OWNER, y = BEDS, fill = TYPE)) +
  stat_summary(geom = "col", fun.y = "sum") +
  theme_bw() + # To change the plot theme
  theme(axis.text.x = element_text(angle = 90, hjust=1)) + # To turn x-axis ticks 90 degrees
  scale_y_continuous(labels = scales::comma)  # To set x-axis ticks to comma notation
```

    ## Warning: Removed 662 rows containing non-finite values (stat_summary).

![](lab6_files/figure-gfm/unnamed-chunk-52-1.png)<!-- -->

``` r
#Dodged column plot of the total hospital beds by owner and type
hospitals %>%
  ggplot(aes(x = OWNER, y = BEDS, fill = TYPE)) +
  stat_summary(geom = "col", fun.y = "sum", position = "dodge") +
  theme_bw() + # To change the plot theme
  theme(axis.text.x = element_text(angle = 90, hjust=1)) + # To turn x-axis ticks 90 degrees
  scale_y_continuous(labels = scales::comma)  # To set x-axis ticks to comma notation
```

    ## Warning: Removed 662 rows containing non-finite values (stat_summary).

![](lab6_files/figure-gfm/unnamed-chunk-52-2.png)<!-- -->

``` r
#Filled column plot of the total hospital beds by owner and type
hospitals %>%
  ggplot(aes(x = OWNER, y = BEDS, fill = TYPE)) +
  stat_summary(geom = "col", fun.y = "sum", position = "fill") +
  theme_bw() + # To change the plot theme
  theme(axis.text.x = element_text(angle = 90, hjust=1)) + # To turn x-axis ticks 90 degrees
  scale_y_continuous(labels = scales::comma)  # To set x-axis ticks to comma notation
```

    ## Warning: Removed 662 rows containing non-finite values (stat_summary).

![](lab6_files/figure-gfm/unnamed-chunk-52-3.png)<!-- -->
