# 7.3 Terminaciones de línea (`stroke-linecap`)

`stroke-linecap` controla la forma del **extremo** de líneas abiertas. Solo aplica a `<line>`, `<polyline>` y paths sin `Z`.

---

## Los tres valores

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Guías de los puntos exactos -->
  <line x1="60" y1="10" x2="60" y2="140" stroke="#fecaca" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="240" y1="10" x2="240" y2="140" stroke="#fecaca" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="60"  y="8" text-anchor="middle" font-size="7" fill="#ef4444">x1</text>
  <text x="240" y="8" text-anchor="middle" font-size="7" fill="#ef4444">x2</text>

  <!-- butt (default): corte recto exactamente en el punto -->
  <line x1="60" y1="40" x2="240" y2="40"
        stroke="#3b82f6" stroke-width="18" stroke-linecap="butt"/>
  <text x="10" y="38" text-anchor="middle" font-size="8" fill="#3b82f6">butt</text>
  <text x="10" y="50" text-anchor="middle" font-size="7" fill="#94a3b8">defecto</text>

  <!-- round: semicírculo en los extremos -->
  <line x1="60" y1="90" x2="240" y2="90"
        stroke="#10b981" stroke-width="18" stroke-linecap="round"/>
  <text x="10" y="88" text-anchor="middle" font-size="8" fill="#10b981">round</text>
  <text x="10" y="100" text-anchor="middle" font-size="7" fill="#94a3b8">+9px</text>

  <!-- square: rectángulo extendido -->
  <line x1="60" y1="135" x2="240" y2="135"
        stroke="#f59e0b" stroke-width="18" stroke-linecap="square"/>
  <text x="10" y="133" text-anchor="middle" font-size="8" fill="#f59e0b">square</text>
  <text x="10" y="145" text-anchor="middle" font-size="7" fill="#94a3b8">+9px</text>

  <!-- Indicadores de extensión (round y square añaden sw/2 = 9px) -->
  <line x1="240" y1="78" x2="249" y2="78" stroke="#10b981" stroke-width="1.5"/>
  <line x1="249" y1="78" x2="249" y2="102" stroke="#10b981" stroke-width="1.5"/>
  <line x1="249" y1="102" x2="240" y2="102" stroke="#10b981" stroke-width="1.5"/>
  <text x="270" y="92" text-anchor="middle" font-size="7" fill="#10b981">sw/2</text>
</svg>
```

---

## `round` vs `square` — la diferencia

Ambos se **extienden** `stroke-width/2` más allá del punto. La única diferencia es la forma del extremo.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Zoom en el extremo derecho para ver la diferencia -->
  <line x1="20" y1="35" x2="210" y2="35"
        stroke="#10b981" stroke-width="20" stroke-linecap="round"/>
  <text x="240" y="33" font-size="8" fill="#10b981">round</text>
  <text x="240" y="44" font-size="7" fill="#94a3b8">semicírculo</text>

  <line x1="20" y1="80" x2="210" y2="80"
        stroke="#f59e0b" stroke-width="20" stroke-linecap="square"/>
  <text x="240" y="78" font-size="8" fill="#f59e0b">square</text>
  <text x="240" y="89" font-size="7" fill="#94a3b8">rectángulo</text>

  <!-- Punto exacto de referencia -->
  <line x1="210" y1="5" x2="210" y2="100" stroke="#ef4444" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="210" y="4" text-anchor="middle" font-size="6.5" fill="#ef4444">punto final</text>
</svg>
```

---

## Truco: puntos circulares con `linecap="round"` y longitud 0

Una línea de longitud 0 con `stroke-linecap="round"` produce un círculo perfecto.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Puntos con line de longitud 0 + round -->
  <line x1="30"  y1="40" x2="30"  y2="40" stroke="#3b82f6" stroke-width="20" stroke-linecap="round"/>
  <line x1="80"  y1="40" x2="80"  y2="40" stroke="#10b981" stroke-width="14" stroke-linecap="round"/>
  <line x1="125" y1="40" x2="125" y2="40" stroke="#f59e0b" stroke-width="8"  stroke-linecap="round"/>
  <line x1="160" y1="40" x2="160" y2="40" stroke="#a855f7" stroke-width="4"  stroke-linecap="round"/>

  <text x="150" y="70" text-anchor="middle" font-size="7.5" fill="#64748b">
    x1=x2, y1=y2 + stroke-linecap="round" → círculo (diámetro = stroke-width)
  </text>
</svg>
```

---

## `stroke-linecap` no aplica a formas cerradas

En `<rect>`, `<circle>`, `<polygon>` o paths con `Z`, los extremos no existen — el trazo forma un contorno cerrado continuo.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- En formas cerradas, stroke-linecap no tiene efecto visible -->
  <rect x="10" y="10" width="60" height="55" fill="none" stroke="#3b82f6" stroke-width="8" stroke-linecap="round"/>
  <text x="40" y="76" text-anchor="middle" font-size="7" fill="#64748b">rect + linecap round</text>
  <text x="40" y="87" text-anchor="middle" font-size="6.5" fill="#94a3b8">(sin efecto)</text>

  <!-- En paths abiertos, sí tiene efecto -->
  <path d="M 100 60 L 150 15 L 200 60"
        stroke="#10b981" stroke-width="8" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
  <text x="150" y="76" text-anchor="middle" font-size="7" fill="#10b981">path abierto + round</text>
  <text x="150" y="87" text-anchor="middle" font-size="6.5" fill="#94a3b8">(extremos redondeados)</text>
</svg>
```
