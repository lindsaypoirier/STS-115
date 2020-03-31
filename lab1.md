Lab 1 - Introduction to R
================

  - [Instructions and Overview](#instructions-and-overview)
  - [Course Themes](#course-themes)
  - [Introduction to RStudio](#introduction-to-rstudio)
  - [R Markdown](#r-markdown)
  - [Research Plan](#research-plan)
  - [Dataset Hopping](#dataset-hopping)

<script id="twitter-wjs" type="text/javascript" async defer src="//platform.twitter.com/widgets.js"></script>

## Instructions and Overview

If you’ve reached this point in the lab, congratulations\! You have
successfully cloned your GitHub repo for this class into RStudio. At the
end of the assignment, I will explain how to commit your changes and
push them back to GitHub, where they will be accessible to me for
grading.

As we move through this notebook, we will begin to get acquainted with
the themes of the course, the statistical programming language R, and R
Markdown - the format in which these notebooks are written.
Collectively, the nine labs in this course will give you an opportunity
to engage with the course themes, data manipulation, and data
presentation directly in R notebooks. Labs 2-3 will involve research
planning, data discovery, and background research. Labs 4-7 will involve
data exploration, analysis, and critique. Finally, labs 8 and 9 will
involve summarizing findings, documenting knowledge gaps, and presenting
uncertainty. In lab 5, you will begin aggregating your code into a Shiny
data dashboard - an interactive digital tool for displaying data
analysis. Each week, we will take pieces of our labs to build upon this
dashboard.

For those of you that are Stats majors, some of the content of labs 4-7
will feel like review. Please keep in mind though that the primary goal
of this course is not to teach you statistical analysis or data science.
Instead, the primary goal of this course is to encourage you to think
critically about a data practice - recognizing the human forces that are
always involved in shaping data, responsibly documenting uncertainties
in data, and communicating ways that data can both produce and delimit
insight. While analyzing data in appropriate ways are important parts of
each assignment, the reflections that you will write about the analyses
you produce are perhaps even more important.

## Course Themes

On March 30, 2020, Nate Silver - who had been vociferoulsy vocal in
sharing his insights into data regarding the Covid-19 pandemic - tweeted
out to his networks:

<blockquote class="twitter-tweet">

<p lang="en" dir="ltr">

Don't make pretty graphs with the data either. That just obscures how
messy it is.

</p>

— Nate Silver (@NateSilver538)
<a href="https://twitter.com/NateSilver538/status/1244681742168010755?ref_src=twsrc%5Etfw">March
30,
2020</a>

</blockquote>

<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

The tweet really beautifully sums up an overriding theme of this course.
We can learn all sorts of techniques to produce beautiful charts,
tables, and visualizations with data. But when we are dealing with
messy, complex, and dynamic phenomena, such techniques can hide just how
much uncertainty there is in data and just how much situated human
perspective is interwoven through numeric calculations. This course is
designed to encourage you to look at a dataset to

Do we all know who Nate Silver is? Nate Silver gained notoriety
following the 2012 election, when he first \_\_\_. His organization
FiveThirtyEight became a go-to spot for data-based election forecasting.
However, Nate Silver is well-acquainted with the dangers of assuming
that numbers can speak for themselves.

### Ethnography

Every discipline has a set of methods that they leverage to advance
research in their fields. Disciplinary research methods refer to the
techniques and practice a particular discipline engages in order to
gather data, analyze evidence, and deepen knowledge on a particular
topic. For chemists, such methods might include experimentation and
observation. For psychologists, such methods might include case studies
and neuro-imaging. I’m an anthropologist, and in anthropology, one of
our key methods is known as ethnography. Ethnography is the practice of
studying people and cultures. To do so, ethnographers may observe people
as they interact in, move through, or make decisions within a particular
space. Alternatively, they may interview people, eliciting narratives
about what they find important, how they think about the world, and how
their personal backgrounds have motivated their current commitments.
Importantly, ethnography is often immersive; ethnographers deeply engage
in a particular space, gathering as much detail as they can (and often
more detail than they know what to do with) in order to later analyze
and relay how cultural forces are operating within it.

In this course, we are going to conduct an ethnography of a dataset. You
may be asking - if ethnography is about studying people, why are we
using it to study a dataset? The reason for this is that, in this
course, we are taking as a given that all data has in certain ways been
shaped by human assumptions, judgments, and politics. Let me provide an
example. Let’s say that we are looking at a dataset documenting

In this course, we are not only going to consider what quantitative
insights we can extrapolate from data. We are also going to analyze the
data for what it tells us about culture - how people perceive difference
and belonging, how people represent complex ideas numerically, and how
prioritize certain forms of knowledge over others.

## Introduction to RStudio

## R Markdown

R Markdown is an authoring framework for R. It enables you to style and
publish documents with code for data analysis embedded within it. In
future labs, we will embed code in our documents. However for now, here
are a few tricks for styling documents in R Markdown.

### Code Chunks

```` r
This grey box between ```{r} and ``` is a code chunk. 

Typically, when we put code in this chunk and then publish our R Markdown document, 
the code written here will be run and output on the screen. 

However, by placing eval=FALSE in the brackets, we tell R not to evaluate the code. 

This is useful when we are just filling text into the code chunk.
````

### Headings

``` r
Use '#' to create headings:
  
# Heading 1
## Heading 2
### Heading 3
```

### Tables

``` r
Tables can be formated like this:
  
Header 1 | Header 1
-------- | --------
Cell 1  | Cell 2
Cell 3  | Cell 4
```

| Header 1 | Header 1 |
| -------- | -------- |
| Cell 1   | Cell 2   |
| Cell 3   | Cell 4   |

### Bullets and Numbering

``` r
You can bullet content using the '*':

* Point 1
* Point 2
```

This will output:

  - Point 1
  - Point 2

<!-- end list -->

``` r
You can number content like this:

1. Point 1
2. Point 2
```

This will output:

1.  Point 1
2.  Point 2

For more information about formatting documents in R Markdown see
[this](https://rstudio.com/wp-content/uploads/2015/02/rmarkdown-cheatsheet.pdf)
cheatsheet. Note how I created a hyperlink
here\!

-----

## Research Plan

### What topic am I interested in investigating?

``` r
Fill response here. 
```

### Why is this topic relevant to contemporary public health or social vulnerability in the wake of a disaster?

``` r
Fill response here. 
```

*Empirical research questions* are questions that an analyst can assess
evidence to address. Examples of social-theoretical questions include:

  - Do United States hospitals have enough beds to accommodate the
    expected influx in Covid-19 cases? At what point will there not be
    enough beds available?
  - In which states are hospitals better equipped with beds and staff to
    take on an influx in
patients?

### What empirical research questions do I intend to address in my research? List at least two.

``` r
Fill response here. 
```

*Social-Theoretical questions* are questions about how certain social,
political, or environmental phenomena operate at a broad scale. We often
cannot answer these questions with one research project. However, we
often aim to increase understanding of these questions through a
research project. Examples of social-theoretical questions include:

  - How has the United States prioritized critical health
    infrastructure?
  - How do national hospital business models implicate social
    vulnerability in the wake of a public health
crisis?

### What social-theoretical questions do I intend to address in my research? List at least two.

``` r
Fill response here. 
```

### What data do I believe exists about this topic?

Fill response in the table
below:

| Dataset                              | Do you think this data is accessible? |
| ------------------------------------ | ------------------------------------- |
| Number of cases of Covid-19 globally | Yes                                   |
| Fill                                 | Fill                                  |
| Fill                                 | Fill                                  |
| Fill                                 | Fill                                  |
| Fill                                 | Fill                                  |
| Fill                                 | Fill                                  |

### What are the key events, dates, or years relevant to this topic?

``` r
Fill response here. 
```

### What are the key sites, locations, or geographies relevant to this topic?

``` r
Fill response here. 
```

### What social groups are impacted by this topic?

``` r
Fill response here. 
```

### How is the environment implicated in this topic?

``` r
Fill response here. 
```

### How is the economy implicated in this topic?

``` r
Fill response here. 
```

### How are politics implicated in this topic?

``` r
Fill response here. 
```

### How am I implicated in this topic?

``` r
Fill response here. 
```

### What are some of the most common ways different social groups talk about this topic?

``` r
Fill response here. 
```

-----

## Dataset Hopping

At this point in the quarter, you will begin to search for open
government or academic datasets.

Questions

  - Dataset?: What is the title of the dataset?
  - Found?: Where did you find it?
  - Formats?: In what formats is it available for download?
  - DD?: Is there a data dictionary or data documentation available for
    download?
  - Timespan?: What range of time does it cover?
  - Geography? What geographic range does it cover?
  - Row?: What does each row in the dataset represent?
  - Update?: How often is the dataset
updated?

| Dataset?                        | Found?                                       | Formats? | DD?  | Timespan?               | Geography?                 | Row?                              | Update? |
| ------------------------------- | -------------------------------------------- | -------- | ---- | ----------------------- | -------------------------- | --------------------------------- | ------- |
| 2019 Novel Coronavirus COVID-19 | <https://github.com/CSSEGISandData/COVID-19> | .csv     | No   | Jan 22, 2020 to Present | Global by Province/Country | Cases per day by Province/Country | Daily   |
| Fill                            | Fill                                         | Fill     | Fill | Fill                    | Fill                       | Fill                              | Fill    |
| Fill                            | Fill                                         | Fill     | Fill | Fill                    | Fill                       | Fill                              | Fill    |
| Fill                            | Fill                                         | Fill     | Fill | Fill                    | Fill                       | Fill                              | Fill    |
| Fill                            | Fill                                         | Fill     | Fill | Fill                    | Fill                       | Fill                              | Fill    |
