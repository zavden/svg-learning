# 1.4 Gráficos Vectoriales vs Rasterizados

Entender la diferencia fundamental entre cómo se almacena un gráfico vectorial versus uno raster es esencial para tomar decisiones correctas.

---

## Cómo funciona cada uno

Un **gráfico raster** (bitmap) almacena una cuadrícula de píxeles. Cada píxel tiene un color. La resolución está fija en el momento de creación.

Un **gráfico vectorial** almacena instrucciones matemáticas: "traza un círculo con centro en (50, 50) y radio 30". El resultado se recalcula siempre, para cualquier tamaño.

```svg
<svg width="300" height="150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- RASTER (izquierda) -->
  <rect x="10" y="10" width="130" height="130" rx="6"
        fill="#fff7ed" stroke="#f59e0b" stroke-width="1.5"/>
  <text x="75" y="30" text-anchor="middle"
        font-size="10" fill="#92400e" font-weight="600">Raster (PNG)</text>

  <!-- Cuadrícula de píxeles simulada -->
  <g opacity="0.9">
    <!-- fila 1 -->
    <rect x="22" y="40" width="10" height="10" fill="#94a3b8"/>
    <rect x="32" y="40" width="10" height="10" fill="#60a5fa"/>
    <rect x="42" y="40" width="10" height="10" fill="#3b82f6"/>
    <rect x="52" y="40" width="10" height="10" fill="#3b82f6"/>
    <rect x="62" y="40" width="10" height="10" fill="#60a5fa"/>
    <rect x="72" y="40" width="10" height="10" fill="#94a3b8"/>
    <!-- fila 2 -->
    <rect x="22" y="50" width="10" height="10" fill="#60a5fa"/>
    <rect x="32" y="50" width="10" height="10" fill="#3b82f6"/>
    <rect x="42" y="50" width="10" height="10" fill="#1d4ed8"/>
    <rect x="52" y="50" width="10" height="10" fill="#1d4ed8"/>
    <rect x="62" y="50" width="10" height="10" fill="#3b82f6"/>
    <rect x="72" y="50" width="10" height="10" fill="#60a5fa"/>
    <!-- fila 3 -->
    <rect x="22" y="60" width="10" height="10" fill="#3b82f6"/>
    <rect x="32" y="60" width="10" height="10" fill="#1d4ed8"/>
    <rect x="42" y="60" width="10" height="10" fill="#1e3a8a"/>
    <rect x="52" y="60" width="10" height="10" fill="#1e3a8a"/>
    <rect x="62" y="60" width="10" height="10" fill="#1d4ed8"/>
    <rect x="72" y="60" width="10" height="10" fill="#3b82f6"/>
    <!-- fila 4 -->
    <rect x="22" y="70" width="10" height="10" fill="#3b82f6"/>
    <rect x="32" y="70" width="10" height="10" fill="#1d4ed8"/>
    <rect x="42" y="70" width="10" height="10" fill="#1e3a8a"/>
    <rect x="52" y="70" width="10" height="10" fill="#1e3a8a"/>
    <rect x="62" y="70" width="10" height="10" fill="#1d4ed8"/>
    <rect x="72" y="70" width="10" height="10" fill="#3b82f6"/>
    <!-- fila 5 -->
    <rect x="22" y="80" width="10" height="10" fill="#60a5fa"/>
    <rect x="32" y="80" width="10" height="10" fill="#3b82f6"/>
    <rect x="42" y="80" width="10" height="10" fill="#1d4ed8"/>
    <rect x="52" y="80" width="10" height="10" fill="#1d4ed8"/>
    <rect x="62" y="80" width="10" height="10" fill="#3b82f6"/>
    <rect x="72" y="80" width="10" height="10" fill="#60a5fa"/>
    <!-- fila 6 -->
    <rect x="22" y="90" width="10" height="10" fill="#94a3b8"/>
    <rect x="32" y="90" width="10" height="10" fill="#60a5fa"/>
    <rect x="42" y="90" width="10" height="10" fill="#3b82f6"/>
    <rect x="52" y="90" width="10" height="10" fill="#3b82f6"/>
    <rect x="62" y="90" width="10" height="10" fill="#60a5fa"/>
    <rect x="72" y="90" width="10" height="10" fill="#94a3b8"/>
  </g>

  <text x="75" y="115" text-anchor="middle"
        font-size="8" fill="#92400e">cuadrícula de píxeles fija</text>
  <text x="75" y="127" text-anchor="middle"
        font-size="8" fill="#92400e">resolución: 6×6px (amplificada)</text>
  <text x="75" y="139" text-anchor="middle"
        font-size="7" fill="#94a3b8">almacenado: color de cada píxel</text>

  <!-- VECTORIAL (derecha) -->
  <rect x="160" y="10" width="130" height="130" rx="6"
        fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="225" y="30" text-anchor="middle"
        font-size="10" fill="#1e40af" font-weight="600">Vectorial (SVG)</text>

  <circle cx="225" cy="78" r="36" fill="#3b82f6"/>

  <text x="225" y="128" text-anchor="middle"
        font-size="8" fill="#1e40af">geometría matemática</text>
  <text x="225" y="139" text-anchor="middle"
        font-size="7" fill="#94a3b8">almacenado: cx=225 cy=78 r=36</text>
</svg>
```

