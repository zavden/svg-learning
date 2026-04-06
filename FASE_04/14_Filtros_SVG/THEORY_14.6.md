# 14.6 Otras primitivas intermedias

Además de las primitivas básicas, SVG ofrece un arsenal de operaciones avanzadas: mezcla de modos, morfología, ruido procesal, iluminación 3D, convolución… Esta sección es una **gira por las más útiles** en la práctica.

---

## `<feBlend>` — modos de mezcla

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feBlend: mezclar dos imágenes</text>

  <defs>
    <filter id="b-mul" x="0" y="0" width="100%" height="100%">
      <feFlood flood-color="#f59e0b" result="c"/>
      <feComposite in="c" in2="SourceGraphic" operator="in" result="c2"/>
      <feBlend in="SourceGraphic" in2="c2" mode="multiply"/>
    </filter>
    <filter id="b-screen" x="0" y="0" width="100%" height="100%">
      <feFlood flood-color="#3b82f6" result="c"/>
      <feComposite in="c" in2="SourceGraphic" operator="in" result="c2"/>
      <feBlend in="SourceGraphic" in2="c2" mode="screen"/>
    </filter>
    <filter id="b-diff" x="0" y="0" width="100%" height="100%">
      <feFlood flood-color="#10b981" result="c"/>
      <feComposite in="c" in2="SourceGraphic" operator="in" result="c2"/>
      <feBlend in="SourceGraphic" in2="c2" mode="difference"/>
    </filter>

    <linearGradient id="b-grad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#ec4899"/>
      <stop offset="1" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <rect x="15" y="40" width="60" height="60" rx="4" fill="url(#b-grad)"/>
  <text x="45" y="115" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <rect x="85" y="40" width="60" height="60" rx="4" fill="url(#b-grad)" filter="url(#b-mul)"/>
  <text x="115" y="115" text-anchor="middle" font-size="7" fill="#64748b">multiply</text>
  <text x="115" y="125" text-anchor="middle" font-size="6" fill="#94a3b8">con naranja</text>

  <rect x="155" y="40" width="60" height="60" rx="4" fill="url(#b-grad)" filter="url(#b-screen)"/>
  <text x="185" y="115" text-anchor="middle" font-size="7" fill="#64748b">screen</text>
  <text x="185" y="125" text-anchor="middle" font-size="6" fill="#94a3b8">con azul</text>

  <rect x="225" y="40" width="60" height="60" rx="4" fill="url(#b-grad)" filter="url(#b-diff)"/>
  <text x="255" y="115" text-anchor="middle" font-size="7" fill="#64748b">difference</text>
  <text x="255" y="125" text-anchor="middle" font-size="6" fill="#94a3b8">con verde</text>

  <text x="150" y="160" text-anchor="middle" font-size="7" fill="#94a3b8">
    mode: normal, multiply, screen, overlay, darken, lighten,
  </text>
  <text x="150" y="172" text-anchor="middle" font-size="7" fill="#94a3b8">
    color-dodge, hard-light, soft-light, difference, exclusion…
  </text>
  <text x="150" y="188" text-anchor="middle" font-size="7" fill="#94a3b8">
    idénticos a los mix-blend-mode de CSS
  </text>
