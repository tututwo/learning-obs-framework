---
toc: false
---

# Barba.js demo(index.html modified)

<script src="https://cdn.jsdelivr.net/npm/@barba/core"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>

<style>

.hero {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: var(--sans-serif);
  margin: 4rem 0 8rem;
  text-wrap: balance;
  text-align: center;
}

.hero h1 {
  margin: 2rem 0;
  max-width: none;
     font-size: 90px;
  font-weight: 900;
  line-height: 1;
  background: linear-gradient(30deg, var(--theme-foreground-focus), currentColor);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

</style>

```js
// document
//   .querySelector("body")
//   .setAttribute("data-barba", "container");
```

`data-barba="wrapper"` is added to the `<div id="observablehq-center">` tag to prevent the initial flash before the transition happens.
<section id='index-barba' >
  <div class="hero" data-barba="container">
  <h1>Hello, Observable Framework</h1>
</div>

</section>

```html echo run=false
<section id="index-barba" >
  <div class="hero" data-barba="container">
    <h1>Hello, Observable Framework</h1>
  </div>
</section>
```

```js echo
const animationEnter = (container) => {
  return gsap.from(container, {
    autoAlpha: 0,
    duration: 4,
    clearProps: "all",
    ease: "none",
  });
};

const animationLeave = (container) => {
  return gsap.to(container, {
    autoAlpha: 0,
    duration: 2,
    clearProps: "all",
    ease: "none",
  });
};
```

```js echo
barba.init({
  transitions: [
    {
      once({ next }) {
        animationEnter(next.container);
      },
      enter({ next }) {
        animationEnter(next.container);
      },
      leave({ next }) {
        animationLeave(next.container);
      },
    },
  ],
});
```
