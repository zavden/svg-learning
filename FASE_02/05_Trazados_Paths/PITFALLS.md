# Errores comunes — Trazados (Paths)

---

## 1. Path sin `M` inicial — no se renderiza

Todo `d` debe comenzar con `M` o `m`. Sin él, el comportamiento es indefinido.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: empieza con L -->
  <rect x="5" y="5" width="135" height="75" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <path d="L 60 20 L 120 70" stroke="#ef4444" stroke-width="2" fill="none"/>
  <text x="70" y="88" text-anchor="middle" font-size="8" fill="#ef4444">sin M → indefinido</text>

  <!-- BIEN -->
  <rect x="155" y="5" width="140" height="75" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <path d="M 170 70 L 225 20 L 285 70" stroke="#10b981" stroke-width="2" fill="none" stroke-linejoin="round"/>
  <text x="225" y="88" text-anchor="middle" font-size="8" fill="#10b981">M primero ✓</text>
</svg>
```

---

## 2. `d` vacío o inválido — silencioso, sin error

Un `d=""` o con valores incorrectos no lanza error en consola. El path simplemente no se ve.

```svg
<svg width="300" height="70"
     viewBox="0 0 300 70"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- d vacío: invisible, sin error -->
  <path d="" stroke="#ef4444" stroke-width="3" fill="none"/>
  <text x="100" y="40" text-anchor="middle" font-size="8" fill="#ef4444">d="" → invisible, sin error</text>

  <!-- Válido -->
  <path d="M 170 35 H 285" stroke="#10b981" stroke-width="3" fill="none" stroke-linecap="round"/>
  <text x="227" y="55" text-anchor="middle" font-size="7" fill="#10b981">visible ✓</text>
</svg>
```

---

## 3. Olvidar `fill="none"` en paths de línea

Un `<path>` sin `fill="none"` rellena el área encerrada con negro. En paths que parecen líneas, esto produce rellenos inesperados.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: path "de línea" con fill negro -->
  <rect x="5" y="5" width="135" height="90" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <path d="M 15 80 L 55 25 L 95 65 L 130 20"
        stroke="#ef4444" stroke-width="2"/>
  <text x="70" y="104" text-anchor="middle" font-size="8" fill="#ef4444">fill negro inesperado</text>

  <!-- BIEN: fill="none" -->
  <rect x="155" y="5" width="140" height="90" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <path d="M 165 80 L 205 25 L 245 65 L 285 20"
        stroke="#10b981" stroke-width="2" fill="none"/>
  <text x="225" y="104" text-anchor="middle" font-size="8" fill="#10b981">fill="none" ✓</text>
</svg>
```

---

## 4. Confundir absoluto y relativo — forma deformada

Mezclar inconsistentemente mayúsculas y minúsculas produce formas que no van donde se espera.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- El triángulo esperado: M absoluto + L absolutos -->
  <rect x="5" y="5" width="135" height="85" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <path d="M 20 80 L 70 15 L 120 80 Z"
        fill="#10b981" opacity="0.75"/>
  <text x="70" y="96" text-anchor="middle" font-size="7.5" fill="#10b981">todo mayúsculas ✓</text>

  <!-- MAL: mezcla accidental (L relativo cuando se quería absoluto) -->
  <rect x="155" y="5" width="140" height="85" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <!-- M absoluto a (175,80), luego l relativo: mueve 70 arriba, luego 50 abajo →
       los puntos van a (175,80), (245,15), (295,80) en lugar del triángulo esperado -->
  <path d="M 175 80 l 70 -65 l 50 65 Z"
        fill="#ef4444" opacity="0.6"/>
  <text x="225" y="96" text-anchor="middle" font-size="7.5" fill="#ef4444">l minúscula = relativo</text>
</svg>
```

---

## 5. `Z` al centro equivocado en múltiples sub-trazados

`Z` regresa al punto del último `M`, no al inicio de todo el path.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Correcto: Z regresa al M de su sub-trazado -->
  <path d="M 30 95 L 65 20 L 100 95 Z
           M 140 95 L 175 20 L 210 95 Z"
        fill="#3b82f6" opacity="0.75"/>
  <text x="120" y="115" text-anchor="middle" font-size="7.5" fill="#3b82f6">2 triángulos correctos ✓</text>

  <!-- Señalar la lógica: cada Z a su M -->
  <line x1="100" y1="95" x2="35"  y2="95" stroke="#1d4ed8" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="210" y1="95" x2="145" y2="95" stroke="#1d4ed8" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="68"  y="108" text-anchor="middle" font-size="6.5" fill="#64748b">Z → M₁(30,95)</text>
  <text x="178" y="108" text-anchor="middle" font-size="6.5" fill="#64748b">Z → M₂(140,95)</text>
</svg>
```

---

## 6. `A` con círculo completo en un solo arco — no funciona

Si el punto inicial y final de un arco coinciden, el arco se ignora. Para un círculo completo, hay que usar dos arcos.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- MAL: un solo arco con inicio=fin → invisible -->
  <rect x="5" y="5" width="135" height="80" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <path d="M 72 55 A 30 30 0 1 1 72 55"
        stroke="#ef4444" stroke-width="2.5" fill="none"/>
  <text x="72" y="93" text-anchor="middle" font-size="8" fill="#ef4444">inicio=fin → ignorado</text>

  <!-- BIEN: dos arcos forman el círculo -->
  <rect x="155" y="5" width="140" height="80" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <path d="M 225 25 A 30 30 0 1 1 224.99 25.01"
        stroke="#10b981" stroke-width="2.5" fill="none"/>
  <!-- Técnica: desplazar ligeramente el punto final -->
  <text x="225" y="93" text-anchor="middle" font-size="7.5" fill="#10b981">inicio ≠ fin (0.01 off) ✓</text>
</svg>
```

**Alternativa limpia para círculos:** usa `<circle>` o dos arcos con punto intermedio.
