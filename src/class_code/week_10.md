---
title: "Week 10: Faceting, SVGs, and Maps"
toc: true
---
<!-- 
```js
Inputs.table(penguins)
``` -->

```js
Plot.plot({
  title: "Penguins facets",
  marginLeft: 100,
  marginRight:100,
  y: { grid: true },
  x: { grid: true },
  marks: [
    Plot.frame(),
    Plot.barX(penguins, 
      Plot.groupY(
        { x: "count" },
        { y: "island", tip: true, fill: "sex", fy: "species" }
      )
    )
  ],
})
```

<div id="important">HTML</div>

```js
document.getElementById("important").innerText = "CHANGED"
```

```js
Plot.plot({
  title: "Penguins scatterplot",
  y: { grid: true },
  x: { grid: true },
  height: 400, 
  width: 600,
  marks: [
    Plot.frame(),
    Plot.dot(penguins, {
      x: "culmen_length_mm",
      y: "culmen_depth_mm",
      tip: true,
      fill: "species"
    }
    )
  ],
})
```

```js

// const width = 600 

const xDataDomain = d3.extent(penguins.map(p => p.culmen_length_mm))
display(xDataDomain)

const yDataDomain = d3.extent(penguins.map(p => p.culmen_depth_mm))
display(yDataDomain)

const d3XScale = d3.scaleLinear()
  .domain(xDataDomain)
  .range([0, 600])

// display(d3XScale(33))
// display(d3XScale(40))
// display(d3XScale(58))

const d3YScale = d3.scaleLinear()
  .domain(yDataDomain)
  .range([400, 0])

// display(d3YScale(23))
// display(d3YScale(18))
// display(d3YScale(20))
display(d3YScale(50))
```



```js
const svg = d3.create('svg')
  .attr("width", 600)
  .attr("height", 400)

svg.selectAll()
  .data(penguins)
  .join("rect")
  .attr("x", p => d3XScale(p.culmen_length_mm))
  // .attr("cy", 10)
  .attr("y", p => d3YScale(p.culmen_depth_mm))
  .attr("height", 10)
  .attr("width", 10)
  .attr("fill", penguin => penguin.species === "Adelie" ? "#efb118" : penguin.species === "Chinstrap" ? "#4269d0" : "#ff725c")

view(svg.node())
```


```js
const geo = await FileAttachment('unemployment_data/geo.json').json()
display(geo)

const unemployment = await FileAttachment('unemployment_data/us-county-unemployment.csv')
  .csv({ typed: false })
  .then((data) => data.map(d => ({ ...d, rate: +d.rate})))
display(unemployment)
```

```js
const counties = topojson.feature(geo, geo.objects.counties)
display(counties)

const states = topojson.feature(geo, geo.objects.states)
display(states)

const nation = topojson.feature(geo, geo.objects.nation)
display(nation)
```

```js
const iterationResults = unemployment.map(d => [d.id, d.rate])
display(iterationResults)
const lookup = new Map(iterationResults)
display(lookup)
display(lookup.get("01001"))
// new Map([[0, 1], [2, 3]])
```

```js
Plot.plot({
  projection: "albers-usa",
  color: {
    type: "quantile",
    n: 9,
    scheme: "blues",
  },
  marks: [
    Plot.geo(counties, {
      // fill: "pink",
      stroke: "white",
      tip: true,
      title: county => `${county.properties.name} (${county.properties.id})
        Unemployment: ${lookup.get(county.properties.id)}
      `,
      fill: county => lookup.get(county.properties.id)
    }),
    Plot.geo(states, { stroke: "blue" }),
    Plot.geo(nation, { stroke: "red" }),
  ]
})
```

