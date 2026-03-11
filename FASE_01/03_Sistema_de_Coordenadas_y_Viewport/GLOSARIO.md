# Glosario visual — Sección 03: Sistema de Coordenadas y Viewport

Cada término con 2–3 ejemplos SVG mínimos que ilustran su significado.

---

## viewport

El espacio físico que ocupa el SVG en pantalla. Se define con `width` y `height` en el elemento `<svg>`. Es la "caja" visible, independientemente de las coordenadas internas.

```svg
<svg width="240" height="100"
     style="border:3px solid #3b82f6;background:#f8fafc;display:block;">
  <!-- El borde azul ES el viewport: 240×100 píxeles en pantalla -->
  <text x="120" y="45" text-anchor="middle"
        font-size="11" fill="#475569">viewport</text>
  <text x="120" y="62" text-anchor="middle"
        font-size="10" fill="#3b82f6">240 × 100 px</text>
  <circle cx="0"   cy="0"   r="4" fill="tomato"/>
  <circle cx="240" cy="0"   r="4" fill="tomato"/>
  <circle cx="0"   cy="100" r="4" fill="tomato"/>
  <circle cx="240" cy="100" r="4" fill="tomato"/>
</svg>
```

```svg
<svg width="120" height="100"
     style="border:3px solid #ef4444;background:#f8fafc;display:block;">
  <!-- Mismo contenido, viewport más estrecho (120×100) -->
  <text x="60" y="45" text-anchor="middle"
        font-size="11" fill="#475569">viewport</text>
  <text x="60" y="62" text-anchor="middle"
        font-size="10" fill="#ef4444">120 × 100 px</text>
  <circle cx="0"   cy="0"   r="4" fill="tomato"/>
  <circle cx="120" cy="0"   r="4" fill="tomato"/>
  <circle cx="0"   cy="100" r="4" fill="tomato"/>
  <circle cx="120" cy="100" r="4" fill="tomato"/>
</svg>
```

---

## viewBox

El sistema de coordenadas interno del SVG. Define qué región del espacio de usuario se mapea al viewport. Es el "mapa" que el navegador escala al viewport.

```svg
<svg width="200" height="200" viewBox="0 0 100 100"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- viewBox="0 0 100 100": el espacio interno va de 0 a 100 -->
  <!-- El viewport es 200×200, entonces 1 unidad = 2px (escala 2x) -->
  <!-- Un círculo de r=30 ocupa 60px en pantalla -->
  <circle cx="50" cy="50" r="30" fill="#3b82f6" opacity="0.8"/>
  <text x="50" y="54" text-anchor="middle" font-size="7" fill="white">
    r=30 unidades → 60px
  </text>
  <text x="50" y="92" text-anchor="middle" font-size="5" fill="#94a3b8">
    viewBox="0 0 100 100" | viewport 200×200
  </text>
</svg>
```

```svg
<svg width="200" height="200" viewBox="0 0 400 400"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- viewBox="0 0 400 400": escala 0.5x (400 unidades en 200px) -->
  <!-- El mismo r=30 ahora ocupa 15px en pantalla -->
  <circle cx="200" cy="200" r="30" fill="#10b981" opacity="0.8"/>
  <text x="200" y="205" text-anchor="middle" font-size="15" fill="white">
    r=30u → 15px
  </text>
  <text x="200" y="380" text-anchor="middle" font-size="12" fill="#94a3b8">
    viewBox="0 0 400 400" | viewport 200×200
  </text>
</svg>
```

---

## unidades de usuario (user units)

Coordenadas sin sufijo de unidad (`width="100"`, `cx="50"`). Sin `viewBox`, equivalen a px CSS. Con `viewBox`, su tamaño en pantalla depende de la relación viewBox/viewport.

