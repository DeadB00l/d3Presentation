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
        let width = 960,
            height = 500;

        let y = scaleLinear()
            .range([height, 0])
            .domain([0, d3.max(this.data, function(d) { return d.value; })]);

        var chart = d3.select(".chart")
            .attr("width", width)
            .attr("height", height);        

        let barWidth = width / this.data.length;

        let bar = chart.selectAll("g")
            .data(this.data)
        .enter().append("g")
            .attr("transform", function(d, i) { return "translate(" + i * barWidth + ",0)"; });

        bar.append("rect")
            .attr("y", function(d) { return y(d.value); })
            .attr("height", function(d) { return height - y(d.value); })
            .attr("width", barWidth - 1);

        bar.append("text")
            .attr("x", barWidth / 2)
            .attr("y", function(d) { return y(d.value) + 3; })
            .attr("dy", ".75em")
            .text(function(d) { return d.value; });
    }
}
</script>

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
