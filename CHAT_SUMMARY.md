# Bimbi Project - Palloncino Animato

## Stack
- **SVG inline** (no Three.js) — tre varianti del palloncino come `<g>` annidati
- **Anime.js** 3.2.2 via CDN — animazioni CSS transform su `<g>` SVG
- **Single HTML file** — `index.html` con tutto inline (SVG + CSS + JS)

## File SVG sorgente (in `img/`)
| File | Variante | Contenuto |
|------|----------|-----------|
| `balloon-boy3D_PALLONCINO.svg` | Base | Palloncino 3D renderizzato + corda + fiocco rosso |
| `balloon-boy3D_QRCODE.svg` | QR | Stesso PNG base + overlay QR code mascherato |
| `balloon-boy3D_BIMBO.svg` | Faccina | Stesso PNG base + occhi/bocca/sopracciglia SVG |

Tutti e tre hanno viewBox `0 0 1080 1920`, background `#f2e6d3`, e il balloon PNG è transform `translate(296.13 324.46)` 486×536.

## Architettura HTML
```
<body style="perspective: 800px">
  <svg.poster viewBox="0 0 1080 1920">
    <rect.cls-4/> (background beige)

    <g#balloon-wrapper style="transform-origin: 539px 592px; transform-box: view-box">
      <g#PALLONCINO>    → balloon PNG + corda + fiocco (sempre visibile)
      <g#qrcode-overlay>  → QR code mascherato (iniz. nascosto, clip-path: url(#qrcode-clip))
      <g#bimbo-overlay>   → occhi + bocca (iniz. nascosto, clip-path: url(#clippath)/(#clippath-1))
    </g>
  </svg>
</body>
```

## Sequenza animazione
1. **Salita** — `translateY: [startY, 0]`, easeOutExpo, 3.2s
2. **1° rotazione Y** — `rotateY: [0, 360]`, 1.5s, easeInOutQuad → QR fade-in (ultimi 500ms)
3. **2° rotazione Y** — `rotateY: '+=360'`, 1.5s, easeInOutQuad → QR fade-out + BIMBO fade-in (ultimi 500ms)
4. **Oscillazione** — `translateY: [0, -10, 0]`, easeInOutSine, 2s loop

## 3D Rotation Setup
- `perspective: 800px` sul `<body>` (genitore HTML)
- `transformOrigin: '539px 592px'` nei comandi anime.js
- `transform-origin: 539px 592px; transform-box: view-box` inline sul `<g#balloon-wrapper>`

## ClipPath
| ID | Uso | Shape |
|----|-----|-------|
| `#qrcode-clip` | QR overlay | ellisse (cx:546.6 cy:596.19 rx:242.67 ry:267.56) |
| `#clippath` | Occhio sinistro BIMBO | path |
| `#clippath-1` | Occhio destro BIMBO | path |

## Poster responsive
- `height: 100vh; width: auto; max-width: 100vw`
- startY = `(window.innerHeight / scale) + 300` dove scale = viewportHeight / 1920

## Convenzioni
- Nessun commento nel codice HTML
- Classi SVG cls-1..cls-4 preservate dai file originali
- overlay nascosti con `opacity: 0; display: none`
- Committare solo su richiesta esplicita
