# Challenge 02 — Zoom con viewBox

**Tema:** viewBox, zoom óptico, min-x/min-y
**Dificultad:** ⭐⭐☆☆

---

## Tarea

Tienes un mapa de constelaciones con estrellas distribuidas en un área de 500×500 unidades. Debes crear **tres vistas del mismo contenido** usando únicamente el atributo `viewBox` — sin mover ni duplicar las estrellas.

**Requisitos:**
1. **Vista 1 — completa:** muestra todo el mapa (500×500 unidades)
2. **Vista 2 — zoom ×2:** muestra solo el cuadrante superior izquierdo (250×250 unidades comenzando en 0,0)
3. **Vista 3 — encuadre:** muestra solo la región donde se concentran las estrellas (centra en x=200, y=150, región de 200×200)

Los tres SVGs deben tener el mismo `width` y `height` en pantalla (250×250 px). Solo cambia el `viewBox`.

---

## Código base (mapa de constelaciones — úsalo en los 3 SVGs)

```svg
<svg width="250" height="250"
     viewBox="0 0 500 500"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #1e293b;background:#0f172a;display:block;">

  <!-- Estrellas dispersas -->
  <circle cx="60"  cy="80"  r="3" fill="white" opacity="0.9"/>
  <circle cx="120" cy="40"  r="5" fill="#fde68a" opacity="1"/>
  <circle cx="200" cy="120" r="4" fill="white" opacity="0.8"/>
  <circle cx="180" cy="80"  r="2" fill="white" opacity="0.7"/>
  <circle cx="250" cy="60"  r="3" fill="#fde68a" opacity="0.9"/>
  <circle cx="160" cy="200" r="6" fill="#93c5fd" opacity="1"/>
  <circle cx="220" cy="180" r="3" fill="white" opacity="0.8"/>
  <circle cx="300" cy="300" r="4" fill="white" opacity="0.7"/>
  <circle cx="380" cy="150" r="3" fill="white" opacity="0.6"/>
  <circle cx="420" cy="400" r="5" fill="#fde68a" opacity="0.9"/>
  <circle cx="80"  cy="350" r="2" fill="white" opacity="0.6"/>
  <circle cx="450" cy="80"  r="3" fill="white" opacity="0.7"/>

  <!-- Líneas de constelación (zona principal: x=150-270, y=60-220) -->
  <line x1="120" y1="40"  x2="180" y2="80"  stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="180" y1="80"  x2="200" y2="120" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="200" y1="120" x2="160" y2="200" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="200" y1="120" x2="220" y2="180" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="180" y1="80"  x2="250" y2="60"  stroke="white" stroke-width="0.8" opacity="0.4"/>

  <!-- Etiqueta de coordenada de referencia -->
  <text x="10" y="490" font-size="12" fill="#475569" font-family="monospace">
    viewBox="0 0 500 500"
  </text>
</svg>
```

---

## Pistas

<details>
<summary>Pista — Vista 2 (zoom ×2)</summary>

Para ver la mitad superior izquierda (250×250 unidades) del espacio de 500×500, el viewBox es `"0 0 250 250"`. Como el viewport en pantalla sigue siendo 250×250 px, todo se ve el doble de grande.

</details>

<details>
<summary>Pista — Vista 3 (encuadre personalizado)</summary>

Quieres ver desde x=100 hasta x=300 y desde y=0 hasta y=200 (ancho=200, alto=200, centrado en 200,100). El viewBox sería `"100 0 200 200"`.

</details>

---

## Resultado esperado

**Vista 1 — Completa:**

