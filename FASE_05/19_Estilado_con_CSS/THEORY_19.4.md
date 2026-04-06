# 19.4 Propiedades CSS generales que funcionan en SVG

Además de las propiedades específicas de SVG (`fill`, `stroke`, etc.), una buena parte del CSS "de HTML" también funciona dentro de SVG: visibilidad, transformaciones, transiciones, modos de mezcla, variables. Esto convierte al SVG en un ciudadano de primera clase de la cascada moderna.

---

## Visibilidad y layout

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    display, visibility, overflow
  </text>

  <!-- display: none -->
  <rect x="20" y="48" width="60" height="60" fill="#3b82f6" stroke="#1e293b" stroke-width="1.5"/>
  <rect x="20" y="48" width="60" height="60" fill="white" opacity="0.9"/>
  <text x="50" y="82" text-anchor="middle" font-size="7" fill="#94a3b8">no se ve</text>
  <text x="50" y="120" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">display: none</text>
  <text x="50" y="130" text-anchor="middle" font-size="6" fill="#991b1b">no ocupa espacio</text>

  <!-- visibility: hidden -->
  <rect x="120" y="48" width="60" height="60" fill="#3b82f6" stroke="#1e293b" stroke-width="1.5"
        visibility="hidden"/>
  <rect x="120" y="48" width="60" height="60" fill="none" stroke="#cbd5e1" stroke-dasharray="2 2"/>
  <text x="150" y="120" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">visibility: hidden</text>
  <text x="150" y="130" text-anchor="middle" font-size="6" fill="#92400e">oculto pero ocupa</text>

  <!-- normal -->
  <rect x="220" y="48" width="60" height="60" fill="#3b82f6" stroke="#1e293b" stroke-width="1.5"/>
  <text x="250" y="120" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">visible (defecto)</text>
  <text x="250" y="130" text-anchor="middle" font-size="6" fill="#166534">renderiza normal</text>

  <rect x="15" y="148" width="270" height="84" fill="#0f172a" rx="3"/>
  <text x="25" y="164" font-size="8" font-weight="bold" fill="#94a3b8">overflow</text>
  <text x="25" y="180" font-size="7" fill="#e2e8f0">• por defecto el &lt;svg&gt; root recorta su contenido (overflow: hidden)</text>
  <text x="25" y="194" font-size="7" fill="#e2e8f0">• los grupos &lt;g&gt; NO recortan: el contenido se desborda libremente</text>
  <text x="25" y="208" font-size="7" fill="#e2e8f0">• overflow: visible en el &lt;svg&gt; deja salir el dibujo</text>
  <text x="25" y="222" font-size="7" fill="#fbbf24">útil para sombras o glow que se extienden fuera del viewBox</text>
</svg>
```

---

## Interacción: cursor y pointer-events

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cursor y pointer-events
  </text>

  <!-- cursor -->
  <rect x="20" y="40" width="120" height="80" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="80" y="68" text-anchor="middle" font-size="9" font-weight="bold" fill="#1e40af">cursor</text>
  <text x="80" y="84" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">cursor: pointer</text>
  <text x="80" y="98" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">cursor: grab</text>
  <text x="80" y="112" text-anchor="middle" font-size="6" fill="#94a3b8">misma sintaxis que HTML</text>

  <!-- pointer-events -->
  <rect x="160" y="40" width="120" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="220" y="58" text-anchor="middle" font-size="9" font-weight="bold" fill="#166534">pointer-events</text>
  <text x="220" y="74" text-anchor="middle" font-size="6" font-family="monospace" fill="#475569">none → no recibe eventos</text>
  <text x="220" y="86" text-anchor="middle" font-size="6" font-family="monospace" fill="#475569">visible → solo lo visible</text>
  <text x="220" y="98" text-anchor="middle" font-size="6" font-family="monospace" fill="#475569">stroke → solo el trazo</text>
  <text x="220" y="110" text-anchor="middle" font-size="6" font-family="monospace" fill="#475569">all → todo (defecto)</text>

  <rect x="15" y="138" width="270" height="92" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="8" font-weight="bold" fill="#94a3b8">caso típico</text>
  <text x="25" y="170" font-size="7" fill="#e2e8f0">/* hacer un círculo "vacío" clickeable solo en el borde */</text>
  <text x="25" y="186" font-size="7" fill="#60a5fa">circle.solo-borde {</text>
  <text x="25" y="200" font-size="7" fill="#34d399">  fill: none;</text>
  <text x="25" y="214" font-size="7" fill="#34d399">  pointer-events: stroke;</text>
  <text x="25" y="226" font-size="7" fill="#60a5fa">}</text>
</svg>
```

---

