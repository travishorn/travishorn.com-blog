---
title: "Data Types: Continuous, Discrete, and Related Categories"
datePublished: Sat Nov 01 2025 15:42:32 GMT+0000 (Coordinated Universal Time)
cuid: cmhggaxji000002jpc9354vof
slug: data-types-continuous-discrete-and-related-categories
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762011655156/ab2481d1-ecc1-46e0-883f-5496c71467dd.png
tags: variables, statistics, data-science, analysis, measurement

---

Recognizing data types is a foundational concept in statistics and data science because the type of data you have dictates the appropriate statistical methods, visualization, and analyses to use. This a quick guide to help you identify and differentiate between the main categories of data, explain their characteristics, recognize the appropriate analyses, understand distinctions, and avoid common mistakes when analyzing data.

## Quantitative: Continuous

Continuous values are numeric; any value with an interval.

For example:

* Height (inches and centimeters)
    
* Weight (pounds and kilograms)
    
* Temperature (Fahrenheit and Celsius)
    
* Time (seconds and minutes)
    
* Blood pressure (mmHg - millimeters of mercury)
    
* Concentration (mol/L - moles of solute per liter of solution)
    
* Distance (miles and kilometers)
    
* Voltage (volts)
    
* Gross domestic product (GDP) per capita
    
* Reaction time (milliseconds)
    

Analysis on continuous values typically include t-test, ANOVA, linear regression, density plots, and histograms.

