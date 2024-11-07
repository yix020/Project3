<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
  
    let data = [];
    let svg, x, y, colorScale;
    let minAge = 0, maxAge = 100; // Default age range
    let minUsage = 0, maxUsage = 600; // Default app usage range
  
    // Function to filter and update the chart based on the input ranges
    function updateChart() {
      // Filter data based on the selected age and usage ranges
      const filteredData = data.filter(d => d.age >= minAge && d.age <= maxAge && d.appUsage >= minUsage && d.appUsage <= maxUsage);
  
      // Update circles with filtered data
      const circles = svg.selectAll('circle').data(filteredData, d => d.age + "-" + d.appUsage);
  
      circles.enter()
        .append('circle')
        .merge(circles)
        .attr('cx', d => x(d.age))
        .attr('cy', d => y(d.appUsage))
        .attr('r', 5)
        .style('fill', d => colorScale(d.appUsage))
        .style('opacity', 0.7)
        .style('stroke', 'black')
        .on('mouseover', function(event, d) {
          d3.select('#tooltip')
            .style('visibility', 'visible')
            .html(`Age: ${d.age}<br>App Usage: ${d.appUsage} min/day`);
        })
        .on('mousemove', function(event) {
          d3.select('#tooltip')
            .style('top', (event.pageY - 40) + 'px')
            .style('left', (event.pageX + 10) + 'px');
        })
        .on('mouseout', function() {
          d3.select('#tooltip')
            .style('visibility', 'hidden');
        });
  
      circles.exit().remove();
    }
  
    onMount(async () => {
      // Load the data
      data = await d3.csv('https://raw.githubusercontent.com/yix020/Project3/refs/heads/main/static/user_behavior_dataset.csv', d => ({
        age: +d['Age'],
        appUsage: +d['App Usage Time (min/day)']
      }));
  
      // Set the dimensions and margins of the graph
      const margin = { top: 20, right: 30, bottom: 40, left: 50 },
            width = 800 - margin.left - margin.right,
            height = 500 - margin.top - margin.bottom;
  
      // Append the SVG object to the page
      svg = d3.select('#chart')
        .attr('width', width + margin.left + margin.right)
        .attr('height', height + margin.top + margin.bottom)
        .append('g')
        .attr('transform', `translate(${margin.left},${margin.top})`);
  
      // Define scales and color scale
      x = d3.scaleLinear()
        .domain([d3.min(data, d => d.age) - 5, d3.max(data, d => d.age) + 5])
        .range([0, width]);
  
      y = d3.scaleLinear()
        .domain([0, d3.max(data, d => d.appUsage) + 50])
        .range([height, 0]);
  
      colorScale = d3.scaleSequential()
        .domain([d3.min(data, d => d.appUsage), d3.max(data, d => d.appUsage)])
        .interpolator(d3.interpolateBlues);
  
      // Add X axis
      svg.append('g')
        .attr('transform', `translate(0, ${height})`)
        .call(d3.axisBottom(x))
        .append('text')
        .attr('x', width)
        .attr('y', -10)
        .attr('fill', 'black')
        .attr('text-anchor', 'end')
        .text('Age');
  
      // Add Y axis
      svg.append('g')
        .call(d3.axisLeft(y))
        .append('text')
        .attr('x', -10)
        .attr('y', -10)
        .attr('fill', 'black')
        .attr('text-anchor', 'end')
        .text('App Usage Time (min/day)');
  
      // Initial chart rendering
      updateChart();
    });
  </script>
  
  <!-- Title -->
  <div class="container">
    <h2 class="title">
      How does daily app usage time vary across different age groups?
    </h2>
  
    <!-- Range Input Controls -->
    <div class="filter">
      <label>Age Range:</label>
      <input type="number" min="0" max="100" bind:value={minAge} on:input={updateChart} placeholder="Min Age" />
      <input type="number" min="0" max="100" bind:value={maxAge} on:input={updateChart} placeholder="Max Age" />
    </div>
  
    <div class="filter">
      <label>App Usage Time Range (min/day):</label>
      <input type="number" min="0" max="600" bind:value={minUsage} on:input={updateChart} placeholder="Min Usage" />
      <input type="number" min="0" max="600" bind:value={maxUsage} on:input={updateChart} placeholder="Max Usage" />
    </div>
  
    <!-- Chart -->
    <svg id="chart"></svg>
  </div>
  
  <div id="tooltip"></div>
  
  <style>
    /* Center Container */
    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
    }
  
    /* Title Styling */
    .title {
      font-size: 1.2em;
      max-width: 800px;
      margin-bottom: 20px;
    }
  
    /* Filter Controls Styling */
    .filter {
      margin-bottom: 10px;
    }
  
    /* Tooltip Styling */
    #tooltip {
      position: absolute;
      visibility: hidden;
      background-color: rgba(255, 255, 255, 0.9);
      border: 1px solid #aaa;
      border-radius: 5px;
      padding: 8px;
      font-size: 12px;
      color: #333;
      pointer-events: none;
    }
  
    label {
      font-weight: bold;
      margin-right: 10px;
    }
  
    input[type="number"] {
      margin: 5px;
      padding: 5px;
      width: 80px;
      font-size: 1em;
    }
  </style>
  