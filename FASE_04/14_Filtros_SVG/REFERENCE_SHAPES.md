# Cheat sheet — Filtros SVG

---

## Anatomía de un filtro

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    estructura de &lt;filter&gt;
  </text>

  <!-- Caja del filtro -->
  <rect x="15" y="38" width="270" height="190" fill="#f1f5f9" stroke="#cbd5e1" stroke-width="1.5" rx="5"/>

  <text x="25" y="56" font-size="8" font-family="monospace" fill="#1e293b">
    &lt;filter id="miFiltro"
  </text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#1e293b">
    <tspan fill="#64748b">        </tspan>x="-10%" y="-10%"
  </text>
  <text x="25" y="80" font-size="8" font-family="monospace" fill="#1e293b">
    <tspan fill="#64748b">        </tspan>width="120%" height="120%"&gt;
  </text>

  <!-- Primitivas -->
  <rect x="30" y="90" width="255" height="18" fill="#dbeafe" stroke="#3b82f6" rx="2"/>
  <text x="38" y="103" font-size="7" font-family="monospace" fill="#1e40af">
    &lt;feGaussianBlur in="SourceAlpha" stdDeviation="3" result="blur"/&gt;
  </text>

  <rect x="30" y="112" width="255" height="18" fill="#dcfce7" stroke="#10b981" rx="2"/>
  <text x="38" y="125" font-size="7" font-family="monospace" fill="#166534">
    &lt;feOffset in="blur" dx="4" dy="5" result="offset"/&gt;
  </text>

  <rect x="30" y="134" width="255" height="18" fill="#fef9c3" stroke="#f59e0b" rx="2"/>
  <text x="38" y="147" font-size="7" font-family="monospace" fill="#92400e">
    &lt;feFlood flood-color="#000" flood-opacity="0.5"/&gt;
  </text>

  <rect x="30" y="156" width="255" height="18" fill="#fce7f3" stroke="#ec4899" rx="2"/>
  <text x="38" y="169" font-size="7" font-family="monospace" fill="#9d174d">
    &lt;feComposite in2="offset" operator="in" result="shadow"/&gt;
  </text>

  <rect x="30" y="178" width="255" height="30" fill="#ede9fe" stroke="#8b5cf6" rx="2"/>
  <text x="38" y="191" font-size="7" font-family="monospace" fill="#5b21b6">
    &lt;feMerge&gt;
  </text>
  <text x="38" y="202" font-size="7" font-family="monospace" fill="#5b21b6">
    <tspan fill="#94a3b8">  </tspan>&lt;feMergeNode in="shadow"/&gt; &lt;feMergeNode in="SourceGraphic"/&gt;
  </text>

  <text x="25" y="222" font-size="8" font-family="monospace" fill="#1e293b">
    &lt;/filter&gt;
  </text>
</svg>
```

---

## Primitivas más usadas — qué hace cada una

```svg
<svg width="300" height="320"
     viewBox="0 0 300 320"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    guía rápida de primitivas
  </text>

  <!-- feGaussianBlur -->
  <rect x="10" y="34" width="280" height="28" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="18" y="48" font-size="8" font-weight="bold" fill="#1e40af">feGaussianBlur</text>
  <text x="18" y="58" font-size="7" fill="#64748b">desenfoque · stdDeviation="N" · la más costosa</text>

  <!-- feOffset -->
  <rect x="10" y="66" width="280" height="28" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="18" y="80" font-size="8" font-weight="bold" fill="#166534">feOffset</text>
  <text x="18" y="90" font-size="7" fill="#64748b">desplaza la imagen · dx, dy · sin cambiar pixels</text>

  <!-- feFlood -->
  <rect x="10" y="98" width="280" height="28" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="18" y="112" font-size="8" font-weight="bold" fill="#92400e">feFlood</text>
  <text x="18" y="122" font-size="7" fill="#64748b">genera un color plano · flood-color, flood-opacity</text>

  <!-- feComposite -->
  <rect x="10" y="130" width="280" height="28" fill="#fce7f3" stroke="#ec4899" rx="3"/>
  <text x="18" y="144" font-size="8" font-weight="bold" fill="#9d174d">feComposite</text>
  <text x="18" y="154" font-size="7" fill="#64748b">combina 2 imágenes · operator: over · in · out · atop · xor · arithmetic</text>

  <!-- feMerge -->
  <rect x="10" y="162" width="280" height="28" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="18" y="176" font-size="8" font-weight="bold" fill="#5b21b6">feMerge</text>
  <text x="18" y="186" font-size="7" fill="#64748b">apila capas · feMergeNode in="..." · orden = bottom → top</text>

  <!-- feColorMatrix -->
  <rect x="10" y="194" width="280" height="28" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="18" y="208" font-size="8" font-weight="bold" fill="#991b1b">feColorMatrix</text>
  <text x="18" y="218" font-size="7" fill="#64748b">transforma colores · type: matrix · saturate · hueRotate · luminanceToAlpha</text>

  <!-- feDropShadow -->
  <rect x="10" y="226" width="280" height="28" fill="#f1f5f9" stroke="#64748b" rx="3"/>
  <text x="18" y="240" font-size="8" font-weight="bold" fill="#1e293b">feDropShadow</text>
  <text x="18" y="250" font-size="7" fill="#64748b">atajo: dx dy stdDeviation flood-color flood-opacity</text>

  <!-- feMorphology -->
  <rect x="10" y="258" width="280" height="28" fill="#d1fae5" stroke="#059669" rx="3"/>
  <text x="18" y="272" font-size="8" font-weight="bold" fill="#047857">feMorphology</text>
  <text x="18" y="282" font-size="7" fill="#64748b">erode (contraer) · dilate (expandir) · para contornos</text>

  <!-- feTurbulence -->
  <rect x="10" y="290" width="280" height="28" fill="#fef3c7" stroke="#d97706" rx="3"/>
  <text x="18" y="304" font-size="8" font-weight="bold" fill="#92400e">feTurbulence</text>
  <text x="18" y="314" font-size="7" fill="#64748b">genera ruido · fractalNoise · turbulence · baseFrequency · numOctaves</text>
