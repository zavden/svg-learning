# Errores comunes — Texto en SVG

---

## 1. `y=0` — el texto desaparece

El valor `y` es la **línea base**, no el borde superior. Con `y=0`, el texto queda encima del viewport.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="5" y="5" width="135" height="75" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <!-- Solo se ven los descendentes (si los hay) -->
  <text x="20" y="5" font-size="20" fill="#ef4444">Texto</text>
  <text x="70" y="65" text-anchor="middle" font-size="8" fill="#ef4444">y=0 → casi invisible</text>

  <rect x="155" y="5" width="140" height="75" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <text x="175" y="32" font-size="20" fill="#10b981">Texto</text>
  <text x="225" y="65" text-anchor="middle" font-size="8" fill="#10b981">y ≥ font-size ✓</text>
</svg>
```

---

## 2. No poner `text-anchor="middle"` para centrar

Para centrar texto en `x=150` (mitad de un SVG de 300px), `text-anchor="middle"` es obligatorio. Sin él, el texto empieza en x=150 y se desvía a la derecha.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <line x1="150" y1="0" x2="150" y2="90" stroke="#ef4444" stroke-width="1" stroke-dasharray="3,2"/>

  <!-- MAL: sin text-anchor -->
  <text x="150" y="35" font-size="14" fill="#ef4444">Título centrado</text>
  <text x="150" y="50" font-size="7" fill="#94a3b8">empieza en x=150, no centrado</text>

  <!-- BIEN: con text-anchor="middle" -->
  <text x="150" y="75" font-size="14" fill="#10b981" text-anchor="middle">Título centrado</text>
  <text x="150" y="88" font-size="7" fill="#10b981" text-anchor="middle">text-anchor="middle" ✓</text>
</svg>
```

---

## 3. Texto que se sale del `viewBox`

SVG no tiene word-wrap. El texto sale del área visible sin avisar.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- El texto largo simplemente se sale -->
  <rect x="5" y="10" width="180" height="40" rx="4" fill="#dbeafe" stroke="#93c5fd" stroke-width="1"/>
  <text x="15" y="35" font-size="13" fill="#1e293b">Texto demasiado largo para el contenedor</text>

  <text x="150" y="70" text-anchor="middle" font-size="7.5" fill="#ef4444">
    No hay word-wrap — hay que partir manualmente con tspan
  </text>
</svg>
```

---

## 4. `<tspan>` sin `x` en texto multilinea — sangría acumulada

Si un `<tspan>` con `dy` no tiene `x`, la posición horizontal continúa desde el final de la línea anterior.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: tspan sin x → sangría por acumulación -->
  <rect x="5" y="5" width="135" height="90" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <text x="15" y="28" font-size="11" fill="#ef4444">
    <tspan>Primera línea</tspan>
    <tspan dy="1.4em">Segunda (sin x)</tspan>
    <tspan dy="1.4em">Tercera (sin x)</tspan>
  </text>
  <text x="70" y="103" text-anchor="middle" font-size="7.5" fill="#ef4444">sin x → desplazada</text>

  <!-- BIEN: tspan con x resetea la posición -->
  <rect x="155" y="5" width="140" height="90" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <text x="165" y="28" font-size="11" fill="#10b981">
    <tspan>Primera línea</tspan>
    <tspan x="165" dy="1.4em">Segunda (con x)</tspan>
    <tspan x="165" dy="1.4em">Tercera (con x)</tspan>
  </text>
  <text x="225" y="103" text-anchor="middle" font-size="7.5" fill="#10b981">x resetea ✓</text>
</svg>
```

---

## 5. `<textPath>` con texto más largo que el path — recortado silenciosamente

Si el texto es más largo que el path, el exceso se recorta sin error visible.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <path id="path-corto" d="M 20 50 H 150"/>
  </defs>
  <use href="#path-corto" stroke="#fecaca" stroke-width="1" fill="none"/>
  <text font-size="11" fill="#ef4444">
    <textPath href="#path-corto">Texto muy largo que no cabe</textPath>
  </text>
  <text x="150" y="72" text-anchor="middle" font-size="7.5" fill="#64748b">
    El texto excede el path → se recorta (sin error)
  </text>
</svg>
```

---

## 6. Fuente no disponible — cambio de métricas

Si la fuente no está instalada, el fallback puede tener métricas diferentes y desalinear el texto respecto a otros elementos SVG.

```svg
<svg width="300" height="70"
     viewBox="0 0 300 70"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Sin fallback: si "FuenteEspecial" no existe, el comportamiento varía -->
  <rect x="5" y="5" width="135" height="55" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <text x="72" y="40" text-anchor="middle" font-size="14" font-family="FuenteEspecial" fill="#ef4444">
    Texto
  </text>
  <text x="72" y="55" text-anchor="middle" font-size="7" fill="#94a3b8">sin fallback</text>

  <!-- Con fallbacks: siempre hay una opción -->
  <rect x="155" y="5" width="140" height="55" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <text x="225" y="40" text-anchor="middle" font-size="14"
        font-family="'FuenteEspecial', Georgia, serif" fill="#10b981">
    Texto
  </text>
  <text x="225" y="55" text-anchor="middle" font-size="7" fill="#64748b">con fallbacks ✓</text>
</svg>
```
