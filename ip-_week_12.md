Independent Project Week 12
================
Roselynn
1/8/2021

### Defining the Question

To identify the individuals that are more likely to click on the ads.

### The Metric for Success

To provide an accurate picture of the people most likely to view the
clients advertisements and provide recommendations to the client based
on the results of this analysis.

### The Context

A Kenyan entrepreneur has created an online cryptography course and
would want to advertise it on her blog. She currently targets audiences
originating from various countries. In the past, she ran ads to
advertise a related course on the same blog and collected data in the
process. She would now like to employ your services as a Data Science
Consultant to help her identify which individuals are most likely to
click on her ads

### The Experimental Design Taken

For this analysis I first loaded the dataset provided , previewed the
dataset and carried out data cleaning. Next I checked for anomalies and
outliers in the dataset and dealt with these. Finally I carried out
Univariate and Bivariate analysis and provided insights and
recommendations to the client based on my findings.

### Appropriateness of the data

Given the task at hand the data provided is appropriate to carry out
this analysis.

# 1\. Loading the Appropriate Libraries and Importing the Dataset

I loaded the tidyverse library onto my editor and the I imported the
dataset and previewed it

``` r
library(tidyverse)
```

    ## -- Attaching packages ----------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.2     v purrr   0.3.4
    ## v tibble  3.0.3     v dplyr   1.0.0
    ## v tidyr   1.1.0     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.5.0

    ## -- Conflicts -------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

I then imported the dataset and previewed the top and bottom columns in
the dataset.

``` r
#Importing  the dataset
advertising <- read_csv("C:/Users/Njeri/Downloads/advertising.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Daily Time Spent on Site` = col_double(),
    ##   Age = col_double(),
    ##   `Area Income` = col_double(),
    ##   `Daily Internet Usage` = col_double(),
    ##   `Ad Topic Line` = col_character(),
    ##   City = col_character(),
    ##   Male = col_double(),
    ##   Country = col_character(),
    ##   Timestamp = col_datetime(format = ""),
    ##   `Clicked on Ad` = col_double()
    ## )

``` r
View(advertising)
```

``` r
# Previewing the dataset
head(advertising)
```

    ## # A tibble: 6 x 10
    ##   `Daily Time Spe~   Age `Area Income` `Daily Internet~ `Ad Topic Line` City 
    ##              <dbl> <dbl>         <dbl>            <dbl> <chr>           <chr>
    ## 1             69.0    35        61834.             256. Cloned 5thgene~ Wrig~
    ## 2             80.2    31        68442.             194. Monitored nati~ West~
    ## 3             69.5    26        59786.             236. Organic bottom~ Davi~
    ## 4             74.2    29        54806.             246. Triple-buffere~ West~
    ## 5             68.4    35        73890.             226. Robust logisti~ Sout~
    ## 6             60.0    23        59762.             227. Sharable clien~ Jami~
    ## # ... with 4 more variables: Male <dbl>, Country <chr>, Timestamp <dttm>,
    ## #   `Clicked on Ad` <dbl>

``` r
tail(advertising)
```

    ## # A tibble: 6 x 10
    ##   `Daily Time Spe~   Age `Area Income` `Daily Internet~ `Ad Topic Line` City 
    ##              <dbl> <dbl>         <dbl>            <dbl> <chr>           <chr>
    ## 1             43.7    28        63127.             173. Front-line bif~ Nich~
    ## 2             73.0    30        71385.             209. Fundamental mo~ Duff~
    ## 3             51.3    45        67782.             134. Grass-roots co~ New ~
    ## 4             51.6    51        42416.             120. Expanded intan~ Sout~
    ## 5             55.6    19        41921.             188. Proactive band~ West~
    ## 6             45.0    26        29876.             178. Virtual 5thgen~ Ronn~
    ## # ... with 4 more variables: Male <dbl>, Country <chr>, Timestamp <dttm>,
    ## #   `Clicked on Ad` <dbl>

The columns in the dataset were :

``` r
names(advertising)
```

    ##  [1] "Daily Time Spent on Site" "Age"                     
    ##  [3] "Area Income"              "Daily Internet Usage"    
    ##  [5] "Ad Topic Line"            "City"                    
    ##  [7] "Male"                     "Country"                 
    ##  [9] "Timestamp"                "Clicked on Ad"

The descriptions of these columns was as follows:

