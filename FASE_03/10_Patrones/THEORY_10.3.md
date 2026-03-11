# 10.3 `patternContentUnits`

Controla el sistema de coordenadas de los **elementos dentro del patrón**. Independiente de `patternUnits`.

---

## `userSpaceOnUse` — coordenadas absolutas del contenido (por defecto)

Los elementos dentro del patrón usan coordenadas absolutas del espacio de usuario. Lo más intuitivo para dibujar el contenido.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- patternContentUnits="userSpaceOnUse" (default) -->
    <!-- El círculo de cx=10, cy=10, r=8 usa coordenadas absolutas -->
    <!-- La celda también mide 20×20 en userSpaceOnUse → todo coherente -->
    <pattern id="pcu-usu" x="0" y="0" width="20" height="20"
             patternUnits="userSpaceOnUse"
             patternContentUnits="userSpaceOnUse">
      <circle cx="10" cy="10" r="8" fill="#3b82f6" fill-opacity="0.7"/>
    </pattern>
  </defs>

  <rect x="10" y="10" width="280" height="75" rx="4" fill="url(#pcu-usu)"/>

  <text x="150" y="96" text-anchor="middle" font-size="7.5" fill="#64748b">
    patternContentUnits="userSpaceOnUse" — cx=10 cy=10 r=8 en unidades absolutas
  </text>
</svg>
```

---

## La combinación más común: `patternUnits="objectBoundingBox"` + viewBox

Cuando `patternUnits="objectBoundingBox"`, la celda mide entre 0 y 1. Para dibujar con coordenadas normales dentro, añadir un `viewBox` a la celda.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Celda relativa (objectBoundingBox) + viewBox para coordenadas normales -->
    <!-- Sin viewBox, el círculo quedaría diminuto (r=8 en espacio 0-1) -->
    <pattern id="obb-vb" x="0" y="0" width="0.1" height="0.1"
             patternUnits="objectBoundingBox"
             viewBox="0 0 20 20">
      <circle cx="10" cy="10" r="8" fill="#10b981" fill-opacity="0.7"/>
    </pattern>

    <!-- Sin viewBox: el círculo usa coordenadas en espacio 0-1 → r=8 es ENORME -->
    <pattern id="obb-novb" x="0" y="0" width="0.1" height="0.1"
             patternUnits="objectBoundingBox">
      <circle cx="0.05" cy="0.05" r="0.04" fill="#ef4444" fill-opacity="0.7"/>
    </pattern>
  </defs>

  <rect x="10"  y="10" width="130" height="75" rx="4" fill="url(#obb-vb)"/>
  <text x="75"  y="97" text-anchor="middle" font-size="7" fill="#10b981">con viewBox ✓</text>
  <text x="75"  y="106" text-anchor="middle" font-size="6.5" fill="#94a3b8">contenido normal</text>

  <rect x="160" y="10" width="130" height="75" rx="4" fill="url(#obb-novb)"/>
  <text x="225" y="97" text-anchor="middle" font-size="7" fill="#64748b">sin viewBox</text>
  <text x="225" y="106" text-anchor="middle" font-size="6.5" fill="#94a3b8">coords 0–1 en contenido</text>
</svg>
```

---

## Tabla de combinaciones

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Combinaciones habituales</text>

  <!-- Fila 1 -->
  <rect x="5" y="27" width="290" height="20" rx="2" fill="#dbeafe"/>
  <text x="10" y="40" font-size="7" fill="#1e40af" font-weight="600">patternUnits=userSpaceOnUse + patternContentUnits=userSpaceOnUse</text>

  <text x="10" y="57" font-size="7" fill="#64748b">Celda de tamaño fijo, contenido con coordenadas normales — más intuitivo</text>

  <!-- Fila 2 -->
  <rect x="5" y="63" width="290" height="20" rx="2" fill="#dcfce7"/>
  <text x="10" y="76" font-size="7" fill="#166534" font-weight="600">patternUnits=objectBoundingBox + viewBox en el pattern</text>

  <text x="10" y="93" font-size="7" fill="#64748b">Celda relativa al elemento, contenido diseñado con coordenadas normales</text>

  <text x="150" y="108" text-anchor="middle" font-size="6.5" fill="#94a3b8">
    regla práctica: si hay dudas, usar userSpaceOnUse para todo
  </text>
</svg>
```
