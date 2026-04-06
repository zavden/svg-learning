# 14.2 Primitivas básicas

Las primitivas son las operaciones atómicas de un filtro. Esta sección cubre las más importantes: `feGaussianBlur`, `feOffset`, `feFlood`, `feComposite`, `feMerge`, `feColorMatrix` y `feDropShadow`.

---

## `<feGaussianBlur>` — desenfoque

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feGaussianBlur: stdDeviation</text>

  <defs>
    <filter id="blur-1"><feGaussianBlur stdDeviation="0"/></filter>
    <filter id="blur-2"><feGaussianBlur stdDeviation="2"/></filter>
    <filter id="blur-5"><feGaussianBlur stdDeviation="5"/></filter>
    <filter id="blur-xy"><feGaussianBlur stdDeviation="6 1"/></filter>
  </defs>

  <circle cx="45"  cy="75" r="25" fill="#ec4899" filter="url(#blur-1)"/>
  <text x="45"  y="120" text-anchor="middle" font-size="7" fill="#64748b">stdDeviation=0</text>
  <text x="45"  y="132" text-anchor="middle" font-size="6" fill="#94a3b8">sin blur</text>

  <circle cx="120" cy="75" r="25" fill="#ec4899" filter="url(#blur-2)"/>
  <text x="120" y="120" text-anchor="middle" font-size="7" fill="#64748b">stdDeviation=2</text>
  <text x="120" y="132" text-anchor="middle" font-size="6" fill="#94a3b8">suave</text>

  <circle cx="195" cy="75" r="25" fill="#ec4899" filter="url(#blur-5)"/>
  <text x="195" y="120" text-anchor="middle" font-size="7" fill="#64748b">stdDeviation=5</text>
  <text x="195" y="132" text-anchor="middle" font-size="6" fill="#94a3b8">marcado</text>

  <circle cx="265" cy="75" r="25" fill="#ec4899" filter="url(#blur-xy)"/>
  <text x="265" y="120" text-anchor="middle" font-size="7" fill="#64748b">"6 1"</text>
  <text x="265" y="132" text-anchor="middle" font-size="6" fill="#94a3b8">X≠Y</text>

  <text x="150" y="152" text-anchor="middle" font-size="7" fill="#94a3b8">
    un solo valor = X e Y iguales; dos valores = horizontal y vertical distintos
  </text>
</svg>
```

---

## `<feOffset>` — desplazamiento

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feOffset: dx, dy</text>

  <defs>
    <filter id="off-1" x="-50%" y="-50%" width="200%" height="200%">
      <feOffset dx="10" dy="5"/>
    </filter>
    <filter id="off-2" x="-50%" y="-50%" width="200%" height="200%">
      <feOffset dx="-8" dy="-10"/>
    </filter>
  </defs>

  <!-- Referencia del original -->
  <circle cx="80" cy="80" r="25" fill="#94a3b8" opacity="0.35"/>
  <circle cx="80" cy="80" r="25" fill="#3b82f6" filter="url(#off-1)"/>
  <text x="80" y="130" text-anchor="middle" font-size="7" fill="#64748b">dx=10 dy=5</text>

  <circle cx="220" cy="80" r="25" fill="#94a3b8" opacity="0.35"/>
  <circle cx="220" cy="80" r="25" fill="#3b82f6" filter="url(#off-2)"/>
  <text x="220" y="130" text-anchor="middle" font-size="7" fill="#64748b">dx=-8 dy=-10</text>

  <text x="150" y="145" text-anchor="middle" font-size="7" fill="#94a3b8">
    solo desplaza — no cambia los pixels
  </text>
</svg>
```

---

