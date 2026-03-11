# 7.5 Líneas discontinuas (`stroke-dasharray` y `stroke-dashoffset`)

`stroke-dasharray` convierte cualquier trazo en una línea de guiones. `stroke-dashoffset` desplaza el patrón — la base de las animaciones de "line drawing".

---

## `stroke-dasharray` — el patrón

Los valores se alternan: trazo, espacio, trazo, espacio...

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Continua (referencia) -->
  <line x1="15" y1="22" x2="235" y2="22" stroke="#3b82f6" stroke-width="3"/>
  <text x="245" y="26" font-size="8" fill="#64748b">sólida</text>

  <!-- Un valor: trazo=espacio -->
  <line x1="15" y1="46" x2="235" y2="46" stroke="#3b82f6" stroke-width="3" stroke-dasharray="8"/>
  <text x="245" y="50" font-size="8" fill="#64748b">"8"</text>

  <!-- Dos valores: trazo, espacio -->
  <line x1="15" y1="70" x2="235" y2="70" stroke="#3b82f6" stroke-width="3" stroke-dasharray="12,4"/>
  <text x="245" y="74" font-size="8" fill="#64748b">"12,4"</text>

  <!-- Punteada: guion pequeño, espacio grande -->
  <line x1="15" y1="94" x2="235" y2="94" stroke="#10b981" stroke-width="3" stroke-dasharray="2,6"/>
  <text x="245" y="98" font-size="8" fill="#64748b">"2,6"</text>

  <!-- Puntos redondos: longitud=0, linecap=round -->
  <line x1="15" y1="118" x2="235" y2="118"
        stroke="#f59e0b" stroke-width="5" stroke-dasharray="0,9" stroke-linecap="round"/>
  <text x="245" y="122" font-size="8" fill="#64748b">"0,9" + round</text>

  <!-- Patrón complejo -->
  <line x1="15" y1="142" x2="235" y2="142"
        stroke="#a855f7" stroke-width="3" stroke-dasharray="12,4,3,4"/>
  <text x="245" y="146" font-size="8" fill="#64748b">"12,4,3,4"</text>

  <!-- Variante gruesa, round -->
  <line x1="15" y1="162" x2="235" y2="162"
        stroke="#ef4444" stroke-width="8" stroke-dasharray="5,10" stroke-linecap="round"/>
  <text x="245" y="166" font-size="8" fill="#64748b">"5,10" round</text>
</svg>
```

---

## `stroke-dashoffset` — desplazar el patrón

Desplaza el punto de inicio del patrón a lo largo de la línea.

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="14" text-anchor="middle" font-size="8.5" fill="#64748b">stroke-dasharray="20,10" — distintos offset</text>

  <!-- offset=0 (referencia) -->
  <line x1="15" y1="35" x2="265" y2="35" stroke="#3b82f6" stroke-width="4"
        stroke-dasharray="20,10" stroke-dashoffset="0"/>
  <text x="275" y="39" font-size="8" fill="#64748b">0</text>

  <!-- offset=10 -->
  <line x1="15" y1="60" x2="265" y2="60" stroke="#3b82f6" stroke-width="4"
        stroke-dasharray="20,10" stroke-dashoffset="10"/>
  <text x="275" y="64" font-size="8" fill="#64748b">10</text>

  <!-- offset=20 (trazo completo desplazado) -->
  <line x1="15" y1="85" x2="265" y2="85" stroke="#3b82f6" stroke-width="4"
        stroke-dasharray="20,10" stroke-dashoffset="20"/>
  <text x="275" y="89" font-size="8" fill="#64748b">20</text>

  <!-- offset negativo -->
  <line x1="15" y1="110" x2="265" y2="110" stroke="#10b981" stroke-width="4"
        stroke-dasharray="20,10" stroke-dashoffset="-10"/>
  <text x="275" y="114" font-size="8" fill="#64748b">-10</text>

  <text x="150" y="138" text-anchor="middle" font-size="7.5" fill="#64748b">
    offset desplaza el inicio del patrón
  </text>
  <text x="150" y="150" text-anchor="middle" font-size="7" fill="#94a3b8">
    offset = longitud del patrón → igual al original (cíclico)
  </text>
</svg>
```

---

## Técnica: animación de "line drawing"

Animar `stroke-dashoffset` de la longitud total del path a `0` crea el efecto de que la línea se dibuja sola.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <style>
    .draw-line {
      stroke-dasharray: 350;
      stroke-dashoffset: 350;
      animation: draw 2.5s ease forwards infinite;
    }
    @keyframes draw {
      0%   { stroke-dashoffset: 350; }
      60%  { stroke-dashoffset: 0; }
      100% { stroke-dashoffset: 0; }
    }
  </style>

  <!-- El path que se "dibuja" -->
  <path d="M 20 80 C 60 20, 120 20, 150 60 S 240 100, 280 40"
        class="draw-line"
        stroke="#3b82f6" stroke-width="3" fill="none" stroke-linecap="round"/>

  <!-- Puntos de inicio y fin -->
  <circle cx="20"  cy="80" r="4" fill="#ef4444"/>
  <circle cx="280" cy="40" r="4" fill="#10b981"/>

  <text x="150" y="105" text-anchor="middle" font-size="8" fill="#64748b">
    dasharray = longitud del path
  </text>
  <text x="150" y="117" text-anchor="middle" font-size="7.5" fill="#475569">
    animando dashoffset: 350 → 0
  </text>
</svg>
```

---

## `stroke-dasharray` en formas cerradas

Las formas cerradas también pueden tener guiones. El patrón continúa alrededor del perímetro.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="10" y="15" width="80" height="65" fill="none" stroke="#3b82f6" stroke-width="3" stroke-dasharray="8,4"/>
  <text x="50" y="96" text-anchor="middle" font-size="7.5" fill="#3b82f6">rect</text>

  <circle cx="160" cy="50" r="36" fill="none" stroke="#10b981" stroke-width="3" stroke-dasharray="5,5"/>
  <text x="160" y="96" text-anchor="middle" font-size="7.5" fill="#10b981">circle</text>

  <polygon points="250,16 282,82 218,82"
           fill="none" stroke="#f59e0b" stroke-width="3" stroke-dasharray="6,3" stroke-linejoin="round"/>
  <text x="250" y="96" text-anchor="middle" font-size="7.5" fill="#f59e0b">polygon</text>
</svg>
```
