# Primary mortgage market survey - Mutable

[Source](https://github.com/tututwo/learning-obs-framework/blob/main/docs/recreate-dashboard-mutable.md)

<script src="https://cdn.tailwindcss.com"></script>

```js
const pmms = FileAttachment("data/pmms.csv").csv({
  typed: true,
});

const tidy = pmms.then((pmms) =>
  pmms.flatMap(({ date, pmms30, pmms15 }) => [
    {
      date,
      rate: pmms30,
      type: "30Y FRM",
    },
    {
      date,
      rate: pmms15,
      type: "15Y FRM",
    },
  ])
);
```

```js
const color = Plot.scale({
  color: {
    domain: ["30Y FRM", "15Y FRM"],
  },
});
```

```js
function tickLineChart(width, height, data, key, stroke, xRange) {
  const margin = { top: 0, right: 10, bottom: 10, left: 10 };
  const svg = d3
    .create("svg")
    .attr("width", width)
    .attr("height", height)
    .attr("class", "line-chart");
  // Define scales
  const xScale = d3
    .scaleLinear()
    .domain(xRange)
    .range([margin.left, width - margin.right]);

  // Draw ticks
  svg
    .selectAll(".tick")
    .data(pmms.slice(-52))
    .enter()
    .append("line")
    .attr("class", "tick")
    .attr("x1", (d) => xScale(d[key]))
    .attr("x2", (d) => xScale(d[key]))
    .attr("y1", height / 2 - 10)
    .attr("y2", height / 2 + 10)
    .attr("stroke", stroke);

  // Draw the black tick for the latest value
  svg
    .append("line")
    .attr("x1", xScale(pmms[pmms.length - 1][key]))
    .attr("x2", xScale(pmms[pmms.length - 1][key]))
    .attr("y1", height / 2 - 10)
    .attr("y2", height / 2 + 10)
    .attr("stroke", "black")
    .attr("stroke-width", 2);

  return svg.node();
}
```

```js
function frmCard(y, pmms) {
  const key = `pmms${y}`;
  const diff1 = pmms.at(-1)[key] - pmms.at(-2)[key];
  const diffY = pmms.at(-1)[key] - pmms.at(-53)[key];
  const range = d3.extent(pmms.slice(-52), (d) => d[key]);
  const stroke = color.apply(`${y}Y FRM`);

  return html.fragment`
  <div class="h-[30px]">${resize((width, height) => {
    return tickLineChart(width, height, pmms, key, stroke, range);
  })}</div>
  
 `;
}
```

```js
const hoveredState = Mutable(null);
const hoveredStateSetter = (value) => {
  hoveredState.value = value;
};
```

```js
function attachMouseListeners() {
  d3.selectAll(".tick").each(function () {
    d3.select(this).on("pointermove", function (event, d) {
      // Update the Mutable state with new data
      hoveredStateSetter([event.pageX, event.pageY, d]);
    });
  });
}

// Call this function once the elements are sure to be in the DOM
setTimeout(attachMouseListeners, 500); // Adjust timeout as needed based on your actual DOM update timing
```

```js
const tooltipCreator = function () {
  if (hoveredState) {
    const [hoveredX, hoveredY, data] = hoveredState;
    return html`
      <div
        class="absolute t-0 l-0"
        style="transform: translate(${hoveredX}px, ${hoveredY}px);"
      >
        ${data.date || ""}
      </div>
    `;
  } else {
    return html``; // Return empty if there is no hovered state
  }
};
```

<div class="grid grid-cols-3 grid-rows-4 gap-4">
  <div class="card col-start-1 row-start-1 relative">${frmCard(30, pmms)}     ${tooltipCreator()}</div>
  <div class="card col-start-1 row-start-2 relative">
    ${frmCard(15, pmms)} ${tooltipCreator()}
  </div>
  <div class="col-span-2 row-span-2 col-start-2 row-start-1">
    ${resize((width, height) => {})}
  </div>
  <div class="col-span-3 row-span-3 col-start-1 row-start-3">

  </div>
</div>

  <div>This is just a test</div><div>${'This is just a test'}</div>
<div>This is just a test</div>