## `<feColorMatrix>` — transformaciones de color

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feColorMatrix: saturate, hueRotate</text>

  <defs>
    <filter id="cm-gris"><feColorMatrix type="saturate" values="0"/></filter>
    <filter id="cm-hiper"><feColorMatrix type="saturate" values="3"/></filter>
    <filter id="cm-hue90"><feColorMatrix type="hueRotate" values="90"/></filter>
    <filter id="cm-hue180"><feColorMatrix type="hueRotate" values="180"/></filter>
    <filter id="cm-hue270"><feColorMatrix type="hueRotate" values="270"/></filter>

    <linearGradient id="cm-g" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#ec4899"/>
      <stop offset="0.5" stop-color="#f59e0b"/>
      <stop offset="1" stop-color="#3b82f6"/>
    </linearGradient>
  </defs>

  <!-- Fila 1: saturación -->
  <rect x="15" y="35" width="60" height="50" fill="url(#cm-g)" rx="4"/>
  <text x="45" y="100" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <rect x="85" y="35" width="60" height="50" fill="url(#cm-g)" rx="4" filter="url(#cm-gris)"/>
  <text x="115" y="100" text-anchor="middle" font-size="7" fill="#64748b">saturate=0</text>
  <text x="115" y="110" text-anchor="middle" font-size="6" fill="#94a3b8">grises</text>

  <rect x="155" y="35" width="60" height="50" fill="url(#cm-g)" rx="4" filter="url(#cm-hiper)"/>
  <text x="185" y="100" text-anchor="middle" font-size="7" fill="#64748b">saturate=3</text>
  <text x="185" y="110" text-anchor="middle" font-size="6" fill="#94a3b8">hipersaturado</text>

  <rect x="225" y="35" width="60" height="50" fill="url(#cm-g)" rx="4" filter="url(#cm-hue90)"/>
  <text x="255" y="100" text-anchor="middle" font-size="7" fill="#64748b">hueRotate 90</text>

  <!-- Fila 2: hueRotate -->
  <rect x="15" y="125" width="60" height="50" fill="url(#cm-g)" rx="4" filter="url(#cm-hue180)"/>
  <text x="45" y="185" text-anchor="middle" font-size="7" fill="#64748b">hueRotate 180</text>

  <rect x="85" y="125" width="60" height="50" fill="url(#cm-g)" rx="4" filter="url(#cm-hue270)"/>
  <text x="115" y="185" text-anchor="middle" font-size="7" fill="#64748b">hueRotate 270</text>

  <!-- Explicación de los valores -->
  <g transform="translate(155, 125)">
    <rect x="0" y="0" width="130" height="50" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
    <text x="65" y="14" text-anchor="middle" font-size="7" font-weight="bold" fill="#92400e">saturate</text>
    <text x="65" y="25" text-anchor="middle" font-size="6" fill="#64748b">0 = grises</text>
    <text x="65" y="34" text-anchor="middle" font-size="6" fill="#64748b">1 = normal</text>
    <text x="65" y="43" text-anchor="middle" font-size="6" fill="#64748b">&gt;1 = hipersaturado</text>
  </g>
</svg>
```

---

## `<feFlood>` + `<feComposite>` — colorear una silueta

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">flood + composite: teñir la silueta</text>

  <defs>
    <!-- Crea una imagen rosa (flood) y la recorta a la silueta del elemento (SourceAlpha) -->
    <filter id="tint-rosa">
      <feFlood flood-color="#ec4899" result="color"/>
      <feComposite in="color" in2="SourceAlpha" operator="in"/>
    </filter>
  </defs>

  <!-- Un "personaje" con colores -->
  <g>
    <circle cx="75" cy="70" r="18" fill="#fef9c3" stroke="#92400e" stroke-width="2"/>
    <rect x="57" y="88" width="36" height="30" fill="#3b82f6" stroke="#1e40af" stroke-width="2"/>
  </g>
  <text x="75" y="140" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <!-- Mismo grupo con el filtro: todo se vuelve rosa sólido -->
  <g filter="url(#tint-rosa)">
    <circle cx="215" cy="70" r="18" fill="#fef9c3" stroke="#92400e" stroke-width="2"/>
    <rect x="197" y="88" width="36" height="30" fill="#3b82f6" stroke="#1e40af" stroke-width="2"/>
  </g>
  <text x="215" y="140" text-anchor="middle" font-size="7" fill="#64748b">flood + composite "in"</text>

  <text x="150" y="165" text-anchor="middle" font-size="7" fill="#94a3b8">
    feFlood genera un rectángulo de color — feComposite lo recorta
  </text>
</svg>
```

---

## `<feMerge>` — apilar capas

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feMerge: sombra + original apilados</text>

  <defs>
    <filter id="sombra-merge" x="-30%" y="-30%" width="160%" height="160%">
      <!-- Capa 1: silueta desenfocada y desplazada -->
      <feGaussianBlur in="SourceAlpha" stdDeviation="3" result="blur"/>
      <feOffset in="blur" dx="3" dy="4" result="sombra"/>
      <!-- Capa 2: el original encima -->
      <feMerge>
        <feMergeNode in="sombra"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>

  <!-- Demostración visual de las dos capas -->
  <g transform="translate(30, 45)">
    <text x="55" y="-8" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">sombra sola</text>
    <circle cx="55" cy="45" r="25" fill="#000" opacity="0.4" filter="url(#only-blur)"/>
  </g>
  <defs>
    <filter id="only-blur"><feGaussianBlur stdDeviation="3"/></filter>
  </defs>

  <g transform="translate(135, 45)">
    <text x="55" y="-8" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">original</text>
    <circle cx="55" cy="45" r="25" fill="#10b981"/>
  </g>

  <g transform="translate(240, 45)">
    <text x="30" y="-8" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">apilado</text>
    <circle cx="30" cy="45" r="25" fill="#10b981" filter="url(#sombra-merge)"/>
  </g>

  <!-- Flechas -->
  <text x="112" y="75" font-size="16" fill="#64748b">+</text>
  <text x="220" y="75" font-size="16" fill="#64748b">=</text>

  <text x="150" y="155" text-anchor="middle" font-size="7" fill="#94a3b8">
    feMerge combina 2+ imágenes en capas (la primera queda abajo)
  </text>