## Transformaciones

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    transform via CSS (no atributo)
  </text>

  <!-- Original -->
  <rect x="20" y="50" width="40" height="40" fill="#cbd5e1"/>
  <text x="40" y="105" text-anchor="middle" font-size="7" fill="#94a3b8">original</text>

  <!-- rotate -->
  <g transform="rotate(15 100 70)">
    <rect x="80" y="50" width="40" height="40" fill="#3b82f6"/>
  </g>
  <text x="100" y="105" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">rotate(15)</text>

  <!-- scale -->
  <g transform="translate(160 50) scale(1.3)">
    <rect x="0" y="0" width="40" height="40" fill="#10b981"/>
  </g>
  <text x="180" y="115" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">scale(1.3)</text>

  <!-- skew -->
  <g transform="translate(240 50) skewX(-15)">
    <rect x="0" y="0" width="40" height="40" fill="#ec4899"/>
  </g>
  <text x="260" y="105" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">skewX(-15)</text>

  <rect x="15" y="130" width="270" height="116" fill="#0f172a" rx="3"/>
  <text x="25" y="146" font-size="8" font-weight="bold" fill="#94a3b8">CSS transform vs atributo transform</text>
  <text x="25" y="162" font-size="7" fill="#e2e8f0">• ambos funcionan; CSS permite transitions y animations</text>
  <text x="25" y="176" font-size="7" fill="#e2e8f0">• transform-origin importante: defecto en SVG es 0 0</text>
  <text x="25" y="190" font-size="7" fill="#e2e8f0">• CSS acepta: rotate(), scale(), translate(), skew(), matrix()</text>
  <text x="25" y="204" font-size="7" fill="#fbbf24">.elemento {</text>
  <text x="25" y="218" font-size="7" fill="#34d399">  transform: rotate(45deg);</text>
  <text x="25" y="232" font-size="7" fill="#34d399">  transform-origin: center;</text>
  <text x="25" y="244" font-size="7" fill="#fbbf24">}</text>
</svg>
```

---

## Transiciones y animaciones

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    transition y @keyframes
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* transición sobre fill */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">circle {</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#34d399">  fill: steelblue;</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#34d399">  transition: fill 0.3s ease;</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <rect x="15" y="130" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#94a3b8">/* keyframes sobre stroke-dashoffset */</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#60a5fa">@keyframes dibujar {</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#34d399">  from { stroke-dashoffset: 100; }</text>
  <text x="25" y="188" font-size="8" font-family="monospace" fill="#34d399">  to   { stroke-dashoffset: 0; }</text>
  <text x="25" y="202" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#fbbf24">path { animation: dibujar 2s; }</text>

  <text x="150" y="248" text-anchor="middle" font-size="7" fill="#94a3b8">se anima fill, stroke, opacity, transform, dashoffset…</text>
</svg>
```

---

## Modos de mezcla

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    mix-blend-mode en SVG
  </text>

  <!-- Normal -->
  <g transform="translate(10 40)">
    <circle cx="30" cy="30" r="22" fill="#3b82f6"/>
    <circle cx="50" cy="40" r="22" fill="#ec4899"/>
    <text x="40" y="80" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">normal</text>
  </g>

  <!-- Multiply -->
  <g transform="translate(100 40)" style="isolation: isolate;">
    <circle cx="30" cy="30" r="22" fill="#3b82f6"/>
    <circle cx="50" cy="40" r="22" fill="#ec4899" style="mix-blend-mode: multiply;"/>
    <text x="40" y="80" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">multiply</text>
  </g>

  <!-- Screen -->
  <g transform="translate(190 40)" style="isolation: isolate;">
    <circle cx="30" cy="30" r="22" fill="#3b82f6"/>
    <circle cx="50" cy="40" r="22" fill="#ec4899" style="mix-blend-mode: screen;"/>
    <text x="40" y="80" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">screen</text>
  </g>

  <rect x="15" y="138" width="270" height="92" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="8" font-weight="bold" fill="#94a3b8">mix-blend-mode + isolation</text>
  <text x="25" y="170" font-size="7" fill="#e2e8f0">• valores: multiply, screen, overlay, darken, lighten…</text>
  <text x="25" y="184" font-size="7" fill="#e2e8f0">• necesita "contexto de mezcla" (isolation: isolate)</text>
  <text x="25" y="198" font-size="7" fill="#e2e8f0">• funciona en cualquier elemento SVG, no solo &lt;g&gt;</text>
  <text x="25" y="212" font-size="7" fill="#fbbf24">útil para efectos de superposición sin filtros pesados</text>
  <text x="25" y="224" font-size="7" fill="#94a3b8">cuidado con el coste si hay muchas capas</text>
</svg>
```

---

## Variables CSS

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    custom properties heredadas
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* CSS del documento */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">:root {</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#34d399">  --color-marca: #005ea5;</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#34d399">  --radio: 40px;</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <rect x="15" y="138" width="270" height="92" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- SVG inline en el HTML --&gt;</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;circle</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#fbbf24">  style="fill: var(--color-marca);"</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#e2e8f0">/&gt;</text>
  <text x="25" y="216" font-size="7" fill="#94a3b8">las variables del :root cruzan la frontera HTML→SVG</text>
</svg>
```
