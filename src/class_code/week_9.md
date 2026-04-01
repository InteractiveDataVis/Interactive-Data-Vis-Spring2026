---
title: "Week 9: Annotations + Data Joins"
toc: true
---

# Week 9: Annotations + Data Joins

### Adding `z` or `stroke`

```js
const stocks = FileAttachment("./stock_data/stocks.csv").csv({ typed: true })
```

```js
Inputs.table(stocks)
```

```js
Plot.plot({
  marks: [
    Plot.ruleX([stocks[0].Date]),
    Plot.ruleY([0]),
    Plot.line(stocks, { 
      x: "Date", 
      y: "Close", 
      // fill: "blue",
      // stroke: "blue",
      stroke: "Ticker", 
      tip: true,
    })
  ]
})
```

### Data join: lookup position v1

```js
Plot.plot({
  marks: [
    Plot.line(stocks.filter(d => d.Ticker !== "AAPL"), { 
      x: "Date", 
      y: "Close", 
      z: "Ticker",
      stroke: "lightgrey", 
    }),
    Plot.line(stocks.filter(d => d.Ticker === "AAPL"), { 
      x: "Date", 
      y: "Close", 
      stroke: "black", 
    }),
    Plot.ruleX([new Date("11/1/2021")]),
    Plot.dot([new Date("11/1/2021")], {
      x: d => d,
      y: datum => {
        console.log("datum", datum)
        const importantDataPiece = stocks
          .find(obj => {
            console.log("obj", obj.Date.toDateString())
            return obj.Date.toDateString() === datum.toDateString() && obj.Ticker === "AAPL"
            })
        // console.log("WE DID IT", importantDataPiece)
        return importantDataPiece.Close
      },
      fill: "pink",
      r: 10
      // tip: true
    }),
    Plot.tip([
      { 
        Date: new Date("11/1/2021"),
        Note: "Something important"
      }
    ], {
      x: "Date",
      y: 200,
      channels: {
        Date: "Date",
        Note: "Note",
        More: () => "something more"
      }
    })
  ]
})
```

### Data join: lookup position v2

```js
const aapl = await FileAttachment("./stock_data/aapl.csv").csv({ typed: true })
const events = await FileAttachment("./stock_data/stock_events.csv").csv({ typed: true })
display(aapl[0])
display(events)
```

```js
const aaplEvents = events.filter(datum => datum["Related Tickers"].includes("AAPL"))
display(aaplEvents)
```

```js
Plot.plot({
  marks: [
    Plot.line(aapl, { 
      x: "Date", 
      y: "Close", 
    }),
    Plot.dot(aaplEvents, {
      x: "Date",
      y: eventDatum => {
        // console.log(eventDatum)
        const dateToFind = eventDatum.Date.toDateString()
        const foundAaplObj = aapl.find(
          aaplObj => aaplObj.Date.toDateString() === dateToFind
        )
        // console.log("foundAaplObj", foundAaplObj)
        if (foundAaplObj) return foundAaplObj.Close
        else return 0
      },
      tip: true,
      fill: "red",
      channels: {
        "Event": "Event Name",
        "Notes": "Notes"
      }
    }),
  ]
})
```
