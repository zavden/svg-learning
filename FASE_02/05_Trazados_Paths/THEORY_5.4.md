# 5.4 Comandos de línea: `L`, `H`, `V`

Los tres comandos de línea trazan segmentos rectos desde la posición actual. `H` y `V` son atajos para líneas horizontales y verticales.

---

## `L x y` — Line To

Traza una línea hasta el punto indicado. La posición actual pasa a ser el punto final.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- L absoluto -->
  <path d="M 20 100 L 150 30 L 280 100"
        stroke="#3b82f6" stroke-width="2.5" fill="none" stroke-linejoin="round" stroke-linecap="round"/>

  <circle cx="20"  cy="100" r="5" fill="#ef4444"/>
  <circle cx="150" cy="30"  r="5" fill="#3b82f6"/>
  <circle cx="280" cy="100" r="5" fill="#3b82f6"/>

  <text x="20"  y="115" text-anchor="middle" font-size="7.5" fill="#ef4444">M 20 100</text>
  <text x="150" y="18"  text-anchor="middle" font-size="7.5" fill="#3b82f6">L 150 30</text>
  <text x="280" y="115" text-anchor="middle" font-size="7.5" fill="#3b82f6">L 280 100</text>

  <text x="150" y="126" text-anchor="middle" font-size="7" fill="#94a3b8">
    L siempre va a la coordenada absoluta indicada
  </text>
</svg>
```

---

## `H x` — Horizontal Line To

Solo necesita la coordenada X. La coordenada Y no cambia.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- H: línea horizontal (Y fija) -->
  <path d="M 20 50 H 280"
        stroke="#10b981" stroke-width="3" fill="none" stroke-linecap="round"/>

  <circle cx="20"  cy="50" r="5" fill="#ef4444"/>
  <circle cx="280" cy="50" r="5" fill="#10b981"/>

  <!-- Guía de Y constante -->
  <line x1="0" y1="50" x2="300" y2="50" stroke="#dcfce7" stroke-width="1" stroke-dasharray="3,3"/>

  <text x="20"  y="38" text-anchor="middle" font-size="8" fill="#ef4444">M 20 50</text>
  <text x="280" y="38" text-anchor="middle" font-size="8" fill="#10b981">H 280</text>
  <text x="150" y="72" text-anchor="middle" font-size="8" fill="#64748b">y=50 no cambia</text>

  <!-- h relativo: desplazamiento -->
  <path d="M 20 85 h 260"
        stroke="#3b82f6" stroke-width="2" fill="none" stroke-linecap="round"/>
  <text x="150" y="82" text-anchor="middle" font-size="7.5" fill="#3b82f6">h 260 (relativo: +260 en X)</text>
</svg>
```

---

## `V y` — Vertical Line To

Solo necesita la coordenada Y. La coordenada X no cambia.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- V: línea vertical (X fija) -->
  <path d="M 80 10 V 120"
        stroke="#f59e0b" stroke-width="3" fill="none" stroke-linecap="round"/>
  <circle cx="80" cy="10"  r="5" fill="#ef4444"/>
  <circle cx="80" cy="120" r="5" fill="#f59e0b"/>
  <text x="80" y="7" text-anchor="middle" font-size="7" fill="#ef4444">M 80 10</text>
  <text x="100" y="122" font-size="7" fill="#f59e0b">V 120</text>

  <!-- Combinando H y V -->
  <path d="M 160 10 H 280 V 120 H 160 Z"
        stroke="#a855f7" stroke-width="2.5" fill="#f3e8ff" fill-opacity="0.5"/>
  <text x="220" y="70" text-anchor="middle" font-size="8" fill="#7c3aed">M H V H Z</text>
  <text x="220" y="118" text-anchor="middle" font-size="7.5" fill="#a855f7">rectángulo con H y V</text>
</svg>
```

---

## Ejemplo práctico: rectángulo con path

Un rectángulo usando solo `M`, `H`, `V` y `Z` — más verboso que `<rect>` pero muestra la lógica.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <path d="M 40 20
           H 260
           V 110
           H 40
           Z"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="2.5"/>

  <!-- Anotaciones de cada comando -->
  <text x="150" y="16" text-anchor="middle" font-size="7.5" fill="#3b82f6">H 260 →</text>
  <text x="270" y="65" font-size="7.5" fill="#10b981">V 110 ↓</text>
  <text x="150" y="123" text-anchor="middle" font-size="7.5" fill="#3b82f6">← H 40</text>
  <text x="28"  y="65" text-anchor="end" font-size="7.5" fill="#f59e0b">↑ Z</text>

  <!-- Punto de inicio -->
  <circle cx="40" cy="20" r="5" fill="#ef4444"/>
  <text x="52" y="17" font-size="7" fill="#ef4444">M 40 20</text>

  <text x="150" y="135" text-anchor="middle" font-size="7" fill="#94a3b8">
    M 40 20  H 260  V 110  H 40  Z
  </text>
</svg>
```

---

## Comparativa L vs H vs V

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- L para diagonal -->
  <path d="M 10 70 L 80 20" stroke="#3b82f6" stroke-width="2.5" fill="none" stroke-linecap="round"/>
  <text x="45" y="85" text-anchor="middle" font-size="7.5" fill="#3b82f6">L (cualquier)</text>
  <text x="45" y="10" text-anchor="middle" font-size="7" fill="#94a3b8" font-family="monospace">L 80 20</text>

  <!-- H para horizontal -->
  <path d="M 110 45 H 190" stroke="#10b981" stroke-width="2.5" fill="none" stroke-linecap="round"/>
  <text x="150" y="63" text-anchor="middle" font-size="7.5" fill="#10b981">H (horizontal)</text>
  <text x="150" y="10" text-anchor="middle" font-size="7" fill="#94a3b8" font-family="monospace">H 190</text>

  <!-- V para vertical -->
  <path d="M 230 15 V 75" stroke="#f59e0b" stroke-width="2.5" fill="none" stroke-linecap="round"/>
  <text x="250" y="82" font-size="7.5" fill="#f59e0b">V (vertical)</text>
  <text x="250" y="10" text-anchor="middle" font-size="7" fill="#94a3b8" font-family="monospace">V 75</text>

  <!-- Separadores verticales -->
  <line x1="100" y1="0" x2="100" y2="90" stroke="#e2e8f0" stroke-width="1"/>
  <line x1="215" y1="0" x2="215" y2="90" stroke="#e2e8f0" stroke-width="1"/>
</svg>
```

**Resumen:** usa `H` y `V` en lugar de `L` cuando la línea es puramente horizontal o vertical. Un solo número en vez de dos — paths más cortos y legibles.
