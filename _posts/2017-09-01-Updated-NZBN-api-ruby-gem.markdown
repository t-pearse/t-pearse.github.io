---
layout: post
title:  "Updated NZBN (Companies Office) API Wrapper"
date:   2017-09-01 12:00:00 +1300
categories: ruby
---
A few years ago I published the [nzcoapi gem](https://github.com/t-pearse/nzcoapi) which was primarily a wrapper for 'difficult to navigate' SOAP API provided by the [New Zealand Companies Office](http://www.business.govt.nz/companies).

The API has been grandfathered and a flash new [NZBN]() API is available which is a lot cleaner and easy to use. I had to update a number of applications and have now released an updated Ruby Gem which makes interacting with the new NZBN API a breeze.


* Refer to Github:  ***[nzbn Gem](https://github.com/t-pearse/nzbn)***

>Simple and fast authenticating API wrapper for the New Zealand Companies Office. Full functionality to search by company name, company number and director.


## Installation

Add this line to your application's Gemfile:

{% highlight ruby %}
    gem 'nzbn'
{% endhighlight %}


And then execute:

    $ bundle

Or install it yourself as:

    $ gem install nzbn

You will also need to set the following ENV variables in order to access the API. These can be found on the business.govt.nz website under your login.

    $ NZBN_ID=

    $ NZBN_SECRET=


## Usage

Two functions have been exposed, as per below.

### Company details by NZBN
{% highlight ruby %}
	NZBN.entity(nzbn)
  {% endhighlight %}

Full company details based on a unique 'Company Number' (nzbn) will be returned.

### Search for a Company
{% highlight ruby %}
	NZBN.entities(search_term, entity_status, page_length)
{% endhighlight %}

All output is returned as parsed JSON. Within a rails view it can be quickly navigated with [Nokogiri](https://github.com/sparklemotion/nokogiri) or similar.
