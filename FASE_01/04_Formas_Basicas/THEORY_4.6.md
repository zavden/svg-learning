# 4.6 Polígono `<polygon>`

El polígono funciona igual que `<polyline>` pero **cierra automáticamente** el contorno: traza una línea desde el último punto de vuelta al primero.

---

## Cierre automático vs polilínea

La única diferencia entre `<polygon>` y `<polyline>` es esa última línea implícita de cierre.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- polyline: NO cierra -->
  <rect x="5" y="5" width="135" height="120" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <polyline points="30,110 75,30 120,110"
            stroke="#ef4444" stroke-width="2" fill="none"/>
  <text x="72" y="128" text-anchor="middle" font-size="8" fill="#ef4444">&lt;polyline&gt; — abierta</text>

  <!-- polygon: SÍ cierra -->
  <rect x="155" y="5" width="140" height="120" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <polygon points="180,110 225,30 270,110"
           stroke="#10b981" stroke-width="2" fill="#10b981" opacity="0.2"/>
  <polygon points="180,110 225,30 270,110"
           stroke="#10b981" stroke-width="2" fill="none"/>
  <text x="225" y="128" text-anchor="middle" font-size="8" fill="#10b981">&lt;polygon&gt; — cerrada ✓</text>
</svg>
```

---

## Triángulo

Tres puntos forman un triángulo. La orientación depende del orden de los puntos.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Triángulo apuntando arriba -->
  <polygon points="75,15 140,110 10,110"
           fill="#3b82f6" opacity="0.8"/>
  <text x="75" y="126" text-anchor="middle" font-size="8" fill="#64748b">arriba</text>

  <!-- Triángulo apuntando derecha -->
  <polygon points="160,15 280,65 160,115"
           fill="#10b981" opacity="0.8"/>
  <text x="220" y="126" text-anchor="middle" font-size="8" fill="#64748b">derecha</text>

  <!-- Etiqueta con código -->
  <text x="75"  y="108" text-anchor="middle" font-size="6.5" fill="white" font-family="monospace">75,15 140,110 10,110</text>
</svg>
```

---

## Polígonos regulares: fórmula

Para un polígono regular de `n` lados, cada vértice `i` se calcula con trigonometría:

```
x = cx + r · cos(2π·i/n − π/2)
y = cy + r · sin(2π·i/n − π/2)
```

El `−π/2` rota el primer vértice hacia arriba.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Triángulo (n=3) cx=40 cy=65 r=40 -->
  <polygon points="40,25 74.64,85 5.36,85"
           fill="#3b82f6" opacity="0.75"/>
  <text x="40" y="110" text-anchor="middle" font-size="8" fill="#64748b">n=3</text>

  <!-- Cuadrado (n=4) cx=115 cy=70 r=40 -->
  <polygon points="115,30 155,70 115,110 75,70"
           fill="#10b981" opacity="0.75"/>
  <text x="115" y="128" text-anchor="middle" font-size="8" fill="#64748b">n=4</text>

  <!-- Pentágono (n=5) cx=190 cy=70 r=40 -->
  <polygon points="190,30 227.02,56.18 212.36,104.72 167.64,104.72 152.98,56.18"
           fill="#f59e0b" opacity="0.75"/>
  <text x="190" y="128" text-anchor="middle" font-size="8" fill="#64748b">n=5</text>

  <!-- Hexágono (n=6) cx=265 cy=70 r=40 -->
  <polygon points="265,30 299.64,50 299.64,90 265,110 230.36,90 230.36,50"
           fill="#a855f7" opacity="0.75"/>
  <text x="265" y="128" text-anchor="middle" font-size="8" fill="#64748b">n=6</text>
</svg>
```

---

## Estrella de 5 puntas

Una estrella se construye alternando vértices en un radio exterior `R` y un radio interior `r`.

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Estrella 5 puntas (R=60, r=25, cx=90, cy=80) -->
  <!-- Puntas: ángulo -90°, luego cada 72°. Interior: +36° entre puntas -->
  <polygon
    points="90,20 101.76,55.27 138.53,55.27 109.39,76.18 120.71,111.45 90,90.71 59.29,111.45 70.61,76.18 41.47,55.27 78.24,55.27"
    fill="#f59e0b" stroke="#fbbf24" stroke-width="1.5" stroke-linejoin="round"/>

  <!-- Estrella relleno hueco con fill-rule -->
  <polygon
    points="210,20 221.76,55.27 258.53,55.27 229.39,76.18 240.71,111.45 210,90.71 179.29,111.45 190.61,76.18 161.47,55.27 198.24,55.27"
    fill="#6366f1" fill-rule="evenodd"
    stroke="#818cf8" stroke-width="1.5" stroke-linejoin="round"/>

  <text x="90"  y="138" text-anchor="middle" font-size="8" fill="#64748b">rellena</text>
  <text x="210" y="138" text-anchor="middle" font-size="8" fill="#64748b">fill-rule="evenodd"</text>
  <text x="150" y="150" text-anchor="middle" font-size="7" fill="#475569">R=60, r=25 — alternando radios</text>
</svg>
```

---

## `fill-rule`: `nonzero` vs `evenodd`

Controla qué regiones se consideran "dentro" cuando los contornos se cruzan.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- nonzero (default) -->
  <polygon
    points="75,15 86.76,50.27 123.53,50.27 94.39,71.18 105.71,106.45 75,85.71 44.29,106.45 55.61,71.18 26.47,50.27 63.24,50.27"
    fill="#3b82f6" fill-rule="nonzero" stroke="#1d4ed8" stroke-width="1"/>
  <text x="75" y="120" text-anchor="middle" font-size="8" fill="#64748b">nonzero (defecto)</text>

  <!-- evenodd: crea "agujero" en el centro -->
  <polygon
    points="225,15 236.76,50.27 273.53,50.27 244.39,71.18 255.71,106.45 225,85.71 194.29,106.45 205.61,71.18 176.47,50.27 213.24,50.27"
    fill="#3b82f6" fill-rule="evenodd" stroke="#1d4ed8" stroke-width="1"/>
  <text x="225" y="120" text-anchor="middle" font-size="8" fill="#64748b">evenodd (hueco)</text>
</svg>
```
