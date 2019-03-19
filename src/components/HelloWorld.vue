<template>
  <svg class="chart" style="border: 1px solid black;" width="420" height="120" />
</template>

<script>
import * as d3 from 'd3';
import {scaleLinear,scaleBand} from "d3-scale";

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

        let x = scaleBand()            
            .rangeRound([0, width], .1)
            .domain(this.data.map(function(d) { return d.name; }));

        let y = scaleLinear()
            .range([height, 0])
            .domain([0, d3.max(this.data, function(d) { return d.value; })]);

        let chart = d3.select(".chart")
            .attr("width", width)
            .attr("height", height);        

        let bar = chart.selectAll("g")
            .data(this.data)
            .enter().append("g")
            .attr("transform", function(d) { return "translate(" + x(d.name) + ",0)"; });

        bar.append("rect")
            .attr("y", function(d) { return y(d.value); })
            .attr("height", function(d) { return height - y(d.value); })
            .attr("width", x.bandwidth() - 1);

        bar.append("text")
            .attr("x", x.bandwidth() / 2)
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