---

## El problema del escalado

Al escalar un raster hacia arriba, el algoritmo "inventa" los píxeles intermedios mediante interpolación. El resultado es borroso. Un vectorial se recalcula siempre con precisión matemática.

```svg
<svg width="300" height="160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="18" text-anchor="middle"
        font-size="10" fill="#64748b">Al escalar al 400%...</text>

  <!-- Raster escalado (borroso simulado) -->
  <text x="75" y="36" text-anchor="middle" font-size="9" fill="#92400e">Raster — borroso</text>
  <g>
    <!-- Círculo "desenfocado" con blur simulado por capas -->
    <circle cx="75" cy="100" r="44" fill="#93c5fd" opacity="0.3"/>
    <circle cx="75" cy="100" r="38" fill="#60a5fa" opacity="0.4"/>
    <circle cx="75" cy="100" r="32" fill="#3b82f6" opacity="0.5"/>
    <circle cx="75" cy="100" r="26" fill="#2563eb" opacity="0.7"/>
    <circle cx="75" cy="100" r="20" fill="#1d4ed8"/>
    <!-- Píxeles cuadrados en el borde para simular pixelación -->
    <rect x="31" y="68" width="8" height="8" fill="#93c5fd"/>
    <rect x="39" y="60" width="8" height="8" fill="#60a5fa"/>
    <rect x="99" y="60" width="8" height="8" fill="#60a5fa"/>
    <rect x="107" y="68" width="8" height="8" fill="#93c5fd"/>
    <rect x="31" y="124" width="8" height="8" fill="#93c5fd"/>
    <rect x="39" y="132" width="8" height="8" fill="#60a5fa"/>
    <rect x="99" y="132" width="8" height="8" fill="#60a5fa"/>
    <rect x="107" y="124" width="8" height="8" fill="#93c5fd"/>
  </g>

  <!-- Vectorial escalado (nítido) -->
  <text x="225" y="36" text-anchor="middle" font-size="9" fill="#1e40af">Vectorial — nítido</text>
  <circle cx="225" cy="100" r="44" fill="#3b82f6"/>

  <!-- Línea separadora -->
  <line x1="150" y1="40" x2="150" y2="155"
        stroke="#e2e8f0" stroke-width="1" stroke-dasharray="4,2"/>

  <text x="75"  y="155" text-anchor="middle" font-size="8" fill="#ef4444">interpolación → blur</text>
  <text x="225" y="155" text-anchor="middle" font-size="8" fill="#10b981">recalculado → perfecto</text>
</svg>
```

---

## Edición programática

Un raster no tiene "partes": es solo píxeles. Un vectorial tiene elementos con propiedades que se pueden leer y modificar desde JavaScript.

