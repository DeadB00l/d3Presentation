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
      data: [4, 8, 42, 16, 32, 1]
    }
  },

  mounted() {
    let width = 420;
    let barHeight = 20;

    let x = scaleLinear()
        .domain([0, d3.max(this.data)])
        .range([0, width]);

    let chart = d3.select(".chart")
        .attr("width", width)
        .attr("height", barHeight * this.data.length);

    let bar = chart.selectAll("g")
        .data(this.data)
      .enter().append("g")
        .attr("transform", function(d, i) { return "translate(0," + i * barHeight + ")"; });

    bar.append("rect")
        .attr("width", x)
        .attr("height", barHeight - 1);

    bar.append("text")
        .attr("x", function(d) { return x(d) - 3; })
        .attr("y", barHeight / 2)
        .attr("dy", ".35em")
        .text(function(d) { return d; });
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
