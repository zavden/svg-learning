# Challenge 05 — preserveAspectRatio a medida

**Tema:** preserveAspectRatio, meet vs slice, alineación
**Dificultad:** ⭐⭐⭐☆

---

## Tarea

Tienes un logo cuadrado (viewBox 100×100) que debes incrustar en **tres contenedores con proporciones distintas**. Para cada contenedor, elige el `preserveAspectRatio` correcto según el requisito dado.

| Caso | Viewport | Requisito | preserveAspectRatio esperado |
|---|---|---|---|
| A | 300×150 (apaisado) | El logo no debe recortarse, centrado | `xMidYMid meet` |
| B | 300×150 (apaisado) | El logo debe cubrir todo el área, aunque se recorte | `xMidYMid slice` |
| C | 300×150 (apaisado) | El logo debe alinearse al borde derecho sin recortar | `xMaxYMid meet` |
| D | 150×300 (portrait) | El logo no debe recortarse, pegado arriba | `xMidYMin meet` |
| E | 300×150 (apaisado) | El logo debe estirarse para llenar exactamente el viewport | `none` |

**Para cada caso**, crea un SVG con el `preserveAspectRatio` correcto. El logo base está dado.

---

## Logo base (úsalo en los 5 SVGs, ajusta solo viewport y preserveAspectRatio)

```svg
<svg width="200" height="200" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid meet"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f1f5f9;display:block;">

  <!-- Logo: letra "A" estilizada -->
  <circle cx="50" cy="50" r="48" fill="#1e40af"/>
  <polygon points="50,12 78,80 22,80"
           fill="none" stroke="white" stroke-width="5"
           stroke-linejoin="round"/>
  <line x1="32" y1="62" x2="68" y2="62"
        stroke="white" stroke-width="5"/>

  <!-- Marca en esquina inferior -->
  <text x="50" y="94" text-anchor="middle"
        font-size="8" fill="#bfdbfe"
        font-family="sans-serif" letter-spacing="2">
    STUDIO
  </text>
</svg>
```

---

## Pistas

<details>
<summary>Pista — ¿Qué hace cada valor?</summary>

- `meet`: escala para que todo el contenido sea visible (puede dejar franjas vacías)
- `slice`: escala para cubrir todo el viewport (puede recortar el contenido)
- `none`: estira independientemente en X e Y (distorsiona las proporciones)
- `xMinYMid`: alinea a la izquierda (X) y centra verticalmente (Y)
- `xMaxYMid`: alinea a la derecha (X) y centra verticalmente (Y)
- `xMidYMin`: centra horizontalmente (X) y alinea arriba (Y)

</details>

---

## Resultado esperado

**Caso A — xMidYMid meet (centrado, sin recorte):**

```svg-only
<svg width="300" height="150" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid meet"
     xmlns="http://www.w3.org/2000/svg"
     style="border:3px solid #3b82f6;background:#f1f5f9;display:block;">
  <circle cx="50" cy="50" r="48" fill="#1e40af"/>
  <polygon points="50,12 78,80 22,80"
           fill="none" stroke="white" stroke-width="5"
           stroke-linejoin="round"/>
  <line x1="32" y1="62" x2="68" y2="62" stroke="white" stroke-width="5"/>
  <text x="50" y="94" text-anchor="middle" font-size="8" fill="#bfdbfe"
        font-family="sans-serif" letter-spacing="2">STUDIO</text>
</svg>
```

**Caso B — xMidYMid slice (cubre todo, recorta):**

```svg-only
<svg width="300" height="150" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid slice"
     xmlns="http://www.w3.org/2000/svg"
     style="border:3px solid #ef4444;background:#f1f5f9;display:block;">
  <circle cx="50" cy="50" r="48" fill="#1e40af"/>
  <polygon points="50,12 78,80 22,80"
           fill="none" stroke="white" stroke-width="5"
           stroke-linejoin="round"/>
  <line x1="32" y1="62" x2="68" y2="62" stroke="white" stroke-width="5"/>
  <text x="50" y="94" text-anchor="middle" font-size="8" fill="#bfdbfe"
        font-family="sans-serif" letter-spacing="2">STUDIO</text>
</svg>
```

**Caso C — xMaxYMid meet (alineado a la derecha):**

```svg-only
<svg width="300" height="150" viewBox="0 0 100 100"
     preserveAspectRatio="xMaxYMid meet"
     xmlns="http://www.w3.org/2000/svg"
     style="border:3px solid #10b981;background:#f1f5f9;display:block;">
  <circle cx="50" cy="50" r="48" fill="#1e40af"/>
  <polygon points="50,12 78,80 22,80"
           fill="none" stroke="white" stroke-width="5"
           stroke-linejoin="round"/>
  <line x1="32" y1="62" x2="68" y2="62" stroke="white" stroke-width="5"/>
  <text x="50" y="94" text-anchor="middle" font-size="8" fill="#bfdbfe"
        font-family="sans-serif" letter-spacing="2">STUDIO</text>
</svg>
```

**Caso D — xMidYMin meet (pegado arriba, portrait):**

```svg-only
<svg width="150" height="300" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMin meet"
     xmlns="http://www.w3.org/2000/svg"
     style="border:3px solid #a855f7;background:#f1f5f9;display:block;">
  <circle cx="50" cy="50" r="48" fill="#1e40af"/>
  <polygon points="50,12 78,80 22,80"
           fill="none" stroke="white" stroke-width="5"
           stroke-linejoin="round"/>
  <line x1="32" y1="62" x2="68" y2="62" stroke="white" stroke-width="5"/>
  <text x="50" y="94" text-anchor="middle" font-size="8" fill="#bfdbfe"
        font-family="sans-serif" letter-spacing="2">STUDIO</text>
</svg>
```

**Caso E — none (estirado, distorsionado):**

```svg-only
<svg width="300" height="150" viewBox="0 0 100 100"
     preserveAspectRatio="none"
     xmlns="http://www.w3.org/2000/svg"
     style="border:3px solid #f59e0b;background:#f1f5f9;display:block;">
  <circle cx="50" cy="50" r="48" fill="#1e40af"/>
  <polygon points="50,12 78,80 22,80"
           fill="none" stroke="white" stroke-width="5"
           stroke-linejoin="round"/>
  <line x1="32" y1="62" x2="68" y2="62" stroke="white" stroke-width="5"/>
  <text x="50" y="94" text-anchor="middle" font-size="8" fill="#bfdbfe"
        font-family="sans-serif" letter-spacing="2">STUDIO</text>
</svg>
```
