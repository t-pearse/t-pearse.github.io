---
layout: post
title:  "Using NLP for smarter web scraping"
date:   2019-09-12 09:00:20 +1300
categories: technology
description: "A few experiences in improving the accuracy of web scraping using Natural Language Processing as a precursor to machine learning"
---

For several projects I've needed to extract specific words from a block of plaintext or extract certain words or types of phrases from different places on a web page. Web scraping works very well if the element is in the same place and same format on every webpage however scraping quickly falls apart when you're trying to do anything more complex.

Below are a few techniques I have picked up from trying to overcome quality issues, without training an entire ML model or relying on high-degree of human input.

#### Example - Block of plaintext

{% highlight html %}

This document notifies you that: On 10 September 2019, an application for putting COMPANY NAME ONE LIMITED into liquidation was filed in the High Court at Christchurch. Its reference number is CIV-2019-409-536. The application is to be heard by the High Court at Christchurch on 14 November 2019 at 10.00am. A person, other than the defendant company, who wants to appear at the hearing of the application, must file an appearance not later than the second working day before that day. The statement of claim and the verifying affidavit may be inspected at the registry of the Court or at the plaintiff’s address for service. The plaintiff is COMPANY NAME TWO Limited, whose address for service is Unit 3, 100 Bush Road, Rosedale, Auckland. The plaintiff’s solicitors are Brent James Norling and Ammar Ayoub, whose address is as noted above. Dated this 1st day of November 2019.

{% endhighlight %}

In this block of text we need to extract the two company names. Regex won't work as the length of the company name is not fixed and could be multiple words. However we can run NLP over it to detect and classify various entities. The two company names will (hopefully) be registered as organizations. From there we can determine the proximity of the substring 'plaintiff' in the body of text to indicate which company is the plaintiff. I have found the [Google Cloud Natural Language](https://cloud.google.com/natural-language/#get-started) product excellent and offers very good value for money.

#### Example - HTML with varying structure

{% highlight html %}

Product page from an ecommerce site

{% endhighlight %}

From an ecommerce page we would like to determine the item price, currency and a description/name of the product.

A potential solution is to scrape the entire web page contents and run NLP over it to classify the various entities. We can extract the price and currency easily from any price entities. If there are multiple price entities, either use some simple logic to resolve (i.e. first or largest price etc). We can then use the salience score, frequency of word occurence to determine the likely description/name of the product.

### Other strategies

##### Text similarity

I have found it useful to use a scoring system (i.e. Jaro Winkler or Levenstein Distance) to calculate the similarity between the scraped output and previous quality validated outputs in order to assess the quality of the scraped output. I have found the gem [String-Similarity](https://github.com/mhutter/string-similarity) and [Fuzzy String Match](https://github.com/kiyoka/fuzzy-string-match) useful starting points.


##### Use Mechanical Turk to refine quality

If there are continual failures in the scraping processes, consider using [Amazon Mechanical Turk](https://www.mturk.com/) as an affordable human-powered quality verification tool. You can use a scoring system to highlight those datapoints which are likely to be inaccurate and flag these for review.


#### Train a ML model

Once you have built up enough data, machine learning could be employed to extract the required elements of a webpage with a high level of accuracy. Companies like [Diffbot](https://www.diffbot.com/) already offer these services and guarantee high quality which may be a better alternative than building your own model.