</svg>
```

---

## `<feMorphology>` — erode / dilate

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feMorphology: expandir o contraer</text>

  <defs>
    <filter id="m-ero"><feMorphology operator="erode" radius="2"/></filter>
    <filter id="m-dil"><feMorphology operator="dilate" radius="2"/></filter>
    <!-- Contorno exterior: dilate + composite out -->
    <filter id="m-outline" x="-30%" y="-30%" width="160%" height="160%">
      <feMorphology operator="dilate" radius="2" in="SourceAlpha" result="d"/>
      <feFlood flood-color="#ef4444" result="c"/>
      <feComposite in="c" in2="d" operator="in" result="stroke"/>
      <feMerge>
        <feMergeNode in="stroke"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>

  <!-- Fila 1: formas -->
  <text x="50"  y="45" text-anchor="middle" font-size="7" fill="#64748b">original</text>
  <rect x="25" y="52" width="50" height="30" fill="#8b5cf6"/>

  <text x="120" y="45" text-anchor="middle" font-size="7" fill="#64748b">erode r=2</text>
  <rect x="95" y="52" width="50" height="30" fill="#8b5cf6" filter="url(#m-ero)"/>

  <text x="190" y="45" text-anchor="middle" font-size="7" fill="#64748b">dilate r=2</text>
  <rect x="165" y="52" width="50" height="30" fill="#8b5cf6" filter="url(#m-dil)"/>

  <text x="260" y="45" text-anchor="middle" font-size="7" fill="#64748b">outline</text>
  <rect x="235" y="52" width="50" height="30" fill="#8b5cf6" filter="url(#m-outline)"/>

  <!-- Fila 2: texto -->
  <text x="50"  y="110" text-anchor="middle" font-size="22" font-weight="bold" fill="#1e293b">A</text>
  <text x="120" y="110" text-anchor="middle" font-size="22" font-weight="bold" fill="#1e293b" filter="url(#m-ero)">A</text>
  <text x="190" y="110" text-anchor="middle" font-size="22" font-weight="bold" fill="#1e293b" filter="url(#m-dil)">A</text>
  <text x="260" y="110" text-anchor="middle" font-size="22" font-weight="bold" fill="#1e293b" filter="url(#m-outline)">A</text>

  <text x="150" y="145" text-anchor="middle" font-size="7" fill="#94a3b8">
    erode = contrae (más delgado) · dilate = expande (más grueso)
  </text>
  <text x="150" y="160" text-anchor="middle" font-size="7" fill="#94a3b8">
    dilate + composite = contorno exterior sin tocar el path original
  </text>
  <text x="150" y="180" text-anchor="middle" font-size="7" fill="#ef4444">
    trabaja sobre pixels: puede producir bordes dentados
  </text>
</svg>
```

---

## `<feTurbulence>` — ruido procesal

```svg
<svg width="300" height="230"
     viewBox="0 0 300 230"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feTurbulence: genera ruido/texturas</text>

  <defs>
    <filter id="t-low" x="0" y="0" width="100%" height="100%">
      <feTurbulence type="fractalNoise" baseFrequency="0.02" numOctaves="2"/>
    </filter>
    <filter id="t-mid" x="0" y="0" width="100%" height="100%">
      <feTurbulence type="fractalNoise" baseFrequency="0.1" numOctaves="3"/>
    </filter>
    <filter id="t-high" x="0" y="0" width="100%" height="100%">
      <feTurbulence type="fractalNoise" baseFrequency="0.4" numOctaves="2"/>
    </filter>
    <filter id="t-turb" x="0" y="0" width="100%" height="100%">
      <feTurbulence type="turbulence" baseFrequency="0.05" numOctaves="4"/>
    </filter>
  </defs>

  <!-- Muestras de ruido -->
  <rect x="15" y="35" width="65" height="65" fill="white" filter="url(#t-low)"/>
  <text x="47" y="115" text-anchor="middle" font-size="7" fill="#64748b">fractalNoise</text>
  <text x="47" y="125" text-anchor="middle" font-size="6" fill="#94a3b8">freq 0.02</text>

  <rect x="85" y="35" width="65" height="65" fill="white" filter="url(#t-mid)"/>
  <text x="117" y="115" text-anchor="middle" font-size="7" fill="#64748b">fractalNoise</text>
  <text x="117" y="125" text-anchor="middle" font-size="6" fill="#94a3b8">freq 0.1</text>

  <rect x="155" y="35" width="65" height="65" fill="white" filter="url(#t-high)"/>
  <text x="187" y="115" text-anchor="middle" font-size="7" fill="#64748b">fractalNoise</text>
  <text x="187" y="125" text-anchor="middle" font-size="6" fill="#94a3b8">freq 0.4</text>

  <rect x="225" y="35" width="65" height="65" fill="white" filter="url(#t-turb)"/>
  <text x="257" y="115" text-anchor="middle" font-size="7" fill="#64748b">turbulence</text>
  <text x="257" y="125" text-anchor="middle" font-size="6" fill="#94a3b8">4 octaves</text>

  <!-- Parámetros -->
  <rect x="15" y="140" width="270" height="78" fill="#f1f5f9" stroke="#cbd5e1" rx="3"/>
  <text x="25" y="156" font-size="7" font-weight="bold" fill="#1e293b">atributos clave:</text>
  <text x="25" y="168" font-size="7" fill="#64748b">• type: fractalNoise (suave) · turbulence (áspero)</text>
  <text x="25" y="180" font-size="7" fill="#64748b">• baseFrequency: 0.01-0.5 → escala del ruido</text>
  <text x="25" y="192" font-size="7" fill="#64748b">• numOctaves: 1-6 → capas de detalle (más = más costoso)</text>
  <text x="25" y="204" font-size="7" fill="#64748b">• seed: misma semilla = mismo patrón reproducible</text>
  <text x="25" y="215" font-size="7" fill="#10b981">ideal para: agua, nubes, papel, madera, fuego, grano</text>
</svg>
```

