---
title: "Week 6: Intro to Observable Plot"
toc: true
---

# Week 6: Intro to Observable Plot

1. Create a plot

Hello world

<div>this is in a div</div>


BASELINE TEMPLATE: 
```js echo
Plot.plot({
  marks: [
    Plot.frame(),
  ]
})
```

```js
const text_to_render = "Hello World!!!";

display(Plot.plot({
  marks: [
    Plot.frame(),
    Plot.text([text_to_render], { frameAnchor: "middle" })
  ]
}))
```

```js echo
const weather = [
  { date: "Mar 7", temperature: 68 },
  { date: "Mar 8", temperature: 49 },
  { date: "Mar 9", temperature: 75 },
  { date: "Mar 10", temperature: 81 },
]
```

```js
Plot.plot({
  title: "Sample Plot",
  subtitle: "something interesting",
  y: {
    domain: [0, 100]
  },
  x: { domain: ["Mar 7", "Mar 8", "Mar 9", "Mar 10"] },
  marks: [
    Plot.frame(),
    Plot.barY(
      weather, 
      {
        x: "date",
        y: "temperature",
        // fill: "temperature"
      }
    ),
    Plot.dot(
      weather, // aapl
      { x: "date", 
      // y: 70, 
      y: "temperature",
      // y: (d) => d.temperature, 
      // y: (d, i) => (i * 10), 
      r: 10,
      fill: "pink"
      }
    ),
    Plot.line(weather, { x: "date", y: "temperature" })
  ]
})
```


2. Add data from a file

```js
const aapl = FileAttachment("stock_data/aapl.csv").csv({ typed: true })
// display(aapl)
```

```js
const filtered = aapl.filter((d,i) => i < 50)
display(filtered)
```

```js
Plot.plot({
  height: 200,
  title: "AAPL Data",
  marks: [
    // Plot.barY(aapl, { x: "Date", y: "Close" }),
    Plot.line(filtered, { x: "Date", y: "Close", stroke: "red" }),
    Plot.line(filtered, { x: "Date", y: "Open", stroke: "green" }),
  ]
})
```


```js
Plot.plot({
  height: 200,
  title: "AAPL Data (candlestick)",
  marks: [
    Plot.line(filtered, { x: "Date", y: "Close", stroke: "red" }),
    Plot.line(filtered, { x: "Date", y: "Open", stroke: "green" }),
    Plot.rect(filtered, {
      x: "Date", // x1 and x2 can both just be date, so can set x as "Date"
      interval: "day", // this ensures the xscale treats it like a single day, UTC date. Without it, we get an error on the chart.
      y1: (d) => Math.min(d.Open, d.Close),
      y2: (d) => Math.max(d.Open, d.Close),
      opacity: 0.5
    })
  ]
})
```


3. Transforms

```js
const data = penguins
```
<!-- ```js
Inputs.table(penguins)
``` -->
<!-- 
```js
Inputs.table(penguins)
```

```js
const counts = [
  { species: "a", count: 100 },
  { species: "b", count: 120 },
  { species: "c", count: 130 },
]
```


```js
Plot.plot({
  marks: [
    // Plot.barY(penguins, { x: (d, i)=> `${d["species"]}_${i}`, y: "culmen_length_mm"})
    // Plot.barY(penguins, 
    //   Plot.groupX(
    //     { y: "count" },
    //     { x: "species", }
    //   )
    // )
    Plot.barY(data, // passing data 
      Plot.groupX(
        { y: "mean" },
        { x: "species", y: "culmen_length_mm"}
      )
    )
  ]
})
``` -->
<!-- 
```js
function countBySpecies(data) {
  const bySpecies = data.reduce((acc, curr) => {
    acc[curr.species] = (acc[curr.species] ?? 0) + 1
    return acc
  }, {})
  return Object.entries(bySpecies).map(([species, counts]) => ({ species, counts }))
}

const countedData = countBySpecies(data)
display(countedData)
```

```js
Plot.plot({
  marks: [
    Plot.barY(countedData, { x: "species", y: "counts" })
  ]
})
``` -->

```js
olympians
```


```js
Plot.plot({
  marks: [
    Plot.rectY(
      olympians, 
      Plot.binX(
        { y: "count" },
        { x: "weight" }
      )
    )
  ]
})
```