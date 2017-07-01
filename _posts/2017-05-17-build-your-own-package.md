---
layout: post
title: Build Your Own R Package to Make Your Life Easier
categories: blog
tags:
- R
- dev
type: post
---

This year is my first time to build my own R package. For the first phase, I build it for myself, to make my work easier. I do not use any comprehensive documentation or any testing yet because I use it for my personal use at my workplace. It includes ggplot2 theme with DIN font, Presto database connection, data reference and Indonesia spatial data. 

It is very helpful especially when I want to retrieve data from database. I do not need to use `dbSendQuery` or `dbGetQuery` anymore which need to pass the connection variable. WIth my own prebuild function, I can query data from Presto using `sf_query` without passing the connection variable because it is already predefined. I also build a function like `sf_glance` using table name as the parameters, which is equivalent to `select * from db.tbl limit 10`. The other features are like ggplot2 theme with DIN font to make my data visualization look elegant and awesome, additional data reference to add more features on my data and Indonesia spatial dataframe to make Indonesia spatial visualization. Building my personal package is very helpful and make my life easier.

I do not know many R users in Indonesia who create their own R packages. For the next phase, I would like to build my R package and publish it to Github. So far, I would like to build packages that retrieve data from API, contain Indonesia spatial data or Indonesia text corpus. These are my R packages to-do-list.

- `rdha`: personal packages
- `jakartapi`: get data from Jakarta Smart City API (or maybe from other sources)
- `indonesia`: get Indonesia spatial data
- `libur`: get Indonesia public holidays (maybe I will combine it with indonesia package)
- `kosakata`: Indonesia corpus in many languages (maybe I will combine it with indonesia package)

For more advanced level, maybe, I can publish my R package on CRAN and build well-documented one on website.