---

## `<feDisplacementMap>` — distorsión con mapa

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feDisplacementMap: turbulence + distorsión</text>

  <defs>
    <filter id="dist-sutil" x="-20%" y="-20%" width="140%" height="140%">
      <feTurbulence type="fractalNoise" baseFrequency="0.02" numOctaves="2" result="noise"/>
      <feDisplacementMap in="SourceGraphic" in2="noise" scale="5"/>
    </filter>
    <filter id="dist-fuerte" x="-20%" y="-20%" width="140%" height="140%">
      <feTurbulence type="turbulence" baseFrequency="0.05" numOctaves="3" result="noise"/>
      <feDisplacementMap in="SourceGraphic" in2="noise" scale="15"/>
    </filter>
  </defs>

  <!-- Texto sin filtro -->
  <g transform="translate(0, 0)">
    <text x="50" y="75" font-size="20" font-weight="bold" fill="#8b5cf6" text-anchor="middle">HOLA</text>
    <text x="50" y="125" text-anchor="middle" font-size="7" fill="#64748b">normal</text>
  </g>

  <!-- Distorsión sutil -->
  <g transform="translate(0, 0)">
    <text x="150" y="75" font-size="20" font-weight="bold" fill="#8b5cf6" text-anchor="middle" filter="url(#dist-sutil)">HOLA</text>
    <text x="150" y="125" text-anchor="middle" font-size="7" fill="#64748b">scale=5</text>
    <text x="150" y="135" text-anchor="middle" font-size="6" fill="#94a3b8">sutil</text>
  </g>

  <!-- Distorsión fuerte -->
  <g transform="translate(0, 0)">
    <text x="250" y="75" font-size="20" font-weight="bold" fill="#8b5cf6" text-anchor="middle" filter="url(#dist-fuerte)">HOLA</text>
    <text x="250" y="125" text-anchor="middle" font-size="7" fill="#64748b">scale=15</text>
    <text x="250" y="135" text-anchor="middle" font-size="6" fill="#94a3b8">derrite</text>
  </g>

  <text x="150" y="160" text-anchor="middle" font-size="7" fill="#94a3b8">
    el color de cada pixel del mapa dice cuánto se desplaza el pixel destino
  </text>
  <text x="150" y="175" text-anchor="middle" font-size="7" fill="#94a3b8">
    combo clásico: turbulence como mapa → efecto líquido/cristal
  </text>
</svg>
```

---

## `<feConvolveMatrix>` — sharpen, edge detect

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feConvolveMatrix: kernels de procesamiento</text>

  <defs>
    <!-- Kernel sharpen (suma = 1) -->
    <filter id="conv-sharp">
      <feConvolveMatrix order="3"
                        kernelMatrix="0 -1 0
                                      -1 5 -1
                                       0 -1 0"/>
    </filter>
    <!-- Kernel emboss -->
    <filter id="conv-emboss">
      <feConvolveMatrix order="3"
                        kernelMatrix="-2 -1 0
                                      -1  1 1
                                       0  1 2"/>
    </filter>
    <!-- Kernel edge detect -->
    <filter id="conv-edge">
      <feConvolveMatrix order="3"
                        kernelMatrix="-1 -1 -1
                                      -1  8 -1
                                      -1 -1 -1"/>
    </filter>

    <radialGradient id="conv-g" cx="0.3" cy="0.3">
      <stop offset="0" stop-color="#fef9c3"/>
      <stop offset="1" stop-color="#f59e0b"/>
    </radialGradient>
  </defs>

  <!-- Original -->
  <circle cx="50" cy="75" r="28" fill="url(#conv-g)"/>
  <text x="50" y="125" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <!-- Sharpen -->
  <circle cx="125" cy="75" r="28" fill="url(#conv-g)" filter="url(#conv-sharp)"/>
  <text x="125" y="125" text-anchor="middle" font-size="7" fill="#64748b">sharpen</text>

  <!-- Emboss -->
  <circle cx="200" cy="75" r="28" fill="url(#conv-g)" filter="url(#conv-emboss)"/>
  <text x="200" y="125" text-anchor="middle" font-size="7" fill="#64748b">emboss</text>

  <!-- Edge detect -->
  <circle cx="260" cy="75" r="25" fill="url(#conv-g)" filter="url(#conv-edge)"/>
  <text x="260" y="125" text-anchor="middle" font-size="7" fill="#64748b">edge detect</text>

  <!-- Explicación -->
  <rect x="15" y="140" width="270" height="50" fill="#f1f5f9" stroke="#cbd5e1" rx="3"/>
  <text x="25" y="155" font-size="7" font-weight="bold" fill="#1e293b">cómo funciona:</text>
  <text x="25" y="167" font-size="7" fill="#64748b">cada pixel del resultado es el promedio ponderado</text>
  <text x="25" y="178" font-size="7" fill="#64748b">de los pixels vecinos, usando la matriz como pesos</text>
</svg>
```

