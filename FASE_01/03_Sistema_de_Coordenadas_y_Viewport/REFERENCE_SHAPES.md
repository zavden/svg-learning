# Referencia visual — Sección 03: Sistema de Coordenadas y Viewport

Guía de consulta rápida. Todos los conceptos de la sección en un solo lugar, con ejemplos visuales mínimos.

---

## viewBox: los 4 valores y su efecto

`viewBox="min-x min-y ancho alto"`

### Sin viewBox — 1 unidad = 1px
```svg
<svg width="200" height="120"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Sin viewBox: coord = px directo -->
  <line x1="0" y1="60" x2="200" y2="60" stroke="#e2e8f0" stroke-width="1"/>
  <line x1="100" y1="0" x2="100" y2="120" stroke="#e2e8f0" stroke-width="1"/>
  <circle cx="100" cy="60" r="40" fill="#3b82f6" opacity="0.8"/>
  <text x="100" y="64" text-anchor="middle" font-size="10" fill="white">
    r=40 → 40px
  </text>
  <text x="100" y="115" text-anchor="middle" font-size="8" fill="#94a3b8">
    sin viewBox
  </text>
</svg>
```

### viewBox igual al viewport — misma escala, coords de usuario
```svg
<svg width="200" height="120" viewBox="0 0 200 120"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- viewBox = viewport → escala 1:1, mismas coords, misma apariencia -->
  <line x1="0" y1="60" x2="200" y2="60" stroke="#e2e8f0" stroke-width="1"/>
  <line x1="100" y1="0" x2="100" y2="120" stroke="#e2e8f0" stroke-width="1"/>
  <circle cx="100" cy="60" r="40" fill="#3b82f6" opacity="0.8"/>
  <text x="100" y="64" text-anchor="middle" font-size="10" fill="white">
    r=40 → 40px
  </text>
  <text x="100" y="115" text-anchor="middle" font-size="8" fill="#94a3b8">
    viewBox="0 0 200 120" (1:1)
  </text>
</svg>
```

### viewBox más pequeño — zoom in
```svg
<svg width="200" height="120" viewBox="0 0 100 60"
     style="border:2px solid #3b82f6;background:#f8fafc;display:block;">
  <!-- viewBox más pequeño → escala 2x → zoom in -->
  <!-- 1 unidad de usuario = 2px en pantalla -->
  <line x1="0" y1="30" x2="100" y2="30" stroke="#e2e8f0" stroke-width="0.5"/>
  <line x1="50" y1="0" x2="50" y2="60" stroke="#e2e8f0" stroke-width="0.5"/>
  <circle cx="50" cy="30" r="20" fill="#3b82f6" opacity="0.8"/>
  <text x="50" y="34" text-anchor="middle" font-size="5" fill="white">
    r=20u → 40px (2x)
  </text>
  <text x="50" y="57" text-anchor="middle" font-size="4" fill="#94a3b8">
    viewBox="0 0 100 60" → 2x zoom
  </text>
</svg>
```

### viewBox más grande — zoom out
```svg
<svg width="200" height="120" viewBox="0 0 400 240"
     style="border:2px solid #94a3b8;background:#f8fafc;display:block;">
  <!-- viewBox más grande → escala 0.5x → zoom out -->
  <!-- 1 unidad de usuario = 0.5px en pantalla -->
  <line x1="0" y1="120" x2="400" y2="120" stroke="#e2e8f0" stroke-width="1"/>
  <line x1="200" y1="0" x2="200" y2="240" stroke="#e2e8f0" stroke-width="1"/>
  <circle cx="200" cy="120" r="80" fill="#94a3b8" opacity="0.8"/>
  <text x="200" y="125" text-anchor="middle" font-size="20" fill="white">
    r=80u → 40px (0.5x)
  </text>
  <text x="200" y="228" text-anchor="middle" font-size="10" fill="#94a3b8">
    viewBox="0 0 400 240" → 0.5x zoom
  </text>
</svg>
```

### min-x / min-y — efecto de cámara (pan)
```svg
<svg width="200" height="120" viewBox="60 30 100 60"
     style="border:2px solid #f59e0b;background:#f8fafc;display:block;">
  <!-- El área visible empieza en (60,30): la cámara se desplaza -->
  <!-- Elementos en x<60 o y<30 no son visibles -->
  <circle cx="30"  cy="20"  r="18" fill="#ef4444" opacity="0.5"/>
  <text x="30" y="24" text-anchor="middle" font-size="8" fill="white">
    fuera
  </text>
  <circle cx="110" cy="60" r="30" fill="#f59e0b" opacity="0.9"/>
  <text x="110" y="64" text-anchor="middle" font-size="8" fill="white">
    visible
  </text>
  <circle cx="160" cy="90" r="22" fill="#3b82f6" opacity="0.7"/>
  <text x="160" y="94" text-anchor="middle" font-size="8" fill="white">
    parcial
  </text>
</svg>
```

---

## preserveAspectRatio: tabla de alineamientos (meet)

Viewport apaisado (260×130), viewBox cuadrado (100×100). El fondo gris muestra el espacio vacío (pillarboxing) y la zona clara muestra el área activa del viewBox.

