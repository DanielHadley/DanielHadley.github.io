---
layout: page
title: About
permalink: /test/
---

I am testing d3.js


At work I was tasked with creating an internal tool to track and display vital company stats. I knew that meant I would likely be displaying some data in charts and graphs. Thus began my search for an easily customizable data visualization library.

I perused the roundup of libraries that Google and Stack Overflow offered: [Highcharts.js](http://www.highcharts.com/), [Flot](http://www.flotcharts.org/), [gRaphael](http://g.raphaeljs.com/), [jsCharts](http://www.jscharts.com/), and finally [d3.js](http://d3js.org/) (I hadn't seen [Chart.js](http://www.chartjs.org/) at the time, but it looks pretty nice). I won't go into comparisons on the various libraries, but I will tell you why I chose d3.js:

- easy to use (after the beginner's hump)
- uses svg: scalable & widely supported
- completely customizable
- flexible & extendable
- extremely powerful
- [lots of examples](https://github.com/mbostock/d3/wiki/Gallery)
- [even more examples](http://bl.ocks.org/mbostock)

At the top of this article I've inserted a simple bar chart as an example. You can see [some screenshots](http://dribbble.com/shots/966448-Monitor-fullview?list=users) of actual real-time data visualizations I created for our company monitor. Let's take a look at how D3 makes these animating bar charts:

First, let's drop in a few simple styles:

{% highlight css %}
svg {
  margin: 2em 0;
  font: 10px sans-serif;
}

.foreground {
  fill: #2D6A99;
}

.background {
  fill: #eee;
}
{% endhighlight %}

And of course we need to include the d3.js library:

{% highlight html %}
<script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
{% endhighlight %}

Now, let's get some basic moving parts. We need to setup what a bar is and establish initial variables:

{% highlight javascript %}
var n = 10, // how many bars?
    random = function() { return Math.floor(Math.random() * 100); }, // randomize some numbers for data
    data = d3.range(n).map(random); // an array of randomized datapoints

var barChart = {
  init: function() {
    this.height = 100;
    this.width = 220;
    this.padding = 12;
    this.el = "article header"; // where we'll put our svg 

    // calculate the bar width from the total chart width
    barWidth = Math.floor((this.width - (this.padding * (data.length - 1))) / data.length);
    barHeight = this.height - 20;


    this.svg = d3.select(this.el).insert('svg', ':first-child')
      .attr('width', this.width)
      .attr("height", this.height);

  }
}
{% endhighlight %}

Now, we have to actually render it:

{% highlight javascript %}
var barChart = {

  init: function() { /***/ },

  // draw our svg container
  draw: function() {
    this.meters = this.svg
      .append("g")
        .attr("class", "meter")
        .selectAll("rect")
          .data(data)
          .enter()
          .append('g')
            .attr("class", "bar");

    this.drawBar().attr("class", "background").attr("y", 0).attr("height", barHeight);
    this.drawBar().attr("class", "foreground").attr("y", barHeight).attr("height", 0);
  },

  // this actually draws a bar
  drawBar: function () {
    var self = this;

    return this.meters.append("rect")
      .attr("x", function (d, i) {
        return i * (barWidth + self.padding);
      })
      .attr("width", barWidth);
  }
}

// initialize the bar chart!
barChart.init();
{% endhighlight %}

Ok. So this is what we've got so far:

<pre class="codepen" data-height="300" data-type="result" data-href="wakhK" data-user="mshwery" data-safe="true"><code></code><a href="http://codepen.io/mshwery/pen/wakhK">Check out this Pen!</a></pre>
<script async src="http://codepen.io/assets/embed/ei.js"></script>

Alright, now let's animate this shiz.

{% highlight javascript %}
var barChart = {

  init:     function() { /***/ },
  draw:     function() { /***/ },
  drawBar:  function() { /***/ },

  update: function () {
      var self = this;
      // select the foreground bars and call the animate method on them
      d3.selectAll("rect.foreground").each(self.animate);
  },

  animate: function (d, i) {
    var total = data[i];
    var bar = d3.select(this);
    if (barHeight - total != bar.attr("y")) {
      bar.transition().duration(1500).attr("height", total).attr("y", barHeight - total);
    }
  }
}

// In this example, every couple seconds update the bar chart with new numbers!
setInterval(function() {
  data = d3.range(n).map(random); // get new random numbers
  barChart.update();
}, 2000);
{% endhighlight %}

<figure class="final">
  <figcaption>The final result</figcaption>
</figure>

<a href="http://codepen.io/mshwery/pen/uCBbn" class="codepen" target="_blank">Check out the code in action on CodePen</a>

<style>
  svg {
    font: 10px sans-serif;
  }

  .foreground {
    fill: #2D6A99;
  }

  .background {
    fill: #eee;
  }
</style>
<script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
<script type="text/javascript">

  var n = 10,
      random = function() { return Math.floor(Math.random() * 100); },
      data = d3.range(n).map(random);
  
  var barChart = {
    init: function(el) {
      this.height = 80;
      this.width = 220;
      this.padding = 12;
      barWidth = Math.floor((this.width - (this.padding * (data.length - 1))) / data.length);
      barHeight = this.height;

      this.svg = d3.select(el).insert('svg', ':first-child')
        .attr('width', this.width)
        .attr("height", this.height);

      this.draw();
    },

    draw: function() {
      var self = this;

      this.meters = this.svg
        .append("g")
          .attr("class", "meter")
          .selectAll("rect")
            .data(data)
            .enter()
            .append('g')
              .attr("class", "bar");

      this.drawBar().attr("class", "background").attr("y", 0).attr("height", barHeight);
      this.drawBar().attr("class", "foreground").attr("y", barHeight).attr("height", 0);

      setInterval(function() {
        data = d3.range(n).map(random);
        self.update();
      }, 2000);
    },

    update: function () {
        var self = this;
        d3.selectAll("rect.foreground").each(self.animate);
    },

    animate: function (d, i) {
      var total = data[i];
      var bar = d3.select(this);
      if (barHeight - total != bar.attr("y")) {
        bar.transition().duration(1500).attr("height", total).attr("y", barHeight - total);
      }
    },

    drawBar: function () {
      var self = this;

      return this.meters.append("rect")
        .attr("x", function (d, i) {
          return i * (barWidth + self.padding);
        })
        .attr("width", barWidth);
    }
  }

  barChart.init('figure.final');
</script>