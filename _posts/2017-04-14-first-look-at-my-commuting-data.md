---
layout: post
title: First Look at My Transportation Data
categories:
- blog
---

<style>
img {
    max-width: 100%;
    height: auto;
}
</style>

> This post is related to Data Gue project that I initialize on [rasyidridha.com/datague](http://rasyidridha.com/datague)

Here is my first look at my transportation data. The first idea is that I would like to know what is the distribution of time when I depart from home and arrive to office. The visualization below is made with ggplot2 and hrbrthemes. 

<img src="/images/dep-arr-1.png">

I love that visualization. It is minimal, simple and elegant. Unfortunately, I made a mistake because the plot is not appropriate to use. My data is paired between observation and that's why placing both arrival and departure is not suggested. It can make a confusion since there is an intersection between departure and arrival distribution. The right way is to seperate departure and arrival distribution in different plot.

<img src="/images/dep-arr-2-1.png">

The other option is using X-Y plot. It is more detailed and I can see the relationship between my arrival and departure time. Here it is.

It is interesting to take a look at my transportation data. I can gain many insights from it. Besides the visualization above, I can build a model which can predict duration of my trip. I also can build a network visualization of departure and arrival place.

Once, I finalize the html file, I will place it on [rasyidridha.com/datague/transportation](http://rasyidridha.com/datague/transportation)








