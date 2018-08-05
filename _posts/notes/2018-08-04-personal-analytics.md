---
layout: notes
title: "Building Your Own Personal Analytics from Scratch"
categories: notes
tags:
- personal
type: post
---

Building a personal analytics framework is not that easy. It needs strong commitment and consistency. Started with the easy one, I tracked my spending during college years. I usually dumped the data manually into a sheet. I told my friends about tracking spending data using Monefy easily and after a week, they stopped logging their spending.

Recently, I started to build a better personal analytics framework with more data variety that can answer what, where and whom. Some of my data are manually collected via TapLog. I want to log the data as much as possible with minimum effort. After gathering the data, I will place them all into one place, a database, using PostgreSQL. I use R for ETL, data preprocessing and cleansing (code available [here](https://github.com/rasyidstat/datague-etl)). Finally, I can use the data for visualization and [analysis](../../datague). In the future, I plan to build my life dashboard during my free time.

<!-- ### Framework -->

### What I track

Check [here](../../blog/collect-all-the-data)

* Financial 
	* *Spending* · I track food, snack, clothing, movie and other spendings manually via TapLog. I also track with whom I have eaten with and where.
	* *Banking* · I track banking transaction manually via TapLog and sometimes, get the transactional history from the bank.

* Transportation
	* *Ride-hailing* · I get my Gojek trip history from email and dump it automatically to Google Sheets via IFTTT. I get my Grab trip history from [here](https://hub.grab.com).
	* *Public transporation* · Everytimes I have a trip using TransJakarta or KRL, I log data when I arrive at the station, depart and arrive at the destination. I also build a logic when I have a transit when using TransJakarta.
	* *Others* · Flight and other trip data are inputted manually as well.

* Others (*most of them are not in database yet*)
	* *Text data* · Text data that I gather are my journal writing, Twitter and messaging.
	* *Call logs* · I can get call logs from IFTTT or third-party app inside my phone.
	* *E-commerce* · I track detail of e-commerce transactions and input them manually into Google Sheets.
	* *Sport activity* · I track how many push-up, plank and other sport activity manually via TapLog.
	* *Health* · I just got my health tracker which can track my heart rate, sleep and steps. I still figure out how can I extract them into csv and then dump them into database.
	* *Google data* · Google know much about us, why don't we also understand more about ourself better? I download my Google data [here](https://takeout.google.com/settings/takeout) such as location, search history, YouTube, etc.

### DataGue: compilation of my personal data analysis

* [Lunch Analysis](../../datague/makan-siang)
* [Journaling Text Analysis](../../datague/data-jurnal)
* [Code Statistics](../../datague/main-data-sf)

### Inspiration and Reference

* [Quantified Bob](https://www.quantifiedbob.com/)
* [What I track and how](https://www.reddit.com/r/QuantifiedSelf/comments/7jdhph/what_i_track_and_how/)
* [Give it a month](https://app.mural.co/t/altierlabs0525/m/altierlabs0525/1514539149116/14538a0223470259ce9a441a37bc76bfd5e4d75b)
* [Love, Life and R: Personal Analytics Gets Real](https://www.wired.com/insights/2013/11/love-life-and-r-personal-analytics-gets-real/)
* [Explore Apple Health Data With Python](http://www.markwk.com/data-analysis-for-apple-health.html)

<!-- ## Data Input Convention Guideline

When inputting the data into TapLog, due to the limitation, I need to put my own data input convention so that the ETL scripts can transform the data into clean format. -->




