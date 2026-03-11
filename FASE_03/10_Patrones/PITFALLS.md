# Errores comunes — Patrones

---

## 1. Patrón sin `width` y `height` — no aparece nada

`width` y `height` son obligatorios. Sin ellos el patrón falla silenciosamente y la forma queda sin relleno.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <defs>
    <!-- MAL: sin width y height -->
    <pattern id="no-size">
      <circle cx="10" cy="10" r="8" fill="#3b82f6"/>
    </pattern>

    <!-- BIEN: width y height obligatorios -->
    <pattern id="with-size" width="20" height="20" patternUnits="userSpaceOnUse">
      <circle cx="10" cy="10" r="8" fill="#3b82f6"/>
    </pattern>
  </defs>

  <rect x="20"  y="10" width="110" height="50" rx="4" fill="url(#no-size)"/>
  <text x="75"  y="72" text-anchor="middle" font-size="7" fill="#ef4444">sin width/height → vacío</text>

  <rect x="170" y="10" width="110" height="50" rx="4" fill="url(#with-size)"/>
  <text x="225" y="72" text-anchor="middle" font-size="7" fill="#10b981">con width/height ✓</text>
</svg>
```

---

## 2. `patternUnits="objectBoundingBox"` sin `viewBox` — contenido desaparecido

Con `objectBoundingBox`, la celda mide entre 0 y 1. Los elementos internos con coordenadas normales (ej: `cx="10"`) quedan enormes (10 veces el bounding box) o completamente fuera de la celda.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <defs>
    <!-- MAL: objectBoundingBox sin viewBox → el círculo de cx=10,r=8 es enorme -->
    <pattern id="obb-bad" width="0.2" height="0.2" patternUnits="objectBoundingBox">
      <circle cx="10" cy="10" r="8" fill="#3b82f6"/>
    </pattern>

    <!-- BIEN: objectBoundingBox + viewBox para coordenadas normales -->
    <pattern id="obb-good" width="0.2" height="0.2" patternUnits="objectBoundingBox"
             viewBox="0 0 20 20">
      <circle cx="10" cy="10" r="8" fill="#10b981"/>
    </pattern>
  </defs>

  <rect x="20"  y="10" width="110" height="60" rx="4" fill="url(#obb-bad)"/>
  <text x="75"  y="82" text-anchor="middle" font-size="7" fill="#ef4444">objectBoundingBox sin viewBox</text>
  <text x="75"  y="91" text-anchor="middle" font-size="6.5" fill="#94a3b8">círculo enorme o invisible</text>

  <rect x="170" y="10" width="110" height="60" rx="4" fill="url(#obb-good)"/>
  <text x="225" y="82" text-anchor="middle" font-size="7" fill="#10b981">+ viewBox ✓</text>
</svg>
```

---

## 3. `patternTransform="rotate(45)"` sin ajuste — esquinas vacías

Al rotar un patrón de rayas, las esquinas de la celda pueden quedar sin cubrir dependiendo del tamaño de la celda respecto al stroke.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <defs>
    <!-- Celda pequeña: el fondo (blanco) aparece en las esquinas al rotar -->
    <pattern id="rot-gap" x="0" y="0" width="10" height="10"
             patternUnits="userSpaceOnUse"
             patternTransform="rotate(45)">
      <rect x="0" y="0" width="10" height="5" fill="#3b82f6"/>
    </pattern>

    <!-- Celda más grande: raya más gruesa relativa a la celda -->
    <pattern id="rot-ok" x="0" y="0" width="14" height="14"
             patternUnits="userSpaceOnUse"
             patternTransform="rotate(45)">
      <rect x="0" y="0" width="14" height="7" fill="#10b981"/>
    </pattern>
  </defs>

  <rect x="20"  y="10" width="110" height="65" rx="4" fill="url(#rot-gap)"/>
  <text x="75"  y="85" text-anchor="middle" font-size="7" fill="#ef4444">celda pequeña → huecos</text>

  <rect x="170" y="10" width="110" height="65" rx="4" fill="url(#rot-ok)"/>
  <text x="225" y="85" text-anchor="middle" font-size="7" fill="#10b981">celda proporcional ✓</text>
</svg>
```

---

## 4. `id` de patrón en conflicto al usar el SVG múltiples veces

Si el mismo SVG con un patrón se inline varias veces en HTML, los `id` se duplican y el segundo SVG referencia el patrón del primero. Usar ids únicos generados dinámicamente.

```svg
<svg width="300" height="70"
     viewBox="0 0 300 70"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Problema de id duplicado (conceptual)</text>

  <text x="15" y="38" font-size="7.5" fill="#ef4444">✗</text>
  <text x="27" y="38" font-size="7" fill="#64748b">Dos SVGs idénticos inline → id="miPatron" duplicado en el DOM</text>

  <text x="15" y="52" font-size="7.5" fill="#ef4444">✗</text>
  <text x="27" y="52" font-size="7" fill="#64748b">El segundo SVG usa el patrón del primero → puede romperse</text>

  <text x="15" y="66" font-size="7.5" fill="#10b981">✓</text>
  <text x="27" y="66" font-size="7" fill="#64748b">Solución: ids únicos (hash, timestamp) o sprite SVG con &lt;use&gt;</text>
</svg>
```

---

## 5. Patrón fuera de `<defs>` — se renderiza como elemento visible

Un `<pattern>` fuera de `<defs>` puede aparecer como un elemento vacío en el flujo normal del SVG, ocupando espacio o causando comportamientos inesperados.

```svg
<svg width="300" height="70"
     viewBox="0 0 300 70"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <defs>
    <!-- BIEN: siempre dentro de defs -->
    <pattern id="correct-location" width="10" height="10"
             patternUnits="userSpaceOnUse">
      <circle cx="5" cy="5" r="3" fill="#10b981"/>
    </pattern>
  </defs>

  <rect x="10" y="10" width="280" height="45" rx="4" fill="url(#correct-location)"/>
  <text x="150" y="66" text-anchor="middle" font-size="7.5" fill="#10b981">
    patrón dentro de &lt;defs&gt; — correcto ✓
  </text>
</svg>
```
