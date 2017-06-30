---
layout: post
title: "Proud to be a Statistician"
categories:
- blog
---

I can be a data scientist, data engineer or anything else but at the core, I am a statistician.

This post is a very inspiring one especially for a statistics graduate like me.

http://flowingdata.com/2017/06/19/data-everywhere-statisticians-anywhere/

It strengthens my belief that I have to pursue statistics degree for my master.

## My R Journey

I have been using R for more than 4 years since 2013, when I was on my first year of college.

```r
library(tidytext)
library(dplyr)
library(ggplot2)
library(wordcloud)

qs <- readLines("qs.txt", warn=FALSE) %>%
  data_frame(txt=.) %>%
  filter(grepl("\\w+", txt)) %>%
  mutate(id=row_number())

sw <- readLines("sw.txt", warn=FALSE) %>%
  data_frame(word=.)

qs_token <- unnest_tokens(qs, word, txt)

qs_grouped <- qs_token %>%
  anti_join(sw) %>%
  count(word, sort=TRUE) %>%
  filter(nchar(word)>=3,
         word!="q.s")

qs_grouped %>%
  top_n(10, n) %>%
  ggplot(aes(reorder(word, n), n)) +
  geom_col(fill="steelblue") + coord_flip() +
  theme_minimal() +
  theme(panel.grid.minor=element_blank()) +
  labs(y="Frequency", x=NULL)

wordcloud(qs_grouped$word, qs_grouped$n, 
          random.order = FALSE,
          random.color = FALSE,
          max.words = 70,
          colors=brewer.pal(8, "Dark2")) ppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppppp

names(x)
```




