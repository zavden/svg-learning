# 7.8 `vector-effect`

Por defecto, el grosor del stroke se escala con las transformaciones. `vector-effect="non-scaling-stroke"` mantiene el grosor en píxeles de pantalla, independientemente del zoom.

---

## El problema: stroke que se escala

Cuando un SVG se escala (con `transform`, CSS, o el atributo `viewBox`), el `stroke-width` también se escala.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Stroke de 1px — sin escala -->
  <rect x="20" y="20" width="60" height="60" fill="#dbeafe" stroke="#3b82f6" stroke-width="1"/>
  <text x="50" y="96" text-anchor="middle" font-size="7.5" fill="#64748b">1px normal</text>

  <!-- El mismo rect escalado 2x → stroke de 2px visual -->
  <g transform="translate(120,20) scale(1.5)">
    <rect x="0" y="0" width="60" height="60" fill="#dbeafe" stroke="#3b82f6" stroke-width="1"/>
  </g>
  <text x="167" y="96" text-anchor="middle" font-size="7.5" fill="#ef4444">escala 1.5× → stroke 1.5px</text>
  <text x="167" y="107" text-anchor="middle" font-size="7" fill="#94a3b8">stroke-width se escala</text>

  <!-- El mismo rect escalado pero con non-scaling-stroke -->
  <g transform="translate(225,20) scale(1.5)">
    <rect x="0" y="0" width="60" height="60" fill="#dbeafe" stroke="#3b82f6" stroke-width="1"
          vector-effect="non-scaling-stroke"/>
  </g>
  <text x="270" y="96" text-anchor="middle" font-size="7.5" fill="#10b981">non-scaling ✓</text>
  <text x="270" y="107" text-anchor="middle" font-size="7" fill="#94a3b8">siempre 1px visual</text>
</svg>
```

---

## Cómo funciona `vector-effect="non-scaling-stroke"`

El grosor se calcula en el **espacio de pantalla** (píxeles), no en el espacio de coordenadas del SVG.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Grid con non-scaling-stroke: las líneas siempre son finas -->
  <g stroke="#94a3b8" stroke-width="1" vector-effect="non-scaling-stroke" transform="scale(1)">
    <!-- Líneas horizontales -->
    <line x1="10" y1="20" x2="290" y2="20"/>
    <line x1="10" y1="40" x2="290" y2="40"/>
    <line x1="10" y1="60" x2="290" y2="60"/>
    <line x1="10" y1="80" x2="290" y2="80"/>
    <line x1="10" y1="100" x2="290" y2="100"/>
    <!-- Líneas verticales -->
    <line x1="10"  y1="10" x2="10"  y2="110"/>
    <line x1="70"  y1="10" x2="70"  y2="110"/>
    <line x1="130" y1="10" x2="130" y2="110"/>
    <line x1="190" y1="10" x2="190" y2="110"/>
    <line x1="250" y1="10" x2="250" y2="110"/>
    <line x1="290" y1="10" x2="290" y2="110"/>
  </g>

  <!-- Formas sobre el grid -->
  <circle cx="130" cy="60" r="40" fill="#3b82f6" fill-opacity="0.3"
          stroke="#3b82f6" stroke-width="2" vector-effect="non-scaling-stroke"/>
  <rect x="190" y="20" width="100" height="80" fill="#10b981" fill-opacity="0.2"
        stroke="#10b981" stroke-width="2" vector-effect="non-scaling-stroke"/>

  <text x="150" y="124" text-anchor="middle" font-size="7.5" fill="#64748b">
    non-scaling-stroke: ideal para grids y diagramas técnicos
  </text>
</svg>
```

---

## Casos de uso y limitaciones

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">non-scaling-stroke</text>

  <text x="10" y="36" font-size="8" fill="#10b981" font-weight="700">Casos de uso</text>
  <text x="10" y="50" font-size="7" fill="#64748b">• Grids y cuadrículas en diagramas</text>
  <text x="10" y="63" font-size="7" fill="#64748b">• Bordes de interfaz con zoom del usuario</text>
  <text x="10" y="76" font-size="7" fill="#64748b">• Anotaciones y guías técnicas</text>
  <text x="10" y="89" font-size="7" fill="#64748b">• SVGs que el usuario puede hacer zoom</text>

  <line x1="155" y1="22" x2="155" y2="130" stroke="#e2e8f0" stroke-width="1"/>

  <text x="165" y="36" font-size="8" fill="#ef4444" font-weight="700">Limitaciones</text>
  <text x="165" y="50" font-size="7" fill="#64748b">• Solo afecta al stroke, no al fill</text>
  <text x="165" y="63" font-size="7" fill="#64748b">• No se hereda a hijos (repetir en cada uno)</text>
  <text x="165" y="76" font-size="7" fill="#64748b">• El centro del stroke sigue en el borde</text>
  <text x="165" y="89" font-size="7" fill="#64748b">• Comportamiento sutil en rotaciones</text>
  <text x="165" y="102" font-size="7" fill="#64748b">• Soporte bueno pero verificar en Edge</text>
</svg>
```
