# Errores comunes — Formas Básicas

---

## 1. `<line>` sin `stroke` — invisible

`<line>` no tiene área de relleno. Sin `stroke` no se dibuja nada.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: invisible -->
  <rect x="5" y="5" width="135" height="70" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <line x1="20" y1="40" x2="130" y2="40"/>
  <text x="72" y="85" text-anchor="middle" font-size="8" fill="#ef4444">sin stroke → invisible</text>

  <!-- BIEN -->
  <rect x="155" y="5" width="140" height="70" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <line x1="170" y1="40" x2="280" y2="40" stroke="#10b981" stroke-width="2.5"/>
  <text x="225" y="85" text-anchor="middle" font-size="8" fill="#10b981">stroke="#10b981" ✓</text>
</svg>
```

---

## 2. `<polyline>` con `fill` por defecto — relleno inesperado

`fill` por defecto es `black`. En una polilínea abierta, el navegador rellena el área entre la línea y el eje de cierre implícito.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL -->
  <rect x="5" y="5" width="135" height="90" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <polyline points="15,80 55,30 95,65 130,20"
            stroke="#ef4444" stroke-width="2"/>
  <text x="70" y="105" text-anchor="middle" font-size="8" fill="#ef4444">fill negro inesperado</text>

  <!-- BIEN -->
  <rect x="155" y="5" width="140" height="90" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <polyline points="165,80 205,30 245,65 280,20"
            stroke="#10b981" stroke-width="2" fill="none"/>
  <text x="225" y="105" text-anchor="middle" font-size="8" fill="#10b981">fill="none" ✓</text>
</svg>
```

---

## 3. `<rect>` con `rx` mayor que la mitad del lado — se distorsiona

SVG limita `rx` a `width/2` y `ry` a `height/2`. Valores mayores se recortan al máximo. No es un error, pero puede sorprender.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- rx=20 normal -->
  <rect x="10" y="20" width="80" height="50" fill="#3b82f6" rx="20"/>
  <text x="50" y="85" text-anchor="middle" font-size="8" fill="#64748b">rx=20</text>

  <!-- rx=40 = width/2 → máximo posible (píldora) -->
  <rect x="110" y="20" width="80" height="50" fill="#10b981" rx="40"/>
  <text x="150" y="85" text-anchor="middle" font-size="8" fill="#64748b">rx=40 (=w/2)</text>

  <!-- rx=999 → igual que rx=40 (se recorta) -->
  <rect x="210" y="20" width="80" height="50" fill="#f59e0b" rx="999"/>
  <text x="250" y="85" text-anchor="middle" font-size="8" fill="#64748b">rx=999 → mismo</text>
  <text x="250" y="96" text-anchor="middle" font-size="7" fill="#94a3b8">(recortado a w/2)</text>
</svg>
```

---

## 4. `<circle>` con `cx`/`cy` omitidos — aparece en (0,0)

Sin `cx` y `cy`, el centro va a la esquina superior izquierda. El círculo queda recortado.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: sin cx, cy -->
  <rect x="0" y="0" width="130" height="100" fill="#fef2f2"/>
  <circle r="40" fill="#ef4444" opacity="0.7"/>
  <text x="65" y="90" text-anchor="middle" font-size="8" fill="#ef4444">sin cx,cy → (0,0)</text>

  <!-- BIEN: cx, cy explícitos -->
  <rect x="145" y="0" width="155" height="100" fill="#f0fdf4"/>
  <circle cx="222" cy="50" r="40" fill="#10b981" opacity="0.7"/>
  <text x="222" y="96" text-anchor="middle" font-size="8" fill="#10b981">cx,cy explícitos ✓</text>
</svg>
```

---

## 5. `<polygon>` vs `<polyline>` — confundir el cierre

Si necesitas una figura cerrada (triángulo, estrella) y usas `<polyline>`, el último lado no se dibuja.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: polyline para triángulo -->
  <rect x="5" y="5" width="135" height="90" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <polyline points="30,90 75,15 120,90"
            stroke="#ef4444" stroke-width="2" fill="none"/>
  <text x="72" y="105" text-anchor="middle" font-size="8" fill="#ef4444">&lt;polyline&gt; — falta lado</text>

  <!-- BIEN: polygon cierra solo -->
  <rect x="155" y="5" width="140" height="90" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <polygon points="180,90 225,15 270,90"
           stroke="#10b981" stroke-width="2" fill="none"/>
  <text x="225" y="105" text-anchor="middle" font-size="8" fill="#10b981">&lt;polygon&gt; — cierra ✓</text>
</svg>
```

---

## 6. Stroke se extiende fuera del `viewBox` — recorte inesperado

Un `stroke-width` grueso en un elemento en el borde del `viewBox` se recorta. La solución es añadir margen interior o usar `overflow="visible"`.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: rect pegado al borde con stroke grueso -->
  <rect x="0" y="0" width="130" height="100" fill="#fef2f2"/>
  <rect x="5" y="5" width="120" height="90"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="10"/>
  <text x="65" y="107" text-anchor="middle" font-size="8" fill="#ef4444">stroke recortado</text>

  <!-- BIEN: margen para el stroke (stroke-width/2 = 5px) -->
  <rect x="155" y="0" width="145" height="100" fill="#f0fdf4"/>
  <rect x="165" y="10" width="125" height="80"
        fill="#dcfce7" stroke="#10b981" stroke-width="10"/>
  <text x="228" y="107" text-anchor="middle" font-size="8" fill="#10b981">con margen ✓</text>
</svg>
```
