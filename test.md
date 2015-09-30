---
layout: page
title: About
permalink: /test/
---

I am testing d3.js


Ok. So this is what we've got so far:

<pre class="codepen" data-height="300" data-type="result" data-href="wakhK" data-user="mshwery" data-safe="true"><code></code><a href="http://codepen.io/mshwery/pen/wakhK">Check out this Pen!</a></pre>
<script async src="http://codepen.io/assets/embed/ei.js"></script>

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