1.  ‘Daily Time Spent on Site’: consumer time on site in minutes
2.  ‘Age’: customer age in years
3.  ‘Area Income’: Avg. Income of geographical area of consumer
4.  ‘Daily Internet Usage’: Avg. minutes a day consumer is on the
    internet
5.  ‘Ad Topic Line’: Headline of the advertisement
6.  ‘City’: City of consumer
7.  ‘Male’: Whether or not consumer was male
8.  ‘Country’: Country of consumer
9.  ‘Timestamp’: Time at which consumer clicked on Ad or closed window
10. ‘Clicked on Ad’: 0 or 1 indicated clicking on Ad

I then checked for the data types of the columns in the dataset. The
dataset had 1,000 rows and 10 columns , with three being of type
character , one of type datetime and the others being numeric. The
Numeric variables had four continuous and two categorical variables.

``` r
glimpse(advertising)
```

    ## Rows: 1,000
    ## Columns: 10
    ## $ `Daily Time Spent on Site` <dbl> 68.95, 80.23, 69.47, 74.15, 68.37, 59.99...
    ## $ Age                        <dbl> 35, 31, 26, 29, 35, 23, 33, 48, 30, 20, ...
    ## $ `Area Income`              <dbl> 61833.90, 68441.85, 59785.94, 54806.18, ...
    ## $ `Daily Internet Usage`     <dbl> 256.09, 193.77, 236.50, 245.89, 225.58, ...
    ## $ `Ad Topic Line`            <chr> "Cloned 5thgeneration orchestration", "M...
    ## $ City                       <chr> "Wrightburgh", "West Jodi", "Davidton", ...
    ## $ Male                       <dbl> 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0...
    ## $ Country                    <chr> "Tunisia", "Nauru", "San Marino", "Italy...
    ## $ Timestamp                  <dttm> 2016-03-27 00:53:11, 2016-04-04 01:39:0...
    ## $ `Clicked on Ad`            <dbl> 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 0...

I then used the summary() function to obtain the summary statistics of
the columns in the dataset.

``` r
summary(advertising)
```

    ##  Daily Time Spent on Site      Age         Area Income    Daily Internet Usage
    ##  Min.   :32.60            Min.   :19.00   Min.   :13996   Min.   :104.8       
    ##  1st Qu.:51.36            1st Qu.:29.00   1st Qu.:47032   1st Qu.:138.8       
    ##  Median :68.22            Median :35.00   Median :57012   Median :183.1       
    ##  Mean   :65.00            Mean   :36.01   Mean   :55000   Mean   :180.0       
    ##  3rd Qu.:78.55            3rd Qu.:42.00   3rd Qu.:65471   3rd Qu.:218.8       
    ##  Max.   :91.43            Max.   :61.00   Max.   :79485   Max.   :270.0       
    ##  Ad Topic Line          City                Male         Country         
    ##  Length:1000        Length:1000        Min.   :0.000   Length:1000       
    ##  Class :character   Class :character   1st Qu.:0.000   Class :character  
    ##  Mode  :character   Mode  :character   Median :0.000   Mode  :character  
    ##                                        Mean   :0.481                     
    ##                                        3rd Qu.:1.000                     
    ##                                        Max.   :1.000                     
    ##    Timestamp                   Clicked on Ad
    ##  Min.   :2016-01-01 02:52:10   Min.   :0.0  
    ##  1st Qu.:2016-02-18 02:55:42   1st Qu.:0.0  
    ##  Median :2016-04-07 17:27:29   Median :0.5  
    ##  Mean   :2016-04-10 10:34:06   Mean   :0.5  
    ##  3rd Qu.:2016-05-31 03:18:14   3rd Qu.:1.0  
    ##  Max.   :2016-07-24 00:22:16   Max.   :1.0

# 2\. Data Cleaning

The first step of my data cleaning process was checking for any null
entries in the dataset using the colSums and is.na functions. The
dataset had no null entries.

``` r
colSums(is.na(advertising))
```

    ## Daily Time Spent on Site                      Age              Area Income 
    ##                        0                        0                        0 
    ##     Daily Internet Usage            Ad Topic Line                     City 
    ##                        0                        0                        0 
    ##                     Male                  Country                Timestamp 
    ##                        0                        0                        0 
    ##            Clicked on Ad 
    ##                        0

Next I checked for any duplicated entries in the dataset. I deduced that
there were none.

