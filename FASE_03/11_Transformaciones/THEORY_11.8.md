# 11.8 `transform-origin`

En SVG 1.1, el origen de las transformaciones era siempre `(0,0)`. Con CSS `transform-origin` se puede cambiar el punto de pivote sin encadenar translaciones.

---

## El problema en SVG 1.1 y la solución con CSS

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">SVG 1.1 vs CSS transform-origin</text>

  <!-- SVG 1.1: patrón triple translate -->
  <rect x="20" y="32" width="70" height="50" rx="3"
        fill="#fef2f2" stroke="#ef4444" stroke-width="1.5"
        transform="translate(55,57) rotate(25) translate(-55,-57)"/>
  <text x="55" y="95" text-anchor="middle" font-size="7" fill="#ef4444">SVG 1.1</text>
  <text x="55" y="104" text-anchor="middle" font-size="6" fill="#94a3b8">translate(cx) rotate translate(-cx)</text>

  <!-- CSS transform-origin: más limpio -->
  <rect x="130" y="32" width="70" height="50" rx="3"
        fill="#dcfce7" stroke="#10b981" stroke-width="1.5"
        transform="rotate(25)"
        style="transform-origin: 165px 57px;"/>
  <text x="165" y="95" text-anchor="middle" font-size="7" fill="#10b981">CSS transform-origin ✓</text>
  <text x="165" y="104" text-anchor="middle" font-size="6" fill="#94a3b8">transform-origin: 165px 57px</text>
  <text x="165" y="113" text-anchor="middle" font-size="6" fill="#94a3b8">+ transform="rotate(25)"</text>

  <text x="150" y="126" text-anchor="middle" font-size="7" fill="#94a3b8">
    mismo resultado visual — CSS más conciso
  </text>
</svg>
```

---

## Valores de `transform-origin` en SVG

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">transform-origin — diferentes pivotes</text>

  <!-- Rotación desde la esquina superior izquierda (0 0) -->
  <rect x="15" y="30" width="55" height="45" rx="3"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"
        transform="rotate(20)"
        style="transform-origin: 15px 30px;"/>
  <circle cx="15" cy="30" r="3" fill="#ef4444"/>
  <text x="42" y="90" text-anchor="middle" font-size="7" fill="#3b82f6">esquina</text>
  <text x="42" y="99" text-anchor="middle" font-size="6.5" fill="#94a3b8">origin: 15px 30px</text>

  <!-- Rotación desde el centro -->
  <rect x="115" y="30" width="55" height="45" rx="3"
        fill="#dcfce7" stroke="#10b981" stroke-width="1.5"
        transform="rotate(20)"
        style="transform-origin: 142px 52px;"/>
  <circle cx="142" cy="52" r="3" fill="#ef4444"/>
  <text x="142" y="90" text-anchor="middle" font-size="7" fill="#10b981">centro</text>
  <text x="142" y="99" text-anchor="middle" font-size="6.5" fill="#94a3b8">origin: 142px 52px</text>

  <!-- Rotación desde la esquina inferior derecha -->
  <rect x="215" y="30" width="55" height="45" rx="3"
        fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5"
        transform="rotate(20)"
        style="transform-origin: 270px 75px;"/>
  <circle cx="270" cy="75" r="3" fill="#ef4444"/>
  <text x="242" y="90" text-anchor="middle" font-size="7" fill="#f59e0b">esquina inf-der</text>
  <text x="242" y="99" text-anchor="middle" font-size="6.5" fill="#94a3b8">origin: 270px 75px</text>

  <text x="150" y="114" text-anchor="middle" font-size="7" fill="#94a3b8">
    punto rojo = transform-origin (el pivote)
  </text>
</svg>
```

---

## Advertencia: % en SVG se calcula respecto al viewport

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">⚠ % en SVG ≠ % del elemento</text>

  <text x="10" y="38" font-size="7.5" fill="#ef4444">✗</text>
  <text x="22" y="38" font-size="7" fill="#64748b">transform-origin: 50% 50%  → 50% del VIEWPORT, no del elemento</text>
  <text x="22" y="51" font-size="6.5" fill="#94a3b8">   (diferente al comportamiento HTML donde % es del elemento)</text>

  <text x="10" y="67" font-size="7.5" fill="#10b981">✓</text>
  <text x="22" y="67" font-size="7" fill="#64748b">Usar px absolutos: transform-origin: 150px 75px</text>

  <text x="10" y="82" font-size="7.5" fill="#10b981">✓</text>
  <text x="22" y="82" font-size="7" fill="#64748b">O el patrón SVG 1.1: translate(cx,cy) rotate(a) translate(-cx,-cy)</text>
</svg>
```
