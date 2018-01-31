---
layout: post
title:  "New Zealand Directorships"
date:   2017-12-29 11:47:20 +1300
categories: ruby
---

New Zealand Companies Office data is complex and I have built several internal tools for my [employer](https://www.linkedin.com/in/thomaspearse/) to present and explore complex datasets to satisfy risk management and support analysis.

A more recent project involved developing a self-service Ruby application to explore n-degrees of director relationships using the NZ Companies Office API ([NZBN Ruby Wrapper]({% post_url 2017-09-01-Updated-NZBN-api-ruby-gem %})).

While completing the project I ran into a number of interesting challenges:
- Query optimisation. The first query builds an array of primary directors then new queries are run for each of these primary directors, determining the secondary directors until all layers of the map are built out. Very quickly there are a large number of queries for an external service to handle.
- Unique directors. The director search query is built on the name of directors (as the NZBN API has no unique key for a director) which results in overly cautious matches. A potential solution is to then filter on director addresses, however this may remove relationships where a director has different addresses for different appointments, or has forgotten to update an address on the NZBN register.

Below is a demo of *Air New Zealand Limited* director relationships as at December 2017. The force diagram is built using D3.js, allowing users to explore and interact with the directors and companies in scope.

<p data-height="604" data-theme-id="0" data-slug-hash="mXyyxv" data-default-tab="result" data-user="tpearse" data-embed-version="2" data-pen-title="AIR.NZ Force Diagram of Directors (500 nodes)" class="codepen">See the Pen <a href="https://codepen.io/tpearse/pen/mXyyxv/">AIR.NZ Force Diagram of Directors (500 nodes)</a> by tpearse (<a href="https://codepen.io/tpearse">@tpearse</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

Credit to [jayniehaka](https://github.com/jayniehaka/NZX50-directorships) for inspiration.
