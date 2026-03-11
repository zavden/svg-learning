# 10.5 Creación de patrones comunes

Galería de patrones habituales: rayas, puntos, cuadrículas, ladrillos.

---

## Rayas horizontales, verticales y diagonales

```svg
<svg width="300" height="115"
     viewBox="0 0 300 115"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Rayas horizontales: celda ancha, altura = raya + espacio -->
    <pattern id="stripes-h" x="0" y="0" width="100" height="12"
             patternUnits="userSpaceOnUse">
      <rect x="0" y="0" width="100" height="6" fill="#3b82f6"/>
    </pattern>

    <!-- Rayas verticales: celda alta, ancho = raya + espacio -->
    <pattern id="stripes-v" x="0" y="0" width="12" height="100"
             patternUnits="userSpaceOnUse">
      <rect x="0" y="0" width="6" height="100" fill="#10b981"/>
    </pattern>

    <!-- Rayas diagonales: horizontal + rotate(45) -->
    <pattern id="stripes-d" x="0" y="0" width="100" height="12"
             patternUnits="userSpaceOnUse"
             patternTransform="rotate(45)">
      <rect x="0" y="0" width="100" height="6" fill="#f59e0b"/>
    </pattern>
  </defs>

  <rect x="5"   y="10" width="88" height="75" rx="4" fill="url(#stripes-h)"/>
  <text x="49"  y="96" text-anchor="middle" font-size="7" fill="#3b82f6">horizontales</text>
  <text x="49"  y="105" text-anchor="middle" font-size="6.5" fill="#94a3b8">h=12: 6 color + 6 fondo</text>

  <rect x="106" y="10" width="88" height="75" rx="4" fill="url(#stripes-v)"/>
  <text x="150" y="96" text-anchor="middle" font-size="7" fill="#10b981">verticales</text>
  <text x="150" y="105" text-anchor="middle" font-size="6.5" fill="#94a3b8">w=12: 6 color + 6 fondo</text>

  <rect x="207" y="10" width="88" height="75" rx="4" fill="url(#stripes-d)"/>
  <text x="251" y="96" text-anchor="middle" font-size="7" fill="#f59e0b">diagonales</text>
  <text x="251" y="105" text-anchor="middle" font-size="6.5" fill="#94a3b8">rotate(45)</text>
</svg>
```

---

## Puntos (polka dots)

```svg
<svg width="300" height="115"
     viewBox="0 0 300 115"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Puntos en cuadrícula: círculo centrado en la celda -->
    <pattern id="dots-grid" x="0" y="0" width="20" height="20"
             patternUnits="userSpaceOnUse">
      <circle cx="10" cy="10" r="5" fill="#3b82f6"/>
    </pattern>

    <!-- Puntos pequeños, más espaciados -->
    <pattern id="dots-sparse" x="0" y="0" width="25" height="25"
             patternUnits="userSpaceOnUse">
      <circle cx="12.5" cy="12.5" r="3" fill="#ef4444"/>
    </pattern>

    <!-- Puntos desfasados (disposición hexagonal): dos celdas de puntos -->
    <pattern id="dots-hex" x="0" y="0" width="20" height="20"
             patternUnits="userSpaceOnUse">
      <!-- Punto en el centro de la celda -->
      <circle cx="10" cy="10" r="4" fill="#8b5cf6"/>
      <!-- Punto en la esquina (comparte con celdas vecinas) -->
      <circle cx="0"  cy="0"  r="4" fill="#8b5cf6"/>
      <circle cx="20" cy="0"  r="4" fill="#8b5cf6"/>
      <circle cx="0"  cy="20" r="4" fill="#8b5cf6"/>
      <circle cx="20" cy="20" r="4" fill="#8b5cf6"/>
    </pattern>
  </defs>

  <rect x="5"   y="10" width="88" height="75" rx="4" fill="url(#dots-grid)"/>
  <text x="49"  y="96" text-anchor="middle" font-size="7" fill="#3b82f6">cuadrícula</text>
  <text x="49"  y="105" text-anchor="middle" font-size="6.5" fill="#94a3b8">cx=cy=10, r=5 en celda 20×20</text>

  <rect x="106" y="10" width="88" height="75" rx="4" fill="url(#dots-sparse)"/>
  <text x="150" y="96" text-anchor="middle" font-size="7" fill="#ef4444">espaciados</text>
  <text x="150" y="105" text-anchor="middle" font-size="6.5" fill="#94a3b8">r=3 en celda 25×25</text>

  <rect x="207" y="10" width="88" height="75" rx="4" fill="url(#dots-hex)"/>
  <text x="251" y="96" text-anchor="middle" font-size="7" fill="#8b5cf6">hexagonal</text>
  <text x="251" y="105" text-anchor="middle" font-size="6.5" fill="#94a3b8">punto centro + esquinas</text>
</svg>
```

