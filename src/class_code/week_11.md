---
title: "Week 11 Class"
toc: true
---

hello world

<style>
  * {
    color: red
  }
  /* #my-container {
    background-color: blue
  } */
</style>

```js
const dataForCircles = [10, 20, 30, 40, 50];
const height = 400;
```

```html
<svg id="my-container" class="svg-stuff" width=${width} height=${height} style="border: solid 1px black"></svg>
```

```js
const svg = d3.select("#my-container");
// svg.style("background-color", "blue");
// svg.append("text").text("hello world").attr("x", 50).attr("y", 50);

svg
  .selectAll(".booger")
  .data(dataForCircles)
  .join("rect")
  .attr("class", "booger")
  .attr("x", (d) => (width / dataForCircles.length) * (d / 10 - 1) + 20)
  .attr("y", (d) => height - d)
  .attr("width", 10)
  .attr("height", (d) => d);
```

```js
Plot.plot({
  marks: [
    Plot.frame(),
    Plot.barY(dataForCircles, {
      x: (d, i) => i,
      y: (d, i) => d,
    }),
  ],
})
```