### xMinYMin meet
```svg
<svg width="260" height="130" viewBox="0 0 100 100"
     preserveAspectRatio="xMinYMin meet"
     style="border:2px solid #e2e8f0;background:#94a3b8;display:block;">
  <rect width="100" height="100" fill="#eff6ff"/>
  <rect x="2" y="2" width="96" height="96" fill="none"
        stroke="#3b82f6" stroke-width="2"/>
  <text x="50" y="48" text-anchor="middle" font-size="8" fill="#1e40af">
    xMinYMin
  </text>
  <text x="50" y="60" text-anchor="middle" font-size="6" fill="#64748b">
    arriba-izquierda
  </text>
</svg>
```

### xMidYMid meet (por defecto)
```svg
<svg width="260" height="130" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid meet"
     style="border:2px solid #3b82f6;background:#94a3b8;display:block;">
  <rect width="100" height="100" fill="#eff6ff"/>
  <rect x="2" y="2" width="96" height="96" fill="none"
        stroke="#3b82f6" stroke-width="2"/>
  <line x1="50" y1="0" x2="50" y2="100"
        stroke="#bfdbfe" stroke-width="0.5" stroke-dasharray="3,2"/>
  <line x1="0" y1="50" x2="100" y2="50"
        stroke="#bfdbfe" stroke-width="0.5" stroke-dasharray="3,2"/>
  <text x="50" y="48" text-anchor="middle" font-size="8" fill="#1e40af">
    xMidYMid (default)
  </text>
  <text x="50" y="60" text-anchor="middle" font-size="6" fill="#64748b">
    centrado
  </text>
</svg>
```

### xMaxYMax meet
```svg
<svg width="260" height="130" viewBox="0 0 100 100"
     preserveAspectRatio="xMaxYMax meet"
     style="border:2px solid #e2e8f0;background:#94a3b8;display:block;">
  <rect width="100" height="100" fill="#eff6ff"/>
  <rect x="2" y="2" width="96" height="96" fill="none"
        stroke="#3b82f6" stroke-width="2"/>
  <text x="50" y="48" text-anchor="middle" font-size="8" fill="#1e40af">
    xMaxYMax
  </text>
  <text x="50" y="60" text-anchor="middle" font-size="6" fill="#64748b">
    abajo-derecha
  </text>
</svg>
```

---

## preserveAspectRatio: meet vs slice vs none

### meet — cabe todo, puede haber espacio vacío
```svg
<svg width="260" height="130" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid meet"
     style="border:2px solid #10b981;background:#94a3b8;display:block;">
  <rect width="100" height="100" fill="#f0fdf4"/>
  <circle cx="50" cy="50" r="42" fill="#10b981" opacity="0.85"/>
  <text x="50" y="54" text-anchor="middle" font-size="7" fill="white">
    meet: todo visible
  </text>
</svg>
```

### slice — cubre todo, puede recortarse
```svg
<svg width="260" height="130" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid slice"
     style="border:2px solid #ef4444;background:#fef2f2;display:block;">
  <rect width="100" height="100" fill="#fee2e2"/>
  <circle cx="50" cy="50" r="42" fill="#ef4444" opacity="0.85"/>
  <line x1="0" y1="25" x2="100" y2="25"
        stroke="#fbbf24" stroke-width="1" stroke-dasharray="3,2"/>
  <line x1="0" y1="75" x2="100" y2="75"
        stroke="#fbbf24" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="50" y="54" text-anchor="middle" font-size="7" fill="white">
    slice: llena, recorta
  </text>
</svg>
```

### none — distorsión total
```svg
<svg width="260" height="130" viewBox="0 0 100 100"
     preserveAspectRatio="none"
     style="border:2px solid #a855f7;background:#faf5ff;display:block;">
  <rect width="100" height="100" fill="#f3e8ff"/>
  <circle cx="50" cy="50" r="42" fill="#a855f7" opacity="0.85"/>
  <text x="50" y="54" text-anchor="middle" font-size="7" fill="white">
    none: distorsionado
  </text>
</svg>
```

---

## Sistema de coordenadas: ejes y puntos de referencia

