<template>
  <svg class="chart" style="border:1px solid black" width="420" height="120" />
</template>

<script>
import * as d3 from 'd3';
import {scaleLinear,scaleBand} from "d3-scale";
import { axisBottom,axisLeft } from "d3-axis";
import { easeBounce } from "d3-ease";

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
        let margin = {top: 20, right: 30, bottom: 30, left: 40},
            width = 960 - margin.left - margin.right,
            height = 500 - margin.top - margin.bottom;

        let x = scaleBand()            
            .rangeRound([0, width], .1)
            .domain(this.data.map(function(d) { return d.name; }));

        let xAxis = axisBottom()
            .scale(x);

        let y = scaleLinear()
            .range([height, 0])
            .domain([0, d3.max(this.data, function(d) { return d.value; })]);

        let yAxis = axisLeft()
            .scale(y)
            .ticks(10);

        let chart = d3.select(".chart")
            .attr("width", width + margin.left + margin.right)
            .attr("height", height + margin.top + margin.bottom)
        .append("g")
            .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

        chart.append("g")
            .attr("class", "x axis")
            .attr("transform", "translate(0," + height + ")")
            .call(xAxis);

        chart.append("g")
            .attr("class", "y axis")
            .call(yAxis);

        chart.append("g").selectAll(".bar")
            .data(this.data)
            .enter().append("rect")
            .attr("class", "bar")
            .attr("x", function(d) { return x(d.name); })
            .attr("y", height)
            .attr("height", 0)
            .attr("width", x.bandwidth() - 1);

        chart.selectAll(".bar")
            .transition().duration(1500)
            .attr("y", function(d) { return y(d.value); })
            .attr("height", function(d) { return height - y(d.value); })

        chart.append("g").selectAll("text")
            .data(this.data)
            .enter()
            .append("text")
            .attr("font-famliy", "sans-serif")
            .attr("font-size", "14px")
            .attr("fill", "white")
            .attr("text-anchor", "middle")
            .attr("x", function(d) { return x(d.name) + (x.bandwidth() / 2); })
            .attr("y", function(d) { return y(d.value) + 10; })
            .attr("dy", ".75em")
            .text(function(d) { return d.value; });     
    }
}
</script>

<style>
    .chart .bar text {
        fill: white;
        font: 10px sans-serif;
        text-anchor: end;
        z-index: 10;
    }

    .bar {
        fill: steelblue;
    }

    .axis text {
        font: 10px sans-serif;
    }

    .axis path,
    .axis line {
        fill: none;
        stroke: #000;
        shape-rendering: crispEdges;
    }

    .x.axis path {
        display: none;
    }
</style>
