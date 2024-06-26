# Learn View

<script src="https://cdn.tailwindcss.com"></script>

```js
const team = view(
  Inputs.radio(["Metropolis Meteors", "Rockford Peaches", "Bears"], {
    label: "Favorite team:",
    value: "Metropolis Meteors",
  })
);
```

```js
const viewElement = view(
  (() => {
    const element = html`<div class="w-[350px] h-[350px] bg-red-300"></div>`;
    element.value = "Hello World";
    const selectiond3 = d3.select(element);
    selectiond3.on("mousemove", (event) => {
      element.value = [event.offsetX, event.offsetY].join(", ");
      //   console.log(element.value);
      element.dispatchEvent(new CustomEvent("input"));
    });

    return element;
  })()
);
```

<h1>${viewElement}</h1>
<div id="container"></div>

## custom event on regular html + Generator

```js
const customElement = () => {
  const element = html`<div class="w-[350px] h-[350px] bg-blue-300"></div>`;
  element.value = "Hello World";
  const selectiond3 = d3.select(element);
  selectiond3.on("mousemove", (event) => {
    element.value = [event.offsetX, event.offsetY].join(", ");
      console.log(element.value);
    element.dispatchEvent(new CustomEvent("input"));
  });

  return element;
};
const customElementValue = Generators.input(display(customElement()));
```

${display(customElement())}
${customElementValue}

## Learn Generators

```js
const htmlInput = Inputs.radio(
  ["Metropolis Meteors", "Rockford Peaches", "Bears"],
  {
    label: "Favorite team:",
    value: "Metropolis Meteors",
  }
);
const generatorInputs = Generators.input(htmlInput);
```

```js
console.log(generatorInputs);
```

${htmlInput}
${generatorInputs}
