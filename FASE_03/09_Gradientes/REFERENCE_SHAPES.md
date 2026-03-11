# Referencia — Gradientes

---

## Estructura mínima de un gradiente

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <linearGradient id="min-linear" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>

    <radialGradient id="min-radial" cx="50%" cy="50%" r="50%">
      <stop offset="0%"   stop-color="#fbbf24"/>
      <stop offset="100%" stop-color="#ef4444"/>
    </radialGradient>
  </defs>

  <rect   x="20"  y="12" width="120" height="60" rx="6" fill="url(#min-linear)"/>
  <text   x="80"  y="82" text-anchor="middle" font-size="7" fill="#64748b">linearGradient</text>

  <circle cx="220" cy="42" r="35"              fill="url(#min-radial)"/>
  <text   x="220" y="82" text-anchor="middle" font-size="7" fill="#64748b">radialGradient</text>
</svg>
```

---

## Direcciones de linearGradient

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <linearGradient id="dir-h"  x1="0%"   y1="0%"   x2="100%" y2="0%">   <stop offset="0%" stop-color="#3b82f6"/><stop offset="100%" stop-color="#8b5cf6"/></linearGradient>
    <linearGradient id="dir-v"  x1="0%"   y1="0%"   x2="0%"   y2="100%"> <stop offset="0%" stop-color="#3b82f6"/><stop offset="100%" stop-color="#8b5cf6"/></linearGradient>
    <linearGradient id="dir-d1" x1="0%"   y1="0%"   x2="100%" y2="100%"> <stop offset="0%" stop-color="#3b82f6"/><stop offset="100%" stop-color="#8b5cf6"/></linearGradient>
    <linearGradient id="dir-d2" x1="0%"   y1="100%" x2="100%" y2="0%">   <stop offset="0%" stop-color="#3b82f6"/><stop offset="100%" stop-color="#8b5cf6"/></linearGradient>
    <linearGradient id="dir-a"  gradientTransform="rotate(30, 0.5, 0.5)"> <stop offset="0%" stop-color="#3b82f6"/><stop offset="100%" stop-color="#8b5cf6"/></linearGradient>
  </defs>

  <rect x="5"   y="10" width="52" height="55" rx="3" fill="url(#dir-h)"/>
  <text x="31"  y="76" text-anchor="middle" font-size="6.5" fill="#64748b">→ horiz</text>

  <rect x="62"  y="10" width="52" height="55" rx="3" fill="url(#dir-v)"/>
  <text x="88"  y="76" text-anchor="middle" font-size="6.5" fill="#64748b">↓ vert</text>

  <rect x="119" y="10" width="52" height="55" rx="3" fill="url(#dir-d1)"/>
  <text x="145" y="76" text-anchor="middle" font-size="6.5" fill="#64748b">↘ diag</text>

  <rect x="176" y="10" width="52" height="55" rx="3" fill="url(#dir-d2)"/>
  <text x="202" y="76" text-anchor="middle" font-size="6.5" fill="#64748b">↗ diag</text>

  <rect x="233" y="10" width="62" height="55" rx="3" fill="url(#dir-a)"/>
  <text x="264" y="76" text-anchor="middle" font-size="6.5" fill="#64748b">30° transform</text>

  <text x="150" y="92" text-anchor="middle" font-size="7" fill="#94a3b8">
    para ángulos arbitrarios: gradientTransform="rotate(deg, 0.5, 0.5)"
  </text>
</svg>
```

---

