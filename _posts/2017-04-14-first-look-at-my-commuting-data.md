---
layout: postdev
title: First Look at My Transportation Data
categories: blog
tags:
- R
- data viz
- quantified self
type: post
img: /images/dep-arr-2-1.png
---

> This post is related to Data Gue project that I initialize on [rasyidridha.com/datague](http://rasyidridha.com/datague)

Here is my first look at my transportation data. The first idea is that I would like to know what is the distribution of time when I depart from home and arrive to office. The visualization below is made with `ggplot2` and `hrbrthemes`. 

<img src="/images/dep-arr-1.png">

I love that visualization. It is minimal, simple and elegant. Unfortunately, I made a mistake because the plot is not appropriate to use. My data is paired between observation and that's why placing both arrival and departure is not suggested. It can make a confusion since there is an intersection between departure and arrival distribution. The right way is to seperate departure and arrival distribution in different plot.

The other option is using X-Y plot. It is more detailed and I can see the relationship between my arrival and departure time. Here it is.

<img src="/images/dep-arr-2-1.png">

It is interesting to take a look at my transportation data. I can gain many insights from it. Besides the visualization above, I can build a model which can predict duration of my trip. I also can build a network visualization of departure and arrival place.

You can get the code for the first plot on this [link](https://gist.github.com/rasyidstat/072d050c360659f710c6f83b90120e9e) and for the second plot [here](https://gist.github.com/rasyidstat/03f31381b4ddf64041398ba7f3372337).

Once, I finalize the html file, I will place it on [rasyidridha.com/datague/transportation](http://rasyidridha.com/datague/transportation)








