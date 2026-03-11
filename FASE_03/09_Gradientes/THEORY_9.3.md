# 9.3 Stops de color `<stop>`

Cada `<stop>` define un punto de color dentro del gradiente. El gradiente interpola suavemente entre stops consecutivos.

---

## Atributos: offset, stop-color, stop-opacity

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- 2 stops — mínimo -->
    <linearGradient id="stops-2" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>

    <!-- 3 stops con posición intermedia -->
    <linearGradient id="stops-3" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="60%"  stop-color="#10b981"/>
      <stop offset="100%" stop-color="#f59e0b"/>
    </linearGradient>

    <!-- stop-opacity: gradiente a transparente -->
    <linearGradient id="stops-alpha" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#ef4444" stop-opacity="1"/>
      <stop offset="100%" stop-color="#ef4444" stop-opacity="0"/>
    </linearGradient>
  </defs>

  <rect x="10"  y="10" width="280" height="25" rx="4" fill="url(#stops-2)"/>
  <text x="150" y="46" text-anchor="middle" font-size="7" fill="#64748b">2 stops (mínimo): offset=0% y offset=100%</text>

  <rect x="10"  y="52" width="280" height="25" rx="4" fill="url(#stops-3)"/>
  <text x="150" y="88" text-anchor="middle" font-size="7" fill="#64748b">3 stops: stop intermedio en offset=60%</text>

  <!-- Checker para ver la transparencia -->
  <defs>
    <pattern id="chk" width="8" height="8" patternUnits="userSpaceOnUse">
      <rect width="4" height="4" fill="#e2e8f0"/>
      <rect x="4" y="4" width="4" height="4" fill="#e2e8f0"/>
    </pattern>
  </defs>
  <rect x="10"  y="94" width="280" height="22" rx="4" fill="url(#chk)"/>
  <rect x="10"  y="94" width="280" height="22" rx="4" fill="url(#stops-alpha)"/>
  <text x="150" y="128" text-anchor="middle" font-size="7" fill="#64748b">
    stop-opacity: de opaco (1) a transparente (0)
  </text>
</svg>
```

---

## Stop con cambio abrupto — dos stops en el mismo offset

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Cambio abrupto: dos stops en 50% -->
    <linearGradient id="sharp-stop" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="50%"  stop-color="#3b82f6"/>
      <stop offset="50%"  stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#ef4444"/>
    </linearGradient>

    <!-- Mezcla: suave izquierda, abrupto centro, suave derecha -->
    <linearGradient id="mixed-stop" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#10b981"/>
      <stop offset="40%"  stop-color="#fbbf24"/>
      <stop offset="60%"  stop-color="#fbbf24"/>
      <stop offset="60%"  stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#7c3aed"/>
    </linearGradient>
  </defs>

  <rect x="10" y="10" width="280" height="22" rx="4" fill="url(#sharp-stop)"/>
  <text x="150" y="42" text-anchor="middle" font-size="7" fill="#64748b">
    dos stops en offset=50% → corte duro (sin transición)
  </text>

  <rect x="10" y="48" width="280" height="22" rx="4" fill="url(#mixed-stop)"/>
  <text x="150" y="80" text-anchor="middle" font-size="7" fill="#64748b">
    mezcla: suave + abrupto + suave
  </text>
</svg>
```

---

## stop-opacity: sombras y overlays semitransparentes

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Sombra inferior: negro a transparente -->
    <linearGradient id="shadow-grad" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%"   stop-color="black" stop-opacity="0"/>
      <stop offset="100%" stop-color="black" stop-opacity="0.6"/>
    </linearGradient>

    <!-- Fade out lateral: color a transparente -->
    <linearGradient id="fadeout" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="60%"  stop-color="#3b82f6" stop-opacity="1"/>
      <stop offset="100%" stop-color="#3b82f6" stop-opacity="0"/>
    </linearGradient>
  </defs>

  <!-- Imagen simulada con sombra superpuesta -->
  <rect x="10"  y="10" width="130" height="75" fill="#7dd3fc" rx="4"/>
  <rect x="10"  y="10" width="130" height="75" fill="url(#shadow-grad)" rx="4"/>
  <text x="75"  y="99" text-anchor="middle" font-size="7" fill="#64748b">sombra inferior</text>
  <text x="75"  y="108" text-anchor="middle" font-size="6.5" fill="#94a3b8">(encima de imagen)</text>

  <!-- Texto con fade lateral -->
  <rect x="160" y="30" width="130" height="30" rx="4" fill="url(#fadeout)"/>
  <text x="165" y="52" font-size="8.5" fill="white">Texto largo...</text>
  <text x="225" y="75" text-anchor="middle" font-size="7" fill="#64748b">fade-out lateral</text>
</svg>
```

---

## Tabla resumen de atributos de `<stop>`

| Atributo | Valores | Default | Descripción |
|---|---|---|---|
| `offset` | `0`–`1` o `0%`–`100%` | — | Posición en el gradiente |
| `stop-color` | cualquier color CSS | `black` | Color en este punto |
| `stop-opacity` | `0`–`1` | `1` | Opacidad en este punto |
