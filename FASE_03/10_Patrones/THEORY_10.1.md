# 10.1 El elemento `<pattern>`

Un patrón define una celda gráfica que se repite (como un azulejo) para rellenar una forma. Se define en `<defs>` y se referencia con `url(#id)`.

---

## Estructura básica

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <pattern id="dots-basic" x="0" y="0" width="20" height="20"
             patternUnits="userSpaceOnUse">
      <circle cx="10" cy="10" r="5" fill="#3b82f6"/>
    </pattern>
  </defs>

  <rect x="10" y="10" width="280" height="75" rx="6" fill="url(#dots-basic)"/>
</svg>
```

---

## Anatomía: celda, posición, tamaño

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <pattern id="grid-demo" x="0" y="0" width="30" height="30"
             patternUnits="userSpaceOnUse">
      <!-- La celda: un cuadrado de 30×30 unidades -->
      <!-- Solo pintamos el borde derecho e inferior → al repetir forman una cuadrícula -->
      <line x1="30" y1="0"  x2="30" y2="30" stroke="#cbd5e1" stroke-width="1"/>
      <line x1="0"  y1="30" x2="30" y2="30" stroke="#cbd5e1" stroke-width="1"/>
    </pattern>
  </defs>

  <!-- Fondo con el patrón -->
  <rect x="10" y="10" width="280" height="90" fill="url(#grid-demo)"/>

  <!-- Anotaciones -->
  <!-- Resaltar una celda -->
  <rect x="10" y="10" width="30" height="30" fill="none" stroke="#ef4444" stroke-width="1.5" stroke-dasharray="2,1"/>
  <text x="25" y="47" text-anchor="middle" font-size="7" fill="#ef4444">celda 30×30</text>

  <text x="150" y="115" text-anchor="middle" font-size="7.5" fill="#64748b">
    width y height definen el tamaño de la celda repetida
  </text>
  <text x="150" y="127" text-anchor="middle" font-size="7" fill="#94a3b8">
    el navegador tesela la celda hasta cubrir toda el área
  </text>
</svg>
```

---

## El patrón recorta automáticamente la forma

El patrón rellena el interior de cualquier forma. Lo que queda fuera de la forma se recorta.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <pattern id="stripe-demo" x="0" y="0" width="12" height="12"
             patternUnits="userSpaceOnUse">
      <rect x="0" y="0" width="6" height="12" fill="#3b82f6"/>
    </pattern>
  </defs>

  <!-- Mismo patrón en tres formas distintas -->
  <rect   x="10"  y="15" width="70"  height="70"  rx="4" fill="url(#stripe-demo)"/>
  <circle cx="135" cy="50" r="35"                        fill="url(#stripe-demo)"/>
  <polygon points="230,15 280,85 180,85"                  fill="url(#stripe-demo)"/>

  <text x="45"  y="100" text-anchor="middle" font-size="7" fill="#64748b">rect</text>
  <text x="135" y="100" text-anchor="middle" font-size="7" fill="#64748b">circle</text>
  <text x="230" y="100" text-anchor="middle" font-size="7" fill="#64748b">polygon</text>

  <text x="150" y="112" text-anchor="middle" font-size="6.5" fill="#94a3b8">
    el patrón se recorta automáticamente al contorno de la forma
  </text>
</svg>
```

---

## `viewBox` dentro del patrón

Con `viewBox`, el contenido interno del patrón usa su propio sistema de coordenadas, independiente de las dimensiones de la celda.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Celda de 20×20, pero el contenido se define en un espacio de 100×100 -->
    <!-- La estrella se escala automáticamente para caber en 20×20 -->
    <pattern id="star-pat" x="0" y="0" width="20" height="20"
             patternUnits="userSpaceOnUse"
             viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet">
      <polygon points="50,5 61,35 95,35 68,57 79,91 50,70 21,91 32,57 5,35 39,35"
               fill="#f59e0b"/>
    </pattern>
  </defs>

  <rect x="10" y="10" width="280" height="75" rx="6" fill="url(#star-pat)"/>

  <text x="150" y="98" text-anchor="middle" font-size="7.5" fill="#64748b">
    viewBox="0 0 100 100" — la estrella diseñada en 100×100 se escala a 20×20
  </text>
</svg>
```
