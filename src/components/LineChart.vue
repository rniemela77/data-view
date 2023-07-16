<template>
    <div class="line-chart">
      <svg :width="width" :height="height">
        <g :transform="`translate(${margin.left}, ${margin.top})`">
          <path :d="lineData" fill="none" stroke="steelblue" stroke-width="2" />
          <g v-for="(d, i) in data" :key="i" class="data-point">
            <circle :cx="xScale(i)" :cy="yScale(d)" r="5" fill="steelblue" />
            <text :x="xScale(i)" :y="yScale(d) - 10" fill="steelblue" font-size="12" text-anchor="middle">{{ d }}</text>
          </g>
          <g class="axis">
            <g class="x-axis" transform="translate(0, {{ height - margin.bottom }})">
              <line :x1="margin.left" :x2="width - margin.right" y1="0" y2="0" stroke="black" />
              <text :x="width - margin.right" y="20" fill="black" text-anchor="end">{{ xLabel }}</text>
            </g>
            <g class="y-axis" transform="translate({{ margin.left }}, 0)">
              <line y1="0" y2="0" :x1="-margin.left" :x2="-margin.left" stroke="black" />
              <text x="-10" y="-10" fill="black" text-anchor="end">{{ yLabel }}</text>
            </g>
          </g>
        </g>
      </svg>
    </div>
  </template>
  
  <script>
  import * as d3 from 'd3';
  
  export default {
    name: 'LineChart',
    props: {
      data: {
        type: Array,
        required: true,
      },
      width: {
        type: Number,
        default: 600,
      },
      height: {
        type: Number,
        default: 400,
      },
      margin: {
        type: Object,
        default: () => ({ top: 20, right: 20, bottom: 30, left: 50 }),
      },
      xLabel: {
        type: String,
        default: 'X-axis',
      },
      yLabel: {
        type: String,
        default: 'Y-axis',
      },
    },
    computed: {
      xScale() {
        const { data, width, margin } = this;
  
        return d3
          .scaleLinear()
          .domain([0, data.length - 1])
          .range([margin.left, width - margin.right]);
      },
      yScale() {
        const { data, height, margin } = this;
  
        return d3
          .scaleLinear()
          .domain([0, d3.max(data)])
          .range([height - margin.bottom, margin.top]);
      },
      lineData() {
        const { data, xScale, yScale } = this;
  
        const line = d3
          .line()
          .x((d, i) => xScale(i))
          .y(d => yScale(d));
  
        return line(data);
      },
    },
  };
  </script>
  
  <style scoped>
  .line-chart {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .data-point text {
    pointer-events: none;
  }
  
  .axis text {
    font-size: 12px;
  }
  
  .axis line,
  .axis path {
    fill: none;
    stroke: #ccc;
    shape-rendering: crispEdges;
  }
  
  .axis .domain {
    stroke-width: 0;
  }
  
  .axis .x-axis line {
    stroke: #ccc;
  }
  
  .data-point circle:hover {
    r: 8;
  }
  </style>
  