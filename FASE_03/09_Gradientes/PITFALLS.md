# Errores comunes — Gradientes

---

## 1. Gradiente fuera de `<defs>` — no funciona o aparece como elemento

Un `<linearGradient>` o `<radialGradient>` definido fuera de `<defs>` puede aparecer como un elemento vacío o simplemente no funcionar como referencia de color.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <!-- MAL: linearGradient fuera de defs -->
  <linearGradient id="bad-grad" x1="0%" y1="0%" x2="100%" y2="0%">
    <stop offset="0%"   stop-color="#3b82f6"/>
    <stop offset="100%" stop-color="#8b5cf6"/>
  </linearGradient>
  <rect x="20" y="10" width="110" height="50" rx="4" fill="url(#bad-grad)"/>
  <text x="75" y="72" text-anchor="middle" font-size="7" fill="#ef4444">fuera de &lt;defs&gt; → negro</text>

  <!-- BIEN: dentro de defs -->
  <defs>
    <linearGradient id="good-grad" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>
  <rect x="170" y="10" width="110" height="50" rx="4" fill="url(#good-grad)"/>
  <text x="225" y="72" text-anchor="middle" font-size="7" fill="#10b981">dentro de &lt;defs&gt; ✓</text>
</svg>
```

---

## 2. `id` duplicado — solo funciona el primero

Si dos gradientes tienen el mismo `id`, el navegador usa solo el primero. El segundo se ignora silenciosamente.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <defs>
    <linearGradient id="dup" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
    <!-- MAL: mismo id "dup" — este será ignorado -->
    <linearGradient id="dup" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#fbbf24"/>
    </linearGradient>
  </defs>

  <rect x="50" y="15" width="200" height="40" rx="4" fill="url(#dup)"/>
  <text x="150" y="67" text-anchor="middle" font-size="7" fill="#ef4444">
    esperaba rojo-amarillo, obtiene azul-morado (primer id gana)
  </text>
</svg>
```

---

## 3. `gradientUnits="objectBoundingBox"` en formas de alto o ancho cero

Un `<line>` o una forma con dimensión cero en un eje tiene un bounding box degenerado. El gradiente falla o produce resultados incorrectos.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <defs>
    <linearGradient id="line-obb" gradientUnits="objectBoundingBox"
                    x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>

    <!-- Solución: userSpaceOnUse con coordenadas reales -->
    <linearGradient id="line-usu" gradientUnits="userSpaceOnUse"
                    x1="20" y1="0" x2="280" y2="0">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <!-- line con objectBoundingBox: puede fallar (bounding box sin ancho) -->
  <line x1="20" y1="30" x2="280" y2="30"
        stroke="url(#line-obb)" stroke-width="8"/>
  <text x="150" y="47" text-anchor="middle" font-size="7" fill="#ef4444">
    objectBoundingBox en &lt;line&gt; — puede no funcionar
  </text>

  <!-- Solución: userSpaceOnUse -->
  <line x1="20" y1="65" x2="280" y2="65"
        stroke="url(#line-usu)" stroke-width="8"/>
  <text x="150" y="82" text-anchor="middle" font-size="7" fill="#10b981">
    userSpaceOnUse en &lt;line&gt; ✓
  </text>
</svg>
```

---

## 4. `rotate()` sin centro — gradiente rotado desde la esquina

`gradientTransform="rotate(45)"` rota desde el origen `(0,0)`. Para rotar desde el centro del elemento, usar `rotate(45, 0.5, 0.5)`.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <defs>
    <!-- MAL: rota desde esquina (0,0) -->
    <linearGradient id="bad-rot" gradientTransform="rotate(45)">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>

    <!-- BIEN: rota desde el centro del elemento -->
    <linearGradient id="good-rot" gradientTransform="rotate(45, 0.5, 0.5)">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <rect x="20"  y="10" width="100" height="55" rx="4" fill="url(#bad-rot)"/>
  <text x="70"  y="76" text-anchor="middle" font-size="7" fill="#ef4444">rotate(45) — desde esquina</text>

  <rect x="180" y="10" width="100" height="55" rx="4" fill="url(#good-rot)"/>
  <text x="230" y="76" text-anchor="middle" font-size="7" fill="#10b981">rotate(45, 0.5, 0.5) ✓</text>
</svg>
```

---

## 5. Stops sin `offset="0%"` y `offset="100%"` — comportamiento por defecto

Si el primer stop no empieza en `0%` o el último no termina en `100%`, el color del stop más cercano se extiende hacia los bordes (como `spreadMethod="pad"`). Puede parecer un error si no se espera.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <defs>
    <!-- Stops del 20% al 80% — fuera de ese rango se extiende el color del borde -->
    <linearGradient id="partial-stops" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="20%"  stop-color="#3b82f6"/>
      <stop offset="80%"  stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <rect x="20" y="12" width="260" height="40" rx="4" fill="url(#partial-stops)"/>
  <text x="150" y="65" text-anchor="middle" font-size="7.5" fill="#ef4444">
    stops al 20%–80%: el primer y último color se extienden a los bordes
  </text>
  <text x="150" y="76" text-anchor="middle" font-size="7" fill="#94a3b8">
    puede ser intencional (como padding de color) — pero conocerlo evita sorpresas
  </text>
</svg>
```
