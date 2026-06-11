# Chat Riassunto — 11 Giugno 2026

## Obiettivo
Creare poster animato 1080×1920 con palloncino 3D (Three.js r128) che vola dentro, ruota 360° e mostra QR code sulla superficie.

## Punti Chiave

### 1. Bolla rimossa
- Highlight bianco 3D (`hl` mesh, x=86, y=-172·RX/RY, z=164.8) eliminato perché l'utente lo chiamava "bolla"

### 2. Rotazione non visibile → fix
- Cuciture texture troppo sottili (6% opacità) → rese visibili (30% opacità, 6px, canvas 512×512)
- Rotazione ora chiaramente percepibile

### 3. Evoluzione QR → barcode → QR
- Prima: QR code generato programmaticamente (griglia 21×21 + linea oro)
- Utente: "anzichè qr code lascialo bar code" → barcode a barre verticali
- Utente: "barcode non presente" + "non sono visibili nessuna delle due cose"
- Utente: "torna alla versione con il qrcode già creata"
- Utente: "il qrcode deve essere riconoscibile ed al centro, prendi come reference" il SVG

### 4. Approcci falliti per il QR code
- **Mesh PlaneGeometry figlia della sfera**: non visibile (depth test, scala non uniforme del genitore)
- **Canvas texture ridisegnata ogni frame**: non visibile (needsUpdate non funzionante o altro)
- **Barcode su overlay sphere**: non riconoscibile come QR

### 5. Soluzione finale (funzionante)
- **Sfera overlay**: `SphereGeometry(RX * 1.002, ...)` leggermente più grande, stessa scala e posizione della sfera principale
- **Materiale**: `MeshPhongMaterial` con `transparent: true, opacity: 0, depthWrite: false`
- **Texture**: caricata dal QR code base64 del SVG reference (estratto da `img/balloon-boy3D_QRCODE.svg`, 512×211 px)
- **Base64**: 107K chars, salvato in `qr.js` separato
- **Animazione**: fade-in `opacity: 0→1` durante fase reveal (1s, easeInOutSine)
- **Sequenza**: fly-in (3s) → spin 360° (2s) → reveal QR (1s)

### 6. File creati/modificati
- `index.html` — poster 3D (rimossa bolla, overlay sphere, cuciture visibili, fasi fly/spin/reveal)
- `qr.js` — QR code base64 dal SVG reference
- `docs/chat-riassunto-2026-06-11.md` — questo file

### 7. Parametri Three.js
- FOV: 40°, camera distance: `960 / tan(20°)`
- Sfera: `RX=242.67, RY=267.56`, centro a `y=367.54` (SVG y 596.19 convertito)
- Luci: ambient 0.3, key (600,800,1200) 1.0, fill (-400,300,800) 0.4, rim (0,-600,-800) 0.3
- Sfera overlay: raggio `RX * 1.002`, scale.y `RY/RX`, posizione identica
