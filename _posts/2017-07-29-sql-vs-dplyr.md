---
layout: post
title: "SQL vs dplyr"
categories: blog
tags:
- stats
- bayes
type: post
published: false
---

I have been quite struggling to translate dplyr/tidyr data manipulation to SQL. 

### Long to Wide

Sometimes, when I want to do a modeling, I need to make sure that one row means one observation. I usually face transactional data which needs to be aggregated and transformed. Also, there will be very many scenario and features that I can extract from transactional data. 

### Wide to Long

```r
df %>%
  
```

### Anti Join

Some SQL engine has a standard function to manipulate data. Since, I use Hive, there is a limitation. 

### Hardest Questions ?

### TVLK

The fact is that I have ever failed test as a data analyst in Traveloka. I am not surprised since I am not quite well engaged with SQL. 

It is nice to know both dplyr and SQL. In my first job, I engaged more with SQL to extract the data. I also use sqldf a lot. However, after a few month, I am intrigued to use dplyr for my data manipulation buddy. After using dplyr, I am engaged with tidyr. I never master R intentionally by watching video, tutorial or etc. but I let my hand dirty which I think far better to practice and strengthen your skills.





