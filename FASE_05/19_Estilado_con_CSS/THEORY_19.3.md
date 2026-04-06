# 19.3 Propiedades CSS específicas de SVG

Hay un grupo de propiedades CSS que **solo existen en SVG**: no las puedes usar en `<div>` o `<button>`. Son las que controlan el "pintado vectorial": relleno, trazo, marcadores, alineación de texto sobre líneas base, recorte, máscara y efectos.

---

## Propiedades de relleno

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    fill, fill-opacity, fill-rule
  </text>

  <!-- fill -->
  <rect x="20" y="40" width="60" height="60" fill="#3b82f6"/>
  <text x="50" y="115" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">fill: #3b82f6</text>
  <text x="50" y="124" text-anchor="middle" font-size="6" fill="#94a3b8">color de relleno</text>

  <!-- fill-opacity -->
  <rect x="120" y="40" width="60" height="60" fill="#3b82f6" fill-opacity="0.4"/>
  <text x="150" y="115" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">fill-opacity: 0.4</text>
  <text x="150" y="124" text-anchor="middle" font-size="6" fill="#94a3b8">opacidad del fill</text>

  <!-- fill-rule -->
  <path d="M220,40 L280,40 L280,100 L220,100 Z M235,55 L265,55 L265,85 L235,85 Z"
        fill="#3b82f6" fill-rule="evenodd"/>
  <text x="250" y="115" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">fill-rule: evenodd</text>
  <text x="250" y="124" text-anchor="middle" font-size="6" fill="#94a3b8">rellena solo el anillo</text>

  <rect x="15" y="138" width="270" height="74" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="8" font-weight="bold" fill="#94a3b8">notas</text>
  <text x="25" y="170" font-size="7" fill="#e2e8f0">• fill acepta cualquier &lt;color&gt; CSS, gradientes y url(#patrón)</text>
  <text x="25" y="184" font-size="7" fill="#e2e8f0">• fill-opacity es independiente de opacity (que afecta a todo)</text>
  <text x="25" y="198" font-size="7" fill="#e2e8f0">• fill-rule: nonzero (defecto) | evenodd — afecta paths con huecos</text>
</svg>
```

---

## Propiedades de trazo

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    stroke + sus modificadores
  </text>

  <!-- stroke-width -->
  <line x1="30" y1="50" x2="270" y2="50" stroke="#3b82f6" stroke-width="1"/>
  <line x1="30" y1="62" x2="270" y2="62" stroke="#3b82f6" stroke-width="3"/>
  <line x1="30" y1="76" x2="270" y2="76" stroke="#3b82f6" stroke-width="6"/>
  <text x="280" y="68" font-size="6" fill="#94a3b8">stroke-width</text>

  <!-- stroke-linecap -->
  <line x1="30" y1="100" x2="100" y2="100" stroke="#10b981" stroke-width="8" stroke-linecap="butt"/>
  <text x="105" y="103" font-size="6" font-family="monospace" fill="#475569">butt</text>
  <line x1="135" y1="100" x2="200" y2="100" stroke="#10b981" stroke-width="8" stroke-linecap="round"/>
  <text x="205" y="103" font-size="6" font-family="monospace" fill="#475569">round</text>
  <line x1="230" y1="100" x2="270" y2="100" stroke="#10b981" stroke-width="8" stroke-linecap="square"/>
  <text x="275" y="103" font-size="6" font-family="monospace" fill="#475569">square</text>

  <!-- stroke-linejoin -->
  <polyline points="30,135 50,118 70,135" fill="none" stroke="#f59e0b" stroke-width="6" stroke-linejoin="miter"/>
  <text x="35" y="150" font-size="6" font-family="monospace" fill="#475569">miter</text>
  <polyline points="100,135 120,118 140,135" fill="none" stroke="#f59e0b" stroke-width="6" stroke-linejoin="round"/>
  <text x="105" y="150" font-size="6" font-family="monospace" fill="#475569">round</text>
  <polyline points="170,135 190,118 210,135" fill="none" stroke="#f59e0b" stroke-width="6" stroke-linejoin="bevel"/>
  <text x="175" y="150" font-size="6" font-family="monospace" fill="#475569">bevel</text>

  <!-- stroke-dasharray -->
  <line x1="30" y1="170" x2="270" y2="170" stroke="#ec4899" stroke-width="3" stroke-dasharray="5 3"/>
  <text x="30" y="184" font-size="6" font-family="monospace" fill="#475569">stroke-dasharray: 5 3</text>

  <line x1="30" y1="200" x2="270" y2="200" stroke="#ec4899" stroke-width="3" stroke-dasharray="10 5 2 5"/>
  <text x="30" y="214" font-size="6" font-family="monospace" fill="#475569">stroke-dasharray: 10 5 2 5</text>

  <!-- Lista resumen -->
  <rect x="15" y="225" width="270" height="50" fill="#0f172a" rx="3"/>
  <text x="25" y="240" font-size="7" fill="#e2e8f0">stroke, stroke-width, stroke-opacity</text>
  <text x="25" y="252" font-size="7" fill="#e2e8f0">stroke-linecap, stroke-linejoin, stroke-miterlimit</text>
  <text x="25" y="264" font-size="7" fill="#e2e8f0">stroke-dasharray, stroke-dashoffset</text>
</svg>
```

---

## Propiedades de texto SVG

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    text-anchor y dominant-baseline
  </text>

  <!-- text-anchor -->
  <line x1="150" y1="40" x2="150" y2="120" stroke="#cbd5e1" stroke-dasharray="2 2"/>

  <text x="150" y="58" text-anchor="start" font-size="11" fill="#3b82f6">start</text>
  <text x="150" y="80" text-anchor="middle" font-size="11" fill="#10b981">middle</text>
  <text x="150" y="102" text-anchor="end" font-size="11" fill="#ec4899">end</text>

  <text x="20" y="58" font-size="7" font-family="monospace" fill="#475569">text-anchor:</text>
  <text x="20" y="80" font-size="7" font-family="monospace" fill="#475569">  defaults a "start"</text>
  <text x="20" y="102" font-size="7" font-family="monospace" fill="#475569">  controla alineación X</text>

  <!-- dominant-baseline -->
  <line x1="20" y1="160" x2="280" y2="160" stroke="#cbd5e1" stroke-dasharray="2 2"/>
  <text x="40" y="160" text-anchor="middle" dominant-baseline="alphabetic" font-size="10" fill="#3b82f6">Aa</text>
  <text x="40" y="172" text-anchor="middle" font-size="6" fill="#94a3b8">alphabetic</text>

  <text x="100" y="160" text-anchor="middle" dominant-baseline="middle" font-size="10" fill="#10b981">Aa</text>
  <text x="100" y="172" text-anchor="middle" font-size="6" fill="#94a3b8">middle</text>

  <text x="160" y="160" text-anchor="middle" dominant-baseline="hanging" font-size="10" fill="#f59e0b">Aa</text>
  <text x="160" y="180" text-anchor="middle" font-size="6" fill="#94a3b8">hanging</text>

  <text x="220" y="160" text-anchor="middle" dominant-baseline="text-top" font-size="10" fill="#ec4899">Aa</text>
  <text x="220" y="180" text-anchor="middle" font-size="6" fill="#94a3b8">text-top</text>

  <rect x="15" y="195" width="270" height="38" fill="#0f172a" rx="3"/>
  <text x="25" y="211" font-size="7" fill="#e2e8f0">text-anchor → eje X (alineación horizontal)</text>
  <text x="25" y="223" font-size="7" fill="#e2e8f0">dominant-baseline → eje Y (línea base vertical)</text>
</svg>
```

---

## Marcadores y efectos

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    marker-*, vector-effect, paint-order
  </text>

  <defs>
    <marker id="flecha" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto">
      <path d="M0,0 L6,3 L0,6 z" fill="#3b82f6"/>
    </marker>
    <marker id="punto" markerWidth="6" markerHeight="6" refX="3" refY="3">
      <circle cx="3" cy="3" r="2" fill="#10b981"/>
    </marker>
  </defs>

  <!-- marker-end -->
  <line x1="30" y1="48" x2="200" y2="48" stroke="#1e293b" stroke-width="2" marker-end="url(#flecha)"/>
  <text x="210" y="51" font-size="7" font-family="monospace" fill="#475569">marker-end</text>

  <!-- marker-mid -->
  <polyline points="30,72 80,72 130,72 180,72" fill="none" stroke="#1e293b" stroke-width="2"
            marker-mid="url(#punto)"/>
  <text x="190" y="76" font-size="7" font-family="monospace" fill="#475569">marker-mid</text>

  <!-- vector-effect: non-scaling-stroke -->
  <g transform="scale(2 0.5) translate(15 200)">
    <rect x="0" y="0" width="60" height="40" fill="none" stroke="#ec4899" stroke-width="2"
          vector-effect="non-scaling-stroke"/>
  </g>
  <text x="150" y="125" font-size="7" font-family="monospace" fill="#475569">non-scaling-stroke (no se deforma)</text>

  <!-- paint-order -->
  <text x="60" y="170" font-size="20" font-weight="bold" fill="#fbbf24" stroke="#1e293b" stroke-width="3"
        paint-order="stroke fill">Hi</text>
  <text x="100" y="172" font-size="7" font-family="monospace" fill="#475569">paint-order:</text>
  <text x="100" y="184" font-size="7" font-family="monospace" fill="#475569">  stroke fill</text>
  <text x="100" y="196" font-size="6" fill="#94a3b8">trazo debajo del relleno</text>

  <rect x="15" y="208" width="270" height="44" fill="#0f172a" rx="3"/>
  <text x="25" y="223" font-size="7" fill="#e2e8f0">marker-start, marker-mid, marker-end, marker</text>
  <text x="25" y="236" font-size="7" fill="#e2e8f0">vector-effect, paint-order, color-interpolation</text>
  <text x="25" y="248" font-size="7" fill="#e2e8f0">shape-rendering, text-rendering, image-rendering</text>
</svg>
```

---

## Recorte, máscara y filtros

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    clip-path, mask, filter
  </text>

  <defs>
    <clipPath id="recorte">
      <circle cx="50" cy="80" r="32"/>
    </clipPath>
    <mask id="mascara">
      <rect x="0" y="0" width="100" height="100" fill="white"/>
      <text x="50" y="60" text-anchor="middle" font-size="40" fill="black">M</text>
    </mask>
    <filter id="desenfoque">
      <feGaussianBlur stdDeviation="2"/>
    </filter>
  </defs>

  <!-- clip-path -->
  <rect x="18" y="48" width="64" height="64" fill="#3b82f6" clip-path="url(#recorte)"/>
  <text x="50" y="125" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">clip-path</text>
  <text x="50" y="135" text-anchor="middle" font-size="6" fill="#94a3b8">recorta a una forma</text>

  <!-- mask -->
  <rect x="118" y="48" width="64" height="64" fill="#10b981" mask="url(#mascara)"/>
  <text x="150" y="125" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">mask</text>
  <text x="150" y="135" text-anchor="middle" font-size="6" fill="#94a3b8">opacidad por luminancia</text>

  <!-- filter -->
  <rect x="218" y="48" width="64" height="64" fill="#ec4899" filter="url(#desenfoque)"/>
  <text x="250" y="125" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">filter</text>
  <text x="250" y="135" text-anchor="middle" font-size="6" fill="#94a3b8">efectos (blur, drop-shadow…)</text>

  <rect x="15" y="148" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="164" font-size="8" font-weight="bold" fill="#94a3b8">clave</text>
  <text x="25" y="180" font-size="7" fill="#e2e8f0">clip-path: forma binaria (dentro/fuera)</text>
  <text x="25" y="194" font-size="7" fill="#e2e8f0">mask: gradiente de opacidad (gris = parcial)</text>
  <text x="25" y="208" font-size="7" fill="#e2e8f0">filter: cualquier efecto SVG o función CSS</text>
  <text x="25" y="220" font-size="7" fill="#fbbf24">también disponibles vía CSS (no solo atributo)</text>
</svg>
```
