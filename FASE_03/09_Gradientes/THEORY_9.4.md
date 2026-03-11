# 9.4 Atributo `gradientUnits`

Define el sistema de coordenadas de los puntos del gradiente. Controla si las posiciones son relativas al elemento o absolutas en el viewport.

---

## `objectBoundingBox` — relativo al elemento (por defecto)

Las coordenadas se expresan como fracción del bounding box del elemento que usa el gradiente. `0` = inicio del bounding box, `1` = final.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- objectBoundingBox (default): el gradiente se adapta a cada forma -->
    <linearGradient id="obb-grad" gradientUnits="objectBoundingBox"
                    x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <!-- Rectángulo pequeño -->
  <rect x="10"  y="20" width="60"  height="60" rx="4" fill="url(#obb-grad)"/>
  <text x="40"  y="90" text-anchor="middle" font-size="7" fill="#64748b">60×60</text>

  <!-- Rectángulo grande — el gradiente se escala con él -->
  <rect x="90"  y="20" width="200" height="60" rx="4" fill="url(#obb-grad)"/>
  <text x="190" y="90" text-anchor="middle" font-size="7" fill="#64748b">200×60 — mismo gradiente, se adapta</text>

  <text x="150" y="104" text-anchor="middle" font-size="7.5" fill="#64748b">
    objectBoundingBox: el gradiente cubre siempre todo el elemento
  </text>
</svg>
```

---

## `userSpaceOnUse` — coordenadas absolutas del viewBox

Las coordenadas son posiciones reales en el sistema de coordenadas del SVG. El gradiente tiene posición fija independiente del tamaño del elemento.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- userSpaceOnUse: coordenadas fijas en el viewBox -->
    <linearGradient id="usu-grad" gradientUnits="userSpaceOnUse"
                    x1="10" y1="0" x2="290" y2="0">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <!-- Ambas formas comparten el gradiente continuo -->
  <rect x="10"  y="15" width="100" height="50" rx="4" fill="url(#usu-grad)"/>
  <rect x="190" y="15" width="100" height="50" rx="4" fill="url(#usu-grad)"/>
  <text x="150" y="80" text-anchor="middle" font-size="7.5" fill="#64748b">
    las dos formas tienen el gradiente continuo del viewBox
  </text>
  <text x="150" y="91" text-anchor="middle" font-size="7" fill="#94a3b8">
    como si fueran "ventanas" al mismo gradiente de fondo
  </text>

  <!-- Caso: gradiente no cubre el elemento → se ve parcial -->
  <defs>
    <linearGradient id="usu-partial" gradientUnits="userSpaceOnUse"
                    x1="50" y1="0" x2="150" y2="0">
      <stop offset="0%"   stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#fbbf24"/>
    </linearGradient>
  </defs>
  <rect x="10"  y="100" width="280" height="25" rx="4" fill="url(#usu-partial)"/>
  <text x="150" y="136" text-anchor="middle" font-size="7" fill="#64748b">
    gradiente solo de x=50 a x=150: el resto se rellena con el color del stop más cercano
  </text>
</svg>
```

---

## Comparativa: cuándo usar cada uno

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">gradientUnits — cuándo usar cada uno</text>

  <text x="10" y="36" font-size="8" fill="#3b82f6" font-weight="700">objectBoundingBox (default)</text>
  <text x="10" y="50" font-size="7" fill="#64748b">• Gradiente que cubre cada forma por completo</text>
  <text x="10" y="62" font-size="7" fill="#64748b">• El mismo gradiente reutilizado en formas de distintos tamaños</text>
  <text x="10" y="74" font-size="7" fill="#64748b">• La mayoría de casos de uso</text>

  <line x1="10" y1="80" x2="290" y2="80" stroke="#e2e8f0" stroke-width="1"/>

  <text x="10" y="94" font-size="8" fill="#10b981" font-weight="700">userSpaceOnUse</text>
  <text x="10" y="107" font-size="7" fill="#64748b">• Varias formas comparten un gradiente continuo (como mosaico)</text>
  <text x="10" y="119" font-size="7" fill="#64748b">• Gradiente con dimensión física exacta en el viewport</text>
</svg>
```
