# GSAP

<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>

<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/Flip.min.js"></script>

<script src="https://cdn.tailwindcss.com"></script>

<div class="container final h-[1000px]">
  <div class="letter F">F</div>
  <div class="letter l">L</div>
  <div class="letter i">I</div>
  <div class="letter p">P</div>
  <div class="for">for</div>
  <div class="gsap">GSAP</div>
</div>

<style>
  * {
    box-sizing: border-box;
  }

  body {
    padding: 0;
    margin: 0;
    font-family: "Mori";
    font-weight: 300;
  
    overflow: hidden;
  }
  .container {
    display: flex;

    width: 100%;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }
  .container.grid,
  .container.columns {
    align-content: stretch;
    align-items: stretch;
    flex-wrap: wrap;
  }

  .letter {
    text-align: center;
    color: black;
    font-size: 10vmax;
    font-weight: 400;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2px 6px;
  }
  .container.grid .letter {
    flex-basis: 50%;
  }
  .container.columns .letter {
    flex-basis: 25%;
  }
  .for,
  .gsap {
    font-size: 5vmax;
    color: var(--color-surface-white);
  }
  .for {
    padding: 2px 1.6vmax;
    font-weight: 300;
    display: none;
  }
  .gsap {
    padding: 2px 0;
    font-weight: 600;
    display: none;
  }
  .container.final .for,
  .container.final .gsap {
    display: block;
  }
  .F {
    background: var(--color-shockingly-green);
  }
  .l {
    background: var(--color-lt-green);
  }
  .i {
    background: var(--color-blue);
  }
  .p {
    background: var(--color-lilac);
  }
  .container.plain .letter {
    background: transparent;
    color: var(--color-surface-white);
    padding: 0;
  }

  .logo {
    position: fixed;
    width: 100px;
    bottom: 20px;
    right: 30px;
  }
</style>

```js


gsap.registerPlugin(Flip);

let layouts = ["final", "plain", "columns", "grid"],
  container = document.querySelector(".container"),
  curLayout = 0; // index of the current layout

function nextState() {
  const state = Flip.getState(".letter, .for, .gsap", {
    props: "color,backgroundColor",
    simple: true,
  }); // capture current state

  container.classList.remove(layouts[curLayout]); // remove old class
  curLayout = (curLayout + 1) % layouts.length; // increment (loop back to the start if at the end)
  container.classList.add(layouts[curLayout]); // add the new class

  Flip.from(state, {
    // animate from the previous state
    absolute: true,
    stagger: 0.07,
    duration: 0.7,
    ease: "power2.inOut",
    spin: curLayout === 0, // only spin when going to the "final" layout
    simple: true,
    onEnter: (elements, animation) =>
      gsap.fromTo(
        elements,
        { opacity: 0 },
        { opacity: 1, delay: animation.duration() - 0.1 }
      ),
    onLeave: (elements) => gsap.to(elements, { opacity: 0 }),
  });

  gsap.delayedCall(curLayout === 0 ? 3.5 : 1.5, nextState);
}

gsap.delayedCall(1, nextState);
```
