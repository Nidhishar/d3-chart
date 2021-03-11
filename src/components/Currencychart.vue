<template>
  <v-col cols="12">
    <svg id="svg" width="960" height="500" />
  </v-col>
</template>

<script>
import * as d3 from "d3";

export default {
  name: "Currencychart",
  mounted() {
    this.getCurrencyChart();
  },
  methods: {
    getCurrencyChart() {
      var url =
        "https://min-api.cryptocompare.com/data/histoday?fsym=BTC&tsym=USD&limit=200&aggregate=3&e=CCCAGG";

      var dataset = [
        { x: 100, y: 110 },
        { x: 83, y: 43 },
        { x: 92, y: 28 },
        { x: 49, y: 74 },
        { x: 51, y: 10 },
        { x: 25, y: 98 },
        { x: 77, y: 30 },
        { x: 20, y: 83 },
        { x: 11, y: 63 },
        { x: 4, y: 55 },
        { x: 0, y: 0 },
        { x: 85, y: 100 },
        { x: 60, y: 40 },
        { x: 70, y: 80 },
        { x: 10, y: 20 },
        { x: 40, y: 50 },
        { x: 25, y: 31 },
      ];

      var margin = { top: 0, right: 0, bottom: 30, left: 0 },
        width = 960 - margin.left - margin.right,
        height = 500 - margin.top - margin.bottom;

      // set the ranges
      var x = d3.scaleTime().range([0, width]);
      var y = d3.scaleLinear().range([height, 0]);

      var xScale = d3
        .scaleLinear()
        .domain([
          0,
          d3.max(dataset, function(d) {
            return d.x + 10;
          }),
        ])
        .range([margin.left, width - margin.right]); // Set margins for x specific

      // We're passing in a function in d3.max to tell it what we're maxing (y value)
      var yScale = d3
        .scaleLinear()
        .domain([
          0,
          d3.max(dataset, function(d) {
            return d.y + 10;
          }),
        ])
        .range([margin.top, height - margin.bottom]); // Set margins for y specific

      const yAxisRight = d3.axisRight(y).ticks(5);

      // gridlines in x axis function
      function make_x_gridlines() {
        return d3.axisBottom(x).ticks(5);
      }

      // gridlines in y axis function
      function make_y_gridlines() {
        return yAxisRight;
      }

      function handleMouseOver(d, i) {
        // Add interactivity
        console.log(d, "dddddddddddddddddddddddddiiiiiiiiiiiiiiiiiii");
        // Use D3 to select element, change color and size

        // Specify where to put label of text
        d3.select(this).attr({
          fill: "orange",
          r: 4 * 2,
        });
        d3.select('#svg')
          .append("text")
          .attr({
            id: "t" + d.x + "-" + d.y + "-" + i, // Create an id for text so we can select it later for removing on mouseout
            x: function() {
              return xScale(d.x) - 30;
            },
            y: function() {
              return yScale(d.y) - 15;
            },
          })
          .text(function() {
            return [d.x, d.y]; // Value of the text
          });
      }

      d3.json(url)
        .then(function(d) {
          var data = d.Data.slice(0, 20);
          data.forEach(function(d) {
            d.time = new Date(d.time * 1000);
          });

          var svg = d3.select("#svg"),
            // margin = {top: 20, right: 20, bottom: 30, left: 50},
            width = +svg.attr("width") - margin.left - margin.right,
            height = +svg.attr("height") - margin.top - margin.bottom,
            g = svg.append("g");
          // .attr("transform", "translate(" + margin.left + "," + margin.top + ")")
          var x = d3.scaleTime().range([0, width]);

          var y = d3.scaleLinear().range([height, 0]);

          var line = d3
            .line()
            .x(function(d) {
              return x(d.time);
            })
            .y(function(d) {
              return y(d.close);
            });

          x.domain(
            d3.extent(data, function(d) {
              return d.time;
            })
          );
          y.domain(
            d3.extent(data, function(d) {
              return d.close;
            })
          );

          svg
            .append("g")
            .attr("class", "grid")
            .attr("transform", "translate(0," + height + ")")
            .call(
              make_x_gridlines()
                .tickSize(-height)
                .tickFormat("")
            );

          // add the Y gridlines
          svg
            .append("g")
            .attr("class", "grid")
            .call(
              make_y_gridlines()
                .tickSize(-width)
                .tickFormat("")
            );

          g.append("g")
            .attr("transform", "translate(0," + height + ")")
            .call(d3.axisBottom(x))
            .attr("stroke-width", 2)
            .attr("fill", "none")
            .style("font-size", ".8em");

          g.append("g")
            .call(yAxisRight)
            .attr("stroke-width", 2)
            .style("font-size", ".8em")
            .attr("transform", "translate(" + width + " ,0)");
          // .append("text")
          //     .attr("fill", "#000")
          //     .attr("transform", "rotate(-90)")
          //     .attr("y", 20)
          //     .attr("text-anchor", "end")
          //     .attr("font-size", "1.2em")
          //     .text("Price ($)")

          g.append("path")
            .datum(data)
            .attr("fill", "none")
            .attr("stroke", "#33DF90")
            .attr("stroke-linejoin", "round")
            .attr("stroke-linecap", "round")
            .attr("stroke-width", 3)
            .attr("d", line)
            .on("mouseover", handleMouseOver);
        })
        .catch((error) => {
          if (error) throw error;
        });
    },
  },
};
</script>

<style>
.line {
  fill: none;
  stroke: steelblue;
  stroke-width: 2px;
}

.grid line {
  /* stroke: lightgrey;
  stroke-opacity: 0.7; */
  shape-rendering: crispEdges;
}

.grid path {
  stroke-width: 0;
}

body {
  background-color: gray;
}
#svg {
  background-color: #18222f;
}
</style>