```svg
<svg width="200" height="80"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Sin viewBox: 1 user unit = 1px CSS exactamente -->
  <rect x="10" y="20" width="50" height="40"
        fill="#3b82f6" opacity="0.8" rx="2"/>
  <text x="35" y="44" text-anchor="middle" font-size="9" fill="white">
    50u = 50px
  </text>
  <rect x="70" y="20" width="100" height="40"
        fill="#10b981" opacity="0.8" rx="2"/>
  <text x="120" y="44" text-anchor="middle" font-size="9" fill="white">
    100u = 100px
  </text>
  <text x="100" y="74" text-anchor="middle" font-size="8" fill="#94a3b8">
    sin viewBox: 1 user unit = 1px
  </text>
</svg>
```

```svg
<svg width="200" height="80" viewBox="0 0 50 20"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Con viewBox="0 0 50 20": escala 4x en X, 4x en Y -->
  <!-- 1 user unit = 4px en pantalla -->
  <rect x="2" y="4" width="12" height="12"
        fill="#3b82f6" opacity="0.8" rx="0.5"/>
  <text x="8" y="11.5" text-anchor="middle" font-size="2.5" fill="white">
    12u → 48px
  </text>
  <rect x="18" y="4" width="25" height="12"
        fill="#10b981" opacity="0.8" rx="0.5"/>
  <text x="30" y="11.5" text-anchor="middle" font-size="2.5" fill="white">
    25u → 100px
  </text>
  <text x="25" y="19" text-anchor="middle" font-size="2" fill="#94a3b8">
    con viewBox 50×20 en 200×80: 1u = 4px
  </text>
</svg>
```

---

## preserveAspectRatio

Indica cómo alinear y escalar el viewBox dentro del viewport cuando sus proporciones no coinciden. Valor por defecto: `xMidYMid meet`.

```svg
<svg width="240" height="100" viewBox="0 0 80 80"
     preserveAspectRatio="xMinYMid meet"
     style="border:2px solid #e2e8f0;background:#94a3b8;display:block;">
  <!-- xMinYMin meet: contenido cuadrado en viewport apaisado -->
  <!-- El contenido se ancla al borde izquierdo (xMin) -->
  <rect width="80" height="80" fill="#eff6ff"/>
  <circle cx="40" cy="40" r="35" fill="#3b82f6" opacity="0.8"/>
  <text x="40" y="44" text-anchor="middle" font-size="7" fill="white">
    xMinYMid meet
  </text>
</svg>
```

```svg
<svg width="240" height="100" viewBox="0 0 80 80"
     preserveAspectRatio="xMidYMid slice"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block;">
  <!-- xMidYMid slice: el contenido cubre todo el viewport -->
  <!-- El círculo se recorta arriba y abajo -->
  <rect width="80" height="80" fill="#fee2e2"/>
  <circle cx="40" cy="40" r="35" fill="#ef4444" opacity="0.8"/>
  <text x="40" y="44" text-anchor="middle" font-size="7" fill="white">
    xMidYMid slice
  </text>
</svg>
```

---

## pillarboxing

Franjas vacías a los **lados** (izquierda y derecha) que aparecen cuando el contenido cuadrado se muestra en un viewport apaisado con `meet`.

```svg
<svg width="260" height="110" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid meet"
     style="border:2px solid #e2e8f0;background:#94a3b8;display:block;">
  <!-- Gris a los lados = pillarboxing -->
  <!-- Viewport apaisado (260×110) + viewBox cuadrado (100×100) -->
  <!-- scale = 110/100 = 1.1 → contenido: 110×110 centrado en 260 -->
  <rect width="100" height="100" fill="#eff6ff"/>
  <rect x="5" y="5" width="90" height="90"
        fill="#dbeafe" rx="4"/>
  <text x="50" y="52" text-anchor="middle"
        font-size="7" fill="#1e40af">área del viewBox</text>
  <text x="50" y="64" text-anchor="middle"
        font-size="5.5" fill="#64748b">franjas grises = pillarboxing</text>
</svg>
```

---

## letterboxing

Franjas vacías **arriba y abajo** que aparecen cuando el contenido cuadrado se muestra en un viewport portrait con `meet`.