```svg-only
<svg width="250" height="250" viewBox="0 0 500 500"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #1e293b;background:#0f172a;display:block;">
  <circle cx="60"  cy="80"  r="3" fill="white" opacity="0.9"/>
  <circle cx="120" cy="40"  r="5" fill="#fde68a" opacity="1"/>
  <circle cx="200" cy="120" r="4" fill="white" opacity="0.8"/>
  <circle cx="180" cy="80"  r="2" fill="white" opacity="0.7"/>
  <circle cx="250" cy="60"  r="3" fill="#fde68a" opacity="0.9"/>
  <circle cx="160" cy="200" r="6" fill="#93c5fd" opacity="1"/>
  <circle cx="220" cy="180" r="3" fill="white" opacity="0.8"/>
  <circle cx="300" cy="300" r="4" fill="white" opacity="0.7"/>
  <circle cx="380" cy="150" r="3" fill="white" opacity="0.6"/>
  <circle cx="420" cy="400" r="5" fill="#fde68a" opacity="0.9"/>
  <circle cx="80"  cy="350" r="2" fill="white" opacity="0.6"/>
  <circle cx="450" cy="80"  r="3" fill="white" opacity="0.7"/>
  <line x1="120" y1="40"  x2="180" y2="80"  stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="180" y1="80"  x2="200" y2="120" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="200" y1="120" x2="160" y2="200" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="200" y1="120" x2="220" y2="180" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="180" y1="80"  x2="250" y2="60"  stroke="white" stroke-width="0.8" opacity="0.4"/>
  <text x="10" y="490" font-size="12" fill="#475569" font-family="monospace">viewBox="0 0 500 500" — vista completa</text>
</svg>
```

**Vista 2 — Zoom ×2 (cuadrante superior izquierdo):**

```svg-only
<svg width="250" height="250" viewBox="0 0 250 250"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #1e293b;background:#0f172a;display:block;">
  <circle cx="60"  cy="80"  r="3" fill="white" opacity="0.9"/>
  <circle cx="120" cy="40"  r="5" fill="#fde68a" opacity="1"/>
  <circle cx="200" cy="120" r="4" fill="white" opacity="0.8"/>
  <circle cx="180" cy="80"  r="2" fill="white" opacity="0.7"/>
  <circle cx="250" cy="60"  r="3" fill="#fde68a" opacity="0.9"/>
  <circle cx="160" cy="200" r="6" fill="#93c5fd" opacity="1"/>
  <circle cx="220" cy="180" r="3" fill="white" opacity="0.8"/>
  <line x1="120" y1="40"  x2="180" y2="80"  stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="180" y1="80"  x2="200" y2="120" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="200" y1="120" x2="160" y2="200" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="200" y1="120" x2="220" y2="180" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="180" y1="80"  x2="250" y2="60"  stroke="white" stroke-width="0.8" opacity="0.4"/>
  <text x="10" y="240" font-size="6" fill="#475569" font-family="monospace">viewBox="0 0 250 250" — zoom ×2</text>
</svg>
```

**Vista 3 — Encuadre en la constelación:**

```svg-only
<svg width="250" height="250" viewBox="100 0 200 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #1e293b;background:#0f172a;display:block;">
  <circle cx="60"  cy="80"  r="3" fill="white" opacity="0.9"/>
  <circle cx="120" cy="40"  r="5" fill="#fde68a" opacity="1"/>
  <circle cx="200" cy="120" r="4" fill="white" opacity="0.8"/>
  <circle cx="180" cy="80"  r="2" fill="white" opacity="0.7"/>
  <circle cx="250" cy="60"  r="3" fill="#fde68a" opacity="0.9"/>
  <circle cx="160" cy="200" r="6" fill="#93c5fd" opacity="1"/>
  <circle cx="220" cy="180" r="3" fill="white" opacity="0.8"/>
  <line x1="120" y1="40"  x2="180" y2="80"  stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="180" y1="80"  x2="200" y2="120" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="200" y1="120" x2="160" y2="200" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="200" y1="120" x2="220" y2="180" stroke="white" stroke-width="0.8" opacity="0.4"/>
  <line x1="180" y1="80"  x2="250" y2="60"  stroke="white" stroke-width="0.8" opacity="0.4"/>
  <text x="105" y="195" font-size="6" fill="#475569" font-family="monospace">viewBox="100 0 200 200" — encuadre</text>
</svg>
```
