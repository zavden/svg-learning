# 10.6 Referenciar patrones

Los patrones se usan con `url(#id)` exactamente como los gradientes. Se aplican en fill, stroke y CSS.

---

## fill, stroke y CSS

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <pattern id="ref-dots" x="0" y="0" width="12" height="12"
             patternUnits="userSpaceOnUse">
      <circle cx="6" cy="6" r="3" fill="#3b82f6"/>
    </pattern>

    <style>
      .patterned { fill: url(#ref-dots); }
    </style>
  </defs>

  <!-- fill con patrón -->
  <rect x="10"  y="15" width="75" height="60" rx="4" fill="url(#ref-dots)"/>
  <text x="47"  y="87" text-anchor="middle" font-size="7" fill="#64748b">fill="url(#id)"</text>

  <!-- stroke con patrón -->
  <rect x="100" y="15" width="75" height="60" rx="4"
        fill="none" stroke="url(#ref-dots)" stroke-width="8"/>
  <text x="137" y="87" text-anchor="middle" font-size="7" fill="#64748b">stroke="url(#id)"</text>

  <!-- clase CSS -->
  <rect x="195" y="15" width="95" height="60" rx="4" class="patterned"/>
  <text x="242" y="87" text-anchor="middle" font-size="7" fill="#64748b">clase CSS</text>
  <text x="242" y="96" text-anchor="middle" font-size="6.5" fill="#94a3b8">fill: url(#id)</text>

  <text x="150" y="108" text-anchor="middle" font-size="7" fill="#94a3b8">
    misma sintaxis que los gradientes
  </text>
</svg>
```

---

## El patrón en distintas formas

El patrón tesela el interior de cualquier forma. Las zonas fuera de la forma se recortan.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <pattern id="multi-shape" x="0" y="0" width="15" height="15"
             patternUnits="userSpaceOnUse">
      <rect x="0.5" y="0.5" width="14" height="14" fill="none"
            stroke="#8b5cf6" stroke-width="0.5"/>
      <circle cx="7.5" cy="7.5" r="2" fill="#8b5cf6"/>
    </pattern>
  </defs>

  <rect    x="10"  y="10"  width="65"  height="75" rx="6"    fill="url(#multi-shape)"/>
  <circle  cx="120" cy="50" r="38"                            fill="url(#multi-shape)"/>
  <ellipse cx="210" cy="50" rx="55" ry="35"                  fill="url(#multi-shape)"/>

  <text x="42"  y="98" text-anchor="middle" font-size="7" fill="#64748b">rect</text>
  <text x="120" y="98" text-anchor="middle" font-size="7" fill="#64748b">circle</text>
  <text x="210" y="98" text-anchor="middle" font-size="7" fill="#64748b">ellipse</text>

  <text x="150" y="110" text-anchor="middle" font-size="6.5" fill="#94a3b8">
    el patrón se recorta al contorno — funciona con cualquier forma
  </text>
</svg>
```

---

## Patrón en path con agujero (fill-rule evenodd)

En paths con agujeros (evenodd), las zonas interiores no muestran el patrón.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <pattern id="hole-pat" x="0" y="0" width="10" height="10"
             patternUnits="userSpaceOnUse">
      <line x1="10" y1="0" x2="0" y2="10" stroke="#3b82f6" stroke-width="1"/>
    </pattern>
  </defs>

  <!-- Marco: rectángulo exterior con agujero interior (evenodd) -->
  <path d="M 20 10 L 140 10 L 140 90 L 20 90 Z
           M 50 30 L 110 30 L 110 70 L 50 70 Z"
        fill="url(#hole-pat)" fill-rule="evenodd"/>
  <text x="80"  y="105" text-anchor="middle" font-size="7" fill="#64748b">fill-rule=evenodd</text>
  <text x="80"  y="114" text-anchor="middle" font-size="6.5" fill="#94a3b8">el agujero queda vacío</text>

  <!-- Mismo path sin evenodd: el patrón cubre todo -->
  <path d="M 170 10 L 290 10 L 290 90 L 170 90 Z
           M 200 30 L 260 30 L 260 70 L 200 70 Z"
        fill="url(#hole-pat)" fill-rule="nonzero"/>
  <text x="230" y="105" text-anchor="middle" font-size="7" fill="#64748b">fill-rule=nonzero</text>
  <text x="230" y="114" text-anchor="middle" font-size="6.5" fill="#94a3b8">el patrón llena todo</text>
</svg>
```

---

## Modificar el patrón con JavaScript

Los patrones son nodos del DOM. Se pueden modificar dinámicamente para animar texturas.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <pattern id="animated-pat" x="0" y="0" width="20" height="20"
             patternUnits="userSpaceOnUse">
      <rect x="0.5" y="0.5" width="19" height="19" fill="none"
            stroke="#3b82f6" stroke-width="0.5"/>
    </pattern>

    <style>
      #animated-pat {
        animation: rotate-pattern 4s linear infinite;
      }
      @keyframes rotate-pattern {
        to { patternTransform: rotate(360); }
      }
    </style>
  </defs>

  <rect x="10" y="10" width="280" height="65" rx="4" fill="url(#animated-pat)"/>

  <text x="150" y="86" text-anchor="middle" font-size="7.5" fill="#64748b">
    animar patternTransform con CSS para rotar la textura
  </text>
</svg>
```
