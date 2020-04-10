---
layout: post
title:  "Chart.JS with Rails 6 and Webpacker"
date:   2020-03-25 09:00:20 +1300
categories: rails
description: "How to use Chart.js with Rails 6 and Webpacker"
---

Chart.js is a great javascript library to quickly create beautiful SVG charts. 

With Rails 5 it was quick and easy to install a [Chart.js Gem](https://github.com/coderbydesign/chart-js-rails) into the asset pipeline to get started.


Rails 6 introduced [Webpacker](https://github.com/rails/webpacker) as the default Javascript pre-processor and bundler. This is a much tidier and simpler way to manage JS dependencies. Installing Chart.js into your application is now even easier:


Add Chart.js with your favourite package manager (i.e Yarn) in the console.

```
yarn add chart.js
```

Add a require() in your JS pack.

```
// app/javascript/packs/application.js
require("chart.js")
```

All done - you can now use the Chart() function throughout your application and manage Chart.js updates with ease without relying on a gem or manual files.