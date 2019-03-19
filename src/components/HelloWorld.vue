<template>
  <div class="chart" />
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
    let x = scaleLinear()
          .domain([0, d3.max(this.data)])
          .range([0, 420]);
          
    d3.select(".chart")
      .selectAll("div")
        .data(this.data)
      .enter().append("div")
        .style("width", function(d) { return x(d) + "px"; })
        .text(function(d) { return d; });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .chart div {
    font: 10px sans-serif;
    background-color: steelblue;
    text-align: right;
    padding: 3px;
    margin: 1px;
    color: white;
  }
</style>
