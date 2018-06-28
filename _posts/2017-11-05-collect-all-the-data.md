---
layout: post
title: "Collect All the Data!"
categories: blog
tags:
- quantified self
type: post
---

*Updated at July 2018*

I have been running my personal quantified self project for almost one year. My first focus is to gather all possible data. After that, I would like to integrate them all in a single database. Then, I can focus on important metrics, build an alarm, build a screenshot of my life data and have a deeper analysis. I also have a plan to build my dashboard of life to monitor the metrics. Therefore, I am building my own personal data pipeline from A to Z: data preparation, data integration, data visualization and data analysis.

What are the data that I collect?

- Financial
  - Spending
  - Banking transactions (withdrawal, transfer, top-up)

- Transportation
  - Gojek
  - Transjakarta
  - KRL

- Text
  - Twitter
  - Line
  - Telegram
  - Life Architecture Writing

- Others
  - Call logs
  - Lunch
  - E-commerce
  - Flight history
  - Movies
  - Books
  - Google search
  - Attendance marks

 I wonder how can I get other data like e-mail and mobile phone usage/activity. 

## Data Integration

I should integrate all the data in a single database and create a robust data ingestion scripts which will be executed periodically. The scripts will feed the data into a single database with a proper structure. The database does not only contain my quantified self data but it also contains other data which are unrelated with my data. 

The tools that I use for data integration are:

- Cloud for storage (Dropbox, Onedrive, Google Drive)
- Database (Google Sheets, PostgreSQL)
- Data staging and source (Google Sheets, Excel)
- IFTTT (automation)
- Scripting and executor (R, Python)
- Scheduler (Cron, Windows Task Management)

Where are the data sitting?

- Google Sheets: GO-JEK Receipt (raw), phone log (outgoing calls), phone call log (incoming calls)
- TapLog: transportation (Transjakarta, KRL), banking transactions
- Excel: spending, lunch, e-commerce, flight history, movies, books

## Reference and Inspiration

- [http://giveitamonth.com/e1/](http://giveitamonth.com/e1/)
- [Give it a Month](https://app.mural.co/t/altierlabs0525/m/altierlabs0525/1514539149116/14538a0223470259ce9a441a37bc76bfd5e4d75b)
- [Overthinking Everything](https://www.youtube.com/watch?v=DCAWNyZ_PgQ)