```svg
<svg width="300" height="140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <text x="16" y="24" font-size="9" fill="#6a9955">
    // JS puede hacer esto en SVG:
  </text>

  <text x="16" y="44" font-size="9" fill="#abb2bf">
    const circle = svg.
  </text>
  <text x="110" y="44" font-size="9" fill="#61afef">
    querySelector
  </text>
  <text x="190" y="44" font-size="9" fill="#abb2bf">
    ('circle');
  </text>

  <text x="16" y="62" font-size="9" fill="#e06c75">
    circle.
  </text>
  <text x="56" y="62" font-size="9" fill="#d19a66">
    setAttribute
  </text>
  <text x="130" y="62" font-size="9" fill="#abb2bf">
    ('fill',
  </text>
  <text x="178" y="62" font-size="9" fill="#98c379">
    '#ef4444'
  </text>
  <text x="228" y="62" font-size="9" fill="#abb2bf">
    );
  </text>

  <text x="16" y="80" font-size="9" fill="#e06c75">
    circle.
  </text>
  <text x="56" y="80" font-size="9" fill="#d19a66">
    setAttribute
  </text>
  <text x="130" y="80" font-size="9" fill="#abb2bf">
    ('r',
  </text>
  <text x="165" y="80" font-size="9" fill="#98c379">
    '50'
  </text>
  <text x="185" y="80" font-size="9" fill="#abb2bf">
    );
  </text>

  <text x="16" y="100" font-size="9" fill="#6a9955">
    // En un PNG: imposible sin reprocesar toda la imagen
  </text>

  <!-- Círculo resultante como referencia -->
  <circle cx="258" cy="70" r="28" fill="#ef4444"/>
  <text x="258" y="75" text-anchor="middle"
        font-size="7" fill="white">resultado</text>
</svg>
```

---

## Complejidad vs tamaño de archivo

SVG no siempre gana en peso. La ventaja desaparece cuando el gráfico es muy complejo.

```svg
<svg width="300" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="18" text-anchor="middle"
        font-size="10" fill="#64748b">Complejidad del gráfico vs tamaño de archivo</text>

  <!-- Eje X (complejidad) -->
  <line x1="30" y1="100" x2="280" y2="100" stroke="#cbd5e1" stroke-width="1.5"/>
  <text x="155" y="118" text-anchor="middle" font-size="8" fill="#94a3b8">Complejidad del gráfico →</text>

  <!-- Eje Y (tamaño) -->
  <line x1="30" y1="100" x2="30" y2="30" stroke="#cbd5e1" stroke-width="1.5"/>
  <text x="22" y="65" text-anchor="middle" font-size="8" fill="#94a3b8"
        transform="rotate(-90, 22, 65)">Tamaño →</text>

  <!-- Curva SVG (sube rápido) -->
  <path d="M30,95 C80,90 130,70 200,45 S260,30 280,25"
        fill="none" stroke="#3b82f6" stroke-width="2.5"/>
  <text x="260" y="22" font-size="8" fill="#3b82f6" font-weight="600">SVG</text>

  <!-- Curva PNG (casi plana luego sube suave) -->
  <path d="M30,88 C80,85 150,80 200,75 S260,72 280,70"
        fill="none" stroke="#f59e0b" stroke-width="2.5"/>
  <text x="262" y="68" font-size="8" fill="#f59e0b" font-weight="600">PNG</text>

  <!-- Punto de cruce -->
  <circle cx="190" cy="78" r="5" fill="#ef4444"/>
  <text x="190" y="68" text-anchor="middle" font-size="7" fill="#ef4444">cruce</text>

  <!-- Zona SVG gana -->
  <text x="90" y="90" text-anchor="middle" font-size="7.5" fill="#10b981">SVG más ligero</text>
  <!-- Zona PNG gana -->
  <text x="240" y="90" text-anchor="middle" font-size="7.5" fill="#f59e0b">PNG más ligero</text>
</svg>
```
