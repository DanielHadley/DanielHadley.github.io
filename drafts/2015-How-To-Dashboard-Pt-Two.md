---
layout: post
title: How to Build a Data Dashboard using R (knitr), HTML, and Javascript (Highcharts, Leaflet)
visible: 1
---

R has been the perfect language for the back end of [this](http://www.somervillema.gov/dashboard/daily.html) government data dashboard I am developing. It has excellent packages to pipe in data from every significant source; and it is ideal for automating analysis, whether that means calculating an average of several data points, or creating a complex algorithm for finding which variables to display based on standardized week-over-week changes.  

The reason I am writing this tutorial is to outline an undocumented method in the "knitr" package that I want to share because it opens up dozens of new tech stack combinations. The basic idea is to perform all of the data manipulations and analysis in R, and then "weave" it into popular Javascript visualization tools, like Highcharts and Leaflet. 

Here is an example in it's simplest form. First, I will show some analysis in R, and then I will show how that fits into an .Rhtml file, which can be "woven" into HTML/Javascript and finally uploaded to your web server.  

{% highlight R %}
library(knitr)

# Make up some data for a timeseries chart
dates <- c("1/1/2013", "1/1/2014", "1/1/2015", "1/1/2016")
values <- c(11, 13, 15, 12)

# Calculate an average
Mean_of_Values <- mean(values)

## Now knit together the data and HTML in highcharts and save it to the server ##
knit("./daily.Rhtml", output = "./MyWebServer/daily.html")

{% endhighlight %}

Now this is what your daily.Rhtml file will look like, in it's most basic form

{% highlight HTML %}

<!DOCTYPE html>
<html>
<head>
<title>Page Title: Daily Dashboard</title>

<!-- Highcharts scripts -->
<script src="https://code.highcharts.com/highcharts.js"></script>

</head>
<body>

<h1>This is a Heading</h1>
<p>This is a paragraph.</p>

</body>
</html>

{% endhighlight %}