``` r
sum(duplicated(advertising))
```

    ## [1] 0

Given that the dataset had neither null nor duplicated values I carried
out my exploratory data analysis.

# 3\. Exploratory Data Analysis

### 3.1. Univariate Data Analysis

For my univariate data analysis I opted to analyze all the columns
individually to allow me to be able to pay close attention to my
findings.

``` r
time <- advertising$`Daily Time Spent on Site`
age <- advertising$Age
income <-  advertising$`Area Income`
usage <- advertising$`Daily Internet Usage`
```

I used the moments package in R to check for the skewness and kurtosis
of the columns in the dataset.

``` r
#install.packages('moments')
library(moments)
```

#### Daily Time Spent on the Site

As per the column descriptions , this column recorded the daily amount
of time users spent on the site.

``` r
summary(time)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   32.60   51.36   68.22   65.00   78.55   91.43

``` r
# Calculating the mode
getmode <- function(v) #getmode is the function name
  {
   uniqv <- unique(v)
   uniqv[which.max(tabulate(match(v, uniqv)))]
}
# Calculate the mode using the user function.
mode <- getmode(time)
print(mode)
```

    ## [1] 62.26

``` r
# The Standard Deviation and Variance
var(time)
```

    ## [1] 251.3371

``` r
sd(time)
```

    ## [1] 15.85361

``` r
#The range of the Variable
range(time)
```

    ## [1] 32.60 91.43

``` r
#The Interquartile Range
IQR(age)
```

    ## [1] 13

``` r
#The Skew and Kurtosis of the column
skewness(time)
```

    ## [1] -0.3712026

``` r
#The Kurtosis
kurtosis(time)
```

    ## [1] 1.903942

``` r
# Histogram with density plot
ggplot(advertising, aes(x=`Daily Time Spent on Site`)) + 
 geom_histogram(colour="black", fill="grey",bins=10)#+
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
 #geom_density(alpha=.2, fill="#FF6666") 
# PLotting a boxplot to check for outliers
ggplot(advertising, aes(x=`Daily Time Spent on Site`)) + geom_boxplot(outlier.color = 'black',fill='grey')
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-11-2.png)<!-- -->

From this column I observed that :

  - The mean amount of time that users spent on the site was 65 minutes
    while the maximum amount of time a user spent on the site was 91.43
    minutes.
  - The Variance of the column was 251.3371 with a standard deviation of
    15.85361.
  - The data was negatively but fairly symmetrical with a value of
    -0.3712026 and the distribution can be categorized as platykurtic
    with a kurtosis value of 1.903942
  - Furthermore plotting a boxplot of the variable I observed that it
    didnot have any outliers .
  - Plotting a histogram for the column we can see that approxiamtely
    125 users spent over 80 minutes daily on the site , and many users
    spent over 60 minutes on the site daily.

#### Age

``` r
summary(age)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   19.00   29.00   35.00   36.01   42.00   61.00

``` r
# Calculating the mode
getmode <- function(v) #getmode is the function name
  {
   uniqv <- unique(v)
   uniqv[which.max(tabulate(match(v, uniqv)))]
}
# Calculate the mode using the user function.
mode <- getmode(age)
print(mode)
```

    ## [1] 31

``` r
# The Standard Deviation and Variance
var(age)
```

    ## [1] 77.18611

``` r
sd(age)
```

    ## [1] 8.785562

``` r
#The range of the Variable
range(age)
```

    ## [1] 19 61

``` r
#The interquartile range
IQR(age)
```

    ## [1] 13

``` r
#The Skew and Kurtosis of the column
skewness(age)
```

    ## [1] 0.4784227

``` r
#The Kurtosis
kurtosis(age)
```

    ## [1] 2.595482

``` r
# Histogram with density plot
ggplot(advertising, aes(x=age)) + 
 geom_histogram(colour="black", fill="grey",bins=10)#+
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

``` r
 #geom_density(alpha=.2, fill="#FF6666") 
# PLotting a boxplot to check for outliers
ggplot(advertising, aes(x=age)) + geom_boxplot(outlier.color = 'black',fill='grey')
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-12-2.png)<!-- -->

From the Age column I observed that :

  - The mean age of the consumers was 36.01 while the median age was 35
    and the modal age was 31.
  - The variable was positively skewed and fairly symmetrical with a
    skew value of 0.4777052 , the distribution was platykurtic with a
    kurtosis value of 2.595482
  - The age of the consumers ranged between 19 and 61 , with majority of
    the users ranging between 30 and 45. The interquartile age for the
    upper and lower quartile was 13.
  - The were no outliers in this column as observed from the boxplot.

#### Area Income Variable

``` r
summary(income)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   13996   47032   57012   55000   65471   79485