![A bell curve graph showing the height distribution of U.S. women, with the highest frequency around 5'4" to 5'6".](https://cdn.hashnode.com/res/hashnode/image/upload/v1762008980483/1381ce0e-a74a-4fa7-bb0c-c98fb888ed89.jpeg align="center")

A bell curve plot showing the height of U.S. women. From [Lumen Learning](https://courses.lumenlearning.com/suny-hvcc-psychology-1/chapter/measures-of-intelligence/).

![Histogram showing customer age distribution, with the highest frequency around ages 40-50, decreasing steadily in older age groups. Frequency is on the left axis and percent of total on the right.](https://cdn.hashnode.com/res/hashnode/image/upload/v1762009265703/44c165db-be87-4b3a-bff1-675e78af85cd.png align="center")

A histogram showing customer age groups. From [Paul Turley’s SQL Server BI Blog](https://sqlserverbi.blog/10-histogram-chart/).

![Scatter plot showing the relationship between square footage and price for homes. The x-axis represents square footage, and the y-axis represents price in thousands. Data points are scattered, generally showing an upward trend.](https://cdn.hashnode.com/res/hashnode/image/upload/v1762009423798/efcb04f8-9033-44b0-a780-48a7ceabf93e.jpeg align="center")

A scatter plot showing square footage vs price for homes. From [Visme](https://visme.co/blog/scatter-plot/).

Think about measuring someone's height. While height is continuous, your measuring tape has its limits. You might record 5'8", but a more precise tool could reveal it's actually 5'8.125". The tools we use always put a limit on how precise our continuous data can be, and it's something to keep in mind when you're analyzing it.

## Quantitative: Discrete

Discrete values are also numeric, but represent distinct separate values (usually integers).

For example:

* Number of children
    
* Defect count
    
* Number of visits
    
* Eggs laid
    
* Phone call counts
    
* Number of transactions
    
* Dice outcomes
    
* Goals scored
    

Discrete values are typically analyzed using Poisson/negative binomial regression, chi-square (counts), bar charts, and frequency tables.

![Bar graph showing the number of students for five periods. Period 1 has 3 students, Period 2 has 9, Period 3 has 6, Period 4 has 15, and Period 5 has 12.](https://cdn.hashnode.com/res/hashnode/image/upload/v1762009537918/35520d0a-65b4-4566-b097-b88808b5520e.png align="center")

Bar chart showing number of students in various classes. From [Animalia Life](https://animalia-life.club/qa/pictures/bar-graphs-examples).

![Scatter plot titled "Store's Visitors Analysis," showing the number of visitors per hour from 12 to 24. Peaks are visible around 17:00 and 21:00 hours.](https://cdn.hashnode.com/res/hashnode/image/upload/v1762009664449/60a227f1-de3e-48fd-8bd2-8f8c64f68a01.jpeg align="center")

Dot plot showing visitors per hour of the day. From [Anamalia Life](https://animalia-life.club/qa/pictures/dot-plot).

Discrete values can further be broken down into *small-range* and *count* data. Think about the difference between the number on a dice roll and the number of views a YouTube video has? Both are discrete, but we think about them differently. A dice roll is *small-range* discrete data. it has a fixed, limited set of outcomes (1 through 6). Now, the number of views is *count* data. It could be 0, 10, or 10,000 (there’s no upper limit).

## Categorical: Nominal and Ordinal

Nominal values are categories without order.

For example:

* Eye colors (brown, blue, and green)
    
* Blood types (A+, A-, B+, B-, AB+, AB-, O+, O-)
    
* Species (tigers, elephants, blue whales, oak trees, and sunflowers)
    
* Cities (Tokyo, Sao Paulo, Cairo, and New York City)
    
* Brands (Apple, Samsung, and Mercedes-Benz)
    

You can plot nominal values in a contingency table, chi-squares, and bar charts.

Ordinal values are ordered categories where the intervals are not equal.

For example:

* Likert scale (strongly disagree, disagree, neutral, agree, strongly agree)
    
* Education level (less than high school, high school graduate, some college, bachelor’s degree, master’s degree, doctorate)
    
* Customer satisfaction (dissatisfied, neutral, satisfied)
    
* Medal rank (bronze, silver, and gold)
    

Ordinal values are typically plotted using ordinal logistic regression, median/IQR, and stacked bar charts.

![Horizontal stacked bar chart titled "Quarterly product sales" showing Q1 to Q4 data. Categories include Air Conditioner (yellow), Television (orange), Electric Heater (red), and Refrigerator (dark red).](https://cdn.hashnode.com/res/hashnode/image/upload/v1762009762567/8cd3511f-b591-4253-b9e3-3929e50c061e.png align="center")

Stacked bar chart showing quarterly product sales. From [Visual Paradigm Online](https://online.visual-paradigm.com/id/charts/templates/stacked-bar-charts/stacked-bar-chart/).

![Treemap showing survival rates on the Titanic based on class and crew status. The chart is divided by class categories: 1st, 2nd, 3rd, and Crew. Colors indicate survival, with red for no and teal for yes.](https://cdn.hashnode.com/res/hashnode/image/upload/v1762009867768/4f173e3e-bf4e-4b20-a29b-e6a0ef36298d.png align="center")

A mosaic plot showing survival per class. From [ggmosaic](https://haleyjeppson.github.io/ggmosaic/reference/geom_mosaic.html).

Sometimes it can be tempting to treat ordinal values as numeric. Say you’ve got a survey with responses from “Very Dissatisfied” to “Very Satisfied".” You could assign numbers from 1 to 5, but is the jump from “Very Dissatisfied” to “Dissatisfied” the same size as the jump from “Neutral” to “Satisfied?” Probably not.

It’s common to treat some ordinal data (like Likert scales) as numeric because it unlocks simpler analysis, but be aware you’re making a big assumption that the distance between each category is equal.

## Binary and Dichotomous

Binary values appear when there are two possible values.

For example:

* Yes/No
    
* True/False
    
* Infected/Not Infected
    
* Pass/Fail
    

Analysis on binary values can be done using logistic regression, confusion matrices, ROC curves, and bar charts.

![Graph showing a ROC curve with a red line representing the trade-off between true positive rate (y-axis) and false positive rate (x-axis). Dashed lines indicate specific points labeled TPRa and FPRa.](https://cdn.hashnode.com/res/hashnode/image/upload/v1762010018620/666345f0-e2b3-4066-9250-32e6ecf3d3ca.png align="center")

A ROC curve showing true positive rate vs false positive rate. From [Evidently AI](https://www.evidentlyai.com/classification-metrics/explain-roc-curve).

## Scale-specific Numeric: Interval vs Ratio

Interval values have meaningful differences, with no true zero.

For example:

* Celsius (0, 37, 100)
    
* Fahrenheit (32, 98.6, 212)
    
* Calendar year (1492, 1776, 1945)
    

Note that an interval value can be zero (like 0 degrees Celsius), but that zero point is arbitrary; it is just a reference point, not a complete absence of the property being measured. 0 degrees Celsius doesn’t mean an absence of temperature.

Ratio values are similar, having meaningful differences, but they also include a true zero.

For example:

* Length (8 feet or 244 centimeters)
    
* Mass (102 grams or 3.6 ounces)
    
* Time elapsed (36 hours or 2160 minutes)
    
* Income ($9.50 or £7.22)
    
* Kelvin (0, 273.15, 373.15)
    

A length of 0 is a true zero; the absence of length. It doesn’t matter if you’re measuring in feet, centimeters, or anything else. It’s always zero.

With a true zero (ratio data), you can say things like "this is twice as long" or "that is half the price” because the starting point (zero) is absolute.

You can't do this without a true zero (interval data). 20 degrees Celsius is not twice as hot as 10 degrees Celsius. This is also why percentage changes like "a 50% increase in income" only make sense for ratio data.

## Count Data & Special Cases

Discrete non-negative integers are often modeled by Poisson-family distributions.

For example:

* Calls per hour
    
* Daily accident counts
    
* Species observed
    

These values are often analyzed with Poisson/negative binomial models and rate-based comparisons.

![A line graph titled "Time Series Chart" showing sales from 2010 to 2023. The sales values fluctuate, peaking around 2012, 2016, and 2022, with noticeable declines in 2011, 2014, and 2020. The y-axis is labeled "Sum of sales" and the x-axis is labeled "Year."](https://cdn.hashnode.com/res/hashnode/image/upload/v1762010113359/d00a36dd-2764-4f76-9782-02be0c2e4457.png align="center")

A time series chart showing sales over time. From [GeeksforGeeks](https://www.geeksforgeeks.org/power-bi/power-bi-how-to-create-a-time-series-chart/).

## Mixed/Hybrid Data & Variable Treatment

Some datasets contain multiple types or variables that can be binned or treated differently.

For example, a survey might include:

* Age (continuous)
    
* Satisfaction (ordinal)
    
* Gender (nominal)
    

Whenever possible, you should analyze your data using its original, most detailed scale. Keeping a variable like age as a continuous number preserves far more information than collapsing it into categories, which ultimately gives your analysis more statistical power to detect relationships.

However, binning is useful when the general group is more important than the exact measurement. This can make complex data easier to interpret. For age, you might bin the data into bins of 18-24, 25-34, 35-44, and so on.

| **Continuous** | **Binned** | **Ordinal** |
| --- | --- | --- |
| 28 years | 20-30 | Young Adult |
| 45.7 years | 40-50 | Middle-aged |
| 19.2 years | 10-20 | Teenager |
| 61 years | 60-70 | Senior |

## Decision Flow for Classifying a Variable

1. Is the variable numeric? If yes, it’s quantitative. If no, it’s categorical.
    
2. If numeric, can it take any real value in a range? If yes, it’s continuous. If no (integers/counts), it’s discrete.
    
3. If categorical, are categories ordered? If yes, it’s ordinal. If no, it’s nominal.
    

![Flowchart for data classification: Starts with "Numeric?" leading to "Categorical" if no, or "Quantitative" if yes. Quantitative splits into "Discrete" for "Take any value? No" and "Continuous" for "Yes." Categorical branches into "Ordered?" leading to "Ordinal" if yes, and "Nominal" if no.](https://cdn.hashnode.com/res/hashnode/image/upload/v1762011147836/127b3c6c-1c8c-49ee-a439-df61eb6d7741.png align="center")

### Example 1: Number of Siblings

Is it numeric? Yes, it’s a count. So it’s **quantitative**.

Can it take any value? No, you can’t have 1.5 siblings. It must be a whole number. So it’s **discrete**.

### Example 2: T-shirt Size

Is it numeric? No. So it’s **categorical**.

Is there a clear order? Yes, “large” is bigger than “medium.” So it’s **ordinal**.

## Statistical Test Quick Reference

For continuous vs continuous data, use correlation, linear regression, t-test, or ANOVA.

For continuous vs categorical data (with two groups), use t-test or Mann-Whitney (if nonparametric).

For categorical vs categorical data, use chi-square or Fisher’s exact.

For ordinal outcomes, use Poisson/negative binomial regression.

## Avoiding Common Pitfalls

* Be cautious of arbitrarily binning continuous data.
    
* Check the distribution.
    
* Consider the measurement scale.
    
* Choose models that match the data type.
    
* Don’t treat ordinal data as interval without justification.
    
* Watch for zero-inflation in counts.
    
* Make sure you’re not misusing parametric tests on skewed continuous data.
    

Getting a handle on data types might seem like a small detail, but it's the foundation for any solid analysis. Choosing the right chart, the right model, or the right statistical test all comes back to this first, crucial step. Get this step right, and you're set up for telling an accurate data story.

Cover image by [Liana S](https://unsplash.com/@cherstve_pechivo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/blurred-green-and-yellow-lights-on-dark-background-Co87kzeFQMI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).