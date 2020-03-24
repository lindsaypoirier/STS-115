Lab 4 - Data Cleaning and Exploration
================

  - [Instructions and Overview](#instructions-and-overview)
  - [Data Download](#data-download)
  - [Data Formatting and Importing](#data-formatting-and-importing)
  - [Data Cleaning](#data-cleaning)
  - [Data Exploration](#data-exploration)
  - [Other Useful Functions (This is only for
    reference.)](#other-useful-functions-this-is-only-for-reference.)

## Instructions and Overview

For this assignment, you will select one dataset from your dataset
hopping lab to import into R, clean, and examine further. Be sure to
fill in the blanks in the document as you
    go.

``` r
library(tidyverse)
```

    ## ── Attaching packages ───────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.0     ✔ purrr   0.3.3
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   0.8.3     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ──────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
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

## Data Download

First, we will need to determine if we should filter your data to
relevant features before downloading it. For some students this will be
possible prior to downloading it. For others, it won’t. In either case,
please fill out the questions
below.

### What timespan represented in the data is relevant to your research questions?

``` r
Fill your response here. 
```

### What geography represented in the data is relevant to your research questions?

``` r
Fill your response here. 
```

### What specific categories in the data are relevant to your research questions?

``` r
Fill your response here. 
```

### Where is the data stored, and how can you download it?

``` r
Fill your response here. 
```

Do you need to/ can you filter the data to the relevant
timeframe/geography/category prior to downloading it? If so, do the
following:

> You may need to create an account in the data portal in order to
> filter the data prior to download.

1.  Create an account.
2.  Filter and Save the data.
3.  Download the filtered file as a csv.

Otherwise, just go ahead and download the data. Move the data to
Documents \> Github \> STS115\_Course\_Project \> datasets.

## Data Formatting and Importing

### In what format is the data stored?

  - If it is stored as an Excel file:
    1.  Download the data, and open it in Excel.
    2.  Ensure that variable names are in the first row of the sheet,
        and all observations are stored in the following rows. Remove
        any extra headings or instructions.
    3.  Click File\>Save As, and Save the file as a .csv. You will be
        prompted to acknowledge that you are only saving one sheet.
        Click OK.

> It is possible to read Excel files into R. However, to reduce
> headaches, I’m going to encourage that everyone put their data in a
> .csv format before importing.

  - If it is stored as a .csv file, you are all set to move
forward.

<!-- end list -->

``` r
# Uncomment the line below. Create an appropriate variable name for your data frame, and fill the file name below to import the data into RStudio

setwd("/Users/lpoirier/Documents/GitHub/STS-115")
hospitals <- read.csv("datasets/Hospitals.csv", stringsAsFactors = FALSE)

# _____ <- read.csv("______", stringsAsFactors=FALSE)
```

### Is the data stored in multiple files?

If not, skip to the next heading.

First, ensure that all files have the same headings. You may open the
.csv files in Excel to examine this.

Read each file into a data frame. Be sure to provide a different
variable name for each data
frame.

``` r
# Uncomment the line below. Create an appropriate variable name for your dataframe, and fill the file name below to import the data into RStudio
# _____ <- read.csv("______", stringsAsFactors=FALSE)
# _____ <- read.csv("______", stringsAsFactors=FALSE)
# _____ <- read.csv("______", stringsAsFactors=FALSE)
# Continue adding this line for each file you need to import. 
```

What makes each data frame unique? Are they separated by geography? By
year? Add a column to each data frame to designate what makes it unique
from the others, using the mutate
function.

``` r
#Uncomment the last line below and follow the format below with your data frame. 

#Example, if by year:
#df <- df %>% mutate(year = 2019)
#Note that 'year' will be the new column name, and the values for that column will be filled with 2019

#Example, if by geography:
#df <- df %>% mutate(geography = "California")

# Remove the quotations below, if the value should be numeric instead of a character. 
#_____ <- _____ %>% mutate(_____ = "_____")
```

Check if column names are the same across data
frames.

``` r
#Uncomment the last line, and check to make sure the column names in each data frame are the same by filling in your data frame variables

#all(colnames(df1) == colnames(df2))
#all(colnames(_____) == colnames(_____))
```

Bind the data frames.

``` r
#Uncomment the last line, and fill your data frame variable names. 

#df_final <- rbind(df1, df2, df3)
#_____ <- rbind(_____, _____, _____)
```

## Data Cleaning

### What is the structure of your dataset?

str() gives us an overview of the structure of the dataset, including
the number of observations, the variable names, and the format of each
variable. For instance, check out how we would run str() on the
hospitals dataset.

``` r
str(hospitals)
```

    ## 'data.frame':    7581 obs. of  34 variables:
    ##  $ X         : num  -115 -117 -117 -117 -117 ...
    ##  $ Y         : num  34.8 32.8 32.8 32.8 32.7 ...
    ##  $ OBJECTID  : int  1001 1002 1003 1004 1005 1006 1007 1008 1009 1010 ...
    ##  $ ID        : int  7892363 1392120 1591942 23292104 34392050 25094116 25294143 50694122 50794115 7490650 ...
    ##  $ NAME      : chr  "COLORADO RIVER MEDICAL CENTER" "ALVARADO HOSPITAL MEDICAL CENTER" "ALVARADO PARKWAY INSTITUTE B.H.S." "KINDRED HOSPITAL - SAN DIEGO" ...
    ##  $ ADDRESS   : chr  "1401 BAILEY AVENUE" "6655 ALVARADO ROAD" "7050 PARKWAY DRIVE" "1940 EL CAJON BOULEVARD" ...
    ##  $ CITY      : chr  "NEEDLES" "SAN DIEGO" "LA MESA" "SAN DIEGO" ...
    ##  $ STATE     : chr  "CA" "CA" "CA" "CA" ...
    ##  $ ZIP       : int  92363 92120 91942 92104 92050 94116 94143 94122 94115 90650 ...
    ##  $ ZIP4      : chr  "NOT AVAILABLE" "NOT AVAILABLE" "NOT AVAILABLE" "NOT AVAILABLE" ...
    ##  $ TELEPHONE : chr  "NOT AVAILABLE" "NOT AVAILABLE" "NOT AVAILABLE" "NOT AVAILABLE" ...
    ##  $ TYPE      : chr  "GENERAL ACUTE CARE" "GENERAL ACUTE CARE" "PSYCHIATRIC" "GENERAL ACUTE CARE" ...
    ##  $ STATUS    : chr  "OPEN" "OPEN" "OPEN" "OPEN" ...
    ##  $ POPULATION: int  25 226 66 70 227 780 67 580 140 117 ...
    ##  $ COUNTY    : chr  "SAN BERNARDINO" "SAN DIEGO" "SAN DIEGO" "SAN DIEGO" ...
    ##  $ COUNTYFIPS: chr  "06071" "06073" "06073" "06073" ...
    ##  $ COUNTRY   : chr  "USA" "USA" "USA" "USA" ...
    ##  $ LATITUDE  : num  34.8 32.8 32.8 32.8 32.7 ...
    ##  $ LONGITUDE : num  -115 -117 -117 -117 -117 ...
    ##  $ NAICS_CODE: int  622110 622110 622210 622110 622110 622110 622210 622110 622110 622110 ...
    ##  $ NAICS_DESC: chr  "GENERAL MEDICAL AND SURGICAL HOSPITALS" "GENERAL MEDICAL AND SURGICAL HOSPITALS" "PSYCHIATRIC AND SUBSTANCE ABUSE HOSPITALS" "GENERAL MEDICAL AND SURGICAL HOSPITALS" ...
    ##  $ SOURCE    : chr  "http://www.oshpd.ca.gov/HID/Facility-Listing.html" "http://www.oshpd.ca.gov/HID/Facility-Listing.html" "http://www.oshpd.ca.gov/HID/Facility-Listing.html" "http://www.oshpd.ca.gov/HID/Facility-Listing.html" ...
    ##  $ SOURCEDATE: chr  "2018-08-08T00:00:00.000Z" "2018-08-08T00:00:00.000Z" "2018-08-08T00:00:00.000Z" "2018-08-08T00:00:00.000Z" ...
    ##  $ VAL_METHOD: chr  "IMAGERY/OTHER" "IMAGERY/OTHER" "IMAGERY/OTHER" "IMAGERY/OTHER" ...
    ##  $ VAL_DATE  : chr  "2014-02-10T00:00:00.000Z" "2014-02-10T00:00:00.000Z" "2014-02-10T00:00:00.000Z" "2014-02-10T00:00:00.000Z" ...
    ##  $ WEBSITE   : chr  "http://www.cityofneedles.com/Hospitals.asp" "http://www.alvaradohospital.com" "http://www.apibhs.com" "http://www.kindredsandiego.com" ...
    ##  $ STATE_ID  : chr  "NOT AVAILABLE" "NOT AVAILABLE" "NOT AVAILABLE" "NOT AVAILABLE" ...
    ##  $ ALT_NAME  : chr  "NOT AVAILABLE" "NOT AVAILABLE" "NOT AVAILABLE" "NOT AVAILABLE" ...
    ##  $ ST_FIPS   : int  6 6 6 6 6 6 6 6 6 6 ...
    ##  $ OWNER     : chr  "GOVERNMENT - LOCAL" "PROPRIETARY" "PROPRIETARY" "PROPRIETARY" ...
    ##  $ TTL_STAFF : int  -999 -999 -999 -999 -999 -999 -999 -999 -999 -999 ...
    ##  $ BEDS      : int  25 226 66 70 227 780 67 580 140 117 ...
    ##  $ TRAUMA    : chr  "NOT AVAILABLE" "NOT AVAILABLE" "NOT AVAILABLE" "NOT AVAILABLE" ...
    ##  $ HELIPAD   : chr  "Y" "N" "N" "N" ...

Run this function for your own dataset.

``` r
#Uncomment the last line, and fill your data frame name.

#str(df)

#str(_____)
```

From running the code above, we can see that hospitals has 7581 rows in
the dataset. How many rows are in your dataset?

``` r
Fill your response here. 
```

There was only one variable in the hospitals dataset that we may want to
convert to a different type. At first glance you may think its
COUNTY\_FIPS, which loaded as a char even though it looks like a number.
We in fact want to keep this a char. County FIPS IDs have two parts -
the first two digits represent a census-standardized state code and the
second three digits represent a census-standardized county code. Just
like we expect 5-digits in a postal code, systems that reference this
number expect a certain number of digits. However, state codes start at
1 and go up through 50+, as do county codes. To ensure that we always
have five digits in the COUNTY FIPS, we need to ensure that leading
zeros do not get stripped when we import our data. This would happen if
the data imported a number, but won’t happen if it gets imported as a
char. With this in mind, we in fact going to want to transform ZIP into
a char and add leading zeros. We will do this in a later step. See the
code below to confirm why:

``` r
hospitals %>%
  filter(ZIP < 10000) %>%
  select(NAME, ZIP) %>%
  head(10)
```

    ##                                        NAME  ZIP
    ## 1                          GAYLORD HOSPITAL 6492
    ## 2              HEBREW REHABILITATION CENTER 2131
    ## 3                   LEMUEL SATTUCK HOSPITAL 2130
    ## 4    HEBREW REHABILITATION CENTER AT DEDHAM 2026
    ## 5  WESTWOOD/PEMBROKE HEALTH SYSTEM PEMBROKE 2359
    ## 6               WALDEN BEHAVIORAL CARE, LLC 2453
    ## 7                NEW ENGLAND SINAI HOSPITAL 2072
    ## 8        ST JOSEPH HOSPITAL MILFORD MED CTR 3055
    ## 9    HOSP UNIVERSITARIO DR RAMON RUIZ ARNAU  956
    ## 10           PUERTO RICO CHILDRENS HOSPITAL  960

See how the ZIP codes above are not five digits? This is because they
imported as numbers and their leading zeros were stripped.

Check out the class of each variable (e.g. int, chr, num, logi) in your
dataset. Are they all the correct type? List any variables that are not
the correct
type.

``` r
Fill your response here. 
```

### Do you have any variables in your dataset that should be numeric but are currently of type char?

If not, skip to the next heading.

This often happens when there are characters like commas, dollar signs,
percent signs in the column. We need to strip these characters before
converting the variable to
numeric.

``` r
#Uncomment the last line, and fill your data frame name and the variable name you'd like to check. Copy and paste this line for each variable you need to check and fill accordingly. 

#View(df$VARIABLE_NAME)
#View(_____$_____)
```

Let’s remove the characters that are appearing in this column with the
gsub function, which replaces a character with another character - in
this case with
nothing.

``` r
#Uncomment the last line, and fill the unwanted character, your data frame name, and the variable name. Copy and paste this line for each variable for which you need to substitute a character and fill accordingly. 

#df$VARIABLE_NAME <- gsub("UNWANTED CHARACTER", "", df$VARIABLE_NAME)
#_____$_____ <- gsub("_____", "", _____$_____)
```

### Do you need to change the type of any variables (including the char to numeric conversion you prepared for above)?

If not, skip to the next heading.

Let’s go ahead and convert that numeric ZIP variable into a char.

``` r
hospitals$ZIP <- as.character(hospitals$ZIP) 
```

Following the same pattern, change the type of any incorrectly typed
variables in your own
dataset.

``` r
#Uncomment the last line, and fill the appropriate conversion type, your data frame name, and the variable name. Copy and paste this line for each variable that you need to convert to a different type and fill accordingly.

# df$VARIABLE_NAME <- as.numeric(df$VARIABLE_NAME) 

# Fill with one of the following: as.numeric, as.character, as.logical)

# _____$_____ <- _____(_____$______)
```

This will overwrite the values in that variable with the same values but
in the correct format. If you have many, many columns that need to be
converted, then we want to apply sapply here, which will perform a
function for each value in a vector. Otherwise, skip the block
below.

``` r
#Uncomment the last line, and fill the appropriate data frame name and variable ranges. 

#sapply(df[,[COLUMN_RANGE1:COLUMN_RANGE2]] function(x) as.numeric(x))
#sapply(_____[,[_____:_____]] function(x) as.numeric(x))
```

### Do you need to add leading zeros to any values in your data?

As described above, sometimes we need characters to values in our
dataset for that value to be an exact number of digits. For instance,
ZIP codes should be 5-digits long regardless of whether they start with
the number 0. After we’ve converted such numeric values to a character,
we can pad the front of the string with a certain character until the
string is the required length. For the hospitals dataset, we will need
to add leading zeros to the ZIP codes we just converted to characters
until they are all 5 characters in length. We can use str\_pad() to do
this.

``` r
hospitals$ZIP <- str_pad(hospitals$ZIP, 5, pad = "0") 
```

If needed, do the same to a variable in your own
dataset.

``` r
#Uncomment the last line, and fill the appropriate data frame, variable, and desired number of digits. 

#df$VARIABLE_NAME <- str_pad(df$VARIABLE_NAME, [number of digits], pad = "0") 
```

### How are Null values represented in your dataset?

``` r
#Uncomment the last line, and fill in your data frame name to view the first ten rows of the data frame.

#View(df[1:10,]) 

#View(_____[1:10,]) 
```

Null values should appear as a greyed-out and italicized *NA*. This
communicates to R that this is an empty field or that there is not data
here. However, if not properly formatted, you may see Null values appear
as: \* “NULL” \* empty strings ("“) \*”NONE" \* “NOT AVAILABLE”

In the hospitals dataset, we can see by calling View() that empty data
is filled with the string “NOT AVAILABLE”. We want to convert such
values to NA values.

``` r
is.na(hospitals) <- hospitals == "NOT AVAILABLE"
```

Where appropriate, convert values to NA in your own
dataset.

``` r
#Uncomment the last line, and fill in your data frame name to view the first ten rows of the data frame.

#is.na(df) <- df == "unwanted string"  

#For example, if "NULL" appears in your dataset:
#is.na(df) <- df == "NULL"

#is.na(_____) <- _____ == "_____" 
```

### Do you have any dates in your dataset? (By this I mean specific dates, not just months or just years).

If not, skip to the next heading.

Dates can be converted to a date format using the lubridate package.
This is a package in the Tidyverse that makes it possible to extract
specific information (such as month or year) from dates, and to compute
with dates.

The hospitals dataset has two date variables: SOURCEDATE and VAL\_DATE.
On import, they both have the following format: yyyy-mm-ddThh:mm:ss.000Z
Because the format is in the year-month-day hour:minute:second format,
we will call ymd\_hms() on the variable.

``` r
hospitals$SOURCEDATE <- ymd_hms(hospitals$SOURCEDATE)
hospitals$VAL_DATE <- ymd_hms(hospitals$VAL_DATE)
```

If they were instead listed in the month-day-year hour:minute:second
format, we would instead call mdy\_hms() on the variable.

If they were just in the year-month-day format, we would instead call
ymd() on the variable.

Check out the format of your date. Is it just a year? Just a month? A
year, month, and day? Are there times listed? What order are each of
these values listed in? [This link](https://lubridate.tidyverse.org/)
will help you determine how to fill in the blank
below.

``` r
#Uncomment the last line, and fill the appropriate data frame name, variable name, and date format. 

#df$VARIABLE_NAME <- date_format(df$VARIABLE_NAME)

#For example, if the date is in month day, year (March 1, 1999) format:
#df$VARIABLE_NAME <- mdy(df$VARIABLE_NAME) 

#_____$_____ <- _____(_____$_____)   
```

### Do you foresee any other issues to working with your dataset?

``` r
Fill your response here. 
```

-----

## Data Exploration

At this point in the assignment, we will begin exploring and getting to
know your data. We will be learning a number of functions that are made
available through dplyr - a package in the Tidyverse that enables us to
manipulate and transform data. The five primary functions we will be
working with through dplyr include:

  - select() : select variables
  - filter() : return only observations that meet a particular criteria
  - count() : count how many times each value appears in a variable
  - group\_by() : group observations according to a common value
  - summarize() : perform an operation and return a single value

### What kinds of variables are in the dataset?

> Note that you may not be able to list three of each below.

*Nominal categorical variables* are variables that identify something
else. They name or categorize something that exists in the world.
Sometimes, nominal categorical variables are obvious. For instance, in
the hospitals dataset, the hospital NAME is a nominal categorical
variable - referring to the actual hospital. CITY is also a nomical
categorical variable - referring to the hospital’s city. The hospital
TYPE and OWNER are all categorical variable - referring to specific
categories the hospital is classed within. However, nominal categorical
variables are not always strings. *Sometimes, numbers are considered
nominal categorical variables.* For instance, a ZIP code is not a value
that we operate on but instead refers to a certain place; it is a
nominal categorical variable. In the hospitals dataset, the NAICS\_CODE
is a numeric reference to a particular industry classification; it is
also a nominal categorical variable. Both OBJECTID and ID are nominal
categorical variables referring to the hospital.

List three nominal categorical variables in your dataset. Use *select()*
to select these variables in your dataset, and use *head(10)* to limit
the display to the first 10
rows.

``` r
#Uncomment the last line, and fill the appropriate data frame name and variable names.

#df %>% select(VARIABLE_NAME1, VARIABLE_NAME2, VARIABLE_NAME3) %>% head(10)

#Here are just a few of the nominal categorical variables in the hospitals dataset
hospitals %>% select(OBJECTID, ID, NAME, COUNTY, NAICS_CODE, TYPE) %>% head(10)
```

    ##    OBJECTID       ID                                            NAME
    ## 1      1001  7892363                   COLORADO RIVER MEDICAL CENTER
    ## 2      1002  1392120                ALVARADO HOSPITAL MEDICAL CENTER
    ## 3      1003  1591942               ALVARADO PARKWAY INSTITUTE B.H.S.
    ## 4      1004 23292104                    KINDRED HOSPITAL - SAN DIEGO
    ## 5      1005 34392050                        PARADISE VALLEY HOSPITAL
    ## 6      1006 25094116 LAGUNA HONDA HOSPITAL AND REHABILITATION CENTER
    ## 7      1007 25294143            LANGLEY PORTER PSYCHIATRIC INSTITUTE
    ## 8      1008 50694122                             UCSF MEDICAL CENTER
    ## 9      1009 50794115               UCSF MEDICAL CENTER AT MOUNT ZION
    ## 10     1010  7490650                    COAST PLAZA DOCTORS HOSPITAL
    ##            COUNTY NAICS_CODE               TYPE
    ## 1  SAN BERNARDINO     622110 GENERAL ACUTE CARE
    ## 2       SAN DIEGO     622110 GENERAL ACUTE CARE
    ## 3       SAN DIEGO     622210        PSYCHIATRIC
    ## 4       SAN DIEGO     622110 GENERAL ACUTE CARE
    ## 5       SAN DIEGO     622110 GENERAL ACUTE CARE
    ## 6   SAN FRANCISCO     622110 GENERAL ACUTE CARE
    ## 7   SAN FRANCISCO     622210        PSYCHIATRIC
    ## 8   SAN FRANCISCO     622110 GENERAL ACUTE CARE
    ## 9   SAN FRANCISCO     622110 GENERAL ACUTE CARE
    ## 10    LOS ANGELES     622110 GENERAL ACUTE CARE

``` r
#_____ %>% select(_____, _____, _____) %>% head(10)
```

*Ordinal categorical variables* are categorical variables that can be
ranked or placed in a particular order. For instance, ‘High’, ‘Medium’,
and ‘Low’ have a particular order. In the hospitals dataset, there is
one ordinal categorical variable - TRAUMA, which characterizes the
hospital’s trauma level designation. Trauma level designations indicate
the extent of resources available at a hospital to deal with certain
categories of trauma. It is most often broken into Level I through Level
V. We can see how a data analyst may want to place trauma categories in
a particular order (for instance, ordering hospitals from highest to
lowest trauma levels). However, this is a particularly complictated
categorical variable to work with. This is because Trauma levels are not
defined according to a national standard. Instead, they are defined on a
state-by-state basis, and our dataset spans all US states. Level II in
one state might mean something different than Level II in another state
despite both being labeled Level II in the dataset. Further, a single
hospital can have multiple trauma levels (e.g. Level I Pediatric and
Level II Adult). We would need to take all of this into consideration
when comparing trauma levels across hospitals on a national scale.

List three ordinal categorical variables in your dataset. Use *select()*
to select these variables in your dataset, and use *head(10)* to limit
the display to the first 10
rows.

``` r
#Uncomment the last line, and fill the appropriate data frame name and variable names.

#df %>% select(VARIABLE_NAME1, VARIABLE_NAME2, VARIABLE_NAME3) %>% head(10)

#Here is the only ordinal categorical variable in the hospitals dataset
hospitals %>% select(TRAUMA) %>% head(10)
```

    ##    TRAUMA
    ## 1    <NA>
    ## 2    <NA>
    ## 3    <NA>
    ## 4    <NA>
    ## 5    <NA>
    ## 6    <NA>
    ## 7    <NA>
    ## 8    <NA>
    ## 9    <NA>
    ## 10   <NA>

``` r
#_____ %>% select(_____, _____, _____) %>% head(10)
```

*Discrete numeric variables* are numeric variables that represent
something that is countable - the number of students in a classroom, the
number pages in a book, the number of beds in a hospital. In the
hospitals dataset, POPULATION, BEDS, and assumably TTL\_STAFF (though
it’s all empty in our dataset), are all discrete numeric variables
because they represent things that have been counted.

List three discrete numerical variables in your dataset. Use *select()*
to select these variables in your dataset, and use *head(10)* to limit
the display to the first 10
rows.

``` r
#Uncomment the last line, and fill the appropriate data frame name and variable names.

#df %>% select(VARIABLE_NAME1, VARIABLE_NAME2, VARIABLE_NAME3) %>% head(10)

#Here are the discrete numeric variables in the hospitals dataset
hospitals %>% select(POPULATION, BEDS, TTL_STAFF) %>% head(10)
```

    ##    POPULATION BEDS TTL_STAFF
    ## 1          25   25      -999
    ## 2         226  226      -999
    ## 3          66   66      -999
    ## 4          70   70      -999
    ## 5         227  227      -999
    ## 6         780  780      -999
    ## 7          67   67      -999
    ## 8         580  580      -999
    ## 9         140  140      -999
    ## 10        117  117      -999

``` r
#_____ %>% select(_____, _____, _____) %>% head(10)
```

*Continuous numeric variables* are variables that would take an infinite
amount of time to precisely count. You can think of these as variables
in which it is always possible to measure the value more precisely. For
instance, time would be considered a continuous numeric variable because
time can be measured with infinite amount of specificity - hours \>
minutes \> seconds \> milliseconds \> microseconds \> nanoseconds … and
so on. Ruler measurements are also continuous because they can also be
measured with infinite more precision. In the hospitals dataset, both
latitude and longitude are continuous numeric variables as we can always
measure them with more precision.

List three continuous numeric variables in your dataset. Use *select()*
to select these variables in your dataset, and use *head(10)* to limit
the display to the first 10
rows.

``` r
#Uncomment the last line, and fill the appropriate data frame name and variable names.

#df %>% select(VARIABLE_NAME1, VARIABLE_NAME2, VARIABLE_NAME3) %>% head(10)

#Here are the continuous numeric variables in the hospitals dataset
hospitals %>% select(LATITUDE, LONGITUDE) %>% head(10)
```

    ##    LATITUDE LONGITUDE
    ## 1  34.83307 -114.6175
    ## 2  32.77659 -117.0571
    ## 3  32.77463 -117.0442
    ## 4  32.75638 -117.1446
    ## 5  32.68534 -117.0823
    ## 6  37.77436 -122.4259
    ## 7  37.76347 -122.4569
    ## 8  37.76308 -122.4579
    ## 9  37.78492 -122.4391
    ## 10 33.91269 -118.0988

``` r
#_____ %>% select(_____, _____, _____) %>% head(10)
```

> This section of the assignment is going to ask you to draw insights
> about your dataset after calling certain functions. When I ask you to
> draw an insight, I’m not asking you to describe what the function does
> or to state the results that you get. Instead I’m asking you to
> interpret those results and consider what this might tell us about the
> issues represented in the dataset or if it might signal issues of data
> quality. For instance, stating “the maximum value in the age column is
> 999,” is not an insight. Instead you should say, “the maximum value in
> the age column is 999, which is much higher than I would expect and
> may signal that the data was input wrong or that the data collectors
> at using 999 to represent null values.”

### Summary

Calling summary() on a data frame returns summary statistics for each of
the variables in that data frame. For numeric values, it returns the
min, max, median, 1st and 3rd quartiles, mean, and number of NAs. We
will go over each of these values more in two weeks.

``` r
#df %>% summary()
hospitals %>% summary()
```

    ##        X                 Y             OBJECTID          ID           
    ##  Min.   :-176.64   Min.   :-14.29   Min.   :   1   Min.   :        4  
    ##  1st Qu.: -98.20   1st Qu.: 33.46   1st Qu.:1896   1st Qu.:  3983422  
    ##  Median : -90.07   Median : 37.97   Median :3791   Median :  9656520  
    ##  Mean   : -92.36   Mean   : 37.33   Mean   :3791   Mean   : 25587207  
    ##  3rd Qu.: -81.77   3rd Qu.: 41.32   3rd Qu.:5686   3rd Qu.: 22634748  
    ##  Max.   : 145.72   Max.   : 71.29   Max.   :7581   Max.   :182120889  
    ##      NAME             ADDRESS              CITY          
    ##  Length:7581        Length:7581        Length:7581       
    ##  Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character  
    ##                                                          
    ##                                                          
    ##                                                          
    ##     STATE               ZIP                ZIP4          
    ##  Length:7581        Length:7581        Length:7581       
    ##  Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character  
    ##                                                          
    ##                                                          
    ##                                                          
    ##   TELEPHONE             TYPE              STATUS         
    ##  Length:7581        Length:7581        Length:7581       
    ##  Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character  
    ##                                                          
    ##                                                          
    ##                                                          
    ##    POPULATION         COUNTY           COUNTYFIPS       
    ##  Min.   :-999.00   Length:7581        Length:7581       
    ##  1st Qu.:  25.00   Class :character   Class :character  
    ##  Median :  65.00   Mode  :character   Mode  :character  
    ##  Mean   :  40.68                                        
    ##  3rd Qu.: 177.00                                        
    ##  Max.   :1592.00                                        
    ##    COUNTRY             LATITUDE        LONGITUDE         NAICS_CODE    
    ##  Length:7581        Min.   :-14.29   Min.   :-176.64   Min.   :622110  
    ##  Class :character   1st Qu.: 33.46   1st Qu.: -98.20   1st Qu.:622110  
    ##  Mode  :character   Median : 37.97   Median : -90.07   Median :622110  
    ##                     Mean   : 37.33   Mean   : -92.36   Mean   :622141  
    ##                     3rd Qu.: 41.32   3rd Qu.: -81.77   3rd Qu.:622110  
    ##                     Max.   : 71.29   Max.   : 145.72   Max.   :622310  
    ##   NAICS_DESC           SOURCE            SOURCEDATE                 
    ##  Length:7581        Length:7581        Min.   :2012-06-05 00:00:00  
    ##  Class :character   Class :character   1st Qu.:2018-08-09 00:00:00  
    ##  Mode  :character   Mode  :character   Median :2018-08-10 00:00:00  
    ##                                        Mean   :2018-05-18 08:50:54  
    ##                                        3rd Qu.:2018-08-13 00:00:00  
    ##                                        Max.   :2019-08-13 00:00:00  
    ##   VAL_METHOD           VAL_DATE                     WEBSITE         
    ##  Length:7581        Min.   :2013-05-29 00:00:00   Length:7581       
    ##  Class :character   1st Qu.:2014-02-10 00:00:00   Class :character  
    ##  Mode  :character   Median :2014-02-10 00:00:00   Mode  :character  
    ##                     Mean   :2014-08-19 12:10:09                     
    ##                     3rd Qu.:2014-03-12 00:00:00                     
    ##                     Max.   :2019-05-16 00:00:00                     
    ##    STATE_ID           ALT_NAME            ST_FIPS         OWNER          
    ##  Length:7581        Length:7581        Min.   : 1.00   Length:7581       
    ##  Class :character   Class :character   1st Qu.:17.00   Class :character  
    ##  Mode  :character   Mode  :character   Median :29.00   Mode  :character  
    ##                                        Mean   :29.47                     
    ##                                        3rd Qu.:44.00                     
    ##                                        Max.   :78.00                     
    ##    TTL_STAFF         BEDS            TRAUMA            HELIPAD         
    ##  Min.   :-999   Min.   :-999.00   Length:7581        Length:7581       
    ##  1st Qu.:-999   1st Qu.:  25.00   Class :character   Class :character  
    ##  Median :-999   Median :  66.00   Mode  :character   Mode  :character  
    ##  Mean   :-999   Mean   :  46.53                                        
    ##  3rd Qu.:-999   3rd Qu.: 178.00                                        
    ##  Max.   :-999   Max.   :1592.00

One thing that should jump out at us right away in scanning the summary
of the hospitals dataset is that the min for all of our discete numeric
values is -999. How is it possible to count -999 beds? This is good
indication that -999 is being used to indicate that data is not
available. In fact, quite often -999 is used to indicate null values.
Let’s go ahead and convert all of the instances of -999 to NA.

``` r
is.na(hospitals) <- hospitals == -999
```

Summarize the values in each variable in your data frame.

``` r
#Uncomment the appropriate lines below, and fill in your data frame.
#_____ %>% summary()

#If you have a dataset with more than 20 columns, you can select which columns to run summary on using the following code:

#df %>% select(VARIABLE_NAME1:VARIABLE_NAME20) %>% summary()
#_____ %>% select(_____:_____) %>% summary()

#or if the columns you would like to summarize are not consecutive, you can call:

#df %>% select(c(VARIABLE_NAME1, VARIABLE_NAME5, VARIABLE_NAME20)) %>% summary()
#_____ %>% select(c(_____, _____, _____)) %>% summary()
```

List three insights that you can draw from calling summary(). These
insights may be conclusions that you immediately draw about something
that is going on in your dataset or questions that you are left with.
You may consider the range (from max to min) of numeric values in a
particular column, the difference between the median and the mean, or
the number of NAs that appear. Are you surprised at any of the values or
do the numbers make sense?

``` r
1. 
2.
3.
```

Have you identified any areas where you may need to conduct more
cleaning? Email me if you are unsure of how to address this issue.

``` r
Fill response here. 
```

### Values in a Key Categorical Variable

*distinct()* lists each of the unique values that appears within a
variable. This can be useful for determining how different issues are
classified in the data. *n\_distinct()* counts the number of distinct
values in a variable. This let’s us know how many categories we are
dealing with. For instance, I can find out the distinct types of
hospitals as well as how many distinct types there are by calling:

``` r
#df %>% select(VARIABLE_NAME) %>% distinct()
#df %>% select(VARIABLE_NAME) %>% n_distinct()
hospitals %>% select(TYPE) %>% distinct()
```

    ##                  TYPE
    ## 1  GENERAL ACUTE CARE
    ## 2         PSYCHIATRIC
    ## 3      LONG TERM CARE
    ## 4     CRITICAL ACCESS
    ## 5      REHABILITATION
    ## 6            CHILDREN
    ## 7             SPECIAL
    ## 8     CHRONIC DISEASE
    ## 9            MILITARY
    ## 10              WOMEN

``` r
hospitals %>% select(TYPE) %>% n_distinct()
```

    ## [1] 10

Often in ethnography, it is our job to take something that seems obvious
or familiar to us and to question it as if it were strange. When running
the function above, we may ask why values are categorized the way that
they are - even if those categories seem obvious at first glance.

> For instance, in the hospitals dataset, we might ask why it is that we
> have separate categories for different types of hospitals. With just a
> bit of research, we find that there is rich history behind these
> hospital types. For instance, “critical access hospitals” was a
> designation created in 1997 to improve access to hospitals in rural
> parts of the US, following an almost two-decade long wave of hospital
> closures in rural communities. See this
> [source](https://www.ruralhealthinfo.org/topics/critical-access-hospitals).
> Psychiatric hospitals, while following a similar timeline to the
> development of general hospitals in the US, developed in response to
> changing attitudes and understandings of what it meant to be mentally
> ill. In the 18th century, mental illness was often considered a moral
> or spiritual shortcoming; however, the increased emphasis on *moral
> treatment* of mentally ill patients ushered in a new wave of
> insitutions and wards devoted to the treatment of such patients. See
> this
> [source](https://www.nursing.upenn.edu/nhhc/nurses-institutions-caring/history-of-psychiatric-hospitals/).

Let’s check a second variable in the hospitals dataset.

``` r
#df %>% select(VARIABLE_NAME) %>% distinct()
#df %>% select(VARIABLE_NAME) %>% n_distinct()
hospitals %>% select(OWNER) %>% distinct()
```

    ##                             OWNER
    ## 1              GOVERNMENT - LOCAL
    ## 2                     PROPRIETARY
    ## 3 GOVERNMENT - DISTRICT/AUTHORITY
    ## 4                            <NA>
    ## 5              GOVERNMENT - STATE
    ## 6                      NON-PROFIT
    ## 7            GOVERNMENT - FEDERAL

``` r
hospitals %>% select(OWNER) %>% n_distinct()
```

    ## [1] 7

> Upon running this, we might ask why these different hospital business
> models exist. With just a bit of research, we can find a [history of
> hospital
> ownership](https://www.nursing.upenn.edu/nhhc/nurses-institutions-caring/history-of-hospitals/).
> From this history, we can see that a number of cultural, political,
> and economic forces has shaped hospital ownership models. In other
> words, these categories have a rich cultural history and tell us not
> only about our data, but also about the cultural context in which data
> gets enumerated.

Choose a categorical variable in your dataset to explore further. Be
sure to select a variable in which the values represented in each row
are likely to appear more than once. Select that variable and then call
distinct() and
n\_distinct().

``` r
#Uncomment the appropriate lines below, and fill in your data frame and categorical variable name.

#Check the distinct values in the variable
#_____ %>% select(_____) %>% distinct()

#Check the number of distinct values in the variable
#_____ %>% select(_____) %>% n_distinct()
```

Reflect on the categorization. How are the categories divided? Do any of
the categories surprise you? Why? In what ways do the categories reflect
a particular cultural moment? Conduct a bit of Web research in order to
better understand why they are divided the way that they are. Be sure to
cite your sources.

``` r
Fill your response here. 
```

Repeat the steps above for a second categorical variable in your
dataset.

``` r
#Uncomment the appropriate lines below, and fill in your data frame and categorical variable name.

#Check the distinct values in the variable
#_____ %>% select(_____) %>% distinct()

#Check the number of distinct values in the variable
#_____ %>% select(_____) %>% n_distinct()
```

Reflect on the categorization. How are the categories divided? Do any of
the categories surprise you? Why? In what ways do the categories reflect
a particular cultural moment? Conduct a bit of Web research in order to
better understand why they are divided the way that they are. Be sure to
cite your sources.

``` r
Fill your response here. 
```

### NAs

Check the number of NAs in each variable in your dataset by filling in
the blanks in the commented code below.

``` r
#colSums(sapply(df, is.na))
colSums(sapply(hospitals, is.na))
```

    ##          X          Y   OBJECTID         ID       NAME    ADDRESS 
    ##          0          0          0          0          0          0 
    ##       CITY      STATE        ZIP       ZIP4  TELEPHONE       TYPE 
    ##          0          0          0       7471       1084          0 
    ##     STATUS POPULATION     COUNTY COUNTYFIPS    COUNTRY   LATITUDE 
    ##          0        702          4          8          0          0 
    ##  LONGITUDE NAICS_CODE NAICS_DESC     SOURCE SOURCEDATE VAL_METHOD 
    ##          0          0          0          0          0          0 
    ##   VAL_DATE    WEBSITE   STATE_ID   ALT_NAME    ST_FIPS      OWNER 
    ##          0        384       3655       6677          0        371 
    ##  TTL_STAFF       BEDS     TRAUMA    HELIPAD 
    ##       7581        662       5427          0

``` r
#Uncomment the appropriate lines below, and fill in your data frame.
#colSums(sapply(_____, is.na)) 
```

Let’s explore a variable with many NAs. This is going to be the first
time we see the function filter(). *filter()* subsets our data to the
observations (or rows) that meet a certain criteria. Below, we will
filter our data to those observations in which a certain variable is an
NA. However, we can filter by a number of criteria; for instance, we can
filter to those rows with a variable that:

  - equals a particular value: == “VALUE”
  - is less than a particular value: \< VALUE
  - is greater than a particular value : \> VALUE
  - is less than or equal to a particular value: \<= VALUE
  - is greater than or equal to a particular value: \>= VALUE
  - is one of a vector of values: %in% c(VALUE1, VALUE2)

Here is how we filter data to the rows in which a certain variable is an
NA.

``` r
#df %>% filter(is.na(VARIABLE_NAME)) %>% head(10)
hospitals %>% filter(is.na(BEDS)) %>% head(10)
```

    ##             X        Y OBJECTID        ID
    ## 1  -104.82378 39.59418     1061 153580112
    ## 2  -104.96804 39.74525     1062 153780218
    ## 3  -104.79948 38.83980     1063 154480909
    ## 4  -104.77088 39.54814     1064 154580138
    ## 5  -105.00010 39.57454     1065 154780120
    ## 6  -104.84112 39.74230     1066 155180045
    ## 7   -92.45233 31.31768     1067 159371301
    ## 8   -89.75375 30.28080     1070 160170458
    ## 9   -72.85676 41.47288     1083   3380649
    ## 10  -83.96493 43.47397     1105        61
    ##                                                           NAME
    ## 1                                     CENTENNIAL MEDICAL PLAZA
    ## 2              CHILDREN'S HOSPITAL COLORADO AT ST JOSEPHS HOSP
    ## 3    CHILDREN'S HOSPITAL COLORADO AT MEMORIAL HOSPITAL CENTRAL
    ## 4    CHILDREN'S HOSPITAL COLORADO AT PARKER ADVENTIST HOSPITAL
    ## 5             HEALTHSOUTH REHABILITATION HOSPITAL OF LITTLETON
    ## 6  UNIVERSITY OF COLORADO HOSPITAL ANSCHUTZ INPATIENT PAVILION
    ## 7            HEALTHSOUTH REHABILITATION HOSPITAL OF ALEXANDRIA
    ## 8                              SLIDELL -AMG SPECIALTY HOSPTIAL
    ## 9                                             GAYLORD HOSPITAL
    ## 10                             ST MARY'S OF MICHIGAN-TOWNE CTR
    ##                     ADDRESS             CITY STATE   ZIP ZIP4
    ## 1     14200 E ARAPAHOE ROAD       CENTENNIAL    CO 80112 <NA>
    ## 2      1830 FRANKLIN STREET           DENVER    CO 80218 <NA>
    ## 3  1400 EAST BOULDER STREET COLORADO SPRINGS    CO 80909 <NA>
    ## 4     9395 CROWN CREST BLVD           PARKER    CO 80138 <NA>
    ## 5  1001 WEST MINERAL AVENUE        LITTLETON    CO 80120 <NA>
    ## 6    12605 EAST 16TH AVENUE           AURORA    CO 80045 <NA>
    ## 7      104 NORTH 3RD STREET       ALEXANDRIA    LA 71301 <NA>
    ## 8       1400 LINDBERG DRIVE          SLIDELL    LA 70458 <NA>
    ## 9        50 GAYLORD FARM RD      WALLINGFORD    CT 06492 2828
    ## 10     4599 TOWNE CENTRE RD          SAGINAW    MI 48604 <NA>
    ##         TELEPHONE               TYPE STATUS POPULATION      COUNTY
    ## 1  (303) 699-3000 GENERAL ACUTE CARE   OPEN         NA    ARAPAHOE
    ## 2  (720) 777-1360           CHILDREN CLOSED         NA      DENVER
    ## 3  (719) 365-5000           CHILDREN   OPEN         NA     EL PASO
    ## 4  (720) 777-1350 GENERAL ACUTE CARE   OPEN         NA     DOUGLAS
    ## 5  (303) 334-1100     REHABILITATION   OPEN         NA    ARAPAHOE
    ## 6  (720) 848-0000 GENERAL ACUTE CARE   OPEN         NA       ADAMS
    ## 7  (318) 449-1370     REHABILITATION   OPEN         NA     RAPIDES
    ## 8  (985) 326-0440            SPECIAL   OPEN         NA ST. TAMMANY
    ## 9  (203) 284-2800    CHRONIC DISEASE   OPEN         NA   NEW HAVEN
    ## 10 (989) 497-3000 GENERAL ACUTE CARE   OPEN         NA     SAGINAW
    ##    COUNTYFIPS COUNTRY LATITUDE  LONGITUDE NAICS_CODE
    ## 1       08005     USA 39.59418 -104.82378     622110
    ## 2       08031     USA 39.74525 -104.96804     622110
    ## 3       08041     USA 38.83980 -104.79948     622110
    ## 4       08035     USA 39.54814 -104.77088     622110
    ## 5       08005     USA 39.57454 -105.00010     622310
    ## 6       08001     USA 39.74230 -104.84112     622110
    ## 7       22079     USA 31.31768  -92.45233     622310
    ## 8       22103     USA 30.28080  -89.75375     622310
    ## 9       09009     USA 41.47288  -72.85676     622310
    ## 10      26145     USA 43.47397  -83.96493     622110
    ##                                                      NAICS_DESC
    ## 1                        GENERAL MEDICAL AND SURGICAL HOSPITALS
    ## 2                                 CHILDREN'S HOSPITALS, GENERAL
    ## 3                                 CHILDREN'S HOSPITALS, GENERAL
    ## 4                        GENERAL MEDICAL AND SURGICAL HOSPITALS
    ## 5  REHABILITATION HOSPITALS (EXCEPT ALCOHOLISM, DRUG ADDICTION)
    ## 6                        GENERAL MEDICAL AND SURGICAL HOSPITALS
    ## 7  REHABILITATION HOSPITALS (EXCEPT ALCOHOLISM, DRUG ADDICTION)
    ## 8  SPECIALTY (EXCEPT PSYCHIATRIC AND SUBSTANCE ABUSE) HOSPITALS
    ## 9  REHABILITATION HOSPITALS (EXCEPT ALCOHOLISM, DRUG ADDICTION)
    ## 10                       GENERAL MEDICAL AND SURGICAL HOSPITALS
    ##                                                                              SOURCE
    ## 1  http://www.hfemsd2.dphe.state.co.us/hfd2003/homebase.aspx?Ftype=hospital&Do=list
    ## 2  http://www.hfemsd2.dphe.state.co.us/hfd2003/homebase.aspx?Ftype=hospital&Do=list
    ## 3  http://www.hfemsd2.dphe.state.co.us/hfd2003/homebase.aspx?Ftype=hospital&Do=list
    ## 4  http://www.hfemsd2.dphe.state.co.us/hfd2003/homebase.aspx?Ftype=hospital&Do=list
    ## 5  http://www.hfemsd2.dphe.state.co.us/hfd2003/homebase.aspx?Ftype=hospital&Do=list
    ## 6  http://www.hfemsd2.dphe.state.co.us/hfd2003/homebase.aspx?Ftype=hospital&Do=list
    ## 7                     http://new.dhh.louisiana.gov/index.cfm/directory/category/169
    ## 8                     http://new.dhh.louisiana.gov/index.cfm/directory/category/169
    ## 9                            https://www.elicense.ct.gov/Lookup/GenerateRoster.aspx
    ## 10                                   https://w2.lara.state.mi.us/VAL/License/Search
    ##    SOURCEDATE    VAL_METHOD   VAL_DATE
    ## 1  2018-08-13 IMAGERY/OTHER 2017-11-29
    ## 2  2016-07-10 IMAGERY/OTHER 2017-11-29
    ## 3  2018-08-13 IMAGERY/OTHER 2015-06-18
    ## 4  2018-08-13 IMAGERY/OTHER 2015-06-18
    ## 5  2018-08-13 IMAGERY/OTHER 2015-06-18
    ## 6  2018-08-13 IMAGERY/OTHER 2015-06-18
    ## 7  2018-08-16 IMAGERY/OTHER 2015-05-30
    ## 8  2018-08-16 IMAGERY/OTHER 2015-05-30
    ## 9  2018-08-13 IMAGERY/OTHER 2016-03-31
    ## 10 2018-08-09 IMAGERY/OTHER 2015-05-21
    ##                                                            WEBSITE
    ## 1                                                             <NA>
    ## 2                                                             <NA>
    ## 3                                                             <NA>
    ## 4                                                             <NA>
    ## 5                                                             <NA>
    ## 6                                                             <NA>
    ## 7                                                             <NA>
    ## 8                                                             <NA>
    ## 9                                          http://www.gaylord.org/
    ## 10 http://www.stmarysofmichigan.org/find_a_location/index.php?i=86
    ##    STATE_ID ALT_NAME ST_FIPS              OWNER TTL_STAFF BEDS   TRAUMA
    ## 1      <NA>     <NA>       8               <NA>        NA   NA LEVEL IV
    ## 2      <NA>     <NA>       8         NON-PROFIT        NA   NA     <NA>
    ## 3      <NA>     <NA>       8               <NA>        NA   NA LEVEL II
    ## 4      <NA>     <NA>       8               <NA>        NA   NA     <NA>
    ## 5      <NA>     <NA>       8               <NA>        NA   NA     <NA>
    ## 6      <NA>     <NA>       8 GOVERNMENT - STATE        NA   NA LEVEL II
    ## 7      <NA>     <NA>      22               <NA>        NA   NA     <NA>
    ## 8      <NA>     <NA>      22               <NA>        NA   NA     <NA>
    ## 9      <NA>     <NA>       9         NON-PROFIT        NA   NA     <NA>
    ## 10     <NA>     <NA>      26         NON-PROFIT        NA   NA     <NA>
    ##    HELIPAD
    ## 1        Y
    ## 2        N
    ## 3        Y
    ## 4        Y
    ## 5        N
    ## 6        Y
    ## 7        N
    ## 8        N
    ## 9        N
    ## 10       N

If your dataset has no columns with NAs just note that below, and move
on to 4. Otherwise, fill in the blanks
below.

``` r
#Uncomment the appropriate lines below, and fill in your data frame and variable

#_____ %>% filter(is.na(_____)) %>% head(10)
```

Is there anything special about these rows? What hypotheses do you have
for why there may be missing data in this column?

``` r
Fill your response here. 
```

To confirm our hypotheses about NAs, we often have to apply additional
filter conditions to the dataset. Let’s say that we hypothesize that
there are missing values in a column because a certain geography didn’t
report data; we may want to filter the dataset to the rows that
represent that geography to see if any of the rows in the dataset have
values listed.

``` r
#df %>% filter(Country == "Luxembourg") 
#...and then we would check to see if there are missing values in the columns under consideration. 
```

Or maybe we hypothesize that a certain geography did not report values
in a certain year. We can join multiple filter conditions by placing an
‘&’ between two statements.

``` r
#df %>% filter(Country == "Luxembourg" & Year == 2017) 
#...and then we would check to see if there are missing values in the columns under consideration. 
```

Also note that we can filter to rows with all NAs in multiple columns by
calling:

``` r
#df %>% filter_at(vars(VARIABLE_NAME1, VARIABLE_NAME2, VARIABLE_NAME5:VARIABLE_NAME8), all_vars(is.na(.)))
```

And that we can filter to rows with any NAs in multiple columns by
calling:

``` r
#df %>% filter_at(vars(VARIABLE_NAME1, VARIABLE_NAME2, VARIABLE_NAME5:VARIABLE_NAME8), any_vars(is.na(.)))
```

For instance, I might check to see if the BEDS variable is NA at only
hospitals whose STATUS is CLOSED or in observations that were sourced
prior to
    2017.

``` r
hospitals %>% filter(STATUS == "CLOSED") %>% select(NAME, BEDS)
```

    ##                                                                            NAME
    ## 1                                                  PACIFIC ORANGE HOSPITAL, LLC
    ## 2                                                     WEST PACES MEDICAL CENTER
    ## 3                                                           SHASTA COUNTY P H F
    ## 4                               CHILDREN'S HOSPITAL COLORADO AT ST JOSEPHS HOSP
    ## 5                                                 LODI MEMORIAL HOSPITAL - WEST
    ## 6                                                        SAUK PRAIRIE MEM HSPTL
    ## 7                                               CENTURY HOSPITAL MEDICAL CENTER
    ## 8                                                      ANSON COMMUNITY HOSPITAL
    ## 9                                                          MERCY MEDICAL CENTER
    ## 10                                          WILLIAM B KESSLER MEMORIAL HOSPITAL
    ## 11                              MOUNT CARMEL GUILD BEHAVIORAL HEALTHCARE SYSTEM
    ## 12                                              MARIAN BEHAVIORAL HEALTH CENTER
    ## 13                                        SAINT CLARES HOSPITAL - SUSSEX CAMPUS
    ## 14                                          SILVER SPRINGS RURAL HEALTH CENTERS
    ## 15                                        KINDRED HOSPITAL ARIZONA - SCOTTSDALE
    ## 16                                                      CORRY MEMORIAL HOSPITAL
    ## 17                                                      RCHP-SIERRA VISTA, INC.
    ## 18                                                          EPIC MEDICAL CENTER
    ## 19                                           UNIVERSITY GENERAL HOSPITAL DALLAS
    ## 20                        NEW HORIZONS OF TREASURE COAST - MENTAL HEALTH CENTER
    ## 21                                      FOUNDATION SURGICAL HOSPITAL OF HOUSTON
    ## 22                                       EAST TEXAS MEDICAL CENTER MOUNT VERNON
    ## 23                                                      BAPTIST ORANGE HOSPITAL
    ## 24                                                  LAKE WHITNEY MEDICAL CENTER
    ## 25                                                KINDRED HOSPITAL EAST HOUSTON
    ## 26                                             HILLCREST BAPTIST MEDICAL CENTER
    ## 27                                               KINDRED HOSPITAL NORTH HOUSTON
    ## 28                                                          HOPEBRIDGE HOSPITAL
    ## 29                                HEALTHSOUTH REHABILITATION HOSPITAL OF AUSTIN
    ## 30                                                     LLANO SPECIALTY HOSPITAL
    ## 31                           UNIVERSITY OF CALIF, SAN DIEGO MEDICAL CTR D/P APH
    ## 32                                          SUTTER MEDICAL CENTER OF SANTA ROSA
    ## 33                                      DOMINICAN HOSPITAL-SANTA CRUZ/FREDERICK
    ## 34                                                 STORY CITY MEMORIAL HOSPITAL
    ## 35                                                             REID HOSPITAL-ER
    ## 36                                                     HOWARD MEMORIAL HOSPITAL
    ## 37                                             SILOAM SPRINGS MEMORIAL HOSPITAL
    ## 38                   UNIVERSITY OF COLORADO HEALTH AT MEMORIAL HOSPITAL CENTRAL
    ## 39                                                    PARKWAY REGIONAL HOSPITAL
    ## 40                                               ALBANY AREA HOSPITAL & MED CTR
    ## 41                                                        MERCY HOSPITAL JOPLIN
    ## 42                                               KINDRED HOSPITAL - KANSAS CITY
    ## 43                                              SHRINERS HOSPITALS FOR CHILDREN
    ## 44                                                           SAC-OSAGE HOSPITAL
    ## 45                                                  HIGHLAND COMMUNITY HOSPITAL
    ## 46                                                   NATCHEZ COMMUNITY HOSPITAL
    ## 47                                               VALDESE GENERAL HOSPITAL, INC.
    ## 48                                          PUNGO DISTRICT HOSPITAL CORPORATION
    ## 49                                                        DAVIE COUNTY HOSPITAL
    ## 50                                                 ST MARY'S COMMUNITY HOSPITAL
    ## 51                                              OREGON STATE HOSPITAL  PORTLAND
    ## 52                                            NORTHEAST REHABILITATION HOSPITAL
    ## 53                                          SIERRA VISTA REGIONAL HEALTH CENTER
    ## 54                                                           WESTFIELD HOSPITAL
    ## 55                                            SAINT CATHERINE REGIONAL HOSPITAL
    ## 56                                 ACUITY SPECIALTY HOSPITAL OF ARIZONA AT MESA
    ## 57                                                        QUINCY MEDICAL CENTER
    ## 58                                              DOUGLAS COMMUNITY HOSPITAL, INC
    ## 59               PARKVIEW ADVENTIST MEDICAL CENTER : PARKVIEW MEMORIAL HOSPITAL
    ## 60                                                    MAYO CLINIC HEALTH SYS CF
    ## 61                                                     BARNWELL COUNTY HOSPITAL
    ## 62                                                   SNOQUALMIE VALLEY HOSPITAL
    ## 63                                         SAINT JOSEPH HOSPITAL - SOUTH CAMPUS
    ## 64                                             BLESSING HOSPITAL AT 14TH STREET
    ## 65                                                   MENDOTA COMMUNITY HOSPITAL
    ## 66                                        SOUTHWEST HOSPITAL AND MEDICAL CENTER
    ## 67                                            CLEARVIEW REGIONAL MEDICAL CENTER
    ## 68                                   ASCENSION GONZALES REHABILITATION HOSPITAL
    ## 69                                                  HUEY P. LONG MEDICAL CENTER
    ## 70                              LSU BOGALUSA MEDICAL CENTER (OUTPATIENT CAMPUS)
    ## 71                                 OPELOUSAS GENERAL HEALTH SYSTEM SOUTH CAMPUS
    ## 72                                                   CORCORAN DISTRICT HOSPITAL
    ## 73                                           DOCTORS MEDICAL CENTER - SAN PABLO
    ## 74                           UNIVERSITY MCDUFFIE COUNTY REGIONAL MEDICAL CENTER
    ## 75                                                PROMISE HOSPITAL OF ASCENSION
    ## 76                                   CALIFORNIA PACIFIC MED CTR-CALIFORNIA EAST
    ## 77                                                JOAN GLANCY MEMORIAL HOSPITAL
    ## 78                                                   DEVEREUX TREATMENT NETWORK
    ## 79                                                           LSU MEDICAL CENTER
    ## 80                                                              SHAWANO MED CTR
    ## 81                                               MEMORIAL HEALTH CENTER CLINICS
    ## 82                                               ORANGE CITY MUNICIPAL HOSPITAL
    ## 83                                                 VIBRA HOSPITAL OF FORT WAYNE
    ## 84                                                  MERCY HOSPITAL INDEPENDENCE
    ## 85                                           THE SURGICAL HOSPITAL OF JONESBORO
    ## 86                                                    PARKER ADVENTIST HOSPITAL
    ## 87                                                       WALTER P CARTER CENTER
    ## 88                                    KIDSPEACE NATIONAL CENTERS OF NEW ENGLAND
    ## 89                                                SCHOOLCRAFT MEMORIAL HOSPITAL
    ## 90                                          ROGERS CITY REHABILITATION HOSPITAL
    ## 91                                                    ESSENTIA HEALTH SANDSTONE
    ## 92                                                CHOCTAW COUNTY MEDICAL CENTER
    ## 93                                                       WINSTON MEDICAL CENTER
    ## 94                                                MADISON COUNTY MEDICAL CENTER
    ## 95                                      MERCY FRANCISCAN HOSPITAL WESTERN HILLS
    ## 96                                                      MERCY ST THERESA CENTER
    ## 97                                              CENTRAL VALLEY GENERAL HOSPITAL
    ## 98                                 ANAHEIM GENERAL HOSPITAL - BUENA PARK CAMPUS
    ## 99                                                        SATILLA PARK HOSPITAL
    ## 100                                                  CHARLTON MEMORIAL HOSPITAL
    ## 101                             EAST LOUISIANA STATE HOSPITAL-GREENWELL SPRINGS
    ## 102                                          BROWN MEMORIAL CONVALESCENT CENTER
    ## 103                                                   KAILO BEHAVIORAL HOSPITAL
    ## 104                                                  EARL K LONG MEDICAL CENTER
    ## 105                                                    STEWART WEBSTER HOSPITAL
    ## 106                           PROVIDENCE LITTLE CO OF MARY SUBACUTE CARE CENTER
    ## 107                                                   TEMPLE COMMUNITY HOSPITAL
    ## 108                                                   KAISER FND HOSP - ANAHEIM
    ## 109                           SADDLEBACK MEMORIAL MEDICAL CENTER - SAN CLEMENTE
    ## 110                                           NEW HORIZONS LANIER PARK HOSPITAL
    ## 111                                          PRAIRIE DU CHIEN MEMORIAL HOSPITAL
    ## 112                                                RAINBOW MENTAL HLTH FACILITY
    ## 113                                             CRITTENDEN HOSPITAL ASSOCIATION
    ## 114                                                ST. VINCENT DOCTORS HOSPITAL
    ## 115                                           HOT SPRINGS REHABILITATION CENTER
    ## 116                  GLADYS SPELLMAN SPECIALTY HOSPITAL AND NURSING CARE CENTER
    ## 117                                                 MAINEGENERAL MEDICAL CENTER
    ## 118                                           MAINEGENERAL MEDICAL CENTER-SETON
    ## 119                                SAINT ANDREWS HOSPITAL AND HEALTHCARE CENTER
    ## 120                                                         ST JOSEPHS HOSPITAL
    ## 121                                                     GARRARD COUNTY HOSPITAL
    ## 122                                MONTEFIORE WESTCHESTER SQUARE MEDICAL CENTER
    ## 123                                                LONG ISLAND COLLEGE HOSPITAL
    ## 124                                         THE ADDICTION INSTITUTE OF NEW YORK
    ## 125                                                        NYULMC - COBBLE HILL
    ## 126                                                   PRESTON MEMORIAL HOSPITAL
    ## 127                                                        SPOONER HOSPITAL SYS
    ## 128                                                   NORMAN SPECIALTY HOSPITAL
    ## 129                                         MEMORIAL HOSPITAL & PHYSICIAN GROUP
    ## 130                                                     JACKSON COUNTY HOSPITAL
    ## 131                                                    REAGAN MEMORIAL HOSPITAL
    ## 132                                          EAST TEXAS MEDICAL CENTER - GILMER
    ## 133                                                 RENAISSANCE HOSPITAL GROVES
    ## 134                                                    FAITH COMMUNITY HOSPITAL
    ## 135                                       GOOD SHEPHERD MEDICAL CENTER - LINDEN
    ## 136                                                RENAISSANCE HOSPITAL TERRELL
    ## 137                                              SHELBY REGIONAL MEDICAL CENTER
    ## 138                                                  WALNUT HILL MEDICAL CENTER
    ## 139                                                               MARIAN CENTER
    ## 140                                                           EDWARD PLAINFIELD
    ## 141                                            ATRIUM MEDICAL CENTER AT CORINTH
    ## 142                                      ALLEGIANCE HEALTH CENTER PERMIAN BASIN
    ## 143                     BAYLOR INSTITUTE FOR REHABILITATION AT NORTHWEST DALLAS
    ## 144                                                     MENTAL HEALTH INSTITUTE
    ## 145                                       BOWDON AREA HOSPITAL AND REHAB CENTER
    ## 146                                 CHARTER BEHAVIORAL HEALTH SYSTEM OF ATLANTA
    ## 147                                         SELECT SPECIALTY HOSPITAL - ATLANTA
    ## 148                                                    SUMTER REGIONAL HOSPITAL
    ## 149                                                       SEASIDE HEALTH SYSTEM
    ## 150                       SOUTHWEST WASHINGTON MEDICAL CENTER - MEMORIAL CAMPUS
    ## 151                                               PUGET SOUND BEHAVIORAL HEALTH
    ## 152                                                         DAYBREAK OF SPOKANE
    ## 153                                  PHYSICIAN'S CHOICE HOSPITAL - FREMONT, LLC
    ## 154                                                   LONG BEACH MEDICAL CENTER
    ## 155                                  SUMMIT PARK HOSPITAL & NURSING CARE CENTER
    ## 156                                                   SELECT SPECIALTY HOSPITAL
    ## 157                                         ST JAMES MERCY HOSPITAL - MERCYCARE
    ## 158                                                 COPPER BASIN MEDICAL CENTER
    ## 159                                              UNITED REGIONAL MEDICAL CENTER
    ## 160                                     METHODIST HEALTHCARE - FAYETTE HOSPITAL
    ## 161                                                        ROANE MEDICAL CENTER
    ## 162                                             MIDDLE TENNESSEE MEDICAL CENTER
    ## 163                                                   HUMBOLDT GENERAL HOSPITAL
    ## 164                                          BAPTIST HOSPITAL OF EAST TENNESSEE
    ## 165                                        SELECT SPECIALTY HOSPITAL - OAK HILL
    ## 166                                                  BAPTIST HOSPITAL FOR WOMEN
    ## 167                                                       ALLEN COUNTY HOSPITAL
    ## 168                                              NORTH TEXAS COMMUNITY HOSPITAL
    ## 169                                     EAST TEXAS MEDICAL CENTER - CLARKSVILLE
    ## 170                                            HUNT REGIONAL COMMUNITY HOSPITAL
    ## 171                                      MIAMI HEART INSTITUTE & MEDICAL CENTER
    ## 172                              NIX COMMUNITY GENERAL HOSPITAL OF DILLEY TEXAS
    ## 173                                            HEREFORD REGIONAL MEDICAL CENTER
    ## 174                                                      ST. ANTHONY'S HOSPITAL
    ## 175                                                SPRING BRANCH MEDICAL CENTER
    ## 176                                             RANKIN COUNTY HOSPITAL DISTRICT
    ## 177                                             MARTIN COUNTY HOSPITAL DISTRICT
    ## 178                                               PRISTINE HOSPITAL OF PASADENA
    ## 179                                                              GRACE HOSPITAL
    ## 180                                                   BAYLOR SPECIALTY HOSPITAL
    ## 181                                                 CLARINDA MUNICIPAL HOSPITAL
    ## 182                                  FORT VALLEY STATE UNIVERSITY HEALTH SYSTEM
    ## 183                                                               PRIDE MEDICAL
    ## 184                                                 SOUTHWESTERN STATE HOSPITAL
    ## 185                                                    SMITH NORTHVIEW HOSPITAL
    ## 186                            TY COBB HEALTHCARE SYSTEM - HART COUNTY HOSPITAL
    ## 187                                                     RIVER PARISHES HOSPITAL
    ## 188                                       SCI-WAYMART FORENSIC TREATMENT CENTER
    ## 189                              LIFECARE HOSPITALS OF PITTSBURGH - MONROEVILLE
    ## 190                                                   TILDEN COMMUNITY HOSPITAL
    ## 191                                                   MONTROSE GENERAL HOSPITAL
    ## 192                                                       SPECIAL CARE HOSPITAL
    ## 193                                              MERCY CONTINUING CARE HOSPITAL
    ## 194                                               DAKOTA PLAINS SURGICAL CENTER
    ## 195                                                  WELLSTAR PAULDING HOSPITAL
    ## 196                                                  FALL RIVER HEALTH SERVICES
    ## 197                                                  NORTH VALLEY HEALTH CENTER
    ## 198                                   UVA KLUGE CHILDRENS REHABILITATION CENTER
    ## 199                                       VERNON M. GEDDY JR. OUTPATIENT CENTER
    ## 200                         CHILDREN'S HOSPITAL OF RICHMOND AT VCU (BROOK ROAD)
    ## 201                                                      MARLBORO PARK HOSPITAL
    ## 202                                              OUR CHILDREN'S HOUSE AT BAYLOR
    ## 203                                                ROCKINGHAM MEMORIAL HOSPITAL
    ## 204                        PEACEHEALTH ST JOHN MEDICAL CENTER - BROADWAY CAMPUS
    ## 205                                                MARK REED HEALTH CARE CLINIC
    ## 206                                              DEWITT ARMY COMMUNITY HOSPITAL
    ## 207                                                         NORTH SIDE HOSPITAL
    ## 208                                    SWEDISH MEDICAL CENTER - ISSAQUAH CAMPUS
    ## 209                                              GROUP HEALTH EASTSIDE HOSPITAL
    ## 210                                               BLUE MOUNTAIN RECOVERY CENTER
    ## 211                                                         ELLIS HEALTH CENTER
    ## 212                                                       LENOX HILL HEALTHPLEX
    ## 213                                         STATEN ISLAND UNIV HOSP-CONCORD DIV
    ## 214                                                      MALLARD FILLMORE GATES
    ## 215                                         GOOD SAMARITAN REGIONAL HLTH CENTER
    ## 216                                                         OAK FOREST HOSPITAL
    ## 217                                                    CHATTOOGA MEDICAL CENTER
    ## 218                                                     QUITMAN COUNTY HOSPITAL
    ## 219                                    PIONEER HEALTH SERVICES OF NEWTON COUNTY
    ## 220                                                BRUNSWICK COMMUNITY HOSPITAL
    ## 221                                               FLORENCE COMMUNITY HEALTHCARE
    ## 222                                                   FLORALA MEMORIAL HOSPITAL
    ## 223                                             DOCTORS HOSPITAL OF NELSONVILLE
    ## 224                                        CAMDEN COUNTY HEALTH SERVICES CENTER
    ## 225                                           SOUTHWEST REGIONAL MEDICAL CENTER
    ## 226                                                    RINGGOLD COUNTY HOSPITAL
    ## 227                                            NEW JERSEY STATE PRISON HOSPITAL
    ## 228                                             SOUTH JERSEY HEALTH CARE CENTER
    ## 229                                        VIRTUA WEST JERSEY HOSPITAL - CAMDEN
    ## 230               JOHN BROOKS RECOVERY CENTER - RESIDENT DRUG TREATMENT (WOMEN)
    ## 231                                             INSPIRA HEALTH CENTER BRIDGETON
    ## 232                                           CRAWFORD COUNTY MEMORIAL HOSPITAL
    ## 233                     MERWICK REHABILITATION HOSPITAL AND NURSING CARE CENTER
    ## 234                                                MEDICAL CENTER OF NEWARK LLC
    ## 235                                                     GOOD SAMARITAN HOSPITAL
    ## 236                                                   HIND GENERAL HOSPITAL LLC
    ## 237                                                              MAJOR HOSPITAL
    ## 238                                                     CHEYENNE RIVER HOSPITAL
    ## 239                                              SIDNEY REGIONAL MEDICAL CENTER
    ## 240                                                          GOOD HANDS MEDICAL
    ## 241                                             WOODBRIDGE DEVELOPMENTAL CENTER
    ## 242                                                  PIONEER COMMUNITY HOSPITAL
    ## 243                                                     TORRANCE STATE HOSPITAL
    ## 244                                      NEW LIFECARE HOSPITAL OF MECHANICSBURG
    ## 245                                                GOLDWATER SPECIALTY HOSPITAL
    ## 246                                   SELECT SPECIALTY HOSPITAL - GROSSE POINTE
    ## 247                                         CHILDREN'S HOSPITAL NAVICENT HEALTH
    ## 248                                                       RINCON MEDICAL CENTER
    ## 249                                  ADIRONDACK MEDICAL CENTER-LAKE PLACID SITE
    ## 250                                                EFFICACY HEALTH SERVICES LLC
    ## 251                                         CALCASIEU OAKS PSYCHIATRIC HOSPITAL
    ## 252                                                   SELECT SPECIALTY HOSPITAL
    ## 253                                                TULSA-AMG SPECIALTY HOSPITAL
    ## 254                                                    KINDRED HOSPITAL BAYTOWN
    ## 255                                     CHRISTUS DUBUIS HOSPITAL OF PORT ARTHUR
    ## 256                                            GREENWOOD AMG SPECIALTY HOSPITAL
    ## 257                                        SELECT SPECIALTY HOSPITAL-FORT WAYNE
    ## 258                          KINDRED HOSPITAL - LAS VEGAS AT DESERT SPRINGS HOS
    ## 259                                           VERMONT PSYCHIATRIC CARE HOSPITAL
    ## 260                                         CHRISTUS DUBUIS HOSPITAL OF HOUSTON
    ## 261                                          VICTORY MEDICAL CENTER CRAIG RANCH
    ## 262                                    SELECT SPECIALTY HOSPITAL-ZANESVILLE INC
    ## 263                                               KINDRED HOSPITAL CENTRAL OHIO
    ## 264                                    SELECT SPECIALTY HOSPITAL - SOUTH DALLAS
    ## 265                                       OCEANS BEHAVIORAL HOSPITAL OF ABILENE
    ## 266                                         BUCKHEAD AMBULATORY SURGICAL CENTER
    ## 267                                            CRESCENT CITY SPECIALTY HOSPITAL
    ## 268                                                 REGENCY HOSPITAL OF JACKSON
    ## 269                                                 WESTBURY COMMUNITY HOSPITAL
    ## 270                                        PELICAN REHABILITATION HOSPITAL, LLC
    ## 271                                    SELECT SPECIALTY HOSPITAL - GRAND RAPIDS
    ## 272                                          KINDRED HOSPITAL - DELAWARE COUNTY
    ## 273                                   KINDRED REHABILITATION HOSPITAL ARLINGTON
    ## 274                                                ACUITY SPECIALTY OHIO VALLEY
    ## 275                                   MADONNA REHABILITATION SPECIALTY HOSPITAL
    ## 276                                    MINDEN FAMILY MEDICINE AND COMPLETE CARE
    ## 277                                                     GIBSON GENERAL HOSPITAL
    ## 278                                              PROFESSIONAL HOSP INC - MANATI
    ## 279                                          CENTRO DE SALUD COMUNAL DE CULEBRA
    ## 280                                            GOLDEN PLAINS COMMUNITY HOSPITAL
    ## 281                                                BROWNSVILLE DOCTORS HOSPITAL
    ## 282                                                        TEXAS RURAL HOSPITAL
    ## 283                                           CORPUS CHRISTI SPECIALTY HOSPITAL
    ## 284                                               TIMBERLANDS HOSPITAL CROCKETT
    ## 285                                                   DOCTORS MEMORIAL HOSPITAL
    ## 286                                    SELECT SPECIALTY HOSPITAL - SOUTH DALLAS
    ## 287                                                 HIGH POINT TREATMENT CENTER
    ## 288                                            TRIUMPH HOSPITAL CENTRAL HOUSTON
    ## 289                                           VICTORY MEDICAL CENTER SOUTHCROSS
    ## 290                                         BAYLOR MEDICAL CENTER AT WAXAHACHIE
    ## 291                                                   GULF COAST MEDICAL CENTER
    ## 292                                                          ALTUS LUMBERTON LP
    ## 293                                                  WESTERVILLE MEDICAL CAMPUS
    ## 294                                                  RIVERSIDE GENERAL HOSPITAL
    ## 295                                             NORTH ALABAMA REGIONAL HOSPITAL
    ## 296                                                       SACRED HEART HOSPITAL
    ## 297                                                        ST JOSEPH'S HOSPITAL
    ## 298                                                 PEAK VIEW BEHAVIORAL HEALTH
    ## 299                                               PEACH REGIONAL MEDICAL CENTER
    ## 300                                                         POLK MEDICAL CENTER
    ## 301                                         NORTHWEST GEORGIA REGIONAL HOSPITAL
    ## 302                          TY COBB HEALTHCARE SYSTEM - COBB MEMORIAL HOSPITAL
    ## 303                                                        DOOLY MEDICAL CENTER
    ## 304                                       WOODLANDS PSYCHIATRIC HEALTH FACILITY
    ## 305                                              GREEN CLINIC SURGICAL HOSPITAL
    ## 306                                                  BARSTOW COMMUNITY HOSPITAL
    ## 307                               ST VINCENT SETON SPECIALTY HOSPITAL LAFAYETTE
    ## 308                                                   SEASIDE BEHAVIORAL CENTER
    ## 309                                           BOGALUSA - AMG SPECIALTY HOSPITAL
    ## 310                                        COMPASS BEHAVIORAL CENTER OF CROWLEY
    ## 311                                                      BELLWOOD HEALTH CENTER
    ## 312                            FALLBROOK HOSP DISTRICT SKILLED NURSING FACILITY
    ## 313                   CHARITY HOSPITAL AND MEDICAL CENTER OF LA. AT NEW ORLEANS
    ## 314                                                  BOULDER COMMUNITY HOSPITAL
    ## 315                                           KAISER FND HOSP - HAYWARD/FREMONT
    ## 316                                                    SUTTER MEMORIAL HOSPITAL
    ## 317                              BRANDEL MANOR - D/P SNF OF EMANUEL MEDICAL CTR
    ## 318                                                        FLOYD MEDICAL CENTER
    ## 319                                               EMORY DUNWOODY MEDICAL CENTER
    ## 320                                                            WOMAN'S HOSPITAL
    ## 321                                    WALTER OLIN MOSS REGIONAL MEDICAL CENTER
    ## 322                                           CHRISTUS SCHUMPERT MEDICAL CENTER
    ## 323                                                               RIPON MED CTR
    ## 324                                                     KIOWA DISTRICT HOSPITAL
    ## 325                                               PIKE COUNTY MEMORIAL HOSPITAL
    ## 326                                      REGENCY HOSPITAL OF NORTHWEST ARKANSAS
    ## 327                                         ST. MARY - ROGERS MEMORIAL HOSPITAL
    ## 328                                               NEA BAPTIST MEMORIAL HOSPITAL
    ## 329                            ACUITY SPECIALTY HOSPITAL OF ARIZONA AT SUN CITY
    ## 330                                            HEARTLAND SURGICAL SPEC HOSPITAL
    ## 331                                                    NICHOLAS COUNTY HOSPITAL
    ## 332                       AROOSTOOK MEDICAL CENTER - COMMUNITY GENERAL DIVISION
    ## 333                                                             MERCY WESTBROOK
    ## 334                                              IONIA COUNTY MEMORIAL HOSPITAL
    ## 335                                             ST JOSEPH MERCY HOSPITAL-SALINE
    ## 336                         PATIENT’S CHOICE MEDICAL CENTER OF HUMPHREYS COUNTY
    ## 337                                                       LIVINGSTON HEALTHCARE
    ## 338                                            FRANKLIN REGIONAL MEDICAL CENTER
    ## 339                                                   CRAWLEY MEMORIAL HOSPITAL
    ## 340                                                 NYE REGIONAL MEDICAL CENTER
    ## 341                                                    GUTHRIE CORNING HOSPITAL
    ## 342                           LIFECARE HOSPITALS OF SOUTH TEXAS - MCALLEN NORTH
    ## 343                           LIFECARE HOSPITALS OF SOUTH TEXAS - MCALLEN SOUTH
    ## 344                                                             APOLLO HOSPITAL
    ## 345                                                  FOREST PARK MEDICAL CENTER
    ## 346                                                  HAWAII MEDICAL CENTER EAST
    ## 347                                                   BETHESDA ARROW SPRINGS-ER
    ## 348                                                  JOHN & MARY KIRBY HOSPITAL
    ## 349                                                     PACIFIC SHORES HOSPITAL
    ## 350                                            SELECT SPECIALTY HOSPITAL-DENVER
    ## 351                                          SPECIALTY LTCH HOSPITAL OF HAMMOND
    ## 352                                              LANTERMAN DEVELOPMENTAL CENTER
    ## 353                                                       SCOTT COUNTY HOSPITAL
    ## 354                                                LIFESTREAM BEHAVIORAL CENTER
    ## 355                                                       EDWARD WHITE HOSPITAL
    ## 356                                                    MANNING GENERAL HOSPITAL
    ## 357                             SIOUX CENTER COMMUNITY HOSPITAL & HEALTH CENTER
    ## 358                                                HENRIETTA D GOODALL HOSPITAL
    ## 359                                    HENRY FORD MACOMB HOSPITAL-WARREN CAMPUS
    ## 360                                                    ST. MARY'S HEALTH CENTER
    ## 361                                        MINERAL AREA REGIONAL MEDICAL CENTER
    ## 362                                     ST. ALEXIUS HOSPITAL - JEFFERSON CAMPUS
    ## 363                                   ST. ALEXIUS HOSPITAL - FOREST PARK CAMPUS
    ## 364                                                      HEDRICK MEDICAL CENTER
    ## 365                                POPLAR BLUFF REGIONAL MEDICAL CENTER - SOUTH
    ## 366                                              MISSOURI REHABILITATION CENTER
    ## 367                            SOUTHWEST MISSOURI PSYCHIATRIC REHABILITATION CT
    ## 368                                           FIELD MEMORIAL COMMUNITY HOSPITAL
    ## 369                                              HARDY WILSON MEMORIAL HOSPITAL
    ## 370                                                         KILMICHAEL HOSPITAL
    ## 371                            BEATRICE COMMUNITY HOSPITAL & HEALTH CENTER, INC
    ## 372                                                      TRINITY MEDICAL CENTER
    ## 373                                                   PIONEER MEMORIAL HOSPITAL
    ## 374                                                         ST ANTHONY HOSPITAL
    ## 375                                        VIRTUA WEST JERSEY HOSPITAL - BERLIN
    ## 376                                                           LAKEWOOD HOSPITAL
    ## 377                                                     TROY COMMUNITY HOSPITAL
    ## 378                                              GOTTSCHE REHABILITATION CENTER
    ## 379                                              SAINT MARYS HOSPITAL - PASSAIC
    ## 380                                                ESSEX COUNTY HOSPITAL CENTER
    ## 381                                      UNIVERSITY MEDICAL CENTER AT PRINCETON
    ## 382                                                    EWING RESIDENTIAL CENTER
    ## 383                                                CARSON TAHOE DAYTON HOSPITAL
    ## 384                                                         MONTGOMERY HOSPITAL
    ## 385                          KING'S DAUGHTERS' HOSPITAL AND HEALTH SERVICES,THE
    ## 386                        DRUG REHABILITATION INCORPORATED - DAY ONE RESIDENCE
    ## 387                                                    EMORY-ADVENTIST HOSPITAL
    ## 388                                                   CALHOUN MEMORIAL HOSPITAL
    ## 389                                                        HOLY INFANT HOSPITAL
    ## 390                                   SOUTHEASTHEALTH CENTER OF REYNOLDS COUNTY
    ## 391                                         WILLIAM N WISHARD MEMORIAL HOSPITAL
    ## 392                                                 COMMUNITY WESTVIEW HOSPITAL
    ## 393                                          RIVERSIDE BEHAVIORAL HEALTH CENTER
    ## 394                                       TENNOVA HEALTHCARE - MCNAIRY REGIONAL
    ## 395                                               RENVILLE COUNTY HOSP & CLINCS
    ## 396                                             SMYTH COUNTY COMMUNITY HOSPITAL
    ## 397                                                  JOHNSTON MEMORIAL HOSPITAL
    ## 398                                                 W J BARGE MEMORIAL HOSPITAL
    ## 399 COLER-GOLDWATER SPECIALTY HOSPITAL & NURSING FACILITY - COLER HOSPITAL SITE
    ## 400                                            METHODIST EXTENDED CARE HOSPITAL
    ## 401                                             JOHNSON CITY SPECIALTY HOSPITAL
    ## 402                                               BEDFORD COUNTY MEDICAL CENTER
    ## 403                                                       METROPOLITAN HOSPITAL
    ## 404                                                PARK PLACE SURGICAL HOSPITAL
    ## 405                                                 WOODLANDS BEHAVIORAL CENTER
    ## 406                                          MAINEGENERAL MEDICAL CENTER-THAYER
    ## 407                                                  SPARROW SPECIALTY HOSPITAL
    ## 408                                    SOUTHWEST REGIONAL REHABILITATION CENTER
    ## 409                                            SUMMA WADSWORTH-RITTMAN HOSPITAL
    ## 410                                                ELLSWORTH MUNICIPAL HOSPITAL
    ## 411                                                  ANAMOSA COMMUNITY HOSPITAL
    ## 412                                     KINDRED HOSPITAL PITTSBURGH NORTH SHORE
    ## 413                                                         MID-VALLEY HOSPITAL
    ## 414                                               NORTH ADAMS REGIONAL HOSPITAL
    ## 415                                    EXCELA HEALTH WESTMORELAND HOSP JEANETTE
    ## 416                                                NORTH GEORGIA MEDICAL CENTER
    ## 417                                             BRADLEY CENTER OF SAINT FRANCIS
    ## 418                                     US AIR FORCE HOSPITAL-GLENDALE - CLOSED
    ## 419                                                         SAN CARLOS HOSPITAL
    ## 420                                               LOUIS SMITH MEMORIAL HOSPITAL
    ## 421                                           ST MARY'S GOOD SAMARITAN HOSPITAL
    ## 422                                                  MADISON COMMUNITY HOSPITAL
    ## 423                                                          COMMUNITY HOSPITAL
    ## 424                                                PRAIRIE RIDGE HOSP HLTH SERV
    ## 425                                           BAPTIST REHABILITATION-GERMANTOWN
    ## 426                                        ST JOSEPH'S HOSPITAL & HEALTH CENTER
    ## 427                                                          TLC HEALTH NETWORK
    ## 428                                     DOCTORS DIAGNOSTIC CENTER- WILLIAMSBURG
    ## 429                                                    SENTARA BAYSIDE HOSPITAL
    ## 430                                            CARILION GILES MEMORIAL HOSPITAL
    ## 431                                                 LEE REGIONAL MEDICAL CENTER
    ## 432                                             CONTINUOUS CARE CENTER OF TULSA
    ## 433                                         MERCY FRANCISCAN HOSPITAL - MT AIRY
    ## 434                                                         ST. JOSEPH HOSPITAL
    ## 435                                             HAYWOOD PARK COMMUNITY HOSPITAL
    ##     BEDS
    ## 1    101
    ## 2    294
    ## 3     15
    ## 4     NA
    ## 5     24
    ## 6     36
    ## 7     34
    ## 8    147
    ## 9     NA
    ## 10   130
    ## 11    40
    ## 12    NA
    ## 13    NA
    ## 14    NA
    ## 15    62
    ## 16    35
    ## 17    NA
    ## 18    10
    ## 19   111
    ## 20   150
    ## 21    69
    ## 22    49
    ## 23    94
    ## 24    49
    ## 25    83
    ## 26   576
    ## 27    NA
    ## 28   135
    ## 29    83
    ## 30    NA
    ## 31    35
    ## 32   115
    ## 33    57
    ## 34    25
    ## 35    NA
    ## 36    25
    ## 37    NA
    ## 38   583
    ## 39    70
    ## 40    17
    ## 41   341
    ## 42   167
    ## 43    42
    ## 44    47
    ## 45    95
    ## 46   101
    ## 47   131
    ## 48    49
    ## 49    81
    ## 50    18
    ## 51    NA
    ## 52    20
    ## 53    83
    ## 54    25
    ## 55    64
    ## 56    NA
    ## 57   116
    ## 58    NA
    ## 59    55
    ## 60    21
    ## 61    53
    ## 62    28
    ## 63    NA
    ## 64    NA
    ## 65    25
    ## 66   125
    ## 67   115
    ## 68    12
    ## 69    60
    ## 70    NA
    ## 71   245
    ## 72    32
    ## 73   189
    ## 74    25
    ## 75    NA
    ## 76    95
    ## 77    NA
    ## 78   100
    ## 79    NA
    ## 80    25
    ## 81   124
    ## 82   104
    ## 83    24
    ## 84    40
    ## 85    12
    ## 86   170
    ## 87   108
    ## 88    NA
    ## 89    25
    ## 90    28
    ## 91    30
    ## 92    25
    ## 93    27
    ## 94    67
    ## 95   156
    ## 96    NA
    ## 97    49
    ## 98    42
    ## 99    53
    ## 100   NA
    ## 101  452
    ## 102  144
    ## 103   NA
    ## 104   NA
    ## 105   25
    ## 106  200
    ## 107  730
    ## 108  326
    ## 109   73
    ## 110   NA
    ## 111   25
    ## 112   36
    ## 113  152
    ## 114  252
    ## 115   35
    ## 116   30
    ## 117  146
    ## 118   75
    ## 119   52
    ## 120  227
    ## 121   15
    ## 122   NA
    ## 123  506
    ## 124   NA
    ## 125   NA
    ## 126   25
    ## 127   25
    ## 128   50
    ## 129   37
    ## 130   NA
    ## 131   14
    ## 132   37
    ## 133   91
    ## 134   41
    ## 135   25
    ## 136   NA
    ## 137   NA
    ## 138  100
    ## 139   NA
    ## 140   NA
    ## 141   40
    ## 142   48
    ## 143   NA
    ## 144   35
    ## 145   41
    ## 146   36
    ## 147   30
    ## 148  143
    ## 149   30
    ## 150   NA
    ## 151  130
    ## 152   34
    ## 153   NA
    ## 154  162
    ## 155  378
    ## 156   24
    ## 157   NA
    ## 158   25
    ## 159   36
    ## 160   10
    ## 161   36
    ## 162   NA
    ## 163   30
    ## 164   NA
    ## 165   33
    ## 166   NA
    ## 167   25
    ## 168   36
    ## 169   49
    ## 170   25
    ## 171  278
    ## 172   18
    ## 173   40
    ## 174   39
    ## 175  299
    ## 176   15
    ## 177   25
    ## 178   NA
    ## 179   NA
    ## 180   68
    ## 181   25
    ## 182   NA
    ## 183   64
    ## 184  400
    ## 185   45
    ## 186   82
    ## 187   62
    ## 188   90
    ## 189   87
    ## 190   12
    ## 191   21
    ## 192   67
    ## 193   54
    ## 194   15
    ## 195  216
    ## 196   25
    ## 197   20
    ## 198   19
    ## 199   NA
    ## 200   36
    ## 201   94
    ## 202   54
    ## 203  270
    ## 204   NA
    ## 205   24
    ## 206   46
    ## 207  139
    ## 208   NA
    ## 209   NA
    ## 210   59
    ## 211   NA
    ## 212   NA
    ## 213   NA
    ## 214   NA
    ## 215  146
    ## 216  148
    ## 217   31
    ## 218   33
    ## 219   30
    ## 220   60
    ## 221   NA
    ## 222   23
    ## 223   25
    ## 224  200
    ## 225  921
    ## 226   16
    ## 227   NA
    ## 228   NA
    ## 229   15
    ## 230   29
    ## 231   NA
    ## 232   25
    ## 233   NA
    ## 234   NA
    ## 235  271
    ## 236    8
    ## 237   72
    ## 238   11
    ## 239   88
    ## 240   NA
    ## 241  125
    ## 242   25
    ## 243  358
    ## 244   68
    ## 245 1000
    ## 246   30
    ## 247  112
    ## 248   NA
    ## 249   NA
    ## 250   NA
    ## 251   24
    ## 252   40
    ## 253   40
    ## 254   31
    ## 255   15
    ## 256   40
    ## 257   32
    ## 258   NA
    ## 259   53
    ## 260   30
    ## 261   24
    ## 262   NA
    ## 263   33
    ## 264   69
    ## 265   35
    ## 266   NA
    ## 267   NA
    ## 268   36
    ## 269  137
    ## 270   NA
    ## 271   20
    ## 272   39
    ## 273   24
    ## 274   24
    ## 275   32
    ## 276   NA
    ## 277   32
    ## 278   NA
    ## 279   NA
    ## 280   25
    ## 281   56
    ## 282  107
    ## 283  189
    ## 284   25
    ## 285   48
    ## 286  100
    ## 287   88
    ## 288   40
    ## 289   20
    ## 290   69
    ## 291  161
    ## 292   NA
    ## 293   NA
    ## 294   98
    ## 295   74
    ## 296  119
    ## 297   25
    ## 298   92
    ## 299   25
    ## 300   18
    ## 301  297
    ## 302   71
    ## 303   47
    ## 304   NA
    ## 305   13
    ## 306   23
    ## 307   30
    ## 308   24
    ## 309   20
    ## 310   18
    ## 311   67
    ## 312   NA
    ## 313   NA
    ## 314  178
    ## 315  213
    ## 316  659
    ## 317  145
    ## 318   54
    ## 319  168
    ## 320  226
    ## 321   74
    ## 322  302
    ## 323   25
    ## 324   58
    ## 325   32
    ## 326   25
    ## 327  165
    ## 328   NA
    ## 329   NA
    ## 330   48
    ## 331   18
    ## 332   16
    ## 333   30
    ## 334   25
    ## 335   NA
    ## 336   34
    ## 337   NA
    ## 338   83
    ## 339   60
    ## 340   44
    ## 341   99
    ## 342   32
    ## 343   62
    ## 344    5
    ## 345   84
    ## 346  240
    ## 347   NA
    ## 348   NA
    ## 349   30
    ## 350   65
    ## 351   20
    ## 352  991
    ## 353   25
    ## 354   46
    ## 355  110
    ## 356   87
    ## 357   90
    ## 358   49
    ## 359  203
    ## 360  134
    ## 361   98
    ## 362   NA
    ## 363  527
    ## 364   25
    ## 365   NA
    ## 366   79
    ## 367   16
    ## 368   25
    ## 369   25
    ## 370   19
    ## 371   25
    ## 372  534
    ## 373   25
    ## 374   25
    ## 375   95
    ## 376  263
    ## 377   25
    ## 378   NA
    ## 379  269
    ## 380  400
    ## 381  338
    ## 382   30
    ## 383   NA
    ## 384  177
    ## 385   77
    ## 386   NA
    ## 387   75
    ## 388   25
    ## 389   26
    ## 390   NA
    ## 391   NA
    ## 392   68
    ## 393  127
    ## 394   45
    ## 395   25
    ## 396  154
    ## 397  135
    ## 398   79
    ## 399  210
    ## 400   36
    ## 401   49
    ## 402  177
    ## 403   64
    ## 404   10
    ## 405   19
    ## 406  127
    ## 407   30
    ## 408   26
    ## 409   50
    ## 410   23
    ## 411   38
    ## 412   72
    ## 413   25
    ## 414   36
    ## 415   NA
    ## 416   50
    ## 417   NA
    ## 418   NA
    ## 419   NA
    ## 420   25
    ## 421   25
    ## 422   25
    ## 423  254
    ## 424   20
    ## 425   68
    ## 426   25
    ## 427   62
    ## 428   NA
    ## 429  158
    ## 430   25
    ## 431   70
    ## 432   60
    ## 433  170
    ## 434  511
    ## 435   36

``` r
hospitals %>% filter(SOURCEDATE < as.Date("2017-01-01")) %>% select(NAME, BEDS)
```

    ##                                                               NAME BEDS
    ## 1                                     PACIFIC ORANGE HOSPITAL, LLC  101
    ## 2                                        WEST PACES MEDICAL CENTER  294
    ## 3                                              SHASTA COUNTY P H F   15
    ## 4                  CHILDREN'S HOSPITAL COLORADO AT ST JOSEPHS HOSP   NA
    ## 5                                    LODI MEMORIAL HOSPITAL - WEST   24
    ## 6                                           SAUK PRAIRIE MEM HSPTL   36
    ## 7                                  CENTURY HOSPITAL MEDICAL CENTER   34
    ## 8                                         ANSON COMMUNITY HOSPITAL  147
    ## 9                                             MERCY MEDICAL CENTER   NA
    ## 10                             WILLIAM B KESSLER MEMORIAL HOSPITAL  130
    ## 11                 MOUNT CARMEL GUILD BEHAVIORAL HEALTHCARE SYSTEM   40
    ## 12                                 MARIAN BEHAVIORAL HEALTH CENTER   NA
    ## 13                           SAINT CLARES HOSPITAL - SUSSEX CAMPUS   NA
    ## 14                             SILVER SPRINGS RURAL HEALTH CENTERS   NA
    ## 15                           KINDRED HOSPITAL ARIZONA - SCOTTSDALE   62
    ## 16                                         CORRY MEMORIAL HOSPITAL   35
    ## 17                                         RCHP-SIERRA VISTA, INC.   NA
    ## 18                                             EPIC MEDICAL CENTER   10
    ## 19                              UNIVERSITY GENERAL HOSPITAL DALLAS  111
    ## 20           NEW HORIZONS OF TREASURE COAST - MENTAL HEALTH CENTER  150
    ## 21                          EAST TEXAS MEDICAL CENTER MOUNT VERNON   49
    ## 22                                         BAPTIST ORANGE HOSPITAL   94
    ## 23                                     LAKE WHITNEY MEDICAL CENTER   49
    ## 24                                   KINDRED HOSPITAL EAST HOUSTON   83
    ## 25                                HILLCREST BAPTIST MEDICAL CENTER  576
    ## 26                                  KINDRED HOSPITAL NORTH HOUSTON   NA
    ## 27                   HEALTHSOUTH REHABILITATION HOSPITAL OF AUSTIN   83
    ## 28                                        LLANO SPECIALTY HOSPITAL   NA
    ## 29              UNIVERSITY OF CALIF, SAN DIEGO MEDICAL CTR D/P APH   35
    ## 30                             SUTTER MEDICAL CENTER OF SANTA ROSA  115
    ## 31                         DOMINICAN HOSPITAL-SANTA CRUZ/FREDERICK   57
    ## 32                                    STORY CITY MEMORIAL HOSPITAL   25
    ## 33                                                REID HOSPITAL-ER   NA
    ## 34                                        HOWARD MEMORIAL HOSPITAL   25
    ## 35                                SILOAM SPRINGS MEMORIAL HOSPITAL   NA
    ## 36      UNIVERSITY OF COLORADO HEALTH AT MEMORIAL HOSPITAL CENTRAL  583
    ## 37                                       PARKWAY REGIONAL HOSPITAL   70
    ## 38                                  ALBANY AREA HOSPITAL & MED CTR   17
    ## 39                                           MERCY HOSPITAL JOPLIN  341
    ## 40                                  KINDRED HOSPITAL - KANSAS CITY  167
    ## 41                                 SHRINERS HOSPITALS FOR CHILDREN   42
    ## 42                                              SAC-OSAGE HOSPITAL   47
    ## 43                                  VALDESE GENERAL HOSPITAL, INC.  131
    ## 44                             PUNGO DISTRICT HOSPITAL CORPORATION   49
    ## 45                                           DAVIE COUNTY HOSPITAL   81
    ## 46                                    ST MARY'S COMMUNITY HOSPITAL   18
    ## 47                                 OREGON STATE HOSPITAL  PORTLAND   NA
    ## 48                               NORTHEAST REHABILITATION HOSPITAL   20
    ## 49                             SIERRA VISTA REGIONAL HEALTH CENTER   83
    ## 50                                              WESTFIELD HOSPITAL   25
    ## 51                               SAINT CATHERINE REGIONAL HOSPITAL   64
    ## 52                    ACUITY SPECIALTY HOSPITAL OF ARIZONA AT MESA   NA
    ## 53                                           QUINCY MEDICAL CENTER  116
    ## 54                                 DOUGLAS COMMUNITY HOSPITAL, INC   NA
    ## 55  PARKVIEW ADVENTIST MEDICAL CENTER : PARKVIEW MEMORIAL HOSPITAL   55
    ## 56                                       MAYO CLINIC HEALTH SYS CF   21
    ## 57                                        BARNWELL COUNTY HOSPITAL   53
    ## 58                                      SNOQUALMIE VALLEY HOSPITAL   28
    ## 59                            SAINT JOSEPH HOSPITAL - SOUTH CAMPUS   NA
    ## 60                                BLESSING HOSPITAL AT 14TH STREET   NA
    ## 61                                      MENDOTA COMMUNITY HOSPITAL   25
    ## 62                           SOUTHWEST HOSPITAL AND MEDICAL CENTER  125
    ## 63                               CLEARVIEW REGIONAL MEDICAL CENTER  115
    ## 64                      ASCENSION GONZALES REHABILITATION HOSPITAL   12
    ## 65                                     HUEY P. LONG MEDICAL CENTER   60
    ## 66                 LSU BOGALUSA MEDICAL CENTER (OUTPATIENT CAMPUS)   NA
    ## 67                    OPELOUSAS GENERAL HEALTH SYSTEM SOUTH CAMPUS  245
    ## 68                                      CORCORAN DISTRICT HOSPITAL   32
    ## 69                              DOCTORS MEDICAL CENTER - SAN PABLO  189
    ## 70              UNIVERSITY MCDUFFIE COUNTY REGIONAL MEDICAL CENTER   25
    ## 71                                   PROMISE HOSPITAL OF ASCENSION   NA
    ## 72                      CALIFORNIA PACIFIC MED CTR-CALIFORNIA EAST   95
    ## 73                                   JOAN GLANCY MEMORIAL HOSPITAL   NA
    ## 74                                      DEVEREUX TREATMENT NETWORK  100
    ## 75                                              LSU MEDICAL CENTER   NA
    ## 76                                                 SHAWANO MED CTR   25
    ## 77                                  MEMORIAL HEALTH CENTER CLINICS  124
    ## 78                                  ORANGE CITY MUNICIPAL HOSPITAL  104
    ## 79                                    VIBRA HOSPITAL OF FORT WAYNE   24
    ## 80                                     MERCY HOSPITAL INDEPENDENCE   40
    ## 81                              THE SURGICAL HOSPITAL OF JONESBORO   12
    ## 82                                       PARKER ADVENTIST HOSPITAL  170
    ## 83                                          WALTER P CARTER CENTER  108
    ## 84                       KIDSPEACE NATIONAL CENTERS OF NEW ENGLAND   NA
    ## 85                                   SCHOOLCRAFT MEMORIAL HOSPITAL   25
    ## 86                             ROGERS CITY REHABILITATION HOSPITAL   28
    ## 87                                       ESSENTIA HEALTH SANDSTONE   30
    ## 88                         MERCY FRANCISCAN HOSPITAL WESTERN HILLS  156
    ## 89                                         MERCY ST THERESA CENTER   NA
    ## 90                                 CENTRAL VALLEY GENERAL HOSPITAL   49
    ## 91                    ANAHEIM GENERAL HOSPITAL - BUENA PARK CAMPUS   42
    ## 92                                           SATILLA PARK HOSPITAL   53
    ## 93                                      CHARLTON MEMORIAL HOSPITAL   NA
    ## 94                 EAST LOUISIANA STATE HOSPITAL-GREENWELL SPRINGS  452
    ## 95                              BROWN MEMORIAL CONVALESCENT CENTER  144
    ## 96                                       KAILO BEHAVIORAL HOSPITAL   NA
    ## 97                                      EARL K LONG MEDICAL CENTER   NA
    ## 98                                        STEWART WEBSTER HOSPITAL   25
    ## 99               PROVIDENCE LITTLE CO OF MARY SUBACUTE CARE CENTER  200
    ## 100                                      TEMPLE COMMUNITY HOSPITAL  730
    ## 101                                      KAISER FND HOSP - ANAHEIM  326
    ## 102              SADDLEBACK MEMORIAL MEDICAL CENTER - SAN CLEMENTE   73
    ## 103                              NEW HORIZONS LANIER PARK HOSPITAL   NA
    ## 104                             PRAIRIE DU CHIEN MEMORIAL HOSPITAL   25
    ## 105                                   RAINBOW MENTAL HLTH FACILITY   36
    ## 106                                CRITTENDEN HOSPITAL ASSOCIATION  152
    ## 107                                   ST. VINCENT DOCTORS HOSPITAL  252
    ## 108                              HOT SPRINGS REHABILITATION CENTER   35
    ## 109     GLADYS SPELLMAN SPECIALTY HOSPITAL AND NURSING CARE CENTER   30
    ## 110                                    MAINEGENERAL MEDICAL CENTER  146
    ## 111                              MAINEGENERAL MEDICAL CENTER-SETON   75
    ## 112                   SAINT ANDREWS HOSPITAL AND HEALTHCARE CENTER   52
    ## 113                                            ST JOSEPHS HOSPITAL  227
    ## 114                                        GARRARD COUNTY HOSPITAL   15
    ## 115                                      PRESTON MEMORIAL HOSPITAL   25
    ## 116                                           SPOONER HOSPITAL SYS   25
    ## 117                                      NORMAN SPECIALTY HOSPITAL   50
    ## 118                            MEMORIAL HOSPITAL & PHYSICIAN GROUP   37
    ## 119                                        JACKSON COUNTY HOSPITAL   NA
    ## 120                                       REAGAN MEMORIAL HOSPITAL   14
    ## 121                             EAST TEXAS MEDICAL CENTER - GILMER   37
    ## 122                                    RENAISSANCE HOSPITAL GROVES   91
    ## 123                                       FAITH COMMUNITY HOSPITAL   41
    ## 124                          GOOD SHEPHERD MEDICAL CENTER - LINDEN   25
    ## 125                                   RENAISSANCE HOSPITAL TERRELL   NA
    ## 126                                 SHELBY REGIONAL MEDICAL CENTER   NA
    ## 127                                                  MARIAN CENTER   NA
    ## 128                                              EDWARD PLAINFIELD   NA
    ## 129                               ATRIUM MEDICAL CENTER AT CORINTH   40
    ## 130                         ALLEGIANCE HEALTH CENTER PERMIAN BASIN   48
    ## 131        BAYLOR INSTITUTE FOR REHABILITATION AT NORTHWEST DALLAS   NA
    ## 132                                        MENTAL HEALTH INSTITUTE   35
    ## 133                          BOWDON AREA HOSPITAL AND REHAB CENTER   41
    ## 134                    CHARTER BEHAVIORAL HEALTH SYSTEM OF ATLANTA   36
    ## 135                            SELECT SPECIALTY HOSPITAL - ATLANTA   30
    ## 136                                       SUMTER REGIONAL HOSPITAL  143
    ## 137                                          SEASIDE HEALTH SYSTEM   30
    ## 138          SOUTHWEST WASHINGTON MEDICAL CENTER - MEMORIAL CAMPUS   NA
    ## 139                                  PUGET SOUND BEHAVIORAL HEALTH  130
    ## 140                                            DAYBREAK OF SPOKANE   34
    ## 141                     PHYSICIAN'S CHOICE HOSPITAL - FREMONT, LLC   NA
    ## 142                                      SELECT SPECIALTY HOSPITAL   24
    ## 143                                    COPPER BASIN MEDICAL CENTER   25
    ## 144                                 UNITED REGIONAL MEDICAL CENTER   36
    ## 145                        METHODIST HEALTHCARE - FAYETTE HOSPITAL   10
    ## 146                                           ROANE MEDICAL CENTER   36
    ## 147                                MIDDLE TENNESSEE MEDICAL CENTER   NA
    ## 148                                      HUMBOLDT GENERAL HOSPITAL   30
    ## 149                             BAPTIST HOSPITAL OF EAST TENNESSEE   NA
    ## 150                           SELECT SPECIALTY HOSPITAL - OAK HILL   33
    ## 151                                     BAPTIST HOSPITAL FOR WOMEN   NA
    ## 152                                          ALLEN COUNTY HOSPITAL   25
    ## 153                                 NORTH TEXAS COMMUNITY HOSPITAL   36
    ## 154                        EAST TEXAS MEDICAL CENTER - CLARKSVILLE   49
    ## 155                               HUNT REGIONAL COMMUNITY HOSPITAL   25
    ## 156                         MIAMI HEART INSTITUTE & MEDICAL CENTER  278
    ## 157                 NIX COMMUNITY GENERAL HOSPITAL OF DILLEY TEXAS   18
    ## 158                               HEREFORD REGIONAL MEDICAL CENTER   40
    ## 159                                         ST. ANTHONY'S HOSPITAL   39
    ## 160                                   SPRING BRANCH MEDICAL CENTER  299
    ## 161                                RANKIN COUNTY HOSPITAL DISTRICT   15
    ## 162                                MARTIN COUNTY HOSPITAL DISTRICT   25
    ## 163                                  PRISTINE HOSPITAL OF PASADENA   NA
    ## 164                                                 GRACE HOSPITAL   NA
    ## 165                                      BAYLOR SPECIALTY HOSPITAL   68
    ## 166                                    CLARINDA MUNICIPAL HOSPITAL   25
    ## 167                     FORT VALLEY STATE UNIVERSITY HEALTH SYSTEM   NA
    ## 168                                                  PRIDE MEDICAL   64
    ## 169                                    SOUTHWESTERN STATE HOSPITAL  400
    ## 170                                       SMITH NORTHVIEW HOSPITAL   45
    ## 171                                        RIVER PARISHES HOSPITAL   62
    ## 172                          SCI-WAYMART FORENSIC TREATMENT CENTER   90
    ## 173                 LIFECARE HOSPITALS OF PITTSBURGH - MONROEVILLE   87
    ## 174                                      TILDEN COMMUNITY HOSPITAL   12
    ## 175                                      MONTROSE GENERAL HOSPITAL   21
    ## 176                                          SPECIAL CARE HOSPITAL   67
    ## 177                                 MERCY CONTINUING CARE HOSPITAL   54
    ## 178                                  DAKOTA PLAINS SURGICAL CENTER   15
    ## 179                                     WELLSTAR PAULDING HOSPITAL  216
    ## 180                                     FALL RIVER HEALTH SERVICES   25
    ## 181                                     NORTH VALLEY HEALTH CENTER   20
    ## 182                      UVA KLUGE CHILDRENS REHABILITATION CENTER   19
    ## 183                          VERNON M. GEDDY JR. OUTPATIENT CENTER   NA
    ## 184            CHILDREN'S HOSPITAL OF RICHMOND AT VCU (BROOK ROAD)   36
    ## 185                                         MARLBORO PARK HOSPITAL   94
    ## 186                                   ROCKINGHAM MEMORIAL HOSPITAL  270
    ## 187           PEACEHEALTH ST JOHN MEDICAL CENTER - BROADWAY CAMPUS   NA
    ## 188                                   MARK REED HEALTH CARE CLINIC   24
    ## 189                                 DEWITT ARMY COMMUNITY HOSPITAL   46
    ## 190                                            NORTH SIDE HOSPITAL  139
    ## 191                       SWEDISH MEDICAL CENTER - ISSAQUAH CAMPUS   NA
    ## 192                                 GROUP HEALTH EASTSIDE HOSPITAL   NA
    ## 193                                  BLUE MOUNTAIN RECOVERY CENTER   59
    ## 194                            GOOD SAMARITAN REGIONAL HLTH CENTER  146
    ## 195                                            OAK FOREST HOSPITAL  148
    ## 196                                       CHATTOOGA MEDICAL CENTER   31
    ## 197                                   BRUNSWICK COMMUNITY HOSPITAL   60
    ## 198                                  FLORENCE COMMUNITY HEALTHCARE   NA
    ## 199                                      FLORALA MEMORIAL HOSPITAL   23
    ## 200                                DOCTORS HOSPITAL OF NELSONVILLE   25
    ## 201                           CAMDEN COUNTY HEALTH SERVICES CENTER  200
    ## 202                              SOUTHWEST REGIONAL MEDICAL CENTER  921
    ## 203                                       RINGGOLD COUNTY HOSPITAL   16
    ## 204                               NEW JERSEY STATE PRISON HOSPITAL   NA
    ## 205                                SOUTH JERSEY HEALTH CARE CENTER   NA
    ## 206                           VIRTUA WEST JERSEY HOSPITAL - CAMDEN   15
    ## 207  JOHN BROOKS RECOVERY CENTER - RESIDENT DRUG TREATMENT (WOMEN)   29
    ## 208                                INSPIRA HEALTH CENTER BRIDGETON   NA
    ## 209                              CRAWFORD COUNTY MEMORIAL HOSPITAL   25
    ## 210        MERWICK REHABILITATION HOSPITAL AND NURSING CARE CENTER   NA
    ## 211                                   MEDICAL CENTER OF NEWARK LLC   NA
    ## 212                                        GOOD SAMARITAN HOSPITAL  271
    ## 213                                      HIND GENERAL HOSPITAL LLC    8
    ## 214                                                 MAJOR HOSPITAL   72
    ## 215                                        CHEYENNE RIVER HOSPITAL   11
    ## 216                                 SIDNEY REGIONAL MEDICAL CENTER   88
    ## 217                                             GOOD HANDS MEDICAL   NA
    ## 218                                WOODBRIDGE DEVELOPMENTAL CENTER  125
    ## 219                                     PIONEER COMMUNITY HOSPITAL   25
    ## 220                                        TORRANCE STATE HOSPITAL  358
    ## 221                         NEW LIFECARE HOSPITAL OF MECHANICSBURG   68
    ## 222                      SELECT SPECIALTY HOSPITAL - GROSSE POINTE   30
    ## 223                            CHILDREN'S HOSPITAL NAVICENT HEALTH  112
    ## 224                                          RINCON MEDICAL CENTER   NA
    ## 225                                   EFFICACY HEALTH SERVICES LLC   NA
    ## 226                            CALCASIEU OAKS PSYCHIATRIC HOSPITAL   24
    ## 227                                      SELECT SPECIALTY HOSPITAL   40
    ## 228                                   TULSA-AMG SPECIALTY HOSPITAL   40
    ## 229                           SELECT SPECIALTY HOSPITAL-FORT WAYNE   32
    ## 230             KINDRED HOSPITAL - LAS VEGAS AT DESERT SPRINGS HOS   NA
    ## 231                            CHRISTUS DUBUIS HOSPITAL OF HOUSTON   30
    ## 232                             VICTORY MEDICAL CENTER CRAIG RANCH   24
    ## 233                       SELECT SPECIALTY HOSPITAL-ZANESVILLE INC   NA
    ## 234                                  KINDRED HOSPITAL CENTRAL OHIO   33
    ## 235                            BUCKHEAD AMBULATORY SURGICAL CENTER   NA
    ## 236                               CRESCENT CITY SPECIALTY HOSPITAL   NA
    ## 237                                    WESTBURY COMMUNITY HOSPITAL  137
    ## 238                           PELICAN REHABILITATION HOSPITAL, LLC   NA
    ## 239                       SELECT SPECIALTY HOSPITAL - GRAND RAPIDS   20
    ## 240                             KINDRED HOSPITAL - DELAWARE COUNTY   39
    ## 241                                   ACUITY SPECIALTY OHIO VALLEY   24
    ## 242                      MADONNA REHABILITATION SPECIALTY HOSPITAL   32
    ## 243                       MINDEN FAMILY MEDICINE AND COMPLETE CARE   NA
    ## 244                                        GIBSON GENERAL HOSPITAL   32
    ## 245                               GOLDEN PLAINS COMMUNITY HOSPITAL   25
    ## 246                                   BROWNSVILLE DOCTORS HOSPITAL   56
    ## 247                                           TEXAS RURAL HOSPITAL  107
    ## 248                              CORPUS CHRISTI SPECIALTY HOSPITAL  189
    ## 249                                      DOCTORS MEMORIAL HOSPITAL   48
    ## 250                       SELECT SPECIALTY HOSPITAL - SOUTH DALLAS  100
    ## 251                                    HIGH POINT TREATMENT CENTER   88
    ## 252                               TRIUMPH HOSPITAL CENTRAL HOUSTON   40
    ## 253                              VICTORY MEDICAL CENTER SOUTHCROSS   20
    ## 254                            BAYLOR MEDICAL CENTER AT WAXAHACHIE   69
    ## 255                                             ALTUS LUMBERTON LP   NA
    ## 256                                     WESTERVILLE MEDICAL CAMPUS   NA
    ## 257                                     RIVERSIDE GENERAL HOSPITAL   98
    ## 258                                NORTH ALABAMA REGIONAL HOSPITAL   74
    ## 259                                          SACRED HEART HOSPITAL  119
    ## 260                                           ST JOSEPH'S HOSPITAL   25
    ## 261                                    PEAK VIEW BEHAVIORAL HEALTH   92
    ## 262                                  PEACH REGIONAL MEDICAL CENTER   25
    ## 263                                            POLK MEDICAL CENTER   18
    ## 264                            NORTHWEST GEORGIA REGIONAL HOSPITAL  297
    ## 265             TY COBB HEALTHCARE SYSTEM - COBB MEMORIAL HOSPITAL   71
    ## 266                                           DOOLY MEDICAL CENTER   47
    ## 267                          WOODLANDS PSYCHIATRIC HEALTH FACILITY   NA
    ## 268                                 GREEN CLINIC SURGICAL HOSPITAL   13
    ## 269                                     BARSTOW COMMUNITY HOSPITAL   23
    ## 270                  ST VINCENT SETON SPECIALTY HOSPITAL LAFAYETTE   30
    ## 271                                      SEASIDE BEHAVIORAL CENTER   24
    ## 272                              BOGALUSA - AMG SPECIALTY HOSPITAL   20
    ## 273                           COMPASS BEHAVIORAL CENTER OF CROWLEY   18
    ## 274                                         BELLWOOD HEALTH CENTER   67
    ## 275               FALLBROOK HOSP DISTRICT SKILLED NURSING FACILITY   NA
    ## 276      CHARITY HOSPITAL AND MEDICAL CENTER OF LA. AT NEW ORLEANS   NA
    ## 277                                     BOULDER COMMUNITY HOSPITAL  178
    ## 278                              KAISER FND HOSP - HAYWARD/FREMONT  213
    ## 279                                       SUTTER MEMORIAL HOSPITAL  659
    ## 280                 BRANDEL MANOR - D/P SNF OF EMANUEL MEDICAL CTR  145
    ## 281                                           FLOYD MEDICAL CENTER   54
    ## 282                                  EMORY DUNWOODY MEDICAL CENTER  168
    ## 283                                               WOMAN'S HOSPITAL  226
    ## 284                       WALTER OLIN MOSS REGIONAL MEDICAL CENTER   74
    ## 285                              CHRISTUS SCHUMPERT MEDICAL CENTER  302
    ## 286                                                  RIPON MED CTR   25
    ## 287                                        KIOWA DISTRICT HOSPITAL   58
    ## 288                                  PIKE COUNTY MEMORIAL HOSPITAL   32
    ## 289                         REGENCY HOSPITAL OF NORTHWEST ARKANSAS   25
    ## 290                            ST. MARY - ROGERS MEMORIAL HOSPITAL  165
    ## 291                                  NEA BAPTIST MEMORIAL HOSPITAL   NA
    ## 292               ACUITY SPECIALTY HOSPITAL OF ARIZONA AT SUN CITY   NA
    ## 293                               HEARTLAND SURGICAL SPEC HOSPITAL   48
    ## 294                                       NICHOLAS COUNTY HOSPITAL   18
    ## 295          AROOSTOOK MEDICAL CENTER - COMMUNITY GENERAL DIVISION   16
    ## 296                                                MERCY WESTBROOK   30
    ## 297                                 IONIA COUNTY MEMORIAL HOSPITAL   25
    ## 298                                ST JOSEPH MERCY HOSPITAL-SALINE   NA
    ## 299                                          LIVINGSTON HEALTHCARE   NA
    ## 300                               FRANKLIN REGIONAL MEDICAL CENTER   83
    ## 301                                      CRAWLEY MEMORIAL HOSPITAL   60
    ## 302                                    NYE REGIONAL MEDICAL CENTER   44
    ## 303                                     HAWAII MEDICAL CENTER EAST  240
    ## 304                                      BETHESDA ARROW SPRINGS-ER   NA
    ## 305                                     JOHN & MARY KIRBY HOSPITAL   NA
    ## 306                                        PACIFIC SHORES HOSPITAL   30
    ## 307                               SELECT SPECIALTY HOSPITAL-DENVER   65
    ## 308                             SPECIALTY LTCH HOSPITAL OF HAMMOND   20
    ## 309                                 LANTERMAN DEVELOPMENTAL CENTER  991
    ## 310                                          SCOTT COUNTY HOSPITAL   25
    ## 311                                   LIFESTREAM BEHAVIORAL CENTER   46
    ## 312                                          EDWARD WHITE HOSPITAL  110
    ## 313                                       MANNING GENERAL HOSPITAL   87
    ## 314                SIOUX CENTER COMMUNITY HOSPITAL & HEALTH CENTER   90
    ## 315                                   HENRIETTA D GOODALL HOSPITAL   49
    ## 316                       HENRY FORD MACOMB HOSPITAL-WARREN CAMPUS  203
    ## 317                                       ST. MARY'S HEALTH CENTER  134
    ## 318                           MINERAL AREA REGIONAL MEDICAL CENTER   98
    ## 319                        ST. ALEXIUS HOSPITAL - JEFFERSON CAMPUS   NA
    ## 320                      ST. ALEXIUS HOSPITAL - FOREST PARK CAMPUS  527
    ## 321                                         HEDRICK MEDICAL CENTER   25
    ## 322                   POPLAR BLUFF REGIONAL MEDICAL CENTER - SOUTH   NA
    ## 323                                 MISSOURI REHABILITATION CENTER   79
    ## 324               SOUTHWEST MISSOURI PSYCHIATRIC REHABILITATION CT   16
    ## 325               BEATRICE COMMUNITY HOSPITAL & HEALTH CENTER, INC   25
    ## 326                                         TRINITY MEDICAL CENTER  534
    ## 327                                      PIONEER MEMORIAL HOSPITAL   25
    ## 328                                            ST ANTHONY HOSPITAL   25
    ## 329                           VIRTUA WEST JERSEY HOSPITAL - BERLIN   95
    ## 330                                              LAKEWOOD HOSPITAL  263
    ## 331                                        TROY COMMUNITY HOSPITAL   25
    ## 332                                 GOTTSCHE REHABILITATION CENTER   NA
    ## 333                                 SAINT MARYS HOSPITAL - PASSAIC  269
    ## 334                                   ESSEX COUNTY HOSPITAL CENTER  400
    ## 335                         UNIVERSITY MEDICAL CENTER AT PRINCETON  338
    ## 336                                       EWING RESIDENTIAL CENTER   30
    ## 337                                   CARSON TAHOE DAYTON HOSPITAL   NA
    ## 338                                            MONTGOMERY HOSPITAL  177
    ## 339             KING'S DAUGHTERS' HOSPITAL AND HEALTH SERVICES,THE   77
    ## 340           DRUG REHABILITATION INCORPORATED - DAY ONE RESIDENCE   NA
    ## 341                                       EMORY-ADVENTIST HOSPITAL   75
    ## 342                                      CALHOUN MEMORIAL HOSPITAL   25
    ## 343                                           HOLY INFANT HOSPITAL   26
    ## 344                      SOUTHEASTHEALTH CENTER OF REYNOLDS COUNTY   NA
    ## 345                            WILLIAM N WISHARD MEMORIAL HOSPITAL   NA
    ## 346                                    COMMUNITY WESTVIEW HOSPITAL   68
    ## 347                          TENNOVA HEALTHCARE - MCNAIRY REGIONAL   45
    ## 348                                  RENVILLE COUNTY HOSP & CLINCS   25
    ## 349                                SMYTH COUNTY COMMUNITY HOSPITAL  154
    ## 350                                     JOHNSTON MEMORIAL HOSPITAL  135
    ## 351                                    W J BARGE MEMORIAL HOSPITAL   79
    ## 352                               METHODIST EXTENDED CARE HOSPITAL   36
    ## 353                                JOHNSON CITY SPECIALTY HOSPITAL   49
    ## 354                                  BEDFORD COUNTY MEDICAL CENTER  177
    ## 355                                          METROPOLITAN HOSPITAL   64
    ## 356                                   PARK PLACE SURGICAL HOSPITAL   10
    ## 357                                    WOODLANDS BEHAVIORAL CENTER   19
    ## 358                             MAINEGENERAL MEDICAL CENTER-THAYER  127
    ## 359                                     SPARROW SPECIALTY HOSPITAL   30
    ## 360                       SOUTHWEST REGIONAL REHABILITATION CENTER   26
    ## 361                               SUMMA WADSWORTH-RITTMAN HOSPITAL   50
    ## 362                                   ELLSWORTH MUNICIPAL HOSPITAL   23
    ## 363                                     ANAMOSA COMMUNITY HOSPITAL   38
    ## 364                        KINDRED HOSPITAL PITTSBURGH NORTH SHORE   72
    ## 365                                            MID-VALLEY HOSPITAL   25
    ## 366                                  NORTH ADAMS REGIONAL HOSPITAL   36
    ## 367                       EXCELA HEALTH WESTMORELAND HOSP JEANETTE   NA
    ## 368                                   NORTH GEORGIA MEDICAL CENTER   50
    ## 369                                BRADLEY CENTER OF SAINT FRANCIS   NA
    ## 370                        US AIR FORCE HOSPITAL-GLENDALE - CLOSED   NA
    ## 371                                            SAN CARLOS HOSPITAL   NA
    ## 372                                  LOUIS SMITH MEMORIAL HOSPITAL   25
    ## 373                              ST MARY'S GOOD SAMARITAN HOSPITAL   25
    ## 374                                     MADISON COMMUNITY HOSPITAL   25
    ## 375                                             COMMUNITY HOSPITAL  254
    ## 376                                   PRAIRIE RIDGE HOSP HLTH SERV   20
    ## 377                              BAPTIST REHABILITATION-GERMANTOWN   68
    ## 378                           ST JOSEPH'S HOSPITAL & HEALTH CENTER   25
    ## 379                        DOCTORS DIAGNOSTIC CENTER- WILLIAMSBURG   NA
    ## 380                                       SENTARA BAYSIDE HOSPITAL  158
    ## 381                               CARILION GILES MEMORIAL HOSPITAL   25
    ## 382                                    LEE REGIONAL MEDICAL CENTER   70
    ## 383                                CONTINUOUS CARE CENTER OF TULSA   60
    ## 384                            MERCY FRANCISCAN HOSPITAL - MT AIRY  170
    ## 385                                            ST. JOSEPH HOSPITAL  511
    ## 386                                HAYWOOD PARK COMMUNITY HOSPITAL   36

For hospitals, both of these filters don’t confirm a hypothesis.
Sometimes there are context clues within the dataset as to why certain
data is unavailable, and other times there are not. We don’t have these
context clues for BEDS in the hospitals dataset. It’s likely that NAs in
BEDS are simply there because the data aggregators couldn’t find the
data.

Apply a few additional filter conditions to test your hypothesis as to
why there are missing values in the variable you selected.

``` r
#Apply filter conditions here. 
```

What did you learn from your applying your own filter conditions?

``` r
Fill your response here. 
```

Does the data dictionary confirm your hypothesis? What does it say?

``` r
Fill your response here. 
```

How might these missing values impact your data analysis? Why might it
be important to remember that these values are missing as we move
forward?

``` r
Fill your response here. 
```

### Summarize Filtered Categorical Data

Since we learned above that one criteria for being designated as a
Critical Access hospital is that the hospital must have 25 or fewer
inpatient beds, we may want to see how the values for BEDS change when
we filter to just those observations representing critical access
hospitals. Below I will do this, select the BEDS variable, and call
summary().

``` r
#df %>% filter(CATEGORICAL_VARIABLE == "VALUE") %>% select(NUMERIC_VARIABLE) %>% summary() 
hospitals %>% filter(TYPE == "CRITICAL ACCESS") %>% select(BEDS) %>% summary()
```

    ##       BEDS      
    ##  Min.   :  3.0  
    ##  1st Qu.: 22.0  
    ##  Median : 25.0  
    ##  Mean   : 27.7  
    ##  3rd Qu.: 25.0  
    ##  Max.   :286.0  
    ##  NA's   :42

We can see that there are some hospitals in the US that have been
designated as critical access hospitals that have more than 25 beds.
Since this does not align with the criteria for critical access
hospitals that we discovered in our research, it is likely something
that we will want to investigate further.

For your own dataset, select one of the values from the categorical
variable that you worked with in Step 2. Filter the dataset to the rows
representing that value, select a numeric variable to explore, and then
call
summary().

``` r
#Uncomment the appropriate lines below, and fill in your data frame, variables, and value. 
#_____ %>% filter(_____ == "_____") %>% select(_____) %>% summary()
```

What insight can you draw from calling summary() on your own filtered
dataset?

``` r
Fill your response here. 
```

### Summarize Filtered Numeric Data

We may also want to see at which states have hospitals with more than
1500 beds. To do so, we would filter the data to those observations
where BEDS is greater than 1500, and then we would check the distinct()
STATES remaining in the
data.

``` r
#df %>% filter(NUMBERIC_VARIABLE > VALUE) %>% distinct(CATEGORICAL_VARIABLE)
hospitals %>% filter(BEDS > 1500) %>% distinct(STATE)
```

    ##   STATE
    ## 1    CT
    ## 2    PA

From this exercise, we can see that only two states have hospitals with
more than 1500 beds available.

Select a numeric variable in your dataset that represents the extent or
scale of the issue you are studying. Pick a number that you believe
serves as a good indicator that this issue is at a notable extent or
scale, and filter the dataset to all the rows greater than (or less
than) this number. Check the remaining distinct values in a categorical
variable in the
dataset.

``` r
#Uncomment the appropriate lines below, and fill in your data frame, variables, condition, and value. 
#_____ %>% filter(_____ _____ _____) %>% distinct(_____)
```

What insight can you draw from calling distinct on the filtered data?

``` r
Fill your response here. 
```

### Group Common Values and Summarize

Select a categorical variable that you would like to group your data by,
so that you can summarize some statistics across each grouping. You may
group your data by a particular year, by a particular location (such as
a state or a region), or by a particular category.

*count()* counts the number of times each value appears in a variable.
In other words, this function groups observations that share a common
variable value and then counts the number of observations in each group.
In the hospitals dataset, if I wanted to know the number of hospitals of
each TYPE in the dataset, I would count by TYPE. Below we calculate the
number of hospitals per state by counting by STATE.

``` r
#df %>% count(CATEGORICAL_VARIABLE)
hospitals %>% count(STATE)
```

    ## # A tibble: 57 x 2
    ##    STATE     n
    ##    <chr> <int>
    ##  1 AK       32
    ##  2 AL      133
    ##  3 AR      122
    ##  4 AS        1
    ##  5 AZ      144
    ##  6 CA      570
    ##  7 CO      119
    ##  8 CT       41
    ##  9 DC       15
    ## 10 DE       15
    ## # … with 47 more rows

Select a variable in your dataset, and count the number of times each
value appears in the variable, or how many observations are associated
with each value in that
variable.

``` r
#Uncomment the appropriate lines below, and fill in your data frame and variable. 
#_____ %>% count(_____)
```

What insight can you draw from counting?

``` r
Fill your response here. 
```

Sometimes, we want to do more than count the number of variables in each
grouping. For instance, we may want to group the variables and then
perform a calculation within each group. In such cases, we can call
*group\_by()* to aggregate the observations with common variable values
into groups. Then we will call *summarize()* to perform a calculation
within each of those groups. *summarize()* takes a set of values and a
calculation method and returns a single value. When called in
conjunction with group\_by(), it takes a set of values for each group
and a calculation method and returns a single value for each group.

For the hospitals dataset, we will group the observations by state and
then use summarize to calculate the sum of beds per
state.

``` r
#df %>% group_by(CATEGORICAL_VARIABLE) %>% summarize(NEW_VARIABLE_NAME = sum(NUMBERIC_VARIABLE, na.rm = TRUE)) %>% ungroup()
hospitals %>% group_by(STATE) %>% summarize(state_beds = sum(BEDS, na.rm = TRUE)) %>% ungroup()
```

    ## # A tibble: 57 x 2
    ##    STATE state_beds
    ##    <chr>      <int>
    ##  1 AK          1813
    ##  2 AL         19000
    ##  3 AR         11699
    ##  4 AS             0
    ##  5 AZ         15452
    ##  6 CA        104034
    ##  7 CO         11899
    ##  8 CT          9451
    ##  9 DC          4304
    ## 10 DE          2780
    ## # … with 47 more rows

Select a numeric variable in your dataset to summarize by the same
variable that you counted above. For instance, you may want to sum the
total number of reports in a given year, or find the average number of
cases reported in a certain
state.

``` r
#Uncomment the appropriate lines below, and fill in your data frame, variables, and summarize variable name, and math function. 
#_____ %>% group_by(_____) %>% summarize(_____ = _____(_____, na.rm = TRUE)) %>% ungroup()
```

> Notice that I close each of these calls with ungroup(). When we
> group\_by() a variable, any subsequent function calls will continue to
> be performed on the grouped data, unless we ungroup() it. This can be
> important if we want to filter to specific values after we summarize()
> the data. Assuming that we don’t want to perform a filter operation
> within each group but on the entire new dataframe created after
> summarzing, we need to ungroup() the data before performing the
> filter() operation. In the function calls above, it is not as
> important to call ungroup() because we are not performing more
> functions after summarizing. However, I left the call in so that we
> can get in the habit of remembering to ungroup() when appropriate.

What insight can you draw from grouping and summarizing?

``` r
Fill your response here. 
```

Combine any combination of the 5 verbs we learned in class this week
(select, filter, group by, summarize, or count) to explore your dataset
further. You may also use arrange, summary, or distinct.

``` r
#Fill your function here. 
```

What insight can you draw from running this function?

``` r
Fill your response here. 
```

Combine any combination of the 5 verbs we learned in class this week
(select, filter, group by, summarize, or count) to explore your dataset
further. You may also use arrange, summary, or distinct.

``` r
#Fill your function here. 
```

What insight can you draw from running this function?

``` r
Fill your response here. 
```

Combine any combination of the 5 verbs we learned in class this week
(select, filter, group\_by, summarize, or count) to explore your dataset
further. You may also use arrange, summary, or distinct.

``` r
#Fill your function here. 
```

What insight can you draw from running this function?

``` r
Fill your response here. 
```

## Other Useful Functions (This is only for reference.)

### Add New Variables

*mutate()* creates a new variable in our dataset and fills it with a
value produced from a formula that we provide.

``` r
#General format
#df %>% mutate(NEW_VARIABLE_NAME = [FORMULA GOES HERE])

#Some more specific examples
#df %>% mutate(Total = VARIABLE_NAME1 + VARIABLE_NAME2 + VARIABLE_NAME3)
#df %>% mutate(Difference = VARIABLE_NAME1 - VARIABLE_NAME2)
#df %>% mutate(Average = VARIABLE_NAME1 + VARIABLE_NAME2 + VARIABLE_NAME3 / 3)
#df %>% mutate(New_String = paste(VARIABLE_NAME1, VARIABLE_NAME2, sep=" ") Remember that we use paste to concatenate strings.

#head() only displays the first six rows
hospitals %>% mutate(BEDS_PER_POP = BEDS/POPULATION) %>% select(NAME, BEDS_PER_POP) %>% head(10)
```

    ##                                               NAME BEDS_PER_POP
    ## 1                    COLORADO RIVER MEDICAL CENTER            1
    ## 2                 ALVARADO HOSPITAL MEDICAL CENTER            1
    ## 3                ALVARADO PARKWAY INSTITUTE B.H.S.            1
    ## 4                     KINDRED HOSPITAL - SAN DIEGO            1
    ## 5                         PARADISE VALLEY HOSPITAL            1
    ## 6  LAGUNA HONDA HOSPITAL AND REHABILITATION CENTER            1
    ## 7             LANGLEY PORTER PSYCHIATRIC INSTITUTE            1
    ## 8                              UCSF MEDICAL CENTER            1
    ## 9                UCSF MEDICAL CENTER AT MOUNT ZION            1
    ## 10                    COAST PLAZA DOCTORS HOSPITAL            1

``` r
#Note running the function above will not permanently add the variable to the dataframe; it will only add it when you run the line above. If you want to permanently add the variable to the dataframe, you need to assign the function back to the dataframe variable like this:

#df <- df %>% mutate(NEW_VARIABLE_NAME = [FORMULA GOES HERE])
```

### Sort Values

*arrange()* sorts the values in a variable from smallest to largest. To
sort from largest to smallest, we need call to arrange in descending
order, using desc().

``` r
#df %>% arrange(VARIABLE_NAME)
#df %>% arrange(desc(VARIABLE_NAME))

#head() only displays the first six rows
hospitals %>% arrange(BEDS) %>% select(NAME, BEDS) %>% head(10)
```

    ##                                                     NAME BEDS
    ## 1  MERCY HOSPITAL - MERCY HOSPITAL ORCHARD PARK DIVISION    2
    ## 2                     PARKLAND HEALTH CENTER-BONNE TERRE    3
    ## 3                           CHRISTIAN HOSPITAL NORTHWEST    3
    ## 4                     COOK CHILDREN'S NORTHEAST HOSPITAL    3
    ## 5                              TEXIENNE HOSPITAL SYSTEMS    4
    ## 6                      SPEARFISH REGIONAL SURGERY CENTER    4
    ## 7                      HENRY FORD MEDICAL CENTER COTTAGE    4
    ## 8                           CLEVELAND EMERGENCY HOSPITAL    4
    ## 9                           TRI TOWN REGIONAL HEALTHCARE    4
    ## 10                 BOZEMAN HEALTH BIG SKY MEDICAL CENTER    4

``` r
hospitals %>% arrange(desc(BEDS)) %>% select(NAME, BEDS) %>% head(10)
```

    ##                                         NAME BEDS
    ## 1                          UPMC PRESBYTERIAN 1592
    ## 2                    YALE-NEW HAVEN HOSPITAL 1541
    ## 3    DEPARTMENT OF STATE HOSPITAL - COALINGA 1500
    ## 4                  JACKSON MEMORIAL HOSPITAL 1493
    ## 5                          MS STATE HOSPITAL 1479
    ## 6                        NAPA STATE HOSPITAL 1418
    ## 7                   FLORIDA HOSPITAL ORLANDO 1368
    ## 8                      PATTON STATE HOSPITAL 1287
    ## 9  DEPARTMENT OF STATE HOSPITAL - ATASCADERO 1275
    ## 10                          CLEVELAND CLINIC 1268
