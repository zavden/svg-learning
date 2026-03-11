# Errores comunes — Colores y Opacidad

---

## 1. Usar `opacity` cuando se quería `fill-opacity` — solapamiento doble inesperado

Con `fill-opacity`, las formas solapadas se ven más opacas en las zonas de intersección. Si se quería evitar ese efecto, había que usar `opacity` en el grupo.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">fill-opacity vs opacity en solapamiento</text>

  <!-- MAL: fill-opacity individual → solapamiento doble -->
  <circle cx="55" cy="60" r="25" fill="#3b82f6" fill-opacity="0.5"/>
  <circle cx="80" cy="60" r="25" fill="#3b82f6" fill-opacity="0.5"/>
  <text x="67" y="94" text-anchor="middle" font-size="7" fill="#ef4444">fill-opacity individual</text>
  <text x="67" y="102" text-anchor="middle" font-size="6.5" fill="#94a3b8">solapamiento más oscuro</text>

  <!-- BIEN: opacity en grupo → solapamiento uniforme -->
  <g opacity="0.5">
    <circle cx="195" cy="60" r="25" fill="#3b82f6"/>
    <circle cx="220" cy="60" r="25" fill="#3b82f6"/>
  </g>
  <text x="207" y="94" text-anchor="middle" font-size="7" fill="#10b981">opacity en grupo ✓</text>
  <text x="207" y="102" text-anchor="middle" font-size="6.5" fill="#94a3b8">solapamiento uniforme</text>
</svg>
```

---

## 2. `fill="none"` donde se quería área clickeable — hover que no funciona

Un botón o área interactiva con `fill="none"` solo responde en el borde (stroke), no en el interior.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: fill=none → interior no responde a clicks -->
  <rect x="20" y="20" width="110" height="45" rx="6"
        fill="none" stroke="#3b82f6" stroke-width="2"/>
  <text x="75" y="47" text-anchor="middle" font-size="8" fill="#3b82f6">Botón</text>
  <text x="75" y="75" text-anchor="middle" font-size="7" fill="#ef4444">fill="none" → no clickeable</text>

  <!-- BIEN: fill=transparent → interior sí responde -->
  <rect x="170" y="20" width="110" height="45" rx="6"
        fill="transparent" stroke="#10b981" stroke-width="2"/>
  <text x="225" y="47" text-anchor="middle" font-size="8" fill="#10b981">Botón</text>
  <text x="225" y="75" text-anchor="middle" font-size="7" fill="#10b981">fill="transparent" ✓</text>
</svg>
```

---

## 3. Colores hex incorrectos — #RGB confundido con #RRGGBB

`#f06` y `#ff0066` son colores distintos. El formato corto solo funciona cuando cada par de dígitos es idéntico.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- #f06 = #ff0066 (magenta) -->
  <rect x="20"  y="15" width="55" height="40" fill="#f06"/>
  <text x="47" y="65" text-anchor="middle" font-size="7" fill="#64748b">#f06 = #ff0066</text>

  <!-- #f46 ≠ #f44466 (incorrecto: f4 → ff, 4 → 44, 6 → 66) -->
  <rect x="90"  y="15" width="55" height="40" fill="#f46"/>
  <text x="117" y="65" text-anchor="middle" font-size="7" fill="#64748b">#f46 = #ff4466</text>

  <!-- Si querías #f44466 — tienes que escribirlo completo -->
  <rect x="160" y="15" width="55" height="40" fill="#f44466"/>
  <text x="187" y="65" text-anchor="middle" font-size="7" fill="#64748b">#f44466 (explícito)</text>

  <!-- diferencia visible -->
  <rect x="225" y="15" width="55" height="40" fill="#f46"/>
  <text x="252" y="65" text-anchor="middle" font-size="7" fill="#ef4444">≠</text>
</svg>
```

---

## 4. `currentColor` sin `color` definido — resultado impredecible

Si el elemento o sus ancestros no tienen `color` definido en CSS, `currentColor` usa el valor por defecto del navegador (generalmente negro). En SVG standalone esto es especialmente problemático.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Sin color definido → currentColor = negro (valor por defecto) -->
  <circle cx="75" cy="45" r="30" fill="currentColor"/>
  <text x="75" y="82" text-anchor="middle" font-size="7" fill="#ef4444">sin color definido</text>
  <text x="75" y="91" text-anchor="middle" font-size="6.5" fill="#94a3b8">currentColor → negro</text>

  <!-- Con color definido → predecible -->
  <circle cx="225" cy="45" r="30" fill="currentColor" style="color: #3b82f6;"/>
  <text x="225" y="82" text-anchor="middle" font-size="7" fill="#10b981">color definido ✓</text>
  <text x="225" y="91" text-anchor="middle" font-size="6.5" fill="#94a3b8">currentColor → #3b82f6</text>
</svg>
```

---

## 5. `opacity` multiplicativo no considerado — hijo más transparente de lo esperado

Al anidar grupos con `opacity`, los valores se multiplican. Un grupo al 0.5 que contiene un elemento al 0.5 resulta en 0.25 visible total.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Lo que querías: 50% opacidad -->
  <rect x="20" y="20" width="70" height="50" fill="#3b82f6" opacity="0.5"/>
  <text x="55" y="80" text-anchor="middle" font-size="7" fill="#3b82f6">opacity=0.5</text>
  <text x="55" y="89" text-anchor="middle" font-size="6.5" fill="#94a3b8">→ 50% visible</text>

  <!-- Lo que ocurre con anidamiento: 0.5 × 0.5 = 0.25 -->
  <g opacity="0.5">
    <rect x="120" y="20" width="70" height="50" fill="#3b82f6" opacity="0.5"/>
  </g>
  <text x="155" y="80" text-anchor="middle" font-size="7" fill="#ef4444">g:0.5 + elem:0.5</text>
  <text x="155" y="89" text-anchor="middle" font-size="6.5" fill="#94a3b8">→ 0.25 (25%)</text>

  <!-- La solución: solo uno de los dos -->
  <g opacity="0.5">
    <rect x="220" y="20" width="70" height="50" fill="#3b82f6"/>
  </g>
  <text x="255" y="80" text-anchor="middle" font-size="7" fill="#10b981">solo g:0.5 ✓</text>
  <text x="255" y="89" text-anchor="middle" font-size="6.5" fill="#94a3b8">→ 50% visible</text>
</svg>
```
