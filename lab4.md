Lab 4 - Data Cleaning and Exploration
================

  - [Instructions and Overview](#instructions-and-overview)
  - [Data Download](#data-download)
  - [Data Formatting and Importing](#data-formatting-and-importing)
  - [Data Cleaning](#data-cleaning)
  - [Data Exploration](#data-exploration)
  - [More examples and Useful Functions (This is only for
    reference.)](#more-examples-and-useful-functions-this-is-only-for-reference.)

## Instructions and Overview

For this assignment, you will select one dataset from your dataset
hopping lab to import into R, clean, and examine further. Be sure to
fill in the blanks in the document as you
    go.

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.0     ✔ purrr   0.3.3
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   0.8.3     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ────────────────────────────────────────────── tidyverse_conflicts() ──
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
timeframe/geography/category prior to downloading it? You may need to
create an account in the data portal in order to filter the data prior
to download. If this is the case:

1.  Create an account.
2.  Filter and Save the data.
3.  Download the filtered file as a csv.

Otherwise, just go ahead and download the data.

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
> headaches, I’m going to encourage that everyone put their data in this
> .csv format before importing.

  - If it is stored as a .csv file, you are all set to move forward.

For these projects, all data will be stored in and accessed via GitHub
so that we need not worry about working with different file paths and
working directories on our computers. To upload your dataset to GitHub,
move the data to Documents \> GitHub \> STS115\_Course\_Project \>
datasets.

I have made the hospitals dataset that we will be working with as an
example throughout the quarter available in my GitHub repo. The Johns
Hopkins team has made the case dataset available in their own GitHub
repo. I can import both by reading these files in as a CSV. If you want
to know why we are setting stringsAsFactors to FALSE …it’s a long story,
which [this
blog](https://simplystatistics.org/2015/07/24/stringsasfactors-an-unauthorized-biography/)
tells much better than I
could.

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
```

Once your data is available in the datasets folder, you will need to
access its URL. Navigate to your GitHub repo in a Web browser, and then
follow the folder structure to your uploaded CSV file. On this page, you
will see *either* a link in the center of a viewer window that says
“View Raw” OR a button at the top of the viewer window that says
“Raw”. Click on this link or button. Copy and paste the URL of the
page that you are directed to below. The format of this URL should look
like this:
“[https://raw.githubusercontent.com/\[REPO\]/\[FILE\].csv](https://raw.githubusercontent.com/%5BREPO%5D/%5BFILE%5D.csv)”.
If it does not, then be sure to reach out to me for
help.

``` r
# Uncomment the line below. Create an appropriate variable name for your data frame, and fill the URL below to import the data into RStudio

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

**str()** gives us an overview of the structure of the dataset,
including the number of observations, the variable names, and each
variable’s type. For instance, check out how we would run str() on the
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

### Are any of the variables in your dataset incorrectly data typed?

In R, the basic data types include:

  - numeric: numbers that may contain decimals
  - integer: whole numbers
  - character: characters
  - logical: TRUE/FALSE
  - complex: complex numbers

When we call str(), we can see the data type of variable in the dataset.

We can check the type of a variable as follows:

``` r
#typeof(df$VARIABLE_NAME)

typeof(hospitals$ID)
```

    ## [1] "integer"

There was only one variable in the hospitals dataset that we may want to
convert to a different type. At first glance you may think its
COUNTY\_FIPS, which loaded as a character even though it looks like a
number. We in fact want to keep this a char. County FIPS IDs have two
parts - the first two digits represent a census-standardized state code
and the second three digits represent a census-standardized county code.
Just like we expect 5-digits in a postal code, systems that reference
this number expect a certain number of digits. However, state codes
start at 1 and go up through 50+, as do county codes. To ensure that we
always have five digits in the COUNTY FIPS, we need to ensure that
leading zeros do not get stripped when we import our data. This would
happen if the data imported a number, but won’t happen if it gets
imported as a char. With this in mind, we in fact going to want to
transform ZIP into a char and add leading zeros. We will do this in a
later step. See the code below to confirm why:

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

Check out the data type of each variable (e.g. int, chr, num, logi) in
your dataset. Are they all the correct type? List any variables that are
not the correct
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

### Do you need to change the data type of any variables (including the char to numeric conversion you prepared for above)?

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

#df$VARIABLE_NAME <- as.numeric(df$VARIABLE_NAME) 

#Fill with one of the following: as.numeric, as.character, as.logical)

#_____$_____ <- _____(_____$______)
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
until they are all 5 characters in length. We can use **str\_pad()** to
do this.

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

#df %>% head(10)

#_____ %>% head(10) 
```

Null values should appear as a greyed-out and italicized *NA* (Not
Available). This communicates to R that this is an empty value or, in
other words, that there is not data here. However, if not properly
formatted when we import the dataset, you may see Null values appear as:
\* “NULL” \* empty strings ("“) \*”NONE" \* “NOT AVAILABLE”

In the world\_health\_econ dataset, we can see that empty data is
formatted correctly with a greyed out NA.

``` r
world_health_econ %>% head(10)
```

    ##                country continent year tot_health_sp_pp
    ## 1          Afghanistan      Asia 1995               NA
    ## 2              Albania    Europe 1995             27.9
    ## 3              Algeria    Africa 1995             62.1
    ## 4              Andorra      <NA> 1995           1390.0
    ## 5               Angola    Africa 1995             15.6
    ## 6  Antigua and Barbuda      <NA> 1995            351.0
    ## 7            Argentina  Americas 1995            615.0
    ## 8              Armenia      <NA> 1995             25.7
    ## 9            Australia   Oceania 1995           1570.0
    ## 10             Austria    Europe 1995           2860.0
    ##    tot_health_sp_per_gdp priv_share_health_sp pocket_share_health_sp
    ## 1                     NA                   NA                     NA
    ## 2                   2.56                 50.0                   50.0
    ## 3                   4.17                 24.6                   23.9
    ## 4                   7.64                 35.6                   26.7
    ## 5                   3.79                 13.2                   13.2
    ## 6                   4.88                 33.5                   29.2
    ## 7                   8.31                 40.2                   28.0
    ## 8                   6.45                 69.0                   65.9
    ## 9                   7.22                 34.2                   16.1
    ## 10                  9.52                 26.1                   15.2
    ##    gov_share_health_sp gov_health_sp_pp gov_prop_health_spending      pop
    ## 1                   NA               NA                       NA 18100000
    ## 2                 50.0            13.90                     5.26  3110000
    ## 3                 75.4            46.80                    10.00 28800000
    ## 4                 64.4           897.00                    23.60    63900
    ## 5                 86.8            13.50                     5.00 13900000
    ## 6                 66.5           233.00                    12.90    68700
    ## 7                 59.8           368.00                    15.30 34800000
    ## 8                 31.0             7.97                     8.30  3220000
    ## 9                 65.8          1030.00                    13.10 18000000
    ## 10                74.0          2110.00                    12.50  7990000
    ##    pop_dens life_exp
    ## 1     26.20     53.3
    ## 2    113.00     74.6
    ## 3     12.10     72.9
    ## 4    136.00     79.7
    ## 5     11.40     49.5
    ## 6    167.00     74.1
    ## 7     12.80     73.3
    ## 8    113.00     69.9
    ## 9      2.35     78.2
    ## 10    97.00     76.8

In the hospitals dataset, we can see by calling head() that empty data
is filled with the string “NOT AVAILABLE”. We want to convert such
values to NA values. In the cases dataset, we can see that empty data is
filled with an empty string "". In the world\_health\_econ dataset, we
can see that empty data is formatted correctly with a greyed out NA. We
know this because empty rows values in the Province.State field are
simply blank. We will also convert such values to NA values.

``` r
is.na(hospitals) <- hospitals == "NOT AVAILABLE"
is.na(cases) <- cases == ""
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

### Do you have any variables in your dataset that refer to specific dates?

If not, skip to the next heading.

> Note that while the headings in the cases dataset refer to dates,
> these are not date variables. Here dates are simply serving as a
> header for other data. The values stored in those variables refer to
> counts of cases (i.e. numbers) not dates.

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
manipulate and transform data. The four primary functions we will be
working with this week and next through dplyr include:

  - select() : select variables
  - filter() : return only observations that meet a particular criteria
  - group\_by() : group observations according to a common value
  - summarize() : perform an operation and return a single value

In this lab, we will focus on the first two - select() and filter(). You
can think of select() as a tool to reference specific columns in a
rectangular dataset, and filter() as a tool to reference specific rows
in a rectangular dataset.

### What kinds of variables are in the dataset?

> Note that you may not be able to list three of each below.

*Nominal categorical variables* are variables that identify something
else. They name or categorize something that exists in the world.
Sometimes, nominal categorical variables are obvious. For instance, in
the hospitals dataset, the hospital NAME is a nominal categorical
variable - referring to the actual hospital. CITY is also a nominal
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

List three nominal categorical variables in your dataset. Use
**select()** to select these variables in your dataset, and use
**head(10)** to limit the display to the first 10
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
lowest trauma levels). However, this is a particularly complicated
categorical variable to work with. This is because Trauma levels are not
defined according to a national standard. Instead, they are defined on a
state-by-state basis, and our dataset spans all US states. Level II in
one state might mean something different than Level II in another state
despite both being labeled Level II in the dataset. Further, a single
hospital can have multiple trauma levels (e.g. Level I Pediatric and
Level II Adult). We would need to take all of this into consideration
when comparing trauma levels across hospitals on a national scale.

List three ordinal categorical variables in your dataset. Use
**select()** to select these variables in your dataset, and use
**head(10)** to limit the display to the first 10
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
hospitals dataset, POPULATION, BEDS, and presumably TTL\_STAFF (though
it’s all empty in our dataset), are all discrete numeric variables
because they represent things that have been counted.

List three discrete numerical variables in your dataset. Use
**select()** to select these variables in your dataset, and use
**head(10)** to limit the display to the first 10
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

> We will also treat ratios as continuous data (such as total health
> spending per person in the world\_health\_econ data), so you may list
> those below.

List three continuous numeric variables in your dataset. Use
**select()** to select these variables in your dataset, and use
**head(10)** to limit the display to the first 10
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

### What makes each observation in your dataset unique?

In starting our data analysis, we need to have a good sense of what each
observation in our dataset refers to - or its *observational unit*.
Think of it this way. If you were to count the number rows in your
dataset, what would that number refer to?

> To create this statement, we will use the R function **paste()**.
> paste() allows you to create strings that concatenate other strings
> that you provide, along with other values. We separate all of the
> components of the string we wish to paste together with
commas.

``` r
paste("I have", nrow(hospitals), "unique _____ represented in my dataset.")
```

    ## [1] "I have 7581 unique _____ represented in my dataset."

``` r
paste("I have", nrow(cases), "unique _____ represented in my dataset.")
```

    ## [1] "I have 253 unique _____ represented in my dataset."

``` r
paste("I have", nrow(world_health_econ), "unique _____ represented in my dataset.")
```

    ## [1] "I have 3040 unique _____ represented in my dataset."

Get this statement started for your
dataset:

``` r
#Uncomment the last line and fill your data frame name in nrow. At this point, you need only fill in the first blank line.

#paste("I have", nrow(_____), "unique _____ represented in my dataset.")
```

To figure out how to fill that blank in the statement, it is often
useful to identify a variable or set of variables that can serve as a
unique key for the data. A *unique key* is a variable (or set of
variables) that uniquely identifies an observation in the dataset. Think
of a unique key as a unique way to identify a row and all of the values
in it. There should never be more than one row in the dataset with the
same unique key. A unique key tells us what each row in the dataset
refers to.

In the hospitals dataset, the unique key is a bit more obvious. There is
a variable called OBJECTID that uniquely refers to the geographic
coordinates in the dataset, and there is a variable called ID that
uniquely refers to the hospital in the dataset. We can confirm that
these are indeed unique keys by counting the number of **distinct()**
(or non-repeating) values in this variable and making sure it is equal
to the number of rows in the entire dataset. If the distinct values in
the variable is equal to the number of rows in the dataset, then we know
that the key never repeats and that it can uniquely identify each row.

``` r
# Count the distinct values in your unique key
n_unique_keys <- 
  hospitals %>% 
  select(ID) %>% 
  n_distinct()

# Count the rows in your dataset
n_rows <- nrow(hospitals)

# Make sure these numbers are equal
n_unique_keys == n_rows
```

    ## [1] TRUE

Since the ID field refers to a specific hospital, in this dataset a
hospital is what makes each observation unique. In other words, the
dataset’s observation unit is a hospital. Now you can confidently
say:

``` r
paste("I have", nrow(hospitals), "unique hospitals represented in my dataset.")
```

    ## [1] "I have 7581 unique hospitals represented in my dataset."

> Note that NAME is typically not an appropriate variable to use as a
> unique key. Let me provide an example to demonstrate this. When I
> worked for BetaNYC, I was trying to build a map of vacant storefronts
> in NYC by mapping all commercially zoned properties in the city, and
> then filtering out those properties where a business was licensed or
> permitted. This way the map would only include properties where there
> wasn’t a business operating. One set of businesses I was filtering out
> was restaurants. The only dataset that the city had made publicly
> available for restaurant permits was broken. It was operating on an
> automated process to update whenever there was a change in the permit;
> however, whenever a permit was updated, rather than updating the
> appropriate fields in the existing dataset, it was creating a new row
> in the dataset that only included the permit holder (the restaurant
> name), the permit type, and the updated fields. Notably the unique
> permit ID was not being included in this new row. We pointed this
> issue out to city officials, but fixing something like this can be
> slow and time-consuming, so in the meantime, we looked into whether we
> could clean the data ourselves by aggregating the rows that referred
> to the same restaurant. However, without the permit ID it was
> impossible to uniquely identify the restaurants in the dataset. Sure,
> we had the restaurant name, but do you know how many Wendy’s there are
> in NYC?

In the world\_health\_econ dataset, the unique ID is less obvious.
Because we have values reported for multiple countries in multiple year,
we need to rely on two variables to signify what makes each observation
unique - country and year.

``` r
# Count the distinct values in your unique key
n_unique_keys <- 
  world_health_econ %>% 
  select(country, year) %>% 
  n_distinct()

# Count the rows in your dataset
n_rows <- nrow(world_health_econ)

# Make sure these numbers are equal
n_unique_keys == n_rows
```

    ## [1] TRUE

In this dataset a country and year is what makes each observation
unique. In other words, the dataset’s observational unit is a country in
a given reporting year. Now you can confidently
say:

``` r
paste("I have", nrow(hospitals), "unique countries in a given reporting year represented in my dataset.")
```

    ## [1] "I have 7581 unique countries in a given reporting year represented in my dataset."

In the cases dataset, the unique ID is also more complicated. Because
there are multiple provinces listed for each country (but not for all
countries), we can’t rely on country to uniquely identify each row. See
below:

``` r
# Count the distinct values in your unique key
n_unique_keys <- 
  cases %>% 
  select(Country.Region) %>% 
  n_distinct()

# Count the rows in your dataset
n_rows <- nrow(cases)

# Make sure these numbers are equal
n_unique_keys == n_rows
```

    ## [1] FALSE

Because this returns FALSE, it would be incorrect to say “I have
nrow(df) many unique countries in my dataset.”

Instead, we need to rely on both the country and province to uniquely
identify each row.

``` r
# Count the distinct values in your unique key
n_unique_keys <- 
  cases %>% 
  select(Country.Region, Province.State) %>% 
  n_distinct()

# Count the rows in your dataset
n_rows <- nrow(cases)

# Make sure these numbers are equal
n_unique_keys == n_rows
```

    ## [1] TRUE

In this case, every row in the dataset is a province and country pair.
In other words, the dataset’s observational unit is a province/country.
Now you can confidently
say:

``` r
paste("I have", nrow(cases), "unique province/countries represented in my dataset.")
```

    ## [1] "I have 253 unique province/countries represented in my dataset."

Why is this so important? Later we are going to perform calculations
across observations in the dataset. When we do so, we assume that all of
the observations in the dataset are reported at the same scale. For
instance, imagine if we want to know the average number of Covid-19
cases reported across the globe. Before we take the average of all the
values reported in the Total.Cases column of the cases dataset, we need
to remember that some cases are reported at just the country scale and
some are reported at the more specific province scale. We would need to
transform our data so that each observation represented a country total
before taking this average, or else we would be averaging data at
different geographic scales.

What variable makes each observation in your dataset unique? Confirm
that you are correct below.

``` r
# Uncomment and count the distinct values in your unique key
#n_unique_keys <- _____ %>% select(_____) %>% n_distinct()

# Uncomment and count the rows in your dataset
#n_rows <- nrow(_____)

# Uncomment and make sure these numbers are equal
# n_unique_keys == n_rows
```

What does your unique key refer to? In other words, what is the
observational unit of your dataset?

``` r
Fill your response here. 
```

Fill in the statement below, and make sure that it makes sense with your
data.

``` r
#Uncomment the line below and fill in the blanks.

#paste("I have", nrow(_____), "unique _____ represented in my dataset.")
```

Anytime we count something in the world, we are not only engaging in a
process of tabulation; we are also engaged in a process of defining. If
I count the number of students in class, I first have to define what
counts as a student. If someone is auditing the class, do they count? If
as the teacher am learning from my students, do I count myself as a
student? As I make decisions about how I’m going to define “student,”
those decisions impact the numbers that I produce. When I change my
definition of “student,” how I go about tabulating students also
changes.

Thus, as we prepare to count observations in a dataset, it is important
to know how those observations are defined. When I say that there are
7581 hospitals in the hospitals dataset, this number does not mean much
until I understand how hospitals were defined in the dataset. Which
hospitals? In what part of the world? From what time period? Are
hospitals that were once open and are now closed included? Are
psychiatric hospitals included, or only medical hospitals? Are nursing
homes included? Who gets to decide what counts as a hospital?

Analyzing the hospitals [data
documentation](https://hifld-geoplatform.opendata.arcgis.com/datasets/6ac5e325468c4cb9b905f1728d6fbf0f_0),
we find the following statement: “This feature class/shapefile contains
locations of Hospitals for 50 US states, Washington D.C., US territories
of Puerto Rico, Guam, American Samoa, Northern Mariana Islands, Palau,
and Virgin Islands.The dataset only includes hospital facilities based
on data acquired from various state departments or federal sources which
has been referenced in the SOURCE field. Hospital facilities which do
not occur in these sources will be not present in the database….The
database does not contain nursing homes or health centers.”

Knowing how hospitals are defined helps us put the count of hospitals in
our dataset into context.

Consider the cases dataset. Throughout the world, due to competing
geopolitics, different countries draw geographic borders and name
countries differently. For instance, while the United Nations and many
other countries recognize ‘Myanmar’ as the official name of the
southeast-Asian country (after a ruling military junta declared the name
change in 1989), the US and the UK refer to the country as ‘Burma’,
arguing that official country name changes should be based on a
democratic process. The borders between Israel, Syria, Lebanon, and
Palestine are constantly in dispute as political processes and religious
factions clash. Check out this long list of \[territorial
disputes\](<https://en.wikipedia.org/wiki/List_of_territorial_disputes_>.
So, how are countries and provinces defined in the cases dataset?

When the data was first released, the Johns Hopkins team producing this
dataset were not following a particular naming standard for countries.
Check out [this
issue](https://github.com/CSSEGISandData/COVID-19/issues/340) in their
GitHub issue queue. Several community members requested that the
[ISO 3166-2](https://www.iso.org/iso-3166-country-codes.html) country
codes be added to the dataset. ISO 3166-2 country codes are numbers
standardized by the International Standards Organization for uniquely
referring to each country or subdivision; each code corresponds to a
country or region recognized by the United Nations. You can keep track
of how this issue is progressing
[here](https://github.com/CSSEGISandData/COVID-19/issues/372). In this
sense, the definitions of provinces/countries is evolving in the dataset
- and notably evolving towards standardized definitions recognized by
the UN.

> Note that one of issues that has come up with making this change is
> what to do about the row in the dataset representing the number of
> cases on the Princess Cruise.

How are the observational units in your dataset defined? Be sure to
refer to the data documentation.

``` r
Fill your response here. 
```

Who or what organization manages these definitions? In other words, who
gets to decide what counts in this data?

``` r
Fill your response here. 
```

### Summary

Calling **summary()** on a data frame returns summary statistics for
each of the variables in that data frame. For numeric values, it returns
the min, max, median, 1st and 3rd quartiles, mean, and number of NAs. We
will go over each of these values more in two weeks. Plus, we have a lot
more work to do before we can take any of these numbers at face value.
At this point, we are simply looking for values that immediately appear
off.

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
of the hospitals dataset is that the min for all of our discrete numeric
values is -999. How is it possible to count -999 beds? This signals to
us that -999 is being used to indicate that data is not available. We
can confirm this in the data dictionary. In fact, quite often -999 is
used to indicate null values. Let’s go ahead and convert all of the
instances of -999 to NA.

``` r
is.na(hospitals) <- hospitals == -999
```

Summarize the values in each variable in your data frame.

``` r
#Uncomment the appropriate lines below, and fill in your data frame.
#_____ %>% summary()
```

Have you identified any areas where you may need to conduct more
cleaning? Email me if you are unsure of how to address this issue.

``` r
Fill response here. 
```

### Values in a Key Categorical Variable

When called on a specific variable, **distinct()** lists each of the
unique values that appears within that variable. This can be useful for
determining how different issues are classified in the data.
**n\_distinct()** counts the number of distinct values in a variable.
This let’s us know how many categories we are dealing with. For
instance, I can find out the distinct types of hospitals as well as how
many distinct types there are by calling:

``` r
#df %>% select(VARIABLE_NAME) %>% distinct()
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
#df %>% select(VARIABLE_NAME) %>% n_distinct()
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
> closures in rural communities. To receive federal funding, critical
> access hospitals should have no more than 25 beds. See this
> [source](https://www.ruralhealthinfo.org/topics/critical-access-hospitals).
> Psychiatric hospitals, while following a similar timeline to the
> development of general hospitals in the US, developed in response to
> changing attitudes and understandings of what it meant to be mentally
> ill. In the 18th century, mental illness was often considered a moral
> or spiritual shortcoming; however, the increased emphasis on *moral
> treatment* of mentally ill patients ushered in a new wave of
> institutions and wards devoted to the treatment of such patients. See
> this
> [source](https://www.nursing.upenn.edu/nhhc/nurses-institutions-caring/history-of-psychiatric-hospitals/).

Let’s check a second variable in the hospitals dataset.

``` r
#df %>% select(VARIABLE_NAME) %>% distinct()
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
#df %>% select(VARIABLE_NAME) %>% n_distinct()
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

### Missing Data

Check the number of NAs in each variable in your dataset by filling in
the blanks in the commented code below.

``` r
#colSums(sapply(df, is.na))
colSums(sapply(world_health_econ, is.na))
```

    ##                  country                continent                     year 
    ##                        0                      928                        0 
    ##         tot_health_sp_pp    tot_health_sp_per_gdp     priv_share_health_sp 
    ##                       64                       32                       32 
    ##   pocket_share_health_sp      gov_share_health_sp         gov_health_sp_pp 
    ##                      701                       32                       33 
    ## gov_prop_health_spending                      pop                 pop_dens 
    ##                       32                        0                        0 
    ##                 life_exp 
    ##                       96

``` r
#Uncomment the appropriate lines below, and fill in your data frame.
#colSums(sapply(_____, is.na)) 
```

Let’s explore a variable with many NAs. This is going to be the first
time we see the function filter(). **filter()** subsets our data to the
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
#df %>% filter(is.na(VARIABLE_NAME)) %>% head(10) #We add head(10) to limit our output to the first ten rows
world_health_econ %>% filter(is.na(tot_health_sp_pp)) %>% head(30)
```

    ##        country continent year tot_health_sp_pp tot_health_sp_per_gdp
    ## 1  Afghanistan      Asia 1995               NA                    NA
    ## 2         Iraq      Asia 1995               NA                    NA
    ## 3      Liberia    Africa 1995               NA                    NA
    ## 4  Afghanistan      Asia 1996               NA                    NA
    ## 5      Liberia    Africa 1996               NA                    NA
    ## 6  Afghanistan      Asia 1997               NA                    NA
    ## 7      Liberia    Africa 1997               NA                    NA
    ## 8  Afghanistan      Asia 1998               NA                    NA
    ## 9  Afghanistan      Asia 1999               NA                    NA
    ## 10 Afghanistan      Asia 2000               NA                    NA
    ## 11 Afghanistan      Asia 2001               NA                    NA
    ## 12     Somalia    Africa 2002               NA                    NA
    ## 13    Zimbabwe    Africa 2002               NA                    NA
    ## 14     Somalia    Africa 2003               NA                    NA
    ## 15    Zimbabwe    Africa 2003               NA                    NA
    ## 16     Somalia    Africa 2004               NA                    NA
    ## 17    Zimbabwe    Africa 2004               NA                    NA
    ## 18     Somalia    Africa 2005               NA                    NA
    ## 19    Zimbabwe    Africa 2005               NA                    NA
    ## 20     Somalia    Africa 2006               NA                    NA
    ## 21    Zimbabwe    Africa 2006               NA                    NA
    ## 22     Somalia    Africa 2007               NA                    NA
    ## 23    Zimbabwe    Africa 2007               NA                    NA
    ## 24     Somalia    Africa 2008               NA                    NA
    ## 25    Zimbabwe    Africa 2008               NA                    NA
    ## 26     Somalia    Africa 2009               NA                    NA
    ## 27    Zimbabwe    Africa 2009               NA                    NA
    ## 28    Honduras  Americas 2010               NA                    NA
    ## 29      Mexico  Americas 2010               NA                    NA
    ## 30   Nicaragua  Americas 2010               NA                    NA
    ##    priv_share_health_sp pocket_share_health_sp gov_share_health_sp
    ## 1                    NA                     NA                  NA
    ## 2                    NA                     NA                  NA
    ## 3                    NA                     NA                  NA
    ## 4                    NA                     NA                  NA
    ## 5                    NA                     NA                  NA
    ## 6                    NA                     NA                  NA
    ## 7                    NA                     NA                  NA
    ## 8                    NA                     NA                  NA
    ## 9                    NA                     NA                  NA
    ## 10                   NA                     NA                  NA
    ## 11                   NA                     NA                  NA
    ## 12                   NA                     NA                  NA
    ## 13                   NA                     NA                  NA
    ## 14                   NA                     NA                  NA
    ## 15                   NA                     NA                  NA
    ## 16                   NA                     NA                  NA
    ## 17                   NA                     NA                  NA
    ## 18                   NA                     NA                  NA
    ## 19                   NA                     NA                  NA
    ## 20                   NA                     NA                  NA
    ## 21                   NA                     NA                  NA
    ## 22                   NA                     NA                  NA
    ## 23                   NA                     NA                  NA
    ## 24                   NA                     NA                  NA
    ## 25                   NA                     NA                  NA
    ## 26                   NA                     NA                  NA
    ## 27                   NA                     NA                  NA
    ## 28                   NA                     NA                  NA
    ## 29                   NA                     NA                  NA
    ## 30                   NA                     NA                  NA
    ##    gov_health_sp_pp gov_prop_health_spending       pop pop_dens life_exp
    ## 1                NA                       NA  18100000     26.2     53.3
    ## 2                NA                       NA  20100000     46.5     65.4
    ## 3                NA                       NA   2040000     21.5     50.0
    ## 4                NA                       NA  18900000     27.3     53.8
    ## 5                NA                       NA   2160000     22.7     48.9
    ## 6                NA                       NA  19400000     28.2     53.7
    ## 7                NA                       NA   2330000     24.5     51.9
    ## 8                NA                       NA  19700000     28.9     52.8
    ## 9                NA                       NA  20200000     29.7     54.4
    ## 10               NA                       NA  20800000     30.8     54.6
    ## 11               NA                       NA  21600000     32.1     54.8
    ## 12               NA                       NA   9500000     15.2     53.3
    ## 13               NA                       NA  12000000     32.3     45.6
    ## 14               NA                       NA   9820000     15.7     53.7
    ## 15               NA                       NA  12000000     32.7     45.4
    ## 16               NA                       NA  10100000     16.1     54.0
    ## 17               NA                       NA  12000000     33.0     45.1
    ## 18               NA                       NA  10400000     16.6     54.7
    ## 19               NA                       NA  12100000     33.4     45.1
    ## 20               NA                       NA  10800000     17.1     55.1
    ## 21               NA                       NA  12200000     33.9     45.4
    ## 22               NA                       NA  11100000     17.6     55.0
    ## 23               NA                       NA  12300000     34.5     45.9
    ## 24               NA                       NA  11400000     18.1     55.5
    ## 25               NA                       NA  12400000     35.0     46.3
    ## 26               NA                       NA  11700000     18.7     55.9
    ## 27               NA                       NA  12500000     35.7     47.2
    ## 28               NA                       NA   8320000     73.2     72.8
    ## 29               NA                       NA 114000000     60.4     75.2
    ## 30               NA                       NA   5820000     47.7     77.6

If your dataset has no columns with NAs just note that below, and move
on to 4. Otherwise, fill in the blanks
below.

``` r
#Uncomment the appropriate lines below, and fill in your data frame and variable

#_____ %>% filter(is.na(_____)) %>% head(10)
```

Is there anything special about these rows? What hypotheses do you have
for why there may be missing data in the variable you selected?

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

world_health_econ %>% filter(country %in% c("Afghanistan", "Liberia"))
```

    ##        country continent year tot_health_sp_pp tot_health_sp_per_gdp
    ## 1  Afghanistan      Asia 1995               NA                    NA
    ## 2      Liberia    Africa 1995               NA                    NA
    ## 3  Afghanistan      Asia 1996               NA                    NA
    ## 4      Liberia    Africa 1996               NA                    NA
    ## 5  Afghanistan      Asia 1997               NA                    NA
    ## 6      Liberia    Africa 1997               NA                    NA
    ## 7  Afghanistan      Asia 1998               NA                    NA
    ## 8      Liberia    Africa 1998             7.29                  5.15
    ## 9  Afghanistan      Asia 1999               NA                    NA
    ## 10     Liberia    Africa 1999             8.55                  5.25
    ## 11 Afghanistan      Asia 2000               NA                    NA
    ## 12     Liberia    Africa 2000             9.96                  5.06
    ## 13 Afghanistan      Asia 2001               NA                    NA
    ## 14     Liberia    Africa 2001             9.73                  5.27
    ## 15 Afghanistan      Asia 2002            14.80                  5.72
    ## 16     Liberia    Africa 2002             8.90                  4.77
    ## 17 Afghanistan      Asia 2003            18.30                  6.82
    ## 18     Liberia    Africa 2003             6.88                  5.09
    ## 19 Afghanistan      Asia 2004            20.70                  6.36
    ## 20     Liberia    Africa 2004             7.56                  5.08
    ## 21 Afghanistan      Asia 2005            21.90                  6.63
    ## 22     Liberia    Africa 2005             8.70                  5.22
    ## 23 Afghanistan      Asia 2006            23.80                  6.77
    ## 24     Liberia    Africa 2006            12.10                  6.93
    ## 25 Afghanistan      Asia 2007            28.80                  7.30
    ## 26     Liberia    Africa 2007            24.80                 11.80
    ## 27 Afghanistan      Asia 2008            31.80                  6.98
    ## 28     Liberia    Africa 2008            27.50                 11.90
    ## 29 Afghanistan      Asia 2009            33.70                  7.58
    ## 30     Liberia    Africa 2009            28.00                 12.20
    ## 31 Afghanistan      Asia 2010            37.70                  7.58
    ## 32     Liberia    Africa 2010            29.20                 11.80
    ##    priv_share_health_sp pocket_share_health_sp gov_share_health_sp
    ## 1                    NA                     NA                  NA
    ## 2                    NA                     NA                  NA
    ## 3                    NA                     NA                  NA
    ## 4                    NA                     NA                  NA
    ## 5                    NA                     NA                  NA
    ## 6                    NA                     NA                  NA
    ## 7                    NA                     NA                  NA
    ## 8                  72.2                   35.7               27.80
    ## 9                    NA                     NA                  NA
    ## 10                 70.9                   35.0               29.10
    ## 11                   NA                     NA                  NA
    ## 12                 73.6                   36.4               26.40
    ## 13                   NA                     NA                  NA
    ## 14                 70.6                   34.9               29.40
    ## 15                 94.4                   88.7                5.62
    ## 16                 78.5                   38.8               21.50
    ## 17                 93.2                   87.6                6.83
    ## 18                 77.2                   38.4               22.80
    ## 19                 92.2                   86.6                7.81
    ## 20                 74.1                   36.7               25.90
    ## 21                 88.5                   83.1               11.60
    ## 22                 69.3                   34.2               30.60
    ## 23                 88.2                   82.9               11.80
    ## 24                 72.2                   36.6               27.80
    ## 25                 87.8                   82.6               12.20
    ## 26                 76.3                   40.1               23.70
    ## 27                 88.2                   82.9               11.80
    ## 28                 67.0                   35.0               33.00
    ## 29                 88.4                   83.1               11.60
    ## 30                 65.5                   34.2               34.50
    ## 31                 88.3                   83.0               11.70
    ## 32                 67.5                   35.2               32.50
    ##    gov_health_sp_pp gov_prop_health_spending      pop pop_dens life_exp
    ## 1                NA                       NA 18100000     26.2     53.3
    ## 2                NA                       NA  2040000     21.5     50.0
    ## 3                NA                       NA 18900000     27.3     53.8
    ## 4                NA                       NA  2160000     22.7     48.9
    ## 5                NA                       NA 19400000     28.2     53.7
    ## 6                NA                       NA  2330000     24.5     51.9
    ## 7                NA                       NA 19700000     28.9     52.8
    ## 8             2.030                     8.86  2520000     26.5     52.5
    ## 9                NA                       NA 20200000     29.7     54.4
    ## 10            2.490                    10.60  2700000     28.4     53.2
    ## 11               NA                       NA 20800000     30.8     54.6
    ## 12            2.630                     8.97  2850000     29.9     54.3
    ## 13               NA                       NA 21600000     32.1     54.8
    ## 14            2.860                    11.50  2950000     31.1     55.2
    ## 15            0.833                     1.48 22600000     33.7     55.6
    ## 16            1.920                     7.17  3020000     31.8     55.0
    ## 17            1.250                     1.48 23700000     35.3     56.4
    ## 18            1.560                    10.60  3080000     32.4     55.1
    ## 19            1.610                     1.48 24700000     36.9     56.9
    ## 20            1.960                    11.90  3140000     33.0     58.0
    ## 21            2.520                     1.48 25700000     38.4     57.4
    ## 22            2.670                    11.10  3220000     33.9     58.7
    ## 23            2.810                     1.48 26400000     39.7     57.6
    ## 24            3.370                    15.20  3330000     35.0     59.4
    ## 25            3.500                     1.48 27100000     40.8     58.0
    ## 26            5.880                    17.30  3460000     36.5     60.0
    ## 27            3.750                     1.48 27700000     41.8     58.8
    ## 28            9.070                    17.20  3610000     38.0     60.5
    ## 29            3.910                     1.58 28400000     42.9     59.3
    ## 30            9.660                    13.80  3750000     39.6     60.9
    ## 31            4.390                     1.59 29200000     44.1     59.9
    ## 32            9.490                    11.10  3890000     41.0     61.3

``` r
#...and then we would check to see if there are missing values in the columns under consideration. 
```

Or maybe we hypothesize that a certain geography did not report values
in a certain year. We can join multiple filter conditions by placing an
‘&’ between two statements.

``` r
#df %>% filter(Country == "Luxembourg" & Year == 2017) 

world_health_econ %>% filter(country %in% c("Afghanistan", "Liberia") & year < 1999)
```

    ##       country continent year tot_health_sp_pp tot_health_sp_per_gdp
    ## 1 Afghanistan      Asia 1995               NA                    NA
    ## 2     Liberia    Africa 1995               NA                    NA
    ## 3 Afghanistan      Asia 1996               NA                    NA
    ## 4     Liberia    Africa 1996               NA                    NA
    ## 5 Afghanistan      Asia 1997               NA                    NA
    ## 6     Liberia    Africa 1997               NA                    NA
    ## 7 Afghanistan      Asia 1998               NA                    NA
    ## 8     Liberia    Africa 1998             7.29                  5.15
    ##   priv_share_health_sp pocket_share_health_sp gov_share_health_sp
    ## 1                   NA                     NA                  NA
    ## 2                   NA                     NA                  NA
    ## 3                   NA                     NA                  NA
    ## 4                   NA                     NA                  NA
    ## 5                   NA                     NA                  NA
    ## 6                   NA                     NA                  NA
    ## 7                   NA                     NA                  NA
    ## 8                 72.2                   35.7                27.8
    ##   gov_health_sp_pp gov_prop_health_spending      pop pop_dens life_exp
    ## 1               NA                       NA 18100000     26.2     53.3
    ## 2               NA                       NA  2040000     21.5     50.0
    ## 3               NA                       NA 18900000     27.3     53.8
    ## 4               NA                       NA  2160000     22.7     48.9
    ## 5               NA                       NA 19400000     28.2     53.7
    ## 6               NA                       NA  2330000     24.5     51.9
    ## 7               NA                       NA 19700000     28.9     52.8
    ## 8             2.03                     8.86  2520000     26.5     52.5

``` r
#...and then we would check to see if there are missing values in the columns under consideration. 
```

Also note that we can filter to rows with all NAs in multiple columns by
calling:

``` r
#df %>% filter_at(vars(VARIABLE_NAME1, VARIABLE_NAME2, VARIABLE_NAME5:VARIABLE_NAME8), all_vars(is.na(.)))

world_health_econ %>% filter_at(vars(tot_health_sp_pp:gov_prop_health_spending), all_vars(is.na(.)))
```

    ##        country continent year tot_health_sp_pp tot_health_sp_per_gdp
    ## 1  Afghanistan      Asia 1995               NA                    NA
    ## 2         Iraq      Asia 1995               NA                    NA
    ## 3      Liberia    Africa 1995               NA                    NA
    ## 4  Afghanistan      Asia 1996               NA                    NA
    ## 5      Liberia    Africa 1996               NA                    NA
    ## 6  Afghanistan      Asia 1997               NA                    NA
    ## 7      Liberia    Africa 1997               NA                    NA
    ## 8  Afghanistan      Asia 1998               NA                    NA
    ## 9  Afghanistan      Asia 1999               NA                    NA
    ## 10 Afghanistan      Asia 2000               NA                    NA
    ## 11 Afghanistan      Asia 2001               NA                    NA
    ## 12     Somalia    Africa 2002               NA                    NA
    ## 13    Zimbabwe    Africa 2002               NA                    NA
    ## 14     Somalia    Africa 2003               NA                    NA
    ## 15    Zimbabwe    Africa 2003               NA                    NA
    ## 16     Somalia    Africa 2004               NA                    NA
    ## 17    Zimbabwe    Africa 2004               NA                    NA
    ## 18     Somalia    Africa 2005               NA                    NA
    ## 19    Zimbabwe    Africa 2005               NA                    NA
    ## 20     Somalia    Africa 2006               NA                    NA
    ## 21    Zimbabwe    Africa 2006               NA                    NA
    ## 22     Somalia    Africa 2007               NA                    NA
    ## 23    Zimbabwe    Africa 2007               NA                    NA
    ## 24     Somalia    Africa 2008               NA                    NA
    ## 25    Zimbabwe    Africa 2008               NA                    NA
    ## 26     Somalia    Africa 2009               NA                    NA
    ## 27    Zimbabwe    Africa 2009               NA                    NA
    ## 28    Honduras  Americas 2010               NA                    NA
    ## 29      Mexico  Americas 2010               NA                    NA
    ## 30   Nicaragua  Americas 2010               NA                    NA
    ## 31     Somalia    Africa 2010               NA                    NA
    ## 32    Zimbabwe    Africa 2010               NA                    NA
    ##    priv_share_health_sp pocket_share_health_sp gov_share_health_sp
    ## 1                    NA                     NA                  NA
    ## 2                    NA                     NA                  NA
    ## 3                    NA                     NA                  NA
    ## 4                    NA                     NA                  NA
    ## 5                    NA                     NA                  NA
    ## 6                    NA                     NA                  NA
    ## 7                    NA                     NA                  NA
    ## 8                    NA                     NA                  NA
    ## 9                    NA                     NA                  NA
    ## 10                   NA                     NA                  NA
    ## 11                   NA                     NA                  NA
    ## 12                   NA                     NA                  NA
    ## 13                   NA                     NA                  NA
    ## 14                   NA                     NA                  NA
    ## 15                   NA                     NA                  NA
    ## 16                   NA                     NA                  NA
    ## 17                   NA                     NA                  NA
    ## 18                   NA                     NA                  NA
    ## 19                   NA                     NA                  NA
    ## 20                   NA                     NA                  NA
    ## 21                   NA                     NA                  NA
    ## 22                   NA                     NA                  NA
    ## 23                   NA                     NA                  NA
    ## 24                   NA                     NA                  NA
    ## 25                   NA                     NA                  NA
    ## 26                   NA                     NA                  NA
    ## 27                   NA                     NA                  NA
    ## 28                   NA                     NA                  NA
    ## 29                   NA                     NA                  NA
    ## 30                   NA                     NA                  NA
    ## 31                   NA                     NA                  NA
    ## 32                   NA                     NA                  NA
    ##    gov_health_sp_pp gov_prop_health_spending       pop pop_dens life_exp
    ## 1                NA                       NA  18100000     26.2     53.3
    ## 2                NA                       NA  20100000     46.5     65.4
    ## 3                NA                       NA   2040000     21.5     50.0
    ## 4                NA                       NA  18900000     27.3     53.8
    ## 5                NA                       NA   2160000     22.7     48.9
    ## 6                NA                       NA  19400000     28.2     53.7
    ## 7                NA                       NA   2330000     24.5     51.9
    ## 8                NA                       NA  19700000     28.9     52.8
    ## 9                NA                       NA  20200000     29.7     54.4
    ## 10               NA                       NA  20800000     30.8     54.6
    ## 11               NA                       NA  21600000     32.1     54.8
    ## 12               NA                       NA   9500000     15.2     53.3
    ## 13               NA                       NA  12000000     32.3     45.6
    ## 14               NA                       NA   9820000     15.7     53.7
    ## 15               NA                       NA  12000000     32.7     45.4
    ## 16               NA                       NA  10100000     16.1     54.0
    ## 17               NA                       NA  12000000     33.0     45.1
    ## 18               NA                       NA  10400000     16.6     54.7
    ## 19               NA                       NA  12100000     33.4     45.1
    ## 20               NA                       NA  10800000     17.1     55.1
    ## 21               NA                       NA  12200000     33.9     45.4
    ## 22               NA                       NA  11100000     17.6     55.0
    ## 23               NA                       NA  12300000     34.5     45.9
    ## 24               NA                       NA  11400000     18.1     55.5
    ## 25               NA                       NA  12400000     35.0     46.3
    ## 26               NA                       NA  11700000     18.7     55.9
    ## 27               NA                       NA  12500000     35.7     47.2
    ## 28               NA                       NA   8320000     73.2     72.8
    ## 29               NA                       NA 114000000     60.4     75.2
    ## 30               NA                       NA   5820000     47.7     77.6
    ## 31               NA                       NA  12000000     19.2     55.0
    ## 32               NA                       NA  12700000     36.4     49.7

With these tests, I can see that it is likely that there are NAs in the
world\_health\_econ dataset because certain countries - such as
Afghanistan, Iraq, Liberia, Zimbabwe, and Somalia did not report these
measures in specific years. Notably these are mostly countries where
there are considerable barriers to data collection, due to conflict and
corruption. Unfortunately, the WHO has not released a data dictionary
for this data, so it is difficult to confirm this.

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

## More examples and Useful Functions (This is only for reference.)

### Add New Variables

**mutate()** creates a new variable in our dataset and fills it with a
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

**arrange()** sorts the values in a variable from smallest to largest.
To sort from largest to smallest, we need call to arrange in descending
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
