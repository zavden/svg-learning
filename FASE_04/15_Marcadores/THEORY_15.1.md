# 15.1 El elemento `<marker>`

Un `<marker>` es un pequeño gráfico que SVG coloca **automáticamente** en los extremos o vértices de una línea, polilínea o path. En lugar de calcular a mano la posición y el ángulo de cada cabeza de flecha, defines el marcador una vez y el navegador lo instancia donde indiques.

---

## Un marcador mínimo: flecha al final de una línea

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">definir una vez, instanciar con marker-end</text>

  <defs>
    <!-- Marcador: un triángulo simple dentro de un viewBox 0 0 10 10 -->
    <marker id="flecha-basica"
            viewBox="0 0 10 10"
            refX="9" refY="5"
            markerWidth="6" markerHeight="6"
            orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#3b82f6"/>
    </marker>
  </defs>

  <!-- Línea sin marcador -->
  <line x1="30" y1="55" x2="270" y2="55"
        stroke="#3b82f6" stroke-width="2"/>
  <text x="150" y="75" text-anchor="middle" font-size="7" fill="#64748b">línea sola</text>

  <!-- Misma línea con marker-end -->
  <line x1="30" y1="105" x2="270" y2="105"
        stroke="#3b82f6" stroke-width="2"
        marker-end="url(#flecha-basica)"/>
  <text x="150" y="125" text-anchor="middle" font-size="7" fill="#64748b">marker-end="url(#flecha-basica)"</text>
</svg>
```

---

## El viewport del marcador: `markerWidth`, `markerHeight`, `viewBox`

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">el viewBox del marker escala su contenido</text>

  <defs>
    <!-- Marcador pequeño -->
    <marker id="m-small" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="4" markerHeight="4" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#10b981"/>
    </marker>
    <!-- Mismo marcador, más grande -->
    <marker id="m-med" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#3b82f6"/>
    </marker>
    <!-- Aún más grande -->
    <marker id="m-big" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="12" markerHeight="12" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#ec4899"/>
    </marker>
  </defs>

  <line x1="30" y1="55"  x2="250" y2="55"
        stroke="#10b981" stroke-width="2" marker-end="url(#m-small)"/>
  <text x="260" y="58" font-size="7" fill="#64748b">4</text>

  <line x1="30" y1="95"  x2="250" y2="95"
        stroke="#3b82f6" stroke-width="2" marker-end="url(#m-med)"/>
  <text x="260" y="98" font-size="7" fill="#64748b">7</text>

  <line x1="30" y1="140" x2="250" y2="140"
        stroke="#ec4899" stroke-width="2" marker-end="url(#m-big)"/>
  <text x="260" y="143" font-size="7" fill="#64748b">12</text>

  <text x="150" y="180" text-anchor="middle" font-size="7" fill="#94a3b8">
    mismo viewBox="0 0 10 10", distintos markerWidth/markerHeight
  </text>
</svg>
```

---

## `refX` y `refY`: el punto de anclaje

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">refX/refY: el punto del marker que "toca" el vértice</text>

  <defs>
    <!-- refX=0: esquina izquierda del marker en el vértice (pasa de largo) -->
    <marker id="r-0" viewBox="0 0 10 10" refX="0" refY="5"
            markerWidth="8" markerHeight="8" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#ef4444"/>
    </marker>
    <!-- refX=5: centro del marker en el vértice -->
    <marker id="r-5" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="8" markerHeight="8" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#f59e0b"/>
    </marker>
    <!-- refX=10: la punta del triángulo en el vértice -->
    <marker id="r-10" viewBox="0 0 10 10" refX="10" refY="5"
            markerWidth="8" markerHeight="8" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#10b981"/>
    </marker>
  </defs>

  <!-- refX=0 -->
  <line x1="30" y1="50" x2="220" y2="50"
        stroke="#ef4444" stroke-width="2" marker-end="url(#r-0)"/>
  <circle cx="220" cy="50" r="1.5" fill="#1e293b"/>
  <text x="245" y="52" font-size="7" fill="#64748b">refX=0</text>
  <text x="245" y="62" font-size="6" fill="#94a3b8">marker pasa de largo</text>

  <!-- refX=5 -->
  <line x1="30" y1="100" x2="220" y2="100"
        stroke="#f59e0b" stroke-width="2" marker-end="url(#r-5)"/>
  <circle cx="220" cy="100" r="1.5" fill="#1e293b"/>
  <text x="245" y="102" font-size="7" fill="#64748b">refX=5</text>
  <text x="245" y="112" font-size="6" fill="#94a3b8">centrado</text>

  <!-- refX=10 -->
  <line x1="30" y1="150" x2="220" y2="150"
        stroke="#10b981" stroke-width="2" marker-end="url(#r-10)"/>
  <circle cx="220" cy="150" r="1.5" fill="#1e293b"/>
  <text x="245" y="152" font-size="7" fill="#64748b">refX=10</text>
  <text x="245" y="162" font-size="6" fill="#94a3b8">punta en el vértice ✓</text>

  <text x="150" y="200" text-anchor="middle" font-size="7" fill="#94a3b8">
    el punto negro marca el vértice donde SVG coloca el marcador
  </text>
