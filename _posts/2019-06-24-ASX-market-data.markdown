---
layout: post
title:  "ASX market data via API"
date:   2019-06-24 09:00:20 +1300
categories: ruby, investing, programming
description: "How to get reliable ASX market data for free via Alphavantage"
---

Supposedly the Yahoo Finance API used to be the holy grail of finance data, providing reliable and accurate finance data for free. I've been working on a recent project which required ASX market data - specifically daily historical data (volume, open, close etc).

[IEX Cloud](https://iexcloud.io/docs/api/#international-exchanges) has a great API and allows for a fairly high volume of calls. Unfortunately ASX market support has not yet been delivered, however appears to be in the [pipeline](https://portal.productboard.com/iexcloud/1-iex-cloud-roadmap/c/41-australian-stock-exchanges) with an unconfirmed delivery date. 

I was about to purchase historical CSV market data however came across [Alphavantage](https://www.alphavantage.co/) and the expanded 

[Alphavantage](https://www.alphavantage.co/) has ASX (as well as a number of other international exchanges) data available in a great API. Plus there are easy wrappers for your favourite language including [Ruby](https://github.com/StefanoMartin/AlphaVantageRB). I would highly recommend using this free service if you require any historical ASX market data.