### Mapa de ejes SVG
```svg
<svg width="260" height="180"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Ejes del sistema de coordenadas SVG -->
  <!-- X: crece a la derecha | Y: crece hacia abajo -->

  <!-- Eje Y (vertical) -->
  <line x1="30" y1="10" x2="30" y2="170"
        stroke="#3b82f6" stroke-width="2"/>
  <polygon points="27,168 30,176 33,168" fill="#3b82f6"/>
  <text x="34" y="176" font-size="9" fill="#3b82f6">+Y (abajo)</text>

  <!-- Eje X (horizontal) -->
  <line x1="30" y1="30" x2="250" y2="30"
        stroke="#10b981" stroke-width="2"/>
  <polygon points="248,27 256,30 248,33" fill="#10b981"/>
  <text x="222" y="26" font-size="9" fill="#10b981">+X</text>

  <!-- Origen -->
  <circle cx="30" cy="30" r="5" fill="tomato"/>
  <text x="35" y="26" font-size="9" fill="tomato">(0,0)</text>

  <!-- Puntos de ejemplo -->
  <circle cx="130" cy="30" r="4" fill="#10b981"/>
  <text x="132" y="26" font-size="8" fill="#10b981">(100,0)</text>

  <circle cx="30" cy="110" r="4" fill="#3b82f6"/>
  <text x="34" y="108" font-size="8" fill="#3b82f6">(0,80)</text>

  <circle cx="130" cy="110" r="4" fill="#475569"/>
  <line x1="130" y1="30"  x2="130" y2="110"
        stroke="#475569" stroke-width="0.8" stroke-dasharray="3,2"/>
  <line x1="30"  y1="110" x2="130" y2="110"
        stroke="#475569" stroke-width="0.8" stroke-dasharray="3,2"/>
  <text x="134" y="108" font-size="8" fill="#475569">(100,80)</text>

  <!-- Nota eje Y -->
  <text x="35" y="155" font-size="8" fill="#64748b">
    ↑ Y negativo (fuera del viewport)
  </text>
</svg>
```

### Punto de referencia según elemento
```svg
<svg width="280" height="180"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Cada elemento usa (x,y) de forma diferente -->
  <!-- El punto rojo es la coordenada que se especifica -->

  <!-- rect: (x,y) = esquina superior izquierda -->
  <rect x="20" y="20" width="60" height="40"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5" rx="2"/>
  <circle cx="20" cy="20" r="4" fill="tomato"/>
  <text x="22" y="16" font-size="8" fill="tomato">x,y</text>
  <text x="50" y="66" text-anchor="middle"
        font-size="8" fill="#3b82f6">rect: esquina sup-izq</text>

  <!-- circle: (cx,cy) = centro -->
  <circle cx="160" cy="40" r="30"
          fill="#d1fae5" stroke="#10b981" stroke-width="1.5"/>
  <circle cx="160" cy="40" r="4" fill="tomato"/>
  <line x1="130" y1="40" x2="190" y2="40"
        stroke="#10b981" stroke-width="0.8" stroke-dasharray="2,1"/>
  <line x1="160" y1="10" x2="160" y2="70"
        stroke="#10b981" stroke-width="0.8" stroke-dasharray="2,1"/>
  <text x="160" y="38" text-anchor="middle" font-size="7" fill="tomato">
    cx,cy
  </text>
  <text x="160" y="86" text-anchor="middle"
        font-size="8" fill="#10b981">circle: centro</text>

  <!-- text: (x,y) = baseline izquierda -->
  <line x1="20" y1="130" x2="260" y2="130"
        stroke="#fbbf24" stroke-width="1.5"/>
  <text x="20" y="130" font-size="28" fill="#475569">Texto</text>
  <circle cx="20" cy="130" r="4" fill="tomato"/>
  <text x="22" y="125" font-size="8" fill="tomato">x,y</text>
  <text x="20" y="155" font-size="8" fill="#64748b">
    text: inicio de baseline ← (la línea amarilla)
  </text>
</svg>
```

---

## Unidades: equivalencias en pantalla (96dpi CSS)

```svg
<svg width="280" height="200"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Equivalencias visuales. Referencia: viewport = 280px ancho -->

  <text x="8" y="18" font-size="10" fill="#475569">
    Unidades absolutas (siempre el mismo px en pantalla):
  </text>

  <!-- 96px = 1in -->
  <rect x="8" y="24" width="96" height="18" fill="#3b82f6" opacity="0.8" rx="2"/>
  <text x="108" y="37" font-size="9" fill="#475569">96px = 1in</text>

  <!-- ~38px = 1cm -->
  <rect x="8" y="48" width="38" height="18" fill="#10b981" opacity="0.8" rx="2"/>
  <text x="50" y="61" font-size="9" fill="#475569">~38px = 1cm</text>

  <!-- ~4px = 1mm -->
  <rect x="8" y="72" width="4" height="18" fill="#f59e0b" opacity="0.9" rx="1"/>
  <text x="16" y="85" font-size="9" fill="#475569">~4px = 1mm</text>

  <!-- 16px = 1pc -->
  <rect x="8" y="96" width="16" height="18" fill="#a855f7" opacity="0.8" rx="2"/>
  <text x="28" y="109" font-size="9" fill="#475569">16px = 1pc</text>

  <!-- ~1.3px = 1pt -->
  <rect x="8" y="120" width="2" height="18" fill="#ef4444" opacity="0.9" rx="1"/>
  <text x="14" y="133" font-size="9" fill="#475569">~1.3px = 1pt</text>

  <line x1="8" y1="148" x2="272" y2="148"
        stroke="#e2e8f0" stroke-width="1"/>
  <text x="8" y="162" font-size="10" fill="#475569">
    Unidades relativas (dependen del contexto):
  </text>
  <text x="8" y="176" font-size="9" fill="#64748b">
    sin sufijo = user units (1:1 con px si no hay viewBox)
  </text>
  <text x="8" y="190" font-size="9" fill="#64748b">
    % en formas SVG = % del viewport del SVG padre
  </text>
</svg>
```
