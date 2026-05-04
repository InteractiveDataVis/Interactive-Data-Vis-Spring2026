# Ridership over time walkthrough

Import all ridership from the lab: 
```js echo
const ridership = await FileAttachment("../lab_2/data/ridership.csv").csv({ typed: true })
display(ridership)
```


## Example 1: Visualizing the total ridership 

```js echo
Plot.plot({
  marks: [
    Plot.line(ridership, {
      x: "date", 
      y: d => d.entrances
    })
  ]
})
```

The problem here is that many stations have data for each date. Although we are coming up with one data point per station per day, we still have many data points due to the many stations. 

We can either show the split by station, or aggregate all stations to show the total trend, or calculate your own data in a separate code block and visualize that. For the sake of focusing on plot, I will just show some in-plot solutions. 

### Aggregate with a transform
To prevent multiple points per day, we will have to aggregate 

```js echo
Plot.plot({
  height: 200,
  marks: [
    Plot.line(ridership, Plot.groupX(
      { y: "sum" },
      { x: "date", y: "entrances" }
    ))
  ]
})
```

### Split by station
Just by adding a `z` dimension, plot will split the data to split each line by each station, so every station gets its own representation. 

```js echo
Plot.plot({
  height: 200,
  marginLeft: 60,
  marks: [
    Plot.line(ridership, 
      { x: "date", y: "entrances", z: "station" }
    )
  ]
})
```

The split by station is interesting, but its a bit noisy. We can simplify this with a couple of directions: 

## Example 2: Filtering to a selection

First, include a selection in a different code block. This will prevent async issues.

```js echo
const allStations = Array.from(new Set(ridership.map(d => d.station)))
const selectedStation = view(Inputs.select(allStations))
```

Now you can access the selected station with the `selectedStation` constant: You've selected <span style="font-family: monospace; font-size: 0.8em; color: purple">${selectedStation}</span>.

Then, you can have a code block referencing the constant that tracks the selector.

```js echo
Plot.plot({
  height: 200,
  marginLeft: 60,
  title: `Ridership for: ${selectedStation}`,
  marks: [
    // show all lines as grey
    Plot.line(ridership, 
      { x: "date", y: "entrances", z: "station", stroke: "lightgrey" }
    ),

    // show selected line as black
    Plot.line(ridership.filter(d => d.station === selectedStation), 
      { x: "date", y: "entrances", z: "station" }
    )
  ]
})
```

Both of these charts are effective ways to show ridership over time, each with their own benefits. 

## Example 3: Visualize a moving average (transform in transform!)

To take it further, you could show all stations, but also include a moving average of ridership. 

```js echo
Plot.plot({
  height: 200,
  marginLeft: 60,
  marks: [
    // first show the grouped ridership
    Plot.line(ridership,
      Plot.groupX(
        { y: "sum" }, 
        { x: "date", y: "entrances",  stroke: "lightgrey"}
      )
    ),
    // next, show the moving average
    Plot.line(ridership,
      Plot.windowY(7, 
        Plot.groupX(
          { y: "sum" }, 
          { x: "date", y: "entrances" }
        )
      )
    )
  ]
})
```