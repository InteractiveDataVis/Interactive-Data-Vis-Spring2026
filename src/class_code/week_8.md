---
title: "Week 8: Data Types <> Scales <> Marks"
toc: true
---


```js echo
Plot.plot({
  marks: [
    // Plot.dot(olympians, { x: "height", y: "weight", fill: "sex" })
    Plot.rectY(
      olympians, 
      Plot.binX(
        { y: "count" },
        // { x: "weight" }
        { x: { thresholds: 20, value: "weight"}, tip: true }
      )
    )
  ]
})
```

```js echo
Plot.plot({
  marks: [
    // Plot.dot(olympians, { x: "height", y: "weight", fill: "sex" })
    Plot.barY(
      olympians, 
      Plot.groupX(
        { y: "count" },
        { x: "weight", tip: true }
      )
    )
  ]
})
```


```js echo
const letters = ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J"];

display(Plot.plot({
  // height: 200,
  // width: 200,
  x: {
    // type: "band",
    // type: "point",
    domain: letters,
    grid: true,
  },
  marks: [
    // Plot.frame()
    Plot.axisX({anchor: "top", textStroke: "pink"})
  ]
}))
```

```js echo
Plot.plot({
  // height: 200,
  // width: 200,
  x: {
    // type: "band",
    // type: "point",
    // domain: letters,
    domain: [1.6, 2.5],
    // range: [0, 100],
    grid: true,
  },
  marks: [
    // Plot.frame()
    Plot.dot(olympians, 
      { x: "height", y: "weight", fill: "sex" }
    )
  ]
})
```



```js echo
Plot.plot({
  x: {
    domain: ["Fair", "Good", "Very Good", "Ideal", "Premium"]
  },
  color: {
    scheme: "Blues",
    legend: true, 
    label: "Average price",
  },
  marks: [
    Plot.cell(diamonds, 
      Plot.group(
        { fill: "mean" },
        { x: "cut", y: "color", fill: "price", tip: true }
      ))
  ]
})
```
























