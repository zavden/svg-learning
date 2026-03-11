# 9.1 Gradiente lineal `<linearGradient>`

Transición de colores a lo largo de una línea recta. Se define en `<defs>` y se referencia con `url(#id)`.

---

## Estructura básica

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <linearGradient id="grad-basic" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <rect x="20" y="20" width="260" height="55" rx="6" fill="url(#grad-basic)"/>
  <text x="150" y="86" text-anchor="middle" font-size="7.5" fill="#64748b">
    fill="url(#grad-basic)" — referencia al gradiente por id
  </text>
</svg>
```

---

## Dirección: x1, y1, x2, y2

Los puntos de inicio y fin se expresan en porcentajes del bounding box del elemento (con `gradientUnits="objectBoundingBox"`, el valor por defecto).

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Horizontal izq→der (por defecto) -->
    <linearGradient id="h-ltr" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
    <!-- Horizontal der→izq -->
    <linearGradient id="h-rtl" x1="100%" y1="0%" x2="0%" y2="0%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
    <!-- Vertical arr→abj -->
    <linearGradient id="v-ttb" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
    <!-- Diagonal ↘ -->
    <linearGradient id="diag" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <rect x="10"  y="10" width="60" height="45" rx="4" fill="url(#h-ltr)"/>
  <text x="40"  y="66" text-anchor="middle" font-size="7" fill="#64748b">→ 0%,0 → 100%,0</text>

  <rect x="85"  y="10" width="60" height="45" rx="4" fill="url(#h-rtl)"/>
  <text x="115" y="66" text-anchor="middle" font-size="7" fill="#64748b">← 100%,0 → 0%,0</text>

  <rect x="160" y="10" width="60" height="45" rx="4" fill="url(#v-ttb)"/>
  <text x="190" y="66" text-anchor="middle" font-size="7" fill="#64748b">↓ 0,0% → 0,100%</text>

  <rect x="235" y="10" width="60" height="45" rx="4" fill="url(#diag)"/>
  <text x="265" y="66" text-anchor="middle" font-size="7" fill="#64748b">↘ 0,0 → 100,100</text>

  <!-- Con gradientTransform para ángulo arbitrario -->
  <defs>
    <linearGradient id="rotated" x1="0%" y1="0%" x2="100%" y2="0%"
                    gradientTransform="rotate(30, 0.5, 0.5)">
      <stop offset="0%"   stop-color="#f59e0b"/>
      <stop offset="100%" stop-color="#ef4444"/>
    </linearGradient>
  </defs>
  <rect x="50" y="90" width="200" height="55" rx="4" fill="url(#rotated)"/>
  <text x="150" y="162" text-anchor="middle" font-size="7.5" fill="#64748b">
    gradientTransform="rotate(30, 0.5, 0.5)" — ángulo arbitrario
  </text>
  <text x="150" y="174" text-anchor="middle" font-size="7" fill="#94a3b8">
    más fácil que calcular x1/y1/x2/y2 para ángulos exactos
  </text>
</svg>
```

---

## Gradiente con múltiples stops

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Arcoíris simplificado -->
    <linearGradient id="rainbow" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"    stop-color="#ef4444"/>
      <stop offset="25%"   stop-color="#f59e0b"/>
      <stop offset="50%"   stop-color="#10b981"/>
      <stop offset="75%"   stop-color="#3b82f6"/>
      <stop offset="100%"  stop-color="#8b5cf6"/>
    </linearGradient>

    <!-- Gradiente con parada abrupta (dos stops iguales) -->
    <linearGradient id="sharp" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="50%"  stop-color="#3b82f6"/>
      <stop offset="50%"  stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#ef4444"/>
    </linearGradient>
  </defs>

  <rect x="10"  y="12" width="280" height="28" rx="4" fill="url(#rainbow)"/>
  <text x="150" y="50" text-anchor="middle" font-size="7" fill="#64748b">5 stops: gradiente multicolor</text>

  <rect x="10"  y="57" width="280" height="25" rx="4" fill="url(#sharp)"/>
  <text x="150" y="92" text-anchor="middle" font-size="7" fill="#64748b">
    dos stops en offset=50% → cambio abrupto (sin transición suave)
  </text>
</svg>
```

---

## Gradiente en stroke

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <linearGradient id="stroke-grad" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#3b82f6"/>
    </linearGradient>
  </defs>

  <!-- stroke con gradiente -->
  <rect x="30" y="15" width="240" height="55" rx="8"
        fill="none" stroke="url(#stroke-grad)" stroke-width="6"/>
  <text x="150" y="86" text-anchor="middle" font-size="7.5" fill="#64748b">
    stroke="url(#stroke-grad)" — el trazo también acepta gradientes
  </text>
</svg>
```
