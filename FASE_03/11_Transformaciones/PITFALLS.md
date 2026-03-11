# Errores comunes — Transformaciones

---

## 1. `rotate(angle)` sin centro — el elemento se aleja de su posición

`rotate(45)` rota desde el origen `(0,0)`. Un elemento que no está en el origen orbita alrededor de ese punto, alejándose de su posición original.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <!-- Posición esperada (referencia) -->
  <rect x="80" y="25" width="50" height="40" fill="none" stroke="#94a3b8" stroke-width="0.5" stroke-dasharray="3,2"/>

  <!-- MAL: rotate sin centro -->
  <rect x="80" y="25" width="50" height="40" rx="3"
        fill="#fef2f2" stroke="#ef4444" stroke-width="1.5"
        transform="rotate(45)"/>
  <text x="150" y="80" text-anchor="middle" font-size="7" fill="#ef4444">rotate(45) — el rect desapareció del área esperada</text>

  <!-- BIEN: rotate con centro -->
  <rect x="80" y="25" width="50" height="40" rx="3"
        fill="#dcfce7" stroke="#10b981" stroke-width="1.5"
        transform="translate(120,0) rotate(45, 105, 45)"/>
  <text x="240" y="80" text-anchor="middle" font-size="7" fill="#10b981">rotate(45, cx, cy) ✓</text>
</svg>
```

---

## 2. `scale(2)` desplaza el elemento — no escala "en su sitio"

`scale(2)` escala todo desde `(0,0)`, incluyendo la posición. Un elemento en `(100,50)` aparece en `(200,100)` tras `scale(2)`.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <!-- Posición original: x=80, y=20 -->
  <rect x="80" y="20" width="40" height="30" fill="none" stroke="#94a3b8" stroke-width="0.5" stroke-dasharray="3,2"/>

  <!-- MAL: scale(2) — el rect se va a x=160, y=40 -->
  <rect x="80" y="20" width="40" height="30" rx="2"
        fill="#fef2f2" stroke="#ef4444" stroke-width="1.5"
        transform="scale(2)"/>
  <text x="150" y="72" text-anchor="middle" font-size="7" fill="#ef4444">scale(2) mueve el rect a (160,40) y lo escala</text>
</svg>
```

---

## 3. El orden de transformaciones cambia el resultado

`translate rotate` ≠ `rotate translate`. Es el error más frecuente al encadenar transformaciones.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <!-- translate(100,0) rotate(45): mueve 100px LUEGO rota -->
  <rect x="0" y="20" width="40" height="30" rx="2"
        fill="#fef2f2" stroke="#ef4444" stroke-width="1.5"
        transform="translate(20,0) rotate(45)"/>
  <text x="50" y="72" text-anchor="middle" font-size="7" fill="#ef4444">translate→rotate</text>

  <!-- rotate(45) translate(100,0): rota el eje LUEGO mueve en eje rotado -->
  <rect x="0" y="20" width="40" height="30" rx="2"
        fill="#dcfce7" stroke="#10b981" stroke-width="1.5"
        transform="translate(160,0) rotate(45) translate(40,0)"/>
  <text x="230" y="72" text-anchor="middle" font-size="7" fill="#10b981">rotate→translate</text>
  <text x="150" y="80" text-anchor="middle" font-size="6.5" fill="#94a3b8">mismo código pero orden distinto → posición final diferente</text>
</svg>
```

---

## 4. `transform-origin: 50% 50%` en SVG — calcula sobre el viewport, no el elemento

En SVG, los porcentajes de `transform-origin` se calculan respecto al viewport del SVG, no del elemento individual (diferente al comportamiento en HTML).

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">⚠ % en SVG ≠ % del elemento</text>

  <text x="10" y="38" font-size="7.5" fill="#ef4444">✗</text>
  <text x="22" y="38" font-size="7" fill="#64748b">style="transform-origin: 50% 50%"  → pivota en el 50% del VIEWPORT SVG</text>

  <text x="10" y="55" font-size="7.5" fill="#10b981">✓</text>
  <text x="22" y="55" font-size="7" fill="#64748b">Usar px absolutos o el patrón translate/rotate/translate</text>

  <text x="10" y="70" font-size="7.5" fill="#10b981">✓</text>
  <text x="22" y="70" font-size="7" fill="#64748b">En SVG inline en HTML con CSS: "center" funciona correctamente</text>
</svg>
```

---

## 5. `scale` afecta al `stroke-width` — trazo más grueso de lo esperado

Al escalar un elemento, el `stroke-width` también se escala. Un `stroke-width="1"` con `scale(3)` se verá como 3px.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#fef2f2;display:block; font-family:sans-serif;">

  <!-- stroke=1, sin scale -->
  <rect x="20" y="15" width="50" height="40" fill="white" stroke="#3b82f6" stroke-width="1"/>
  <text x="45" y="65" text-anchor="middle" font-size="7" fill="#64748b">sw=1</text>

  <!-- stroke=1, scale(3): el stroke se ve como 3px -->
  <rect x="20" y="15" width="50" height="40" fill="white" stroke="#ef4444" stroke-width="1"
        transform="translate(90,0) scale(1.5)"/>
  <text x="155" y="75" text-anchor="middle" font-size="7" fill="#ef4444">scale(1.5) → sw=1.5 visual</text>

  <!-- Solución: vector-effect -->
  <rect x="20" y="15" width="50" height="40" fill="white" stroke="#10b981" stroke-width="1"
        vector-effect="non-scaling-stroke"
        transform="translate(200,0) scale(1.5)"/>
  <text x="265" y="75" text-anchor="middle" font-size="7" fill="#10b981">non-scaling-stroke ✓</text>
</svg>
```