## spreadMethod — los tres valores

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <linearGradient id="sp-pad"     x1="25%" y1="0%" x2="75%" y2="0%" spreadMethod="pad">
      <stop offset="0%" stop-color="#ef4444"/><stop offset="100%" stop-color="#3b82f6"/></linearGradient>
    <linearGradient id="sp-reflect" x1="25%" y1="0%" x2="75%" y2="0%" spreadMethod="reflect">
      <stop offset="0%" stop-color="#ef4444"/><stop offset="100%" stop-color="#3b82f6"/></linearGradient>
    <linearGradient id="sp-repeat"  x1="25%" y1="0%" x2="75%" y2="0%" spreadMethod="repeat">
      <stop offset="0%" stop-color="#ef4444"/><stop offset="100%" stop-color="#3b82f6"/></linearGradient>
  </defs>

  <rect x="10" y="10" width="280" height="18" rx="3" fill="url(#sp-pad)"/>
  <text x="150" y="38" text-anchor="middle" font-size="7" fill="#64748b">pad — extiende color del borde</text>

  <rect x="10" y="44" width="280" height="18" rx="3" fill="url(#sp-reflect)"/>
  <text x="150" y="72" text-anchor="middle" font-size="7" fill="#64748b">reflect — espejo sin saltos</text>

  <rect x="10" y="78" width="280" height="18" rx="3" fill="url(#sp-repeat)"/>
  <text x="150" y="96" text-anchor="middle" font-size="7" fill="#64748b">repeat — reinicia (salto abrupto)</text>
</svg>
```

---

## Patrones útiles: overlay, esfera, fade-out

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Overlay sombra inferior -->
    <linearGradient id="shadow-ov" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%"   stop-color="black" stop-opacity="0"/>
      <stop offset="100%" stop-color="black" stop-opacity="0.6"/>
    </linearGradient>

    <!-- Esfera 3D -->
    <radialGradient id="sphere" cx="35%" cy="35%" r="55%" fx="30%" fy="28%">
      <stop offset="0%"   stop-color="white" stop-opacity="0.9"/>
      <stop offset="35%"  stop-color="#60a5fa"/>
      <stop offset="100%" stop-color="#1e3a8a"/>
    </radialGradient>

    <!-- Fade lateral (para texto con desbordamiento) -->
    <linearGradient id="fadeout-r" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="60%"  stop-color="#f8fafc" stop-opacity="0"/>
      <stop offset="100%" stop-color="#f8fafc" stop-opacity="1"/>
    </linearGradient>
  </defs>

  <!-- Overlay: imagen simulada + sombra -->
  <rect x="10"  y="10" width="80" height="80" rx="4" fill="#7dd3fc"/>
  <rect x="10"  y="10" width="80" height="80" rx="4" fill="url(#shadow-ov)"/>
  <text x="50"  y="100" text-anchor="middle" font-size="6.5" fill="#64748b">overlay sombra</text>

  <!-- Esfera -->
  <circle cx="150" cy="50" r="40" fill="url(#sphere)"/>
  <text x="150"  y="100" text-anchor="middle" font-size="6.5" fill="#64748b">esfera (fx/fy lateral)</text>

  <!-- Fade-out lateral -->
  <text x="215" y="55" font-size="9" fill="#334155">Texto largo que desborda</text>
  <rect x="210" y="40" width="80" height="30" fill="url(#fadeout-r)"/>
  <text x="250"  y="100" text-anchor="middle" font-size="6.5" fill="#64748b">fade-out lateral</text>
</svg>
```

---

## Tabla de atributos

| Atributo | Elemento | Valores | Default |
|---|---|---|---|
| `x1`, `y1`, `x2`, `y2` | `linearGradient` | `0%`–`100%` o `0`–`1` | `0%,0%,100%,0%` |
| `cx`, `cy`, `r` | `radialGradient` | `0%`–`100%` o `0`–`1` | `50%,50%,50%` |
| `fx`, `fy` | `radialGradient` | `0%`–`100%` | igual a cx/cy |
| `gradientUnits` | ambos | `objectBoundingBox` / `userSpaceOnUse` | `objectBoundingBox` |
| `spreadMethod` | ambos | `pad` / `reflect` / `repeat` | `pad` |
| `gradientTransform` | ambos | transform SVG (`rotate`, `scale`...) | — |
| `href` | ambos | `#id` | — |
| `offset` | `stop` | `0`–`1` o `0%`–`100%` | — |
| `stop-color` | `stop` | cualquier color CSS | `black` |
| `stop-opacity` | `stop` | `0`–`1` | `1` |