``` r
# Calculating the mode
getmode <- function(v) #getmode is the function name
  {
   uniqv <- unique(v)
   uniqv[which.max(tabulate(match(v, uniqv)))]
}
## Calculate the mode using the user function.
mode <- getmode(income)
print(mode)
```

    ## [1] 61833.9

``` r
# The Standard Deviation and Variance
var(income)
```

    ## [1] 179952406

``` r
sd(income)
```

    ## [1] 13414.63

``` r
#The range of the Variable
range(income)
```

    ## [1] 13996.5 79484.8

``` r
#The Interquartile range
IQR(income)
```

    ## [1] 18438.83

``` r
#The Skew and Kurtosis of the column
skewness(income)
```

    ## [1] -0.6493967

``` r
#The Kurtosis 
kurtosis(income)
```

    ## [1] 2.894694

``` r
# Histogram with density plot
ggplot(advertising, aes(x=income)) + 
 geom_histogram(colour="black", fill="grey",bins=15)#+
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

``` r
 #geom_density(alpha=.2, fill="#FF6666") 
# PLotting a boxplot to check for outliers
ggplot(advertising, aes(x=income)) + geom_boxplot(fill='grey',outlier.color = 'black')
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-13-2.png)<!-- -->

From the Income column we can deduce that :

  - The median income of the users was 57,012 while the mean income was
    55,000 and the modal income was 61,833.9.
  - The income of the site users ranged from 13,996 to 79,485 , with the
    interquarile range being 18483.83.
  - The data had a negative skew and was moderately skewed with a value
    of -0.6484229, the data had a platykurtic distribution with a value
    of 2.894694.
  - There were several outliers in the column

#### Daily Internet Usage Variable

``` r
summary(usage)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   104.8   138.8   183.1   180.0   218.8   270.0

``` r
# Calculating the mode
getmode <- function(v) #getmode is the function name
  {
   uniqv <- unique(v)
   uniqv[which.max(tabulate(match(v, uniqv)))]
}
# Calculate the mode using the user function.
mode <- getmode(usage)
print(mode)
```

    ## [1] 167.22

``` r
# The Standard Deviation and Variance
var(usage)
```

    ## [1] 1927.415

``` r
sd(usage)
```

    ## [1] 43.90234

``` r
#The range of the Variable
range(usage)
```

    ## [1] 104.78 269.96

``` r
#The Interquartile Range
IQR(usage)
```

    ## [1] 79.9625

``` r
#The Skew and Kurtosis of the column
skewness(usage)
```

    ## [1] -0.03348703

``` r
# Kurtosis
kurtosis(usage)
```

    ## [1] 1.727701

``` r
# Histogram with density plot
ggplot(advertising, aes(x=usage)) + 
 geom_histogram(colour="black", fill="grey",bins=10)#+
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
 #geom_density(alpha=.2, fill="#FF6666") 
# PLotting a boxplot to check for outliers
ggplot(advertising, aes(x=usage)) + geom_boxplot(outlier.color = 'black',fill='grey')
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-14-2.png)<!-- -->

From the daily internet usage we can see that:

  - The Average Hours spent by users on the Internet is 180 minutes
    while the median is 183.1 and the mode is 167.22.
  - The Interquartile Range is 79.9625.
  - The data is negatively but fairly skewed with a skew value of
    -0.03343681 and the data is platykurtic with a value of 1.727701
  - There were no outliers in this column as observed in the boxplot
    plotted for this column.

#### Plotting Countplots for the Categorical variables.

**I then plotted countplots for the categorical data in the dataset and
observed that:**

``` r
ggplot(advertising, aes(x=Male)) + geom_bar(fill=rgb(0.4,0.1,0.5))
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

There were more non-male than male users who visited the site.

``` r
ggplot(advertising, aes(x=factor(`Clicked on Ad`))) + geom_bar( fill=rgb(0.4,0.1,0.5))
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

The number of users on the site who clicked on the ad is equal to those
that did not.

### 3.2. Bivariate Data Analysis

