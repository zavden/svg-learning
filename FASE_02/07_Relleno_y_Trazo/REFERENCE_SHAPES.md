# Referencia — Relleno y Trazo

---

## Fill: valores y resultados

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">fill — valores</text>

  <!-- fill con nombre -->
  <rect x="10" y="32" width="52" height="38" fill="tomato"/>
  <text x="36" y="82" text-anchor="middle" font-size="7" fill="#64748b">nombre</text>
  <text x="36" y="91" text-anchor="middle" font-size="6.5" fill="#94a3b8">tomato</text>

  <!-- fill hex -->
  <rect x="72" y="32" width="52" height="38" fill="#3b82f6"/>
  <text x="98" y="82" text-anchor="middle" font-size="7" fill="#64748b">hex</text>
  <text x="98" y="91" text-anchor="middle" font-size="6.5" fill="#94a3b8">#3b82f6</text>

  <!-- fill none -->
  <rect x="134" y="32" width="52" height="38" fill="none" stroke="#94a3b8" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="160" y="82" text-anchor="middle" font-size="7" fill="#64748b">none</text>
  <text x="160" y="91" text-anchor="middle" font-size="6.5" fill="#94a3b8">transparente</text>

  <!-- fill con opacidad -->
  <rect x="196" y="27" width="52" height="48" fill="#1e293b"/>
  <rect x="196" y="32" width="52" height="38" fill="#f59e0b" fill-opacity="0.5"/>
  <text x="222" y="82" text-anchor="middle" font-size="7" fill="#64748b">fill-opacity</text>
  <text x="222" y="91" text-anchor="middle" font-size="6.5" fill="#94a3b8">0.5</text>

  <text x="150" y="108" text-anchor="middle" font-size="7.5" fill="#64748b">
    currentColor hereda la prop. CSS "color" del ancestro
  </text>
  <text x="150" y="120" text-anchor="middle" font-size="7" fill="#94a3b8">
    url(#id) referencia a gradiente o patrón
  </text>
</svg>
```

---

## Stroke: propiedades básicas

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">stroke — propiedades básicas</text>

  <!-- sin stroke -->
  <rect x="10" y="30" width="55" height="55" fill="#dbeafe"/>
  <text x="37" y="95" text-anchor="middle" font-size="7" fill="#94a3b8">sin stroke</text>

  <!-- stroke delgado -->
  <rect x="80" y="30" width="55" height="55" fill="#dbeafe" stroke="#3b82f6" stroke-width="1"/>
  <text x="107" y="95" text-anchor="middle" font-size="7" fill="#3b82f6">sw=1</text>

  <!-- stroke medio -->
  <rect x="150" y="30" width="55" height="55" fill="#dbeafe" stroke="#3b82f6" stroke-width="3"/>
  <text x="177" y="95" text-anchor="middle" font-size="7" fill="#3b82f6">sw=3</text>

  <!-- stroke grueso + opacity -->
  <rect x="220" y="30" width="55" height="55" fill="#dbeafe" stroke="#3b82f6" stroke-width="8" stroke-opacity="0.4"/>
  <text x="247" y="95" text-anchor="middle" font-size="7" fill="#3b82f6">sw=8 op=0.4</text>

  <!-- anotación centrado -->
  <text x="150" y="112" text-anchor="middle" font-size="7" fill="#94a3b8">
    el stroke se centra en el borde: sw/2 dentro + sw/2 fuera
  </text>
</svg>
```

---

## stroke-linecap — comparativa

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">stroke-linecap</text>

  <!-- Guías de alineación -->
  <line x1="50" y1="22" x2="50" y2="95" stroke="#fca5a5" stroke-width="0.5" stroke-dasharray="3,2"/>
  <line x1="200" y1="22" x2="200" y2="95" stroke="#fca5a5" stroke-width="0.5" stroke-dasharray="3,2"/>

  <!-- butt -->
  <line x1="50" y1="42" x2="200" y2="42" stroke="#3b82f6" stroke-width="14" stroke-linecap="butt"/>
  <text x="25" y="46" text-anchor="end" font-size="7.5" fill="#64748b">butt</text>
  <text x="235" y="46" font-size="7" fill="#94a3b8">plana, sin ext.</text>

  <!-- round -->
  <line x1="50" y1="68" x2="200" y2="68" stroke="#10b981" stroke-width="14" stroke-linecap="round"/>
  <text x="25" y="72" text-anchor="end" font-size="7.5" fill="#64748b">round</text>
  <text x="235" y="72" font-size="7" fill="#94a3b8">+7px redondeado</text>

  <!-- square -->
  <line x1="50" y1="94" x2="200" y2="94" stroke="#f59e0b" stroke-width="14" stroke-linecap="square"/>
  <text x="25" y="98" text-anchor="end" font-size="7.5" fill="#64748b">square</text>
  <text x="235" y="98" font-size="7" fill="#94a3b8">+7px cuadrado</text>
</svg>
```

---

## stroke-linejoin — comparativa

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">stroke-linejoin</text>

  <!-- miter -->
  <polyline points="20,85 50,35 80,85"
            stroke="#3b82f6" stroke-width="10" fill="none" stroke-linejoin="miter"/>
  <text x="50" y="100" text-anchor="middle" font-size="7.5" fill="#3b82f6">miter</text>

  <!-- round -->
  <polyline points="120,85 150,35 180,85"
            stroke="#10b981" stroke-width="10" fill="none" stroke-linejoin="round"/>
  <text x="150" y="100" text-anchor="middle" font-size="7.5" fill="#10b981">round</text>

  <!-- bevel -->
  <polyline points="220,85 250,35 280,85"
            stroke="#f59e0b" stroke-width="10" fill="none" stroke-linejoin="bevel"/>
  <text x="250" y="100" text-anchor="middle" font-size="7.5" fill="#f59e0b">bevel</text>
</svg>
```

---

## stroke-dasharray — galería de patrones

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">stroke-dasharray — patrones</text>

  <!-- sólida -->
  <line x1="10" y1="36" x2="220" y2="36" stroke="#3b82f6" stroke-width="3"/>
  <text x="228" y="40" font-size="7.5" fill="#64748b">sólida</text>

  <!-- 8 (trazo=espacio) -->
  <line x1="10" y1="55" x2="220" y2="55" stroke="#3b82f6" stroke-width="3" stroke-dasharray="8"/>
  <text x="228" y="59" font-size="7.5" fill="#64748b">"8"</text>

  <!-- 12,4 -->
  <line x1="10" y1="74" x2="220" y2="74" stroke="#3b82f6" stroke-width="3" stroke-dasharray="12,4"/>
  <text x="228" y="78" font-size="7.5" fill="#64748b">"12,4"</text>

  <!-- 2,6 punteada -->
  <line x1="10" y1="93" x2="220" y2="93" stroke="#10b981" stroke-width="3" stroke-dasharray="2,6"/>
  <text x="228" y="97" font-size="7.5" fill="#64748b">"2,6"</text>

  <!-- puntos redondos -->
  <line x1="10" y1="112" x2="220" y2="112"
        stroke="#f59e0b" stroke-width="6" stroke-dasharray="0,9" stroke-linecap="round"/>
  <text x="228" y="116" font-size="7.5" fill="#64748b">"0,9" round</text>

  <!-- patrón complejo -->
  <line x1="10" y1="131" x2="220" y2="131"
        stroke="#a855f7" stroke-width="3" stroke-dasharray="12,4,3,4"/>
  <text x="228" y="135" font-size="7.5" fill="#64748b">"12,4,3,4"</text>

  <!-- grueso round -->
  <line x1="10" y1="150" x2="220" y2="150"
        stroke="#ef4444" stroke-width="7" stroke-dasharray="5,10" stroke-linecap="round"/>
  <text x="228" y="154" font-size="7.5" fill="#64748b">"5,10" r</text>
</svg>
```

---

## Tabla de prioridad de estilos

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Cascada SVG — menor → mayor prioridad</text>

  <rect x="10" y="28" width="280" height="14" rx="2" fill="#f1f5f9"/>
  <text x="18" y="38" font-size="7" fill="#94a3b8">① Herencia del padre</text>

  <rect x="10" y="45" width="260" height="14" rx="2" fill="#dbeafe"/>
  <text x="18" y="55" font-size="7" fill="#3b82f6">② Atributos de presentación   fill="red"</text>

  <rect x="10" y="62" width="240" height="14" rx="2" fill="#dcfce7"/>
  <text x="18" y="72" font-size="7" fill="#15803d">③ Reglas CSS / clases   .clase { fill: red }</text>

  <rect x="10" y="79" width="220" height="14" rx="2" fill="#fef9c3"/>
  <text x="18" y="89" font-size="7" fill="#92400e">④ Style inline   style="fill: red;"</text>

  <rect x="10" y="96" width="200" height="14" rx="2" fill="#fecaca"/>
  <text x="18" y="106" font-size="7" fill="#dc2626">⑤ CSS !important</text>

  <text x="298" y="56" text-anchor="end" font-size="7" fill="#64748b">↑ más bajo</text>
  <text x="298" y="106" text-anchor="end" font-size="7" fill="#64748b">↑ más alto</text>
</svg>
```

---

## paint-order y vector-effect en una línea

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">paint-order · vector-effect</text>

  <!-- sin paint-order -->
  <text x="75" y="55" text-anchor="middle" font-size="28" font-weight="900"
        fill="#3b82f6" stroke="white" stroke-width="5">SVG</text>
  <text x="75" y="72" text-anchor="middle" font-size="7" fill="#ef4444">stroke encima (default)</text>

  <!-- con paint-order -->
  <text x="225" y="55" text-anchor="middle" font-size="28" font-weight="900"
        fill="#3b82f6" stroke="white" stroke-width="5"
        paint-order="stroke fill">SVG</text>
  <text x="225" y="72" text-anchor="middle" font-size="7" fill="#10b981">paint-order="stroke fill" ✓</text>

  <text x="150" y="92" text-anchor="middle" font-size="7" fill="#64748b">
    vector-effect="non-scaling-stroke" → stroke ignora transform scale
  </text>
  <text x="150" y="104" text-anchor="middle" font-size="6.5" fill="#475569">
    ideal para grids y diagramas con zoom
  </text>
</svg>
```

---

## Tabla resumen de atributos

| Atributo | Valores clave | Default |
|---|---|---|
| `fill` | color / `none` / `currentColor` / `url()` | `black` |
| `fill-opacity` | `0`–`1` | `1` |
| `fill-rule` | `nonzero` / `evenodd` | `nonzero` |
| `stroke` | color / `none` / `currentColor` | `none` |
| `stroke-width` | número (unidades usuario) | `1` |
| `stroke-opacity` | `0`–`1` | `1` |
| `stroke-linecap` | `butt` / `round` / `square` | `butt` |
| `stroke-linejoin` | `miter` / `round` / `bevel` | `miter` |
| `stroke-miterlimit` | número | `4` |
| `stroke-dasharray` | lista de números | `none` |
| `stroke-dashoffset` | número | `0` |
| `paint-order` | `fill stroke markers` (cualquier orden) | `fill stroke markers` |
| `vector-effect` | `none` / `non-scaling-stroke` | `none` |
