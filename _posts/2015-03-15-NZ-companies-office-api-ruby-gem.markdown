---
layout: post
title:  "NZ Companies Office API - Ruby Gem"
date:   2015-03-15 12:00:00 +1300
categories: ruby
---
The [New Zealand Companies Office](http://www.business.govt.nz/companies) is pretty special - the extent of publicly available information is world class. Particularly when compared with ASIC where users have to pay for search results. Access to full historical directorships, major shareholdings and key data is pretty useful.  

<br />

There are some [technical difficulties](http://lancewiggs.com/2009/11/30/in-praise-of-coys/) with the 'Restful' API  which can make even the simplest functionality or reporting incredibly a little frustrating in a production application! Some significant functionality is not available through the API but can be retrieved with a little bit of HTTP trickery.
<br /><br />


***
<br />

## Packaged in a Gem

<br />
I've just released a Ruby Gem to make any Rails integration a walk in the park. The key features include full authentication (a real lifesaver), search functions and returning ready-to-parse xml.



* Refer to Github:  ***[nzcoapi Gem](https://github.com/t-pearse/nzcoapi)***

>Simple and fast authenticating API wrapper for the New Zealand Companies Office. Full functionality to search by company name, company number and director.

### Usage Example:


{% highlight ruby %}

    # View
	<%= NZcoapi.find_company(973228).css('companyName').text %>

    # Returns
	Trade Me Limited

    # (Magic !)
{% endhighlight %}


*Flick me a message @t_pearse if there are any queries on development using the above gem (or API).*