</svg>
```

---

## Entradas predefinidas y cuándo usar cada una

```svg
<svg width="300" height="210"
     viewBox="0 0 300 210"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    el atributo in="..."
  </text>

  <rect x="10" y="34" width="280" height="32" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="20" y="48" font-size="8" font-weight="bold" fill="#1e40af">SourceGraphic</text>
  <text x="20" y="60" font-size="7" fill="#64748b">elemento completo con colores · default cuando se omite "in"</text>

  <rect x="10" y="72" width="280" height="32" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="20" y="86" font-size="8" font-weight="bold" fill="#166534">SourceAlpha</text>
  <text x="20" y="98" font-size="7" fill="#64748b">solo silueta (canal alfa) · base ideal de sombras</text>

  <rect x="10" y="110" width="280" height="32" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="20" y="124" font-size="8" font-weight="bold" fill="#92400e">FillPaint / StrokePaint</text>
  <text x="20" y="136" font-size="7" fill="#64748b">el color/gradiente del fill o del stroke como imagen</text>

  <rect x="10" y="148" width="280" height="32" fill="#fce7f3" stroke="#ec4899" rx="3"/>
  <text x="20" y="162" font-size="8" font-weight="bold" fill="#9d174d">result="nombre"</text>
  <text x="20" y="174" font-size="7" fill="#64748b">referencia al output de otra primitiva del mismo filter</text>

  <text x="150" y="198" text-anchor="middle" font-size="7" fill="#94a3b8">
    sin "in" explícito → output de la primitiva anterior en el código
  </text>
</svg>
```

---

## Recetas copy-paste: efectos frecuentes

```svg
<svg width="300" height="340"
     viewBox="0 0 300 340"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    recetas listas
  </text>

  <!-- drop shadow -->
  <rect x="10" y="34" width="280" height="42" fill="#e0e7ff" stroke="#6366f1" rx="3"/>
  <text x="18" y="48" font-size="8" font-weight="bold" fill="#312e81">sombra proyectada</text>
  <text x="18" y="60" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;feDropShadow dx="3" dy="4" stdDeviation="3"</text>
  <text x="18" y="70" font-size="6.5" fill="#1e293b" font-family="monospace">               flood-color="#000" flood-opacity="0.4"/&gt;</text>

  <!-- glow -->
  <rect x="10" y="82" width="280" height="58" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="18" y="96" font-size="8" font-weight="bold" fill="#166534">brillo exterior (glow)</text>
  <text x="18" y="108" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;feGaussianBlur stdDeviation="5"/&gt;</text>
  <text x="18" y="118" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;feMerge&gt;&lt;feMergeNode/&gt;&lt;feMergeNode/&gt;&lt;feMergeNode/&gt;</text>
  <text x="18" y="128" font-size="6.5" fill="#1e293b" font-family="monospace">         &lt;feMergeNode in="SourceGraphic"/&gt;&lt;/feMerge&gt;</text>
  <text x="18" y="138" font-size="6.5" fill="#64748b">repetir el blur varias veces intensifica el halo</text>

  <!-- grises -->
  <rect x="10" y="146" width="280" height="32" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="18" y="160" font-size="8" font-weight="bold" fill="#92400e">escala de grises</text>
  <text x="18" y="172" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;feColorMatrix type="saturate" values="0"/&gt;</text>

  <!-- outline -->
  <rect x="10" y="184" width="280" height="58" fill="#fce7f3" stroke="#ec4899" rx="3"/>
  <text x="18" y="198" font-size="8" font-weight="bold" fill="#9d174d">contorno exterior</text>
  <text x="18" y="210" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;feMorphology operator="dilate" radius="2"</text>
  <text x="18" y="220" font-size="6.5" fill="#1e293b" font-family="monospace">               in="SourceAlpha" result="d"/&gt;</text>
  <text x="18" y="230" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;feFlood flood-color="red" result="c"/&gt;</text>
  <text x="18" y="240" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;feComposite in="c" in2="d" operator="in"/&gt;</text>

  <!-- ruido -->
  <rect x="10" y="248" width="280" height="42" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="18" y="262" font-size="8" font-weight="bold" fill="#5b21b6">textura de papel / grano</text>
  <text x="18" y="274" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;feTurbulence type="fractalNoise"</text>
  <text x="18" y="284" font-size="6.5" fill="#1e293b" font-family="monospace">               baseFrequency="0.8" numOctaves="2"/&gt;</text>

  <!-- tint -->
  <rect x="10" y="296" width="280" height="38" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="18" y="310" font-size="8" font-weight="bold" fill="#991b1b">teñir con un color plano</text>
  <text x="18" y="322" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;feFlood flood-color="#ec4899"/&gt;</text>
  <text x="18" y="331" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;feComposite in2="SourceAlpha" operator="in"/&gt;</text>
</svg>
```
