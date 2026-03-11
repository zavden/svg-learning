# 10.4 `patternTransform`

Aplica una transformación SVG al patrón completo: rotarlo, escalarlo o desplazarlo. Equivalente al atributo `transform` de los elementos normales.

---

## Rotar un patrón

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Rayas horizontales como base -->
    <pattern id="base-stripes" x="0" y="0" width="20" height="10"
             patternUnits="userSpaceOnUse">
      <rect x="0" y="0" width="20" height="5" fill="#3b82f6"/>
    </pattern>

    <!-- Mismo patrón rotado 45° -->
    <pattern id="diag-stripes" x="0" y="0" width="20" height="10"
             patternUnits="userSpaceOnUse"
             patternTransform="rotate(45)">
      <rect x="0" y="0" width="20" height="5" fill="#8b5cf6"/>
    </pattern>

    <!-- Rotado 90° = rayas verticales -->
    <pattern id="vert-stripes" x="0" y="0" width="20" height="10"
             patternUnits="userSpaceOnUse"
             patternTransform="rotate(90)">
      <rect x="0" y="0" width="20" height="5" fill="#10b981"/>
    </pattern>
  </defs>

  <rect x="10"  y="10" width="80" height="80" rx="4" fill="url(#base-stripes)"/>
  <text x="50"  y="102" text-anchor="middle" font-size="7" fill="#3b82f6">horizontal</text>
  <text x="50"  y="111" text-anchor="middle" font-size="6.5" fill="#94a3b8">rotate(0)</text>

  <rect x="110" y="10" width="80" height="80" rx="4" fill="url(#diag-stripes)"/>
  <text x="150" y="102" text-anchor="middle" font-size="7" fill="#8b5cf6">diagonal</text>
  <text x="150" y="111" text-anchor="middle" font-size="6.5" fill="#94a3b8">rotate(45)</text>

  <rect x="210" y="10" width="80" height="80" rx="4" fill="url(#vert-stripes)"/>
  <text x="250" y="102" text-anchor="middle" font-size="7" fill="#10b981">vertical</text>
  <text x="250" y="111" text-anchor="middle" font-size="6.5" fill="#94a3b8">rotate(90)</text>
</svg>
```

---

## Escalar un patrón

`patternTransform="scale(n)"` aumenta o reduce el tamaño de la celda sin cambiar los atributos `width`/`height`.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <pattern id="scale-base" x="0" y="0" width="15" height="15"
             patternUnits="userSpaceOnUse">
      <circle cx="7.5" cy="7.5" r="5" fill="none" stroke="#3b82f6" stroke-width="1"/>
    </pattern>

    <pattern id="scale-2x" x="0" y="0" width="15" height="15"
             patternUnits="userSpaceOnUse"
             patternTransform="scale(2)">
      <circle cx="7.5" cy="7.5" r="5" fill="none" stroke="#3b82f6" stroke-width="1"/>
    </pattern>

    <pattern id="scale-half" x="0" y="0" width="15" height="15"
             patternUnits="userSpaceOnUse"
             patternTransform="scale(0.5)">
      <circle cx="7.5" cy="7.5" r="5" fill="none" stroke="#3b82f6" stroke-width="1"/>
    </pattern>
  </defs>

  <rect x="5"   y="10" width="88" height="72" rx="4" fill="url(#scale-base)"/>
  <text x="49"  y="92" text-anchor="middle" font-size="7" fill="#64748b">scale(1) — base</text>

  <rect x="106" y="10" width="88" height="72" rx="4" fill="url(#scale-2x)"/>
  <text x="150" y="92" text-anchor="middle" font-size="7" fill="#64748b">scale(2) — más grande</text>

  <rect x="207" y="10" width="88" height="72" rx="4" fill="url(#scale-half)"/>
  <text x="251" y="92" text-anchor="middle" font-size="7" fill="#64748b">scale(0.5) — más denso</text>
</svg>
```

---

## Combinar transformaciones

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Grid de cuadrados -->
    <pattern id="grid-base" x="0" y="0" width="20" height="20"
             patternUnits="userSpaceOnUse">
      <rect x="0.5" y="0.5" width="19" height="19" fill="none"
            stroke="#94a3b8" stroke-width="0.5"/>
    </pattern>

    <!-- Grid rotado y escalado: rombo denso -->
    <pattern id="grid-diamond" x="0" y="0" width="20" height="20"
             patternUnits="userSpaceOnUse"
             patternTransform="rotate(45) scale(0.7)">
      <rect x="0.5" y="0.5" width="19" height="19" fill="none"
            stroke="#3b82f6" stroke-width="0.5"/>
    </pattern>
  </defs>

  <rect x="10"  y="10" width="130" height="75" rx="4" fill="url(#grid-base)"/>
  <text x="75"  y="95" text-anchor="middle" font-size="7" fill="#64748b">grid normal</text>

  <rect x="160" y="10" width="130" height="75" rx="4" fill="url(#grid-diamond)"/>
  <text x="225" y="95" text-anchor="middle" font-size="7" fill="#3b82f6">rotate(45) scale(0.7)</text>
  <text x="225" y="104" text-anchor="middle" font-size="6.5" fill="#94a3b8">→ rombo</text>
</svg>
```
