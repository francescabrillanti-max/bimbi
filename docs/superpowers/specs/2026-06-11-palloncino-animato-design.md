# Poster animato — Palloncino

## Obiettivo
Poster animato 1080×1920 in HTML/JS in cui il palloncino (da `balloon-boy_PALLONCINO.svg`) parte da fuori area e arriva nella posizione finale con traiettoria ad arco, usando Anime.js.

## Asset
- `img/balloon-boy_PALLONCINO.svg` — SVG base con viewBox 0 0 1080 1920, sfondo beige, solo il palloncino (gruppo `#PALLONCINO`)

## Architettura
Singolo file `index.html` che:
1. Include il SVG inline (o via `<object>`/`<img>`)
2. Carica Anime.js da CDN
3. Esegue l'animazione al load

## Animazione
- **Wrapper:** `<g id="balloon-wrapper">` attorno a `#PALLONCINO`
- **Stato iniziale:** `translate(0, +1500)` — palloncino sotto l'area visibile
- **Movimento Y:** 1500 → 0 con easing `easeOutCubic` (durata ~2s)
- **Movimento X:** parte da 0, va a ~-180 (sinistra) a metà salita, ritorna a 0 in posizione finale — crea un arco naturale
- **Oscillazione finale:** `rotate` lieve (-3° ↔ 3°) con easing `easeInOutSine`, infinita, dopo l'arrivo

## Traiettoria
Il palloncino parte dal basso-centro, sale curvando a sinistra, poi rientra al centro nella posizione finale (cx=539, cy=592).

## File prodotto
- `index.html` — pagina HTML completa con SVG inline e script Anime.js
