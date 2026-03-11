# 7.1 Propiedades de relleno (`fill`)

El **fill** ocupa el interior de una forma. Por defecto es `black` en todas las formas SVG — lo que sorprende a muchos principiantes.

---

## El fill por defecto es `black`

Sin `fill` explícito, todas las formas son negras.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Sin fill explícito → negro -->
  <rect x="10" y="20" width="60" height="60"/>
  <circle cx="110" cy="50" r="30"/>
  <polygon points="165,20 195,80 135,80"/>

  <text x="110" y="96" text-anchor="middle" font-size="7.5" fill="#ef4444">
    sin fill → negro por defecto
  </text>

  <!-- Con fill explícito -->
  <rect x="210" y="20" width="60" height="60" fill="#3b82f6"/>
  <text x="240" y="96" text-anchor="middle" font-size="7.5" fill="#3b82f6">
    fill explícito ✓
  </text>
</svg>
```

---

## Valores de `fill`

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Nombre CSS -->
  <rect x="10" y="10" width="50" height="50" fill="tomato"/>
  <text x="35" y="72" text-anchor="middle" font-size="7" fill="#64748b">nombre</text>
  <text x="35" y="82" text-anchor="middle" font-size="6.5" fill="#94a3b8">"tomato"</text>

  <!-- Hex -->
  <rect x="75" y="10" width="50" height="50" fill="#3b82f6"/>
  <text x="100" y="72" text-anchor="middle" font-size="7" fill="#64748b">hex</text>
  <text x="100" y="82" text-anchor="middle" font-size="6.5" fill="#94a3b8">"#3b82f6"</text>

  <!-- none -->
  <rect x="140" y="10" width="50" height="50" fill="none" stroke="#94a3b8" stroke-width="1.5" stroke-dasharray="3,2"/>
  <text x="165" y="72" text-anchor="middle" font-size="7" fill="#64748b">none</text>
  <text x="165" y="82" text-anchor="middle" font-size="6.5" fill="#94a3b8">transparente</text>

  <!-- currentColor -->
  <rect x="205" y="10" width="50" height="50" fill="currentColor" style="color:#a855f7"/>
  <text x="230" y="72" text-anchor="middle" font-size="7" fill="#64748b">currentColor</text>
  <text x="230" y="82" text-anchor="middle" font-size="6.5" fill="#94a3b8">hereda CSS color</text>

  <!-- url(#id) — gradiente -->
  <defs>
    <linearGradient id="grad71" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#a855f7"/>
    </linearGradient>
  </defs>
  <rect x="10" y="100" width="280" height="40" fill="url(#grad71)" rx="6"/>
  <text x="150" y="126" text-anchor="middle" font-size="9" fill="white" font-weight="600">fill="url(#grad71)"</text>
</svg>
```

---

## `fill-opacity` — transparencia del relleno

Controla solo la opacidad del fill, sin afectar al stroke.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Fondo de referencia con patrón de damero -->
  <pattern id="checker" x="0" y="0" width="10" height="10" patternUnits="userSpaceOnUse">
    <rect width="5" height="5" fill="#e2e8f0"/>
    <rect x="5" y="5" width="5" height="5" fill="#e2e8f0"/>
  </pattern>
  <rect width="300" height="80" fill="url(#checker)"/>

  <!-- fill-opacity variante -->
  <rect x="10"  y="15" width="50" height="50" fill="#3b82f6" fill-opacity="1.0"  stroke="#1d4ed8" stroke-width="2"/>
  <rect x="75"  y="15" width="50" height="50" fill="#3b82f6" fill-opacity="0.75" stroke="#1d4ed8" stroke-width="2"/>
  <rect x="140" y="15" width="50" height="50" fill="#3b82f6" fill-opacity="0.5"  stroke="#1d4ed8" stroke-width="2"/>
  <rect x="205" y="15" width="50" height="50" fill="#3b82f6" fill-opacity="0.2"  stroke="#1d4ed8" stroke-width="2"/>

  <text x="35"  y="92" text-anchor="middle" font-size="7.5" fill="#64748b">1.0</text>
  <text x="100" y="92" text-anchor="middle" font-size="7.5" fill="#64748b">0.75</text>
  <text x="165" y="92" text-anchor="middle" font-size="7.5" fill="#64748b">0.5</text>
  <text x="230" y="92" text-anchor="middle" font-size="7.5" fill="#64748b">0.2</text>

  <text x="150" y="105" text-anchor="middle" font-size="7.5" fill="#64748b">
    fill-opacity: el stroke permanece opaco
  </text>
</svg>
```

---

## `fill="none"` vs `fill="transparent"`

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">none vs transparent</text>

  <!-- none: sin relleno, no responde a eventos en el área interior -->
  <rect x="20" y="32" width="110" height="55" fill="none" stroke="#3b82f6" stroke-width="2"/>
  <text x="75" y="57" text-anchor="middle" font-size="9" fill="#64748b">fill="none"</text>
  <text x="75" y="72" text-anchor="middle" font-size="7" fill="#94a3b8">click en interior: no</text>
  <text x="75" y="83" text-anchor="middle" font-size="7" fill="#94a3b8">(sin área de hit)</text>

  <!-- transparent: invisible pero responde a eventos -->
  <rect x="170" y="32" width="110" height="55" fill="transparent" stroke="#10b981" stroke-width="2"/>
  <text x="225" y="57" text-anchor="middle" font-size="9" fill="#64748b">fill="transparent"</text>
  <text x="225" y="72" text-anchor="middle" font-size="7" fill="#94a3b8">click en interior: sí</text>
  <text x="225" y="83" text-anchor="middle" font-size="7" fill="#94a3b8">(área de hit activa)</text>
</svg>
```