```svg
<svg width="160" height="220" viewBox="0 0 100 100"
     preserveAspectRatio="xMidYMid meet"
     style="border:2px solid #e2e8f0;background:#94a3b8;display:block;">
  <!-- Gris arriba y abajo = letterboxing -->
  <!-- Viewport portrait (160×220) + viewBox cuadrado (100×100) -->
  <!-- scale = 160/100 = 1.6 → contenido: 160×160 centrado en 220 -->
  <rect width="100" height="100" fill="#eff6ff"/>
  <rect x="5" y="5" width="90" height="90"
        fill="#dbeafe" rx="4"/>
  <text x="50" y="52" text-anchor="middle"
        font-size="7" fill="#1e40af">área del viewBox</text>
  <text x="50" y="64" text-anchor="middle"
        font-size="5.5" fill="#64748b">franjas grises = letterboxing</text>
</svg>
```

---

## overflow en SVG

Controla si el contenido fuera del viewport es visible o cortado. Por defecto es `hidden`. Con `overflow="visible"` el contenido desbordante se renderiza.

```svg
<svg width="160" height="100" overflow="hidden"
     style="border:2px solid #ef4444;background:#f8fafc;display:block;">
  <!-- overflow="hidden" (por defecto): todo lo que sale del viewport se corta -->
  <circle cx="80"  cy="50" r="40" fill="#3b82f6" opacity="0.8"/>
  <circle cx="175" cy="50" r="40" fill="#ef4444" opacity="0.8"/>
  <text x="80" y="94" text-anchor="middle" font-size="9" fill="#64748b">
    overflow:hidden → círculo rojo invisible
  </text>
</svg>
```

```svg
<svg width="160" height="100" overflow="visible"
     style="border:2px solid #10b981;background:#f8fafc;
            display:block;margin:0 auto;">
  <!-- overflow="visible": el contenido fuera del viewport SÍ se renderiza -->
  <circle cx="80"  cy="50" r="40" fill="#3b82f6" opacity="0.8"/>
  <circle cx="175" cy="50" r="40" fill="#10b981" opacity="0.8"/>
  <text x="80" y="94" text-anchor="middle" font-size="9" fill="#64748b">
    overflow:visible → círculo verde visible ✓
  </text>
</svg>
```

---

## sistema de coordenadas local (transform)

Una transformación (`translate`, `rotate`, `scale`) en un `<g>` crea un sistema de coordenadas local para sus hijos. Los hijos usan coordenadas relativas al nuevo origen, no al (0,0) global.

```svg
<svg width="280" height="160"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Origen global -->
  <circle cx="0" cy="0" r="5" fill="tomato"/>
  <text x="5" y="14" font-size="9" fill="tomato">(0,0) global</text>

  <!-- Grupo con translate: crea nuevo origen local en (80, 80) -->
  <g transform="translate(80, 80)">
    <!-- Dentro de este grupo, (0,0) = punto (80,80) del SVG padre -->
    <circle cx="0"  cy="0"  r="30" fill="#3b82f6" opacity="0.2"/>
    <circle cx="0"  cy="0"  r="5"  fill="#3b82f6"/>
    <text x="5" y="-2" font-size="8" fill="#3b82f6">(0,0) local = (80,80)</text>
    <circle cx="40" cy="0"  r="12" fill="#10b981" opacity="0.8"/>
    <text x="40" y="4" text-anchor="middle" font-size="7" fill="white">
      (40,0)
    </text>
    <circle cx="0"  cy="40" r="12" fill="#f59e0b" opacity="0.8"/>
    <text x="0" y="44" text-anchor="middle" font-size="7" fill="white">
      (0,40)
    </text>
  </g>

  <!-- Segundo grupo en posición distinta -->
  <g transform="translate(200, 50)">
    <circle cx="0" cy="0" r="5" fill="#a855f7"/>
    <text x="5" y="-2" font-size="8" fill="#a855f7">(0,0) local = (200,50)</text>
    <circle cx="0" cy="0" r="28" fill="#a855f7" opacity="0.15"/>
  </g>
</svg>
```
