# 5.5 Comando de cierre: `Z`

`Z` traza una línea recta desde la posición actual de vuelta al punto establecido por el último `M`. Cierra el contorno del sub-trazado.

---

## Qué hace `Z`

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Sin Z: el triángulo queda abierto -->
  <path d="M 20 100 L 70 20 L 120 100"
        stroke="#ef4444" stroke-width="2.5" fill="none" stroke-linejoin="round"/>
  <circle cx="20"  cy="100" r="4" fill="#ef4444"/>
  <circle cx="70"  cy="20"  r="4" fill="#ef4444"/>
  <circle cx="120" cy="100" r="4" fill="#ef4444"/>
  <text x="70" y="120" text-anchor="middle" font-size="8" fill="#ef4444">sin Z — abierto</text>

  <!-- Separador -->
  <line x1="155" y1="0" x2="155" y2="130" stroke="#e2e8f0" stroke-width="1"/>

  <!-- Con Z: cierra automáticamente -->
  <path d="M 180 100 L 230 20 L 280 100 Z"
        stroke="#10b981" stroke-width="2.5" fill="#dcfce7" stroke-linejoin="round"/>
  <circle cx="180" cy="100" r="4" fill="#10b981"/>
  <circle cx="230" cy="20"  r="4" fill="#10b981"/>
  <circle cx="280" cy="100" r="4" fill="#10b981"/>

  <!-- Flecha de Z -->
  <line x1="280" y1="100" x2="185" y2="100" stroke="#10b981" stroke-width="1.5" stroke-dasharray="3,2"/>
  <text x="232" y="115" text-anchor="middle" font-size="7" fill="#10b981">Z cierra aquí</text>
  <text x="230" y="126" text-anchor="middle" font-size="8" fill="#10b981">con Z — cerrado ✓</text>
</svg>
```

---

## `Z` vs repetir el primer punto con `L`

La diferencia es visual en las esquinas: `Z` usa `stroke-linejoin`, `L` al punto inicial usa `stroke-linecap`.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Con stroke grueso para ver la diferencia en la esquina -->

  <!-- "Cierre" con L: la última esquina se ve como linecap, no linejoin -->
  <path d="M 20 100 L 70 20 L 120 100 L 20 100"
        stroke="#ef4444" stroke-width="10" fill="none"
        stroke-linecap="square" stroke-linejoin="round"/>
  <text x="70" y="125" text-anchor="middle" font-size="7.5" fill="#ef4444">L al inicio</text>
  <text x="70" y="137" text-anchor="middle" font-size="7" fill="#94a3b8">esquina = linecap</text>

  <!-- Separador -->
  <line x1="155" y1="0" x2="155" y2="140" stroke="#e2e8f0" stroke-width="1"/>

  <!-- Con Z: la última esquina usa linejoin -->
  <path d="M 180 100 L 230 20 L 280 100 Z"
        stroke="#10b981" stroke-width="10" fill="none"
        stroke-linecap="square" stroke-linejoin="round"/>
  <text x="230" y="125" text-anchor="middle" font-size="7.5" fill="#10b981">Z</text>
  <text x="230" y="137" text-anchor="middle" font-size="7" fill="#94a3b8">esquina = linejoin ✓</text>
</svg>
```

---

## `Z` y `z` son idénticos

A diferencia de todos los demás comandos, mayúscula y minúscula hacen exactamente lo mismo. No hay diferencia entre coordenadas absolutas y relativas para un cierre.

---

## `Z` en múltiples sub-trazados

`Z` cierra solo el sub-trazado actual (el iniciado por el último `M`).

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Dos sub-trazados, cada uno con su propio Z -->
  <path d="M 20 90 L 60 20 L 100 90 Z
           M 140 90 L 180 20 L 220 90 Z"
        fill="#6366f1" opacity="0.75" stroke="#4338ca" stroke-width="1.5"/>

  <!-- Flechas mostrando que Z regresa al M correspondiente -->
  <line x1="100" y1="90" x2="25"  y2="90" stroke="#4338ca" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="220" y1="90" x2="145" y2="90" stroke="#4338ca" stroke-width="1" stroke-dasharray="2,2"/>

  <text x="60"  y="108" text-anchor="middle" font-size="7.5" fill="#4338ca">Z → regresa a M₁</text>
  <text x="180" y="108" text-anchor="middle" font-size="7.5" fill="#4338ca">Z → regresa a M₂</text>

  <!-- Sin Z en el tercero: queda abierto -->
  <path d="M 240 90 L 265 20 L 290 90"
        stroke="#94a3b8" stroke-width="1.5" fill="none" stroke-linejoin="round"/>
  <text x="265" y="108" text-anchor="middle" font-size="7" fill="#94a3b8">sin Z</text>
</svg>
```

---

## Después de `Z`: la posición actual vuelve al punto de inicio

Después de `Z`, si hay más comandos, la posición actual es el punto establecido por el último `M`. El siguiente `M` inicia un nuevo sub-trazado desde donde quieras.
