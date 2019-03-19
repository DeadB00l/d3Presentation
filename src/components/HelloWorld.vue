<template>
  <svg class="chart" width="420" height="120" />
</template>

<script>
import * as d3 from 'd3';
import {scaleLinear} from "d3-scale";

export default {
  name: 'HelloWorld',
  data() {
    return {
      data: [
        {name: "Locke",    value:  4},
        {name: "Reyes",    value:  8},
        {name: "Ford",     value: 42},
        {name: "Jarrah",   value: 16},
        {name: "Shephard", value: 32},
        {name: "Kwon",     value: 23}
      ]
    }
  },

  mounted() {
    let width = 420;
    let barHeight = 20;

    let x = scaleLinear()
        .domain([0, d3.max(this.data, function(d) { return d.value; })])
        .range([0, width]);

    let chart = d3.select(".chart")
      .attr("width", width)
      .attr("height", barHeight * this.data.length);

    var bar = chart.selectAll("g")
        .data(this.data)
      .enter().append("g")
        .attr("transform", function(d, i) { return "translate(0," + i * barHeight + ")"; });

    bar.append("rect")
        .attr("width", function(d) { return x(d.value); })
        .attr("height", barHeight - 1);

    bar.append("text")
        .attr("x", function(d) { return x(d.value) - 3; })
        .attr("y", barHeight / 2)
        .attr("dy", ".35em")
        .text(function(d) { return d.value; });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .chart rect {
    fill: steelblue;
  }

  .chart text {
    fill: white;
    font: 10px sans-serif;
    text-anchor: end;
  }
</style>