</svg>
```

---

## `<feDropShadow>` — sombra en una línea

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feDropShadow: sombra directa</text>

  <defs>
    <filter id="ds-sutil" x="-30%" y="-30%" width="160%" height="160%">
      <feDropShadow dx="1" dy="2" stdDeviation="1.5" flood-color="#000" flood-opacity="0.25"/>
    </filter>
    <filter id="ds-media" x="-30%" y="-30%" width="160%" height="160%">
      <feDropShadow dx="3" dy="5" stdDeviation="3" flood-color="#000" flood-opacity="0.35"/>
    </filter>
    <filter id="ds-fuerte" x="-50%" y="-50%" width="200%" height="200%">
      <feDropShadow dx="5" dy="8" stdDeviation="5" flood-color="#000" flood-opacity="0.45"/>
    </filter>
    <filter id="ds-color" x="-50%" y="-50%" width="200%" height="200%">
      <feDropShadow dx="2" dy="4" stdDeviation="4" flood-color="#ec4899" flood-opacity="0.6"/>
    </filter>
  </defs>

  <rect x="20"  y="45" width="55" height="55" rx="6" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5" filter="url(#ds-sutil)"/>
  <text x="47"  y="120" text-anchor="middle" font-size="7" fill="#64748b">sutil</text>

  <rect x="95"  y="45" width="55" height="55" rx="6" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5" filter="url(#ds-media)"/>
  <text x="122" y="120" text-anchor="middle" font-size="7" fill="#64748b">media</text>

  <rect x="170" y="45" width="55" height="55" rx="6" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5" filter="url(#ds-fuerte)"/>
  <text x="197" y="120" text-anchor="middle" font-size="7" fill="#64748b">fuerte</text>

  <rect x="245" y="45" width="55" height="55" rx="6" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5" filter="url(#ds-color)"/>
  <text x="272" y="120" text-anchor="middle" font-size="7" fill="#64748b">color</text>

  <text x="150" y="145" text-anchor="middle" font-size="7" fill="#94a3b8">
    dx, dy, stdDeviation, flood-color, flood-opacity
  </text>
  <text x="150" y="160" text-anchor="middle" font-size="7" fill="#94a3b8">
    equivale a blur + offset + flood + composite + merge en un solo paso
  </text>
</svg>
```

---

## `feComponentTransfer` para tintar sombras

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">sombra interior (inset) con composite</text>

  <defs>
    <filter id="inset-shadow">
      <!-- Sombra interior: invertir la silueta, desenfocar y recortar al interior del elemento -->
      <feGaussianBlur in="SourceAlpha" stdDeviation="4"/>
      <feOffset dx="2" dy="3" result="offsetblur"/>
      <feFlood flood-color="#000" flood-opacity="0.6"/>
      <feComposite in2="offsetblur" operator="out"/>
      <feComposite in2="SourceGraphic" operator="in"/>
      <feMerge>
        <feMergeNode in="SourceGraphic"/>
        <feMergeNode/>
      </feMerge>
    </filter>
  </defs>

  <!-- Sin filtro -->
  <rect x="30" y="45" width="90" height="70" rx="8" fill="#dbeafe"/>
  <text x="75" y="135" text-anchor="middle" font-size="7" fill="#64748b">sin inset shadow</text>

  <!-- Con inset shadow -->
  <rect x="180" y="45" width="90" height="70" rx="8" fill="#dbeafe" filter="url(#inset-shadow)"/>
  <text x="225" y="135" text-anchor="middle" font-size="7" fill="#64748b">con inset shadow</text>

  <text x="150" y="150" text-anchor="middle" font-size="7" fill="#94a3b8">
    composite "out" + "in" = recortar la sombra al interior
  </text>
</svg>
```
