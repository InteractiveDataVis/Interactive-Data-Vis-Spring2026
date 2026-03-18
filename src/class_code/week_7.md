---
title: "Week 7: Transforms and Data Manipulation"
toc: true
---

# Week 7: Transforms and Data Manipulation

```js
const aapl = FileAttachment("stock_data/aapl.csv").csv({ typed: true })
```

```js
display(aapl)
```

```js
const filtered = aapl.filter((d,i) => i < 51)
```

```js
Plot.plot({
  marks: [
    Plot.frame(),
    // Plot.line(
    //   filtered,
    //   {
    //     x: "Date",
    //     y: "Open",
    //     stroke: "green"
    //   }
    // ),
    // Plot.line(
    //   filtered,
    //   {
    //     x: "Date",
    //     y: "Close",
    //     stroke: "red"
    //   }
    // ),
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


```js
// penguins
// const somteingElse = "oiejrwrjit"
// view(Inputs.table(penguins))
Inputs.table(penguins)
```

Bar chart of count of penguins per island:
```js
Plot.plot({
  height: 300,
  marks: [
    Plot.frame(),
    Plot.barY(
      penguins,
      Plot.groupX(
        { y: "count" },
        { x: "island" } // options
        // { x: "species" } // options
      )
    )
  ]
})
```

```js
Inputs.table(diamonds)
```

```js
Plot.plot({
  marks: [
    Plot.rectY(
      diamonds, 
      Plot.binX(
        { y: "count" },
        { x: { 
            thresholds: 10, 
            value: "price"
            }, 
          tip: true }
      )
    )
  ]
})
```


```js
Plot.plot({
  y: {
    grid: true
  },
  marks: [
    Plot.ruleY([0]),
    Plot.line(aapl, { x: "Date", y: "Close", stroke: "lightgrey", tip:true }),
    Plot.lineY(aapl, 
      Plot.windowY({
        k: 180
      }, {
        x: "Date", 
        y: "Close",
        // tip: true
      })
    )
  ]
})
```





```js
Plot.plot({
  marks: [
    Plot.dot(penguins, 
      { x: "culmen_length_mm", y: "culmen_depth_mm", tip: true, fill: "species", r: "body_mass_g" }
    )
  ]
})
```


```js
Inputs.table(penguins)
```

```js
// { [island]: count, [island]: count }

const countsByIsland = {
  // "Torgersen": #
};
for (const p of penguins) {
  // display(p)
  // countsByIsland["Torgersen"]
  if (p.island in countsByIsland) {
    countsByIsland[p.island] = countsByIsland[p.island] + 1
  } else {
    countsByIsland[p.island] = 1
  }

  // display(countsByIsland)
}

```


```js
// { [island]: count, [island]: count }

const countsByIsland = {
  // "Torgersen": #
};
penguins.forEach((p) => {
  // display(p)
  // countsByIsland["Torgersen"]
  if (p.island in countsByIsland) {
    countsByIsland[p.island] = countsByIsland[p.island] + 1
  } else {
    countsByIsland[p.island] = 1
  }

  // display(countsByIsland)
}

```

```js
penguins.forEach(p => console.log("haha"))

const newBasket = penguins.map(p => "haha")
console.log(newBasket)
```


```js
Plot.plot({
  title: "hello world", 
  subtitle: "more",
  y: {
    // margin: { left: 10 }
    domain: [100, 300]
  },
  marks: [
    Plot.line(aapl, { 
      x: "Date", 
      y: "Close"
    }),
    // Plot.line(aapl, { 
    //   x: "Date", 
    //   // y: (d) => d["Close"] * 1.123, 
    //   y: (d, i) => d["Close"] + (i * 0.23), 
    //   stroke: "pink" 
    // })
  ]
})
```

<style>
  .my-card {
    color: "blue"
  }

</style>

<div class="my-card" style="font-family: monospace">
${Plot.plot({
  title: "hello world", 
  subtitle: "more",
  marks: [
    Plot.line(aapl, { 
      x: "Date", 
      y: "Close"
    }),
  ]
})}
</div>