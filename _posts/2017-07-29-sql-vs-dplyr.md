---
layout: post
title: "SQL vs dplyr"
categories: blog
tags:
- R
type: post
published: false
---

Data manipulation is very basic and fundamental in data science. No need to know fancy machine learning techniques, if you understand SQL, you can land your first data job as Data Analyst or Business Intelligence Analyst. 

As a statistics graduate, I never get any knowledge in data manipulation like SQL. Many data that I face when I was in college is  clean. The fact is that in real life or industry, you will never get clean data to be analyzed. You can not rely on your data engineer to make your data pristine or to have form of data that you want. Sometimes, you also need to do a little data manipulation before feed data into the model.

I mostly used SQL in my first job even though I am a statistician who never get any teaching about it at college. Nowadays, I love to use `dplyr` and `tidyr`, library in R, to do data manipulation. I use the combination of both. I use SQL for getting aggregated data via Hive or Presto into my local machine and then I use dplyr/tidyr to do further data manipulation in purpose of data exploration, data visualization and modeling.

These lists are comparison between SQL and dplyr/tidyr verbs. 

* TOC
{:toc}

### Select All Columns Except Column C5 and C10

Supposed, we have table with column c1-c10. We do not want to take column c5 and column c10.

```r
tbl %>% select(-c5, -c10)
```

We can state all column except column c5 and column c10 in SQL.

```sql
SELECT c1, c2, c3, c4, c6, c7, c8, c9 FROM tbl;
```

Alternatively, in Hive, we can use this query.

```sql
hive.support.quoted.identifiers=none;
SELECT `(c5|c10)?+.+` FROM tbl;
```

### Long to Wide

Sometimes, when I want to do a modeling, I need to make sure that in the data, one row means one observation. I usually face transactional data which needs to be aggregated and transformed into one row, one observation. 

```r
tbl %>% spread()
```

### Wide to Long



### Anti Join

```r
tbl %>%
  select(-col_one, -col_two) %>%
  mutate(col_new = col_three * col_five)
x <- "apasdasp"
```

Some SQL engine has a standard function to manipulate data. Since, I use Hive, there is a limitation. 

### Take Top 10 From Each Groups

