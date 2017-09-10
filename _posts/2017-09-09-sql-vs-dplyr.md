---
layout: post
title: "SQL vs dplyr-tidyr"
categories: blog
tags:
- R
- data wrangling
type: post
published: true
word-count: false
---

Data manipulation is very basic and fundamental in data science. No need to know fancy machine learning techniques, if you understand SQL, you can land your first data job as a Data Analyst or Business Intelligence Analyst. 

As a statistics graduate, I never got any knowledge	 in data manipulation like SQL. Many data that I face when I was in college is  clean. The fact is that in real life or industry, you will never get clean data to be analyzed. You can not rely on your data engineer to make your data pristine or to have the form of data that you want. Sometimes, you also need to do a little data manipulation before you feed the data into the model.

I mostly used SQL in my first job even though I am a statistician who never get any teaching about it at college. Nowadays, I love to use `dplyr` and `tidyr`, library in R, to do data manipulation. I use the combination of both. I use SQL for getting aggregated data via Hive or Presto into my local machine and then I use dplyr/tidyr to do further data manipulation in purpose of data exploration, data visualization and modeling.

These lists are comparisons between SQL and dplyr/tidyr verbs. 

* TOC
{:toc}

### Select All Columns Except Column 'ce' and 'cj'

Supposed, we have table with column 'ca-cj'. We do not want to take column 'ce' and column 'cj'. In `dplyr`, we can use this.

```r
tbl %>% select(-ce, -cj)
```

We can state all column except column 'ce' and column 'cj' in SQL.

```sql
SELECT ca, cb, cc, cd, cf, cg, ch, ci FROM tbl;
```

Alternatively, in Hive, we can use this query.

```sql
hive.support.quoted.identifiers=none;
SELECT `(ce|cj)?+.+` FROM tbl;
```

### Long to Wide (Pivot)

Sometimes, when I want to do a modeling, I need to make sure that in the data, one row means one observation. I usually face transactional data which needs to be aggregated and transformed into one row, one observation. Below is the illustration on transforming long to wide data format, both in `tidyr` and SQL.

```r
tbl <- data.frame(id = rep(1, 3), cat = c("a", "b", "c"), val1 = c(3, 4, 5))
tbl
```

```r
##   id cat val1
## 1  1   a    3
## 2  1   b    4
## 3  1   c    5
```

In `tidyr`, we can use `spread` function.

```r
tbl %>% spread(cat, val1)
```

```r
##   id a b c
## 1  1 3 4 5
```

In SQL, we can use this query.

```sql
SELECT 
	id,
	MAX(CASE WHEN cat = 1 THEN val1 ELSE NULL END) as `a`,
	MAX(CASE WHEN cat = 2 THEN val1 ELSE NULL END) as `b`,
	MAX(CASE WHEN cat = 3 THEN val1 ELSE NULL END) as `c`
FROM 
	tbl
GROUP BY
	id
```

If we have 1000 categories, then we need to state `MAX(CASE WHEN..)` 1000 times. However, in some SQL like MS SQL Server, there is already prebuilt-in function to do it.

### Wide to Long (Unpivot)

In other hand, I need to transform data from wide to long format, usually when I would like to visualize data via `ggplot2`. 

```r
tbl <- data.frame(id = 1, a = 3, b = 5, c = 6)
tbl
```

```r
##   id a b c
## 1  1 3 5 6
```

In `tidyr`, we can use `gather` function.

```r
tbl %>% gather(cat, val1, -id)
```

```r
##   id cat val1
## 1  1   a    3
## 2  1   b    5
## 3  1   c    6
```

In SQL, we can use `UNION` like this.

```sql
SELECT id, 'a' as cat, a as val1 FROM tbl UNION
SELECT id, 'b' as cat, b as val1 FROM tbl UNION
SELECT id, 'c' as cat, c as val1 FROM tbl
```

You can learn more about `tidyr` [here](http://tidyr.tidyverse.org/).

### Take Top n From Each Groups

This case is quite popular in an SQL test. Using USA baby names data from package `babynames`, the purpose is to take three most popular names from each year group.

```r
library(babynames)
head(babynames)
```

```r
## # A tibble: 6 x 5
##    year   sex      name     n       prop
##   <dbl> <chr>     <chr> <int>      <dbl>
## 1  1880     F      Mary  7065 0.07238433
## 2  1880     F      Anna  2604 0.02667923
## 3  1880     F      Emma  2003 0.02052170
## 4  1880     F Elizabeth  1939 0.01986599
## 5  1880     F    Minnie  1746 0.01788861
## 6  1880     F  Margaret  1578 0.01616737
```

We can use the combination of `group_by` and `top_n` function in `dplyr` to get three most popular names from each year group. We also can modified the grouping variable (e.g. using gender or combination of year and gender).

```r
babynames %>%
  group_by(year) %>%
  top_n(3, n) %>%
  arrange(year, desc(n))
```

```r
## # A tibble: 408 x 5
## # Groups:   year [136]
##     year   sex    name     n       prop
##    <dbl> <chr>   <chr> <int>      <dbl>
##  1  1880     M    John  9655 0.08154630
##  2  1880     M William  9531 0.08049899
##  3  1880     F    Mary  7065 0.07238433
##  4  1881     M    John  8769 0.08098299
##  5  1881     M William  8524 0.07872038
##  6  1881     F    Mary  6919 0.06999140
##  7  1882     M    John  9557 0.07831617
##  8  1882     M William  9298 0.07619375
##  9  1882     F    Mary  8148 0.07042594
## 10  1883     M    John  8894 0.07907324
## # ... with 398 more rows
```

In SQL, we can use `ROW_NUMBER() OVER(PARTITION...)` within the subquery to get the rank.

```sql
SELECT year, sex, name, n, prop
FROM
	(SELECT *, ROW_NUMBER() OVER(PARTITION BY year ORDER BY n DESC) AS rank
	FROM babynames) tbl
WHERE rank <= 3
```

---

Learning `dplyr` is not that hard. You can learn basic data manipulation using `dplyr` in just one day. A little practice can level up your data wrangling skills. You also can take a look at `tidyr` which can be very helpful to transform your data from wide to long format, or vice versa. Below is the list of recommended readings in doing further data wrangling.

- [https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf](https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf)
- [Spark and dplyr](https://spark.rstudio.com/dplyr.html)
- [Spreading into multiple columns](https://stackoverflow.com/questions/30592094/r-spreading-multiple-columns-with-tidyr)
- [Why SQL is not for Analysis, but dplyr is](https://blog.exploratory.io/why-sql-is-not-for-analysis-but-dplyr-is-5e180fef6aa7)

Alternatively, you can use `data.table` in R which can perform a little bit faster than `dplyr` while sacrificing the readibility of the code. `dplyr` and `tidyr` are truly game-changing packages in R for data wrangling.