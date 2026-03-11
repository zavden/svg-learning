# 10.2 `patternUnits`

Controla el sistema de coordenadas de los atributos de posición y tamaño de la celda: `x`, `y`, `width`, `height`.

---

## `objectBoundingBox` — relativo al elemento (por defecto)

Las dimensiones de la celda son fracciones del bounding box del elemento que usa el patrón. `width="0.2"` = 20% del ancho del elemento → 5 columnas.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- objectBoundingBox: la celda es 20% del ancho y 33% del alto del elemento -->
    <!-- Resultado: 5 columnas × 3 filas, sin importar el tamaño del elemento -->
    <pattern id="obb-pat" x="0" y="0" width="0.2" height="0.333"
             patternUnits="objectBoundingBox">
      <rect x="1" y="1" width="98%" height="98%" fill="none" stroke="#3b82f6" stroke-width="1"/>
    </pattern>
  </defs>

  <!-- Rectángulo pequeño: 5×3 celdas -->
  <rect x="10"  y="10" width="80"  height="60" fill="url(#obb-pat)"/>
  <text x="50"  y="82" text-anchor="middle" font-size="7" fill="#64748b">80×60 → 5×3 celdas</text>

  <!-- Rectángulo grande: también 5×3 celdas -->
  <rect x="110" y="10" width="180" height="60" fill="url(#obb-pat)"/>
  <text x="200" y="82" text-anchor="middle" font-size="7" fill="#64748b">180×60 → también 5×3 celdas</text>

  <text x="150" y="99" text-anchor="middle" font-size="7.5" fill="#64748b">
    objectBoundingBox: el número de celdas se mantiene constante
  </text>
  <text x="150" y="110" text-anchor="middle" font-size="7" fill="#94a3b8">
    las celdas escalan con el elemento
  </text>
</svg>
```

---

## `userSpaceOnUse` — tamaño fijo en unidades del viewport

Las dimensiones de la celda son absolutas. La celda mide siempre lo mismo; el número de repeticiones varía con el tamaño del elemento.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- userSpaceOnUse: la celda mide siempre 20×20 unidades -->
    <pattern id="usu-pat" x="0" y="0" width="20" height="20"
             patternUnits="userSpaceOnUse">
      <rect x="1" y="1" width="18" height="18" fill="none" stroke="#10b981" stroke-width="1"/>
    </pattern>
  </defs>

  <!-- Rectángulo pequeño: pocas celdas -->
  <rect x="10"  y="10" width="80"  height="60" fill="url(#usu-pat)"/>
  <text x="50"  y="82" text-anchor="middle" font-size="7" fill="#64748b">80×60 → 4×3 celdas</text>

  <!-- Rectángulo grande: más celdas, el tamaño de cada celda no cambia -->
  <rect x="110" y="10" width="180" height="60" fill="url(#usu-pat)"/>
  <text x="200" y="82" text-anchor="middle" font-size="7" fill="#64748b">180×60 → 9×3 celdas</text>

  <text x="150" y="99" text-anchor="middle" font-size="7.5" fill="#64748b">
    userSpaceOnUse: la celda mide siempre 20×20 unidades
  </text>
  <text x="150" y="110" text-anchor="middle" font-size="7" fill="#94a3b8">
    el número de repeticiones cambia con el elemento
  </text>
</svg>
```

---

## Comparativa: cuándo usar cada uno

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">patternUnits — cuándo usar cada uno</text>

  <text x="10" y="36" font-size="8" fill="#3b82f6" font-weight="700">objectBoundingBox (default)</text>
  <text x="10" y="49" font-size="7" fill="#64748b">• Patrón con número fijo de celdas (ej: 5×3 siempre)</text>
  <text x="10" y="61" font-size="7" fill="#64748b">• El patrón debe escalar con el elemento</text>
  <text x="10" y="73" font-size="7" fill="#64748b">• Textura que se adapta a cualquier tamaño</text>

  <line x1="10" y1="79" x2="290" y2="79" stroke="#e2e8f0" stroke-width="1"/>

  <text x="10" y="92" font-size="8" fill="#10b981" font-weight="700">userSpaceOnUse</text>
  <text x="10" y="104" font-size="7" fill="#64748b">• Grid de tamaño fijo (cada cuadrado = 10 unidades)</text>
  <text x="150" y="104" font-size="7" fill="#64748b">• Textura con escala física concreta</text>
</svg>
```
