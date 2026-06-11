# Palloncino 3D Animato Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Sostituire la scena Three.js in `index.html` con un poster animato SVG+Anime.js dove il palloncino 3D sale dal basso con traiettoria ad arco.

**Architecture:** Singolo file `index.html` con SVG inline (preso da `img/balloon-boy3D_PALLONCINO.svg`), Anime.js via CDN. Il `<g id="PALLONCINO">` è wrappato in `<g id="balloon-wrapper">` per animare posizione e rotazione indipendentemente dallo sfondo.

**Tech Stack:** HTML5, SVG, Anime.js (CDN)

---

### Task 1: Sostituire index.html con versione SVG+Anime.js

**Files:**
- Modify: `index.html`
- Remove: nessuno (qr.js non serve più ma lo lasciamo — Three.js non verrà più caricato)

- [ ] **Step 1: Leggere il contenuto completo di `img/balloon-boy3D_PALLONCINO.svg`**

```bash
Get-Content -LiteralPath "img/balloon-boy3D_PALLONCINO.svg" -Raw
```

Estrarre l'SVG completo (41 linee) includendo i tag `<svg>...</svg>` e il contenuto del gruppo `<g id="PALLONCINO">...</g>`. Nota: la riga 36 contiene un `<image>` con dati PNG base64 molto lunghi.

- [ ] **Step 2: Scrivere index.html con SVG inline e Anime.js**

```html
<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Palloncino 3D Animato</title>
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

<!-- SVG inline da balloon-boy3D_PALLONCINO.svg -->
<svg class="poster" viewBox="0 0 1080 1920" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
  <defs>
    <style>
      .cls-1 { isolation: isolate; opacity: .5; stroke: #1a1a1a; stroke-width: 2.63px; }
      .cls-1, .cls-2 { fill: none; stroke-linecap: round; stroke-miterlimit: 3.96; }
      .cls-3 { fill: #c43030; }
      .cls-4 { fill: #f2e6d3; }
      .cls-2 { stroke: #c43030; stroke-width: 3.11px; }
    </style>
  </defs>
  <rect class="cls-4" width="1080" height="1920"/>
  <g id="balloon-wrapper">
    <g id="PALLONCINO">
      <path class="cls-1" d="M539.13,863.75c-3.32,91.44-7.05,182.89-11.2,274.33-3.32,68.58-6.22,137.17-8.71,205.75"/>
      <g>
        <!-- image tag con base64 PNG da balloon-boy3D_PALLONCINO.svg -->
        <image width="486" height="536" transform="translate(296.13 324.46)" xlink:href="data:image/png;base64,..."/>
      </g>
      <ellipse class="cls-3" cx="539.13" cy="857.53" rx="12.44" ry="8.71"/>
      <path class="cls-2" d="M531.66,863.75c-3.32,9.96-1.66,16.59,4.98,19.91,6.64-3.32,9.96-9.96,9.96-19.91"/>
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

Nota: il `<image>` tag con il base64 va copiato esattamente dal file SVG originale (stessi attributi, stesso xlink:href).

- [ ] **Step 3: Aprire index.html nel browser e verificare**

Aprire `index.html` in un browser. Il palloncino deve:
1. Partire da sotto l'area visibile
2. Salire curvando a sinistra
3. Fermarsi nella posizione centrale (cx=539, cy=592)
4. Oscillare leggermente come se galleggiasse

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: sostituito Three.js con SVG+Anime.js, palloncino 3D in arrivo dal basso"
```
