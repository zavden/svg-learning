# 6.1 El elemento `<text>`

El texto SVG es **texto real** (seleccionable, accesible, indexable) pero con posicionamiento por coordenadas, no por flujo. Su punto `y` es la **línea base**, no el borde superior.

---

## La línea base — la trampa más común

`y` en `<text>` no es la esquina superior izquierda, es la **línea base**: la línea invisible sobre la que descansan los caracteres.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Línea base en y=80 -->
  <line x1="0" y1="80" x2="300" y2="80" stroke="#ef4444" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="8" y="77" font-size="7" fill="#ef4444">y=80 (línea base)</text>

  <!-- Texto con y=80 -->
  <text x="20" y="80" font-size="28" fill="#1e293b" font-family="Georgia, serif">SVG</text>

  <!-- Anotaciones: ascendentes y descendentes -->
  <text x="130" y="80" font-size="28" fill="#3b82f6" font-family="Georgia, serif">gpy</text>

  <line x1="185" y1="80" x2="280" y2="80" stroke="#ef4444" stroke-width="1"/>
  <line x1="185" y1="53" x2="280" y2="53" stroke="#10b981" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="185" y1="97" x2="280" y2="97" stroke="#6366f1" stroke-width="1" stroke-dasharray="2,2"/>

  <text x="285" y="58" font-size="7" fill="#10b981">ascendente</text>
  <text x="285" y="84" font-size="7" fill="#ef4444">línea base</text>
  <text x="285" y="101" font-size="7" fill="#6366f1">descendente</text>

  <text x="150" y="124" text-anchor="middle" font-size="7" fill="#94a3b8">
    y=80 → la "g" cuelga por debajo, las mayúsculas quedan encima
  </text>
</svg>
```

---

## El problema de `y=0`

Con `y="0"`, la línea base está en el borde superior del viewport. El texto queda casi invisible.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: y=0 → casi invisible -->
  <rect x="5" y="5" width="130" height="90" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <text x="20" y="5" font-size="22" fill="#ef4444">Hola</text>
  <!-- Solo los descendentes son visibles -->
  <text x="70" y="75" text-anchor="middle" font-size="8" fill="#ef4444">y=0 → casi invisible</text>
  <text x="70" y="87" text-anchor="middle" font-size="7" fill="#94a3b8">línea base en el borde</text>

  <!-- BIEN: y al menos = font-size -->
  <rect x="155" y="5" width="140" height="90" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <text x="175" y="35" font-size="22" fill="#10b981">Hola</text>
  <text x="225" y="75" text-anchor="middle" font-size="8" fill="#10b981">y=35 ✓</text>
  <text x="225" y="87" text-anchor="middle" font-size="7" fill="#94a3b8">y ≥ font-size</text>
</svg>
```

---

## `<text>` como elemento SVG completo

Acepta `fill`, `stroke`, `transform`, `opacity` — igual que cualquier otra forma SVG.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- fill básico -->
  <text x="20" y="35" font-size="22" fill="#3b82f6">fill</text>

  <!-- stroke -->
  <text x="90" y="35" font-size="22" fill="none" stroke="#10b981" stroke-width="1">stroke</text>

  <!-- fill + stroke -->
  <text x="20" y="75" font-size="22" fill="#f59e0b" stroke="#92400e" stroke-width="1.5" paint-order="stroke">bold</text>

  <!-- opacity -->
  <text x="130" y="75" font-size="22" fill="#a855f7" opacity="0.4">opacity</text>

  <!-- rotate -->
  <text x="200" y="120" font-size="18" fill="#ef4444" transform="rotate(-20 200 120)">rotate</text>

  <!-- gradient fill -->
  <defs>
    <linearGradient id="tg" x1="0" x2="1">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#a855f7"/>
    </linearGradient>
  </defs>
  <text x="20" y="120" font-size="22" fill="url(#tg)" font-weight="700">gradient</text>
</svg>
```

---

## SVG `<text>` vs HTML: diferencias clave

```svg
<svg width="300" height="135"
     viewBox="0 0 300 135"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">HTML texto vs SVG &lt;text&gt;</text>

  <!-- Columnas -->
  <text x="75"  y="35" text-anchor="middle" font-size="8" fill="#64748b">HTML</text>
  <text x="225" y="35" text-anchor="middle" font-size="8" fill="#64748b">SVG &lt;text&gt;</text>
  <line x1="150" y1="22" x2="150" y2="135" stroke="#e2e8f0" stroke-width="1"/>

  <!-- Filas -->
  <text x="10"  y="52" font-size="7" fill="#64748b">Word wrap</text>
  <text x="75"  y="52" text-anchor="middle" font-size="7" fill="#10b981">automático ✓</text>
  <text x="225" y="52" text-anchor="middle" font-size="7" fill="#ef4444">no existe</text>

  <text x="10"  y="67" font-size="7" fill="#64748b">Posición Y</text>
  <text x="75"  y="67" text-anchor="middle" font-size="7" fill="#64748b">borde superior</text>
  <text x="225" y="67" text-anchor="middle" font-size="7" fill="#f59e0b">línea base</text>

  <text x="10"  y="82" font-size="7" fill="#64748b">Flujo</text>
  <text x="75"  y="82" text-anchor="middle" font-size="7" fill="#10b981">box model</text>
  <text x="225" y="82" text-anchor="middle" font-size="7" fill="#64748b">coordenadas xy</text>

  <text x="10"  y="97" font-size="7" fill="#64748b">Seleccionable</text>
  <text x="75"  y="97" text-anchor="middle" font-size="7" fill="#10b981">sí ✓</text>
  <text x="225" y="97" text-anchor="middle" font-size="7" fill="#10b981">sí ✓</text>

  <text x="10"  y="112" font-size="7" fill="#64748b">Accesible</text>
  <text x="75"  y="112" text-anchor="middle" font-size="7" fill="#10b981">sí ✓</text>
  <text x="225" y="112" text-anchor="middle" font-size="7" fill="#10b981">sí ✓</text>

  <text x="10"  y="127" font-size="7" fill="#64748b">Transformable</text>
  <text x="75"  y="127" text-anchor="middle" font-size="7" fill="#64748b">con CSS</text>
  <text x="225" y="127" text-anchor="middle" font-size="7" fill="#10b981">fill, stroke, rotate... ✓</text>
</svg>
```
