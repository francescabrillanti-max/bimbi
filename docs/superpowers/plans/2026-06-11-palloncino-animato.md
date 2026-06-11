# Poster animato — Palloncino Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Creare una pagina HTML che mostra il poster 1080×1920 con il palloncino che arriva dal basso con traiettoria ad arco usando Anime.js.

**Architecture:** Singolo file `index.html` con SVG inline, Anime.js via CDN, e uno script che esegue l'animazione al caricamento. Il palloncino è wrappato in un `<g>` separato per poter animare la posizione indipendentemente.

**Tech Stack:** HTML5, SVG, Anime.js (CDN)

---

### Task 1: Creare index.html con animazione

**Files:**
- Create: `index.html`

- [ ] **Step 1: Scrivere index.html con SVG inline e Anime.js**

```html
<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Palloncino Animato</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/3.2.2/anime.min.js"></script>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    background: #1a1a1a;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
  }
  .poster {
    width: 100%;
    max-width: 1080px;
    height: auto;
    display: block;
  }
</style>
</head>
<body>

<svg class="poster" viewBox="0 0 1080 1920" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      .cls-1 { opacity: .5; stroke: #1a1a1a; stroke-width: 2.63px; }
      .cls-1, .cls-2 { isolation: isolate; }
      .cls-1, .cls-3 { fill: none; stroke-linecap: round; stroke-miterlimit: 3.96; }
      .cls-4 { fill: #e24b4a; }
      .cls-5 { fill: #c43030; }
      .cls-2 { fill: #fff; opacity: .12; }
      .cls-6 { fill: #f2e6d3; }
      .cls-3 { stroke: #c43030; stroke-width: 3.11px; }
    </style>
  </defs>
  <rect class="cls-6" width="1080" height="1920"/>
  <g id="balloon-wrapper">
    <g id="PALLONCINO">
      <path class="cls-1" d="M539.13,863.75c-3.32,91.44-7.05,182.89-11.2,274.33-3.32,68.58-6.22,137.17-8.71,205.75"/>
      <g>
        <ellipse class="cls-4" cx="539.13" cy="592.46" rx="242.67" ry="267.56"/>
        <ellipse class="cls-2" cx="476.91" cy="505.34" rx="59.74" ry="44.8"/>
        <ellipse class="cls-5" cx="539.13" cy="857.53" rx="12.44" ry="8.71"/>
        <path class="cls-3" d="M531.66,863.75c-3.32,9.96-1.66,16.59,4.98,19.91,6.64-3.32,9.96-9.96,9.96-19.91"/>
      </g>
    </g>
  </g>
</svg>

<script>
document.addEventListener('DOMContentLoaded', () => {
  const wrapper = document.querySelector('#balloon-wrapper');

  // Posizione iniziale: sotto il poster
  wrapper.setAttribute('transform', 'translate(0, 1500)');

  // Animazione di arrivo: sale dal basso con arco
  anime({
    targets: wrapper,
    translateY: [1500, 0],
    translateX: [0, -180, 0],
    easing: 'easeOutCubic',
    duration: 2200,
    complete: () => {
      // Oscillazione finale (galleggiamento)
      anime({
        targets: wrapper,
        rotate: [-3, 3, -3],
        easing: 'easeInOutSine',
        duration: 2000,
        loop: true
      });
    }
  });
});
</script>
</body>
</html>
```

- [ ] **Step 2: Aprire index.html nel browser e verificare**

Aprire `index.html` in un browser. Il palloncino deve:
1. Partire da sotto l'area visibile
2. Salire curvando a sinistra
3. Fermarsi nella posizione centrale
4. Oscillare leggermente come se galleggiasse

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: poster animato con palloncino in arrivo dal basso"
```