---

## `<feComponentTransfer>` — curvas por canal

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feComponentTransfer: ajustar canales</text>

  <defs>
    <!-- Posterizar: 4 niveles por canal -->
    <filter id="ct-poster">
      <feComponentTransfer>
        <feFuncR type="discrete" tableValues="0 0.33 0.66 1"/>
        <feFuncG type="discrete" tableValues="0 0.33 0.66 1"/>
        <feFuncB type="discrete" tableValues="0 0.33 0.66 1"/>
      </feComponentTransfer>
    </filter>
    <!-- Invertir: linear con slope negativo -->
    <filter id="ct-invert">
      <feComponentTransfer>
        <feFuncR type="linear" slope="-1" intercept="1"/>
        <feFuncG type="linear" slope="-1" intercept="1"/>
        <feFuncB type="linear" slope="-1" intercept="1"/>
      </feComponentTransfer>
    </filter>
    <!-- Aumentar gamma (oscurecer) -->
    <filter id="ct-gamma">
      <feComponentTransfer>
        <feFuncR type="gamma" amplitude="1" exponent="2.5"/>
        <feFuncG type="gamma" amplitude="1" exponent="2.5"/>
        <feFuncB type="gamma" amplitude="1" exponent="2.5"/>
      </feComponentTransfer>
    </filter>

    <linearGradient id="ct-g" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#ec4899"/>
      <stop offset="0.5" stop-color="#f59e0b"/>
      <stop offset="1" stop-color="#10b981"/>
    </linearGradient>
  </defs>

  <rect x="15" y="40" width="60" height="70" rx="4" fill="url(#ct-g)"/>
  <text x="45" y="125" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <rect x="85" y="40" width="60" height="70" rx="4" fill="url(#ct-g)" filter="url(#ct-poster)"/>
  <text x="115" y="125" text-anchor="middle" font-size="7" fill="#64748b">posterizar</text>
  <text x="115" y="135" text-anchor="middle" font-size="6" fill="#94a3b8">discrete</text>

  <rect x="155" y="40" width="60" height="70" rx="4" fill="url(#ct-g)" filter="url(#ct-invert)"/>
  <text x="185" y="125" text-anchor="middle" font-size="7" fill="#64748b">invertir</text>
  <text x="185" y="135" text-anchor="middle" font-size="6" fill="#94a3b8">linear</text>

  <rect x="225" y="40" width="60" height="70" rx="4" fill="url(#ct-g)" filter="url(#ct-gamma)"/>
  <text x="255" y="125" text-anchor="middle" font-size="7" fill="#64748b">gamma 2.5</text>
  <text x="255" y="135" text-anchor="middle" font-size="6" fill="#94a3b8">oscurecer</text>

  <text x="150" y="160" text-anchor="middle" font-size="7" fill="#94a3b8">
    tipos: identity · linear · gamma · table · discrete
  </text>
