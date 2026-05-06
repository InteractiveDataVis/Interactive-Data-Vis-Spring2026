---
title: "Week 12 Class"
toc: true
---

```js
const height = 400;
const ridership = await FileAttachment("../lab_2/data/ridership.csv").csv({ typed: true })
display(ridership)
```


```js
const timesSq = ridership.filter(d => d.station === "Times Sq-42 St")
display(timesSq)
```

```html
<svg 
  id="my-container" 
  class="svg-stuff" 
  width=${width} 
  height=${height} 
  style="border: solid 1px black">
</svg>
```

```js
const margin = 50

const [yMin, yMax] = d3.extent(timesSq.map(d => d.entrances))
const yScale = d3.scaleLinear()
  .domain([yMin, yMax])
  .range([height - margin, margin])

const [xMin, xMax] = d3.extent(timesSq.map(d => d.date))
const xScale = d3.scaleTime()
  .domain([xMin, xMax])
  .range([margin, width - margin])

const pathFn = d3.line() 
  .x(d => xScale(d.date))
  .y(d => yScale(d.entrances))

const svg = d3.select("#my-container")
  .style("background-color", "pink")

const circles = svg.selectAll("circle.entrances")
  .data(timesSq)
  .join("circle")
  .attr("class", "entrances")
  .attr("cx", d => xScale(d.date))
  .attr("cy", d => yScale(d.entrances))
  .attr("r", 2)

const path = svg.selectAll("path")
  .data([timesSq])
  .join("path")
  .attr("d", d => pathFn(d))
  .attr("stroke", "black")
  .attr("fill", "none")

const xAxis = svg.append("g")
  .attr("transform", `translate(0, ${height - margin})`)
  .call(d3.axisBottom(xScale))

const yAxis = svg.append("g")
  .attr("transform", `translate(${margin}, 0)`)
  .call(d3.axisLeft(yScale))
```
