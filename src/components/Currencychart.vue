<template>
    <v-col cols="12">
        <svg id="svg"  width="960" height="500" />
    </v-col>
</template>

<script>
import * as d3 from "d3";

export default {
    name: 'Currencychart',
    mounted() {
        this.getCurrencyChart()
    },
    methods: {
        getCurrencyChart () {
            var url = "https://min-api.cryptocompare.com/data/histoday?fsym=BTC&tsym=USD&limit=200&aggregate=3&e=CCCAGG";

            var margin = {top: 20, right: 20, bottom: 30, left: 50},
            width = 960 - margin.left - margin.right,
            height = 500 - margin.top - margin.bottom;

            // set the ranges
            var x = d3.scaleTime().range([0, width]);
            var y = d3.scaleLinear().range([height, 0]);
            // gridlines in x axis function
            function make_x_gridlines() {		
                return d3.axisBottom(x)
                    .ticks(5)
            }

            // gridlines in y axis function
            function make_y_gridlines() {		
                return d3.axisLeft(y)
                    .ticks(5)
            }

            d3.json(url).then(function(d) {

                var data = d.Data.slice(0, 20)
                data.forEach(function(d){ d.time = new Date(d.time * 1000) });

                

                var svg = d3.select("#svg"),
                margin = {top: 20, right: 20, bottom: 30, left: 50},
                width = +svg.attr("width") - margin.left - margin.right,
                height = +svg.attr("height") - margin.top - margin.bottom,


                g = svg.append("g")
                    .attr("transform", "translate(" + margin.left + "," + margin.top + ")")
                var x = d3.scaleTime()
                    .range([0, width])

                var y = d3.scaleLinear()
                    .range([height, 0]);

                var line = d3.line()
                    .x(function(d) { return x(d.time); })
                    .y(function(d) { return y(d.close); });

                x.domain(d3.extent(data, function(d) { return d.time; }));
                y.domain(d3.extent(data, function(d) { return d.close; }));

                svg.append("g")			
                    .attr("class", "grid")
                    .attr("transform", "translate(0," + height + ")")
                    .call(make_x_gridlines()
                        .tickSize(-height)
                        .tickFormat("")
                    )

                // add the Y gridlines
                svg.append("g")			
                    .attr("class", "grid")
                    .call(make_y_gridlines()
                        .tickSize(-width)
                        .tickFormat("")
                    )

                g.append("g")
                    .attr("transform", "translate(0," + height + ")")
                    .call(d3.axisBottom(x))
                    .attr("stroke-width", 2 )
                    .attr("fill", "none")
                    .style("font-size",".8em");

                g.append("g")
                    .call(d3.axisLeft(y))
                    .attr("stroke-width", 2)
                    .style("font-size",".8em")
                .append("text")
                    .attr("fill", "#000")
                    .attr("transform", "rotate(-90)")
                    .attr("y", 20)
                    .attr("text-anchor", "end")
                    .attr("font-size", "1.2em")
                    .text("Price ($)")


                g.append("path")
                    .datum(data)
                    .attr("fill", "none")
                    .attr("stroke", "#228B22")
                    .attr("stroke-linejoin", "round")
                    .attr("stroke-linecap", "round")
                    .attr("stroke-width", 3)
                    .attr("d", line);

            }).catch(error => {
                if (error) throw error;
            });
        }
    }
}
</script>

<style>
.line {
  fill: none;
  stroke: steelblue;
  stroke-width: 2px;
}

.grid line {
  stroke: lightgrey;
  stroke-opacity: 0.7;
  shape-rendering: crispEdges;
}

.grid path {
  stroke-width: 0;
}

body {
    background-color: gray;
}
</style>