# Referencia — Patrones

---

## Estructura mínima de un patrón

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <pattern id="min-pat"
             x="0" y="0"
             width="20" height="20"
             patternUnits="userSpaceOnUse">
      <!-- contenido de la celda (se repite) -->
      <circle cx="10" cy="10" r="7" fill="none" stroke="#3b82f6" stroke-width="1.5"/>
    </pattern>
  </defs>

  <rect x="10" y="10" width="280" height="55" rx="4" fill="url(#min-pat)"/>

  <text x="150" y="76" text-anchor="middle" font-size="7.5" fill="#64748b">
    patrón mínimo: id + width/height + patternUnits + contenido
  </text>
</svg>
```

---

## Galería de patrones comunes

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <pattern id="ref-h"    width="100" height="10" patternUnits="userSpaceOnUse">
      <rect x="0" y="0" width="100" height="5" fill="#3b82f6"/></pattern>

    <pattern id="ref-v"    width="10" height="100" patternUnits="userSpaceOnUse">
      <rect x="0" y="0" width="5" height="100" fill="#10b981"/></pattern>

    <pattern id="ref-d"    width="100" height="10" patternUnits="userSpaceOnUse"
             patternTransform="rotate(45)">
      <rect x="0" y="0" width="100" height="5" fill="#f59e0b"/></pattern>

    <pattern id="ref-dots" width="16" height="16" patternUnits="userSpaceOnUse">
      <circle cx="8" cy="8" r="4" fill="#8b5cf6"/></pattern>

    <pattern id="ref-grid" width="20" height="20" patternUnits="userSpaceOnUse">
      <line x1="20" y1="0"  x2="20" y2="20" stroke="#cbd5e1" stroke-width="0.5"/>
      <line x1="0"  y1="20" x2="20" y2="20" stroke="#cbd5e1" stroke-width="0.5"/></pattern>

    <pattern id="ref-chess" width="20" height="20" patternUnits="userSpaceOnUse">
      <rect x="0"  y="0"  width="10" height="10" fill="#334155"/>
      <rect x="10" y="10" width="10" height="10" fill="#334155"/></pattern>

    <pattern id="ref-cross" width="15" height="15" patternUnits="userSpaceOnUse">
      <line x1="7.5" y1="0" x2="7.5" y2="15" stroke="#ef4444" stroke-width="1"/>
      <line x1="0" y1="7.5" x2="15" y2="7.5" stroke="#ef4444" stroke-width="1"/></pattern>

    <pattern id="ref-brick" width="40" height="20" patternUnits="userSpaceOnUse">
      <rect x="1"  y="1"  width="38" height="8"  fill="#f97316" rx="1"/>
      <rect x="1"  y="11" width="18" height="8"  fill="#f59e0b" rx="1"/>
      <rect x="21" y="11" width="18" height="8"  fill="#f59e0b" rx="1"/></pattern>
  </defs>

  <!-- Fila 1 -->
  <rect x="5"   y="5"  width="62" height="45" rx="3" fill="url(#ref-h)"/>
  <text x="36"  y="58" text-anchor="middle" font-size="6.5" fill="#3b82f6">horizontal</text>

  <rect x="73"  y="5"  width="62" height="45" rx="3" fill="url(#ref-v)"/>
  <text x="104" y="58" text-anchor="middle" font-size="6.5" fill="#10b981">vertical</text>

  <rect x="141" y="5"  width="62" height="45" rx="3" fill="url(#ref-d)"/>
  <text x="172" y="58" text-anchor="middle" font-size="6.5" fill="#f59e0b">diagonal</text>

  <rect x="209" y="5"  width="87" height="45" rx="3" fill="url(#ref-dots)"/>
  <text x="252" y="58" text-anchor="middle" font-size="6.5" fill="#8b5cf6">puntos</text>

  <!-- Fila 2 -->
  <rect x="5"   y="65" width="62" height="45" rx="3" fill="url(#ref-grid)"/>
  <text x="36"  y="118" text-anchor="middle" font-size="6.5" fill="#64748b">cuadrícula</text>

  <rect x="73"  y="65" width="62" height="45" rx="3" fill="url(#ref-chess)"/>
  <text x="104" y="118" text-anchor="middle" font-size="6.5" fill="#334155">ajedrez</text>

  <rect x="141" y="65" width="62" height="45" rx="3" fill="url(#ref-cross)"/>
  <text x="172" y="118" text-anchor="middle" font-size="6.5" fill="#ef4444">cruces</text>

  <rect x="209" y="65" width="87" height="45" rx="3" fill="url(#ref-brick)"/>
  <text x="252" y="118" text-anchor="middle" font-size="6.5" fill="#f97316">ladrillos</text>

  <text x="150" y="135" text-anchor="middle" font-size="7.5" fill="#64748b">
    patternUnits="userSpaceOnUse" en todos
  </text>
  <text x="150" y="147" text-anchor="middle" font-size="7" fill="#94a3b8">
    rayas: rect que llena la mitad de la celda
  </text>
  <text x="150" y="158" text-anchor="middle" font-size="7" fill="#94a3b8">
    cuadrícula: borde derecho + inferior de la celda
  </text>
  <text x="150" y="169" text-anchor="middle" font-size="7" fill="#94a3b8">
    ajedrez: dos cuadros en esquinas opuestas de celda 2×2
  </text>
  <text x="150" y="180" text-anchor="middle" font-size="7" fill="#94a3b8">
    ladrillos: celda con fila superior + dos medios en inferior
  </text>
</svg>
```

---

## Tabla de atributos de `<pattern>`

| Atributo | Valores | Default | Descripción |
|---|---|---|---|
| `x`, `y` | número | `0` | Posición de inicio del patrón |
| `width`, `height` | número o % | — (obligatorio) | Tamaño de la celda |
| `patternUnits` | `objectBoundingBox` / `userSpaceOnUse` | `objectBoundingBox` | Sistema de coords de x/y/width/height |
| `patternContentUnits` | `objectBoundingBox` / `userSpaceOnUse` | `userSpaceOnUse` | Sistema de coords del contenido interno |
| `patternTransform` | transform SVG | — | Transformación al patrón completo |
| `viewBox` | `x y w h` | — | Sistema de coords del contenido (con escalado) |
| `preserveAspectRatio` | `xMidYMid meet` etc. | `xMidYMid meet` | Con viewBox: cómo encajar el contenido |