---

## Cuadrícula y tablero de ajedrez

```svg
<svg width="300" height="115"
     viewBox="0 0 300 115"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Cuadrícula: solo borde derecho e inferior de la celda -->
    <pattern id="grid-lines" x="0" y="0" width="20" height="20"
             patternUnits="userSpaceOnUse">
      <line x1="20" y1="0"  x2="20" y2="20" stroke="#cbd5e1" stroke-width="0.5"/>
      <line x1="0"  y1="20" x2="20" y2="20" stroke="#cbd5e1" stroke-width="0.5"/>
    </pattern>

    <!-- Tablero: cuatro cuadrados alternos -->
    <pattern id="chess" x="0" y="0" width="20" height="20"
             patternUnits="userSpaceOnUse">
      <rect x="0"  y="0"  width="10" height="10" fill="#334155"/>
      <rect x="10" y="10" width="10" height="10" fill="#334155"/>
    </pattern>

    <!-- Cuadrícula gruesa con relleno -->
    <pattern id="grid-filled" x="0" y="0" width="25" height="25"
             patternUnits="userSpaceOnUse">
      <rect x="0.5" y="0.5" width="24" height="24" fill="#eff6ff" stroke="#3b82f6" stroke-width="0.5"/>
    </pattern>
  </defs>

  <rect x="5"   y="10" width="88" height="75" rx="4" fill="url(#grid-lines)"/>
  <text x="49"  y="96" text-anchor="middle" font-size="7" fill="#64748b">cuadrícula</text>
  <text x="49"  y="105" text-anchor="middle" font-size="6.5" fill="#94a3b8">borde der. + inf.</text>

  <rect x="106" y="10" width="88" height="75" rx="4" fill="url(#chess)"/>
  <text x="150" y="96" text-anchor="middle" font-size="7" fill="#334155">ajedrez</text>
  <text x="150" y="105" text-anchor="middle" font-size="6.5" fill="#94a3b8">celda 20×20, 4 cuadros</text>

  <rect x="207" y="10" width="88" height="75" rx="4" fill="url(#grid-filled)"/>
  <text x="251" y="96" text-anchor="middle" font-size="7" fill="#3b82f6">celdas rellenas</text>
  <text x="251" y="105" text-anchor="middle" font-size="6.5" fill="#94a3b8">rect con stroke</text>
</svg>
```

---

## Ladrillos

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Ladrillo: cada fila desfasada a la mitad -->
    <!-- Celda de 40×20: fila superior centrada, fila inferior desplazada -->
    <pattern id="bricks" x="0" y="0" width="40" height="20"
             patternUnits="userSpaceOnUse">
      <!-- Fila superior: ladrillo completo centrado en la celda -->
      <rect x="1"  y="1"  width="38" height="8"  fill="#ef4444" rx="1"/>
      <!-- Fila inferior: dos medios ladrillos en los bordes -->
      <rect x="1"  y="11" width="18" height="8"  fill="#f97316" rx="1"/>
      <rect x="21" y="11" width="18" height="8"  fill="#f97316" rx="1"/>
    </pattern>
  </defs>

  <rect x="10" y="10" width="280" height="85" fill="url(#bricks)"/>

  <text x="150" y="106" text-anchor="middle" font-size="7.5" fill="#64748b">
    ladrillo: celda 40×20 con fila alternada
  </text>
</svg>
```
