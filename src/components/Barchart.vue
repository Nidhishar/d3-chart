<template>
    <v-col cols="12" class="">
        <h2>Bar Chart I</h2>
        <div class="container-chart text-align-left">
        <div class="chart mx-auto"></div>
        </div>
    </v-col>
</template>

<script>
    import * as d3 from "d3";

    export default {
        name: 'BarChart',
        mounted() {
            this.generateArc();
        },
        methods: {
            generateArc() {
                let data = [20, 8, 15, 16, 23, 42]
                d3.select('.chart')
                .selectAll('div')
                .data(data)
                .enter().append('div')
                .style('width', function (d) { return d * 10 + 'px' })
                .text(function (d) { return d })
            }
        }
    }
</script>

<style>

</style>