</svg>
```

---

## `orient`: rotación automática vs fija

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">orient="auto" sigue la tangente</text>

  <defs>
    <!-- orient="auto" → el marker gira para alinearse con la línea -->
    <marker id="o-auto" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#10b981"/>
    </marker>
    <!-- orient="0" → nunca rota, siempre apunta a la derecha -->
    <marker id="o-0" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="0">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#ef4444"/>
    </marker>
  </defs>

  <!-- orient=auto: todas las flechas siguen la línea -->
  <text x="75" y="40" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">orient="auto"</text>
  <line x1="30" y1="60" x2="120" y2="60"
        stroke="#10b981" stroke-width="2" marker-end="url(#o-auto)"/>
  <line x1="30" y1="85" x2="120" y2="115"
        stroke="#10b981" stroke-width="2" marker-end="url(#o-auto)"/>
  <line x1="30" y1="140" x2="60" y2="180"
        stroke="#10b981" stroke-width="2" marker-end="url(#o-auto)"/>

  <!-- orient=0: las flechas no rotan -->
  <text x="225" y="40" text-anchor="middle" font-size="8" font-weight="bold" fill="#991b1b">orient="0"</text>
  <line x1="180" y1="60" x2="270" y2="60"
        stroke="#ef4444" stroke-width="2" marker-end="url(#o-0)"/>
  <line x1="180" y1="85" x2="270" y2="115"
        stroke="#ef4444" stroke-width="2" marker-end="url(#o-0)"/>
  <line x1="180" y1="140" x2="210" y2="180"
        stroke="#ef4444" stroke-width="2" marker-end="url(#o-0)"/>

  <text x="150" y="205" text-anchor="middle" font-size="7" fill="#94a3b8">
    para flechas casi siempre quieres orient="auto"
  </text>
</svg>
```

---

## `markerUnits`: escalar con el grosor o tamaño fijo

```svg
<svg width="300" height="230"
     viewBox="0 0 300 230"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">markerUnits: escala con el stroke o no</text>

  <defs>
    <!-- Default: strokeWidth → el marker escala con el stroke-width -->
    <marker id="u-stroke" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="5" markerHeight="5"
            markerUnits="strokeWidth" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#3b82f6"/>
    </marker>
    <!-- userSpaceOnUse → tamaño fijo absoluto -->
    <marker id="u-fixed" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="12" markerHeight="12"
            markerUnits="userSpaceOnUse" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#8b5cf6"/>
    </marker>
  </defs>

  <!-- Columna: strokeWidth -->
  <text x="75" y="38" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e40af">
    markerUnits="strokeWidth"
  </text>
  <text x="75" y="50" text-anchor="middle" font-size="6" fill="#94a3b8">
    marker escala con el stroke
  </text>
  <line x1="20" y1="70"  x2="130" y2="70"  stroke="#3b82f6" stroke-width="1" marker-end="url(#u-stroke)"/>
  <line x1="20" y1="100" x2="130" y2="100" stroke="#3b82f6" stroke-width="3" marker-end="url(#u-stroke)"/>
  <line x1="20" y1="135" x2="130" y2="135" stroke="#3b82f6" stroke-width="6" marker-end="url(#u-stroke)"/>
  <text x="145" y="72"  font-size="6" fill="#64748b">sw=1</text>
  <text x="145" y="102" font-size="6" fill="#64748b">sw=3</text>
  <text x="145" y="137" font-size="6" fill="#64748b">sw=6</text>

  <!-- Columna: userSpaceOnUse -->
  <text x="220" y="38" text-anchor="middle" font-size="8" font-weight="bold" fill="#5b21b6">
    markerUnits="userSpaceOnUse"
  </text>
  <text x="220" y="50" text-anchor="middle" font-size="6" fill="#94a3b8">
    tamaño fijo
  </text>
  <line x1="170" y1="70"  x2="270" y2="70"  stroke="#8b5cf6" stroke-width="1" marker-end="url(#u-fixed)"/>
  <line x1="170" y1="100" x2="270" y2="100" stroke="#8b5cf6" stroke-width="3" marker-end="url(#u-fixed)"/>
  <line x1="170" y1="135" x2="270" y2="135" stroke="#8b5cf6" stroke-width="6" marker-end="url(#u-fixed)"/>

  <text x="150" y="185" text-anchor="middle" font-size="7" fill="#94a3b8">
    strokeWidth (por defecto): marker proporcional al trazo
  </text>
  <text x="150" y="200" text-anchor="middle" font-size="7" fill="#94a3b8">
    userSpaceOnUse: marker idéntico aunque la línea engorde
  </text>
  <text x="150" y="215" text-anchor="middle" font-size="6" fill="#94a3b8">
    regla: strokeWidth multiplica markerWidth × stroke-width
  </text>
</svg>
```

---

## Marcadores con cualquier contenido

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">un marker puede contener cualquier SVG</text>

  <defs>
    <!-- Círculo -->
    <marker id="m-circle" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="6" markerHeight="6">
      <circle cx="5" cy="5" r="4" fill="#3b82f6"/>
    </marker>
    <!-- Estrella -->
    <marker id="m-star" viewBox="0 0 20 20" refX="10" refY="10"
            markerWidth="8" markerHeight="8">
      <polygon points="10,1 12,7 18,7 13,11 15,18 10,14 5,18 7,11 2,7 8,7"
               fill="#f59e0b"/>
    </marker>
    <!-- Cuadrado rotado (rombo) -->
    <marker id="m-diamond" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="7" markerHeight="7">
      <rect x="2" y="2" width="6" height="6" fill="#ec4899" transform="rotate(45 5 5)"/>
    </marker>
    <!-- Flecha hueca -->
    <marker id="m-hollow" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="white" stroke="#10b981" stroke-width="1.5"/>
    </marker>
  </defs>

  <line x1="30" y1="55" x2="270" y2="55" stroke="#3b82f6" stroke-width="1.5"
        marker-start="url(#m-circle)" marker-end="url(#m-circle)"/>
  <text x="30" y="75" font-size="6" fill="#64748b">círculos en los extremos</text>

  <polyline points="30,95 90,85 150,100 210,80 270,90"
            fill="none" stroke="#f59e0b" stroke-width="1.5"
            marker-start="url(#m-star)" marker-mid="url(#m-star)" marker-end="url(#m-star)"/>
  <text x="30" y="115" font-size="6" fill="#64748b">estrellas en cada vértice</text>

  <line x1="30" y1="140" x2="270" y2="140" stroke="#ec4899" stroke-width="1.5"
        marker-start="url(#m-diamond)" marker-end="url(#m-diamond)"/>
  <text x="30" y="160" font-size="6" fill="#64748b">rombos</text>

  <line x1="30" y1="175" x2="270" y2="175" stroke="#10b981" stroke-width="1.5"
        marker-end="url(#m-hollow)"/>
  <text x="30" y="193" font-size="6" fill="#64748b">flecha hueca</text>
</svg>
```