</svg>
```

---

## Iluminación 3D: `feDiffuseLighting` + fuentes de luz

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">iluminación 3D sobre el canal alfa</text>

  <defs>
    <!-- Luz difusa desde arriba a la izquierda -->
    <filter id="light-diff" x="-20%" y="-20%" width="140%" height="140%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="4" result="bump"/>
      <feDiffuseLighting in="bump" surfaceScale="5" diffuseConstant="1.2" lighting-color="#fef9c3" result="light">
        <feDistantLight azimuth="135" elevation="45"/>
      </feDiffuseLighting>
      <feComposite in="light" in2="SourceAlpha" operator="in" result="lit"/>
      <feBlend in="SourceGraphic" in2="lit" mode="multiply"/>
    </filter>
    <!-- Luz especular (brillo) -->
    <filter id="light-spec" x="-20%" y="-20%" width="140%" height="140%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="3" result="bump"/>
      <feSpecularLighting in="bump" surfaceScale="4" specularConstant="1.5"
                          specularExponent="20" lighting-color="white" result="spec">
        <fePointLight x="30" y="10" z="60"/>
      </feSpecularLighting>
      <feComposite in="spec" in2="SourceAlpha" operator="in" result="spec2"/>
      <feMerge>
        <feMergeNode in="SourceGraphic"/>
        <feMergeNode in="spec2"/>
      </feMerge>
    </filter>
  </defs>

  <circle cx="60"  cy="85" r="35" fill="#3b82f6"/>
  <text x="60"  y="140" text-anchor="middle" font-size="7" fill="#64748b">plano</text>

  <circle cx="150" cy="85" r="35" fill="#3b82f6" filter="url(#light-diff)"/>
  <text x="150" y="140" text-anchor="middle" font-size="7" fill="#64748b">diffuseLighting</text>
  <text x="150" y="150" text-anchor="middle" font-size="6" fill="#94a3b8">distantLight</text>

  <circle cx="240" cy="85" r="35" fill="#3b82f6" filter="url(#light-spec)"/>
  <text x="240" y="140" text-anchor="middle" font-size="7" fill="#64748b">specularLighting</text>
  <text x="240" y="150" text-anchor="middle" font-size="6" fill="#94a3b8">pointLight</text>

  <rect x="15" y="165" width="270" height="45" fill="#f1f5f9" stroke="#cbd5e1" rx="3"/>
  <text x="25" y="180" font-size="7" font-weight="bold" fill="#1e293b">fuentes de luz:</text>
  <text x="25" y="192" font-size="7" fill="#64748b">• feDistantLight: luz paralela (sol) — azimuth, elevation</text>
  <text x="25" y="203" font-size="7" fill="#64748b">• fePointLight: luz puntual — x, y, z · feSpotLight: cono de luz</text>
</svg>
```

---

## `<feImage>` y `<feTile>`

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">feImage y feTile: fuentes externas</text>

  <defs>
    <!-- feImage: usa un SVG externo (data URI) como entrada -->
    <filter id="fi-img" x="0" y="0" width="100%" height="100%">
      <feImage href="data:image/svg+xml;utf8,&lt;svg xmlns='http://www.w3.org/2000/svg' width='60' height='60'&gt;&lt;rect width='60' height='60' fill='%23f59e0b'/&gt;&lt;circle cx='30' cy='30' r='15' fill='%23ec4899'/&gt;&lt;/svg&gt;" result="ext"/>
      <feComposite in="ext" in2="SourceAlpha" operator="in"/>
    </filter>

    <!-- feTile: repite una pequeña forma para rellenar -->
    <filter id="fi-tile" x="0" y="0" width="100%" height="100%">
      <feImage href="data:image/svg+xml;utf8,&lt;svg xmlns='http://www.w3.org/2000/svg' width='20' height='20'&gt;&lt;circle cx='10' cy='10' r='4' fill='%233b82f6'/&gt;&lt;/svg&gt;" x="0" y="0" width="20" height="20" result="dot"/>
      <feTile in="dot" result="tiled"/>
      <feComposite in="tiled" in2="SourceAlpha" operator="in"/>
    </filter>
  </defs>

  <!-- feImage -->
  <circle cx="75" cy="80" r="35" fill="#e2e8f0" filter="url(#fi-img)"/>
  <text x="75" y="130" text-anchor="middle" font-size="7" fill="#64748b">feImage</text>
  <text x="75" y="140" text-anchor="middle" font-size="6" fill="#94a3b8">SVG externo como fuente</text>

  <!-- feTile -->
  <circle cx="225" cy="80" r="35" fill="#e2e8f0" filter="url(#fi-tile)"/>
  <text x="225" y="130" text-anchor="middle" font-size="7" fill="#64748b">feImage + feTile</text>
  <text x="225" y="140" text-anchor="middle" font-size="6" fill="#94a3b8">mosaico repetido</text>

  <text x="150" y="170" text-anchor="middle" font-size="7" fill="#94a3b8">
    feImage carga una imagen (URL, data URI o referencia a otro SVG)
  </text>
  <text x="150" y="185" text-anchor="middle" font-size="7" fill="#94a3b8">
    feTile la repite como mosaico dentro del área del filtro
  </text>
</svg>
```
