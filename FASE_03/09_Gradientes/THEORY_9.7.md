# 9.7 Referenciar gradientes

Los gradientes se definen en `<defs>` y se usan con `url(#id)`. Se pueden reutilizar, heredar y aplicar en fill, stroke y texto.

---

## Uso en fill, stroke y CSS

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <linearGradient id="ref-grad" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <!-- fill con gradiente -->
  <rect x="10" y="15" width="80" height="55" rx="4" fill="url(#ref-grad)"/>
  <text x="50" y="82" text-anchor="middle" font-size="7" fill="#64748b">fill="url(#id)"</text>

  <!-- stroke con gradiente -->
  <rect x="110" y="15" width="80" height="55" rx="4"
        fill="none" stroke="url(#ref-grad)" stroke-width="5"/>
  <text x="150" y="82" text-anchor="middle" font-size="7" fill="#64748b">stroke="url(#id)"</text>

  <!-- gradiente en texto -->
  <text x="220" y="55" text-anchor="middle" font-size="32" font-weight="900"
        fill="url(#ref-grad)">Abc</text>
  <text x="220" y="82" text-anchor="middle" font-size="7" fill="#64748b">texto fill="url()"</text>

  <text x="150" y="100" text-anchor="middle" font-size="7.5" fill="#64748b">
    fill, stroke y texto aceptan url(#id) como valor de color
  </text>
</svg>
```

---

## Herencia con `href` — gradientes derivados

Un gradiente puede heredar los stops de otro y cambiar solo la dirección u otros atributos.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Gradiente base: define los stops y colores -->
    <linearGradient id="base-colors" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#ef4444"/>
      <stop offset="50%"  stop-color="#f97316"/>
      <stop offset="100%" stop-color="#fbbf24"/>
    </linearGradient>

    <!-- Variante horizontal (hereda stops, cambia solo la dirección) -->
    <linearGradient id="horiz" href="#base-colors"
                    x1="0%" y1="0%" x2="100%" y2="0%"/>

    <!-- Variante vertical (mismos colores, dirección vertical) -->
    <linearGradient id="vert" href="#base-colors"
                    x1="0%" y1="0%" x2="0%" y2="100%"/>

    <!-- Variante diagonal (mismos colores, diagonal) -->
    <linearGradient id="diag" href="#base-colors"
                    gradientTransform="rotate(45, 0.5, 0.5)"/>
  </defs>

  <rect x="10"  y="15" width="80" height="70" rx="4" fill="url(#horiz)"/>
  <text x="50"  y="97" text-anchor="middle" font-size="7" fill="#64748b">horizontal</text>

  <rect x="110" y="15" width="80" height="70" rx="4" fill="url(#vert)"/>
  <text x="150" y="97" text-anchor="middle" font-size="7" fill="#64748b">vertical</text>

  <rect x="210" y="15" width="80" height="70" rx="4" fill="url(#diag)"/>
  <text x="250" y="97" text-anchor="middle" font-size="7" fill="#64748b">diagonal 45°</text>

  <text x="150" y="112" text-anchor="middle" font-size="7.5" fill="#64748b">
    href="#base-colors" — los tres usan los mismos stops
  </text>
</svg>
```

---

## Gradiente en texto — `gradientUnits` importa

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <defs>
    <!-- objectBoundingBox: el gradiente se ajusta al bounding box del texto -->
    <linearGradient id="text-obb" gradientUnits="objectBoundingBox"
                    x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%"   stop-color="#60a5fa"/>
      <stop offset="100%" stop-color="#c084fc"/>
    </linearGradient>

    <!-- userSpaceOnUse: posición absoluta en el viewport -->
    <linearGradient id="text-usu" gradientUnits="userSpaceOnUse"
                    x1="10" y1="0" x2="290" y2="0">
      <stop offset="0%"   stop-color="#60a5fa"/>
      <stop offset="100%" stop-color="#c084fc"/>
    </linearGradient>
  </defs>

  <!-- objectBoundingBox: gradiente va de inicio a fin del texto -->
  <text x="150" y="45" text-anchor="middle" font-size="30" font-weight="900"
        fill="url(#text-obb)">Gradiente</text>
  <text x="150" y="60" text-anchor="middle" font-size="7" fill="#475569">
    objectBoundingBox — se adapta al texto
  </text>

  <!-- userSpaceOnUse: gradiente fijo de x=10 a x=290 -->
  <text x="150" y="90" text-anchor="middle" font-size="30" font-weight="900"
        fill="url(#text-usu)">Gradiente</text>
  <text x="150" y="107" text-anchor="middle" font-size="7" fill="#475569">
    userSpaceOnUse — gradiente fijo en el viewport
  </text>
</svg>
```

---

## Gradiente aplicado en CSS

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <linearGradient id="css-grad" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#10b981"/>
      <stop offset="100%" stop-color="#3b82f6"/>
    </linearGradient>

    <style>
      .with-gradient {
        fill: url(#css-grad);
      }
    </style>
  </defs>

  <rect x="20" y="12" width="120" height="50" rx="4" class="with-gradient"/>
  <rect x="160" y="12" width="120" height="50" rx="4" class="with-gradient"/>

  <text x="150" y="76" text-anchor="middle" font-size="7.5" fill="#64748b">
    fill: url(#id) — también funciona en CSS con clases
  </text>
</svg>
```