I plotted scatter plots for the numeric variables in the dataset

``` r
ggplot(advertising, aes(x=income , y = age )) + geom_point(aes(colour= factor(`Clicked on Ad`)))
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

  - The scatter plot for the Area Income against Age showed that ,
    majority of the users who did not click on the ad were the high
    income earners and many of these were aged between 20 and 40 years.

  - This is a particularly interesting fact , a survey by the
    WebStrategies company that showed that high income earners are less
    tolerant to ads thus less likely to click on banner or mobile ads on
    the internet. [How Does Income Level Affect Online
    Behavior](https://www.webstrategiesinc.com/blog/how-does-income-level-affect-online-behavior-in-central-virginia)

Next I plotted a scatter plot for the Income and Time, time spent of the
site i.e, variables.

``` r
ggplot(advertising, aes(x=income , y = time )) + geom_point(aes(colour= factor(`Clicked on Ad`)))
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

  - Once more I noted that the people who were least likely to click on
    the ad were the higher income earners , this was despite the fact
    that they seemed to spend a over an hour a day on the site.

  - The same sentiments can be echoed for the Usage , total internet
    usage per day , variable . When plotted against income we see that
    those who spend over 200 minutes online all day and earn more than
    50,000 are the least likely to click on ads on the internet.

<!-- end list -->

``` r
ggplot(advertising, aes(x=income , y = usage)) + geom_point(aes(colour= factor(`Clicked on Ad`)))
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

``` r
ggplot(advertising, aes(x = age , y = time )) + geom_point(aes(colour= factor(`Clicked on Ad`)))
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

  - Further plotting the Age against Time spent on the site variable we
    see that the younger demographic are less tolerant to ads despite
    spending significant amounts of time on the site .

  - The reason for this may be that younger people , are more tech savvy
    and therefore are more likely to detect ads and avoid them while
    using the internet compared to their older counterparts.

  - Similar behavior can be seen for the Amount of time spent by the
    users online.According to the article linked here [Baby Boomers Far
    More Likely to Click Online
    Ads](https://www.prnewswire.com/news-releases/baby-boomers-far-more-likely-to-click-online-ads-than-younger-generations---but-irrelevant-ads-frustrate-everyone-130265733.html)

<!-- end list -->

``` r
ggplot(advertising, aes(x= age , y = usage )) + geom_point(aes(colour= factor(`Clicked on Ad`)))
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

Finally I plotted a correlation matrix .

  - Plotting a correlation matrix for the numeric variables :

<!-- end list -->

``` r
# Selecting the Numerical Variables of the dataset
corr <- select(advertising , Age , 'Area Income', `Clicked on Ad`,
               `Daily Internet Usage`, `Daily Time Spent on Site` , Male )
```

``` r
# Plotting the Correlation Heatmap
library(ggcorrplot)
ggcorrplot(cor(corr), hc.order = F,type = 
"lower", lab = T ,
  ggtheme = ggplot2::theme_gray,
  colors = c("#00798c", "white", "#edae49"))
```

![](ip-_week_12_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

  - Here I noted that :
    
      - There was a strong negative correlation between the Daily
        Internet usage and Clicked on Ad variables. This means that the
        higher ones income the less likely they are to click on the blog
        ads. The same can also be said for the Daily Time Spent on Site
        and Click on ad variables.
    
      - The Click on Ad variable had a strong positive correlation with
        the Age Variable, the older users were more likely to click on
        the ad , as we observed above in our analysis.
    
      - The clicked on ad variable was also strongly negatively
        correlated with the Area Income , where the higher ones income
        was the less likely they were to click on the ad.

# 4\. Conclusions and Recommendations.

  - In analyzing this data I deduced that:
      - Older people , those over 35 were more likely to click on the
        course ad.
      - The individuals earning higher salaries were more likely not to
        click on the ad.
      - The probability that a consumer would click on the ad was 0.5.
      - The more time users spent on the blog , the less likely they
        were to click on the advertisement.
  - Thus given these observations I would recommend that:

<!-- end list -->

1.  The Blogger should target users who were aged over 35 , as they were
    more likely to click on the ad.

2.  Furthermore focusing more on those earning a lower income i.e less
    than 60,000 would prove to be more beneficial as these consumers
    click on ads more.

3.  Finally the users who spend less time on the site and on the
    internet in general would prove a better demographic for the ads.
