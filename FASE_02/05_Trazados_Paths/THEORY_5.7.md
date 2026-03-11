# 5.7 Curvas de Bézier cuadráticas: `Q` y `T`

Las curvas cuadráticas tienen **un solo punto de control**. Son más simples que las cúbicas — menos flexibles pero suficientes para curvas simétricas.

---

## `Q x1 y1, x y` — Quadratic Bézier

Un solo punto de control influye simultáneamente en el inicio y el final de la curva.

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- La curva -->
  <path d="M 40 120 Q 150 20 260 120"
        stroke="#f59e0b" stroke-width="2.5" fill="none"/>

  <!-- Líneas de control -->
  <line x1="40"  y1="120" x2="150" y2="20" stroke="#fef9c3" stroke-width="1.5" stroke-dasharray="3,2"/>
  <line x1="260" y1="120" x2="150" y2="20" stroke="#fef9c3" stroke-width="1.5" stroke-dasharray="3,2"/>

  <!-- Puntos -->
  <circle cx="40"  cy="120" r="5" fill="white"/>
  <circle cx="260" cy="120" r="5" fill="white"/>
  <circle cx="150" cy="20"  r="6" fill="#f59e0b"/>

  <!-- Etiquetas -->
  <text x="40"  y="138" text-anchor="middle" font-size="8" fill="white">inicio</text>
  <text x="260" y="138" text-anchor="middle" font-size="8" fill="white">fin</text>
  <text x="150" y="12"  text-anchor="middle" font-size="8" fill="#f59e0b">CP único (x1,y1)</text>

  <text x="150" y="148" text-anchor="middle" font-size="7.5" fill="#475569">
    Q 150 20, 260 120
  </text>
</svg>
```

---

## Cúbica (`C`) vs Cuadrática (`Q`)

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Cuadrática: 1 CP, curva simple -->
  <path d="M 15 110 Q 60 20 105 110"
        stroke="#f59e0b" stroke-width="2.5" fill="none"/>
  <line x1="15"  y1="110" x2="60" y2="20" stroke="#fef9c3" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="105" y1="110" x2="60" y2="20" stroke="#fef9c3" stroke-width="1" stroke-dasharray="2,2"/>
  <circle cx="60" cy="20" r="4" fill="#f59e0b"/>
  <text x="60" y="135" text-anchor="middle" font-size="7.5" fill="#f59e0b">Q: 1 CP</text>
  <text x="60" y="146" text-anchor="middle" font-size="7" fill="#94a3b8">simétrica</text>

  <!-- Cúbica: 2 CP, más control -->
  <path d="M 130 110 C 148 20, 222 20, 240 110"
        stroke="#3b82f6" stroke-width="2.5" fill="none"/>
  <line x1="130" y1="110" x2="148" y2="20" stroke="#dbeafe" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="240" y1="110" x2="222" y2="20" stroke="#dbeafe" stroke-width="1" stroke-dasharray="2,2"/>
  <circle cx="148" cy="20" r="4" fill="#3b82f6"/>
  <circle cx="222" cy="20" r="4" fill="#3b82f6"/>
  <text x="185" y="135" text-anchor="middle" font-size="7.5" fill="#3b82f6">C: 2 CP</text>
  <text x="185" y="146" text-anchor="middle" font-size="7" fill="#94a3b8">más control</text>

  <!-- Cúbica asimétrica (imposible con Q) -->
  <path d="M 255 110 C 260 15, 285 90, 290 30"
        stroke="#10b981" stroke-width="2.5" fill="none"/>
  <line x1="255" y1="110" x2="260" y2="15" stroke="#dcfce7" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="290" y1="30" x2="285" y2="90" stroke="#dcfce7" stroke-width="1" stroke-dasharray="2,2"/>
  <circle cx="260" cy="15" r="4" fill="#10b981"/>
  <circle cx="285" cy="90" r="4" fill="#10b981"/>
  <text x="272" y="135" text-anchor="middle" font-size="7.5" fill="#10b981">C: en S</text>
  <text x="272" y="146" text-anchor="middle" font-size="7" fill="#94a3b8">imposible con Q</text>
</svg>
```

---

## `T` — Smooth Quadratic Bézier

`T x y` encadena curvas cuadráticas suaves. El punto de control se calcula automáticamente como reflejo del CP del `Q` o `T` anterior.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Q inicial + T encadenados -->
  <path d="M 10 70 Q 40 20 70 70 T 130 70 T 190 70 T 250 70 T 290 70"
        stroke="#f59e0b" stroke-width="2.5" fill="none"/>

  <!-- Puntos de paso -->
  <circle cx="10"  cy="70" r="3" fill="#ef4444"/>
  <circle cx="70"  cy="70" r="3" fill="#f59e0b"/>
  <circle cx="130" cy="70" r="3" fill="#f59e0b"/>
  <circle cx="190" cy="70" r="3" fill="#f59e0b"/>
  <circle cx="250" cy="70" r="3" fill="#f59e0b"/>
  <circle cx="290" cy="70" r="3" fill="#f59e0b"/>

  <!-- CP del Q inicial -->
  <circle cx="40" cy="20" r="4" fill="#f59e0b"/>
  <line x1="10" y1="70" x2="40" y2="20" stroke="#fef9c3" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="70" y1="70" x2="40" y2="20" stroke="#fef9c3" stroke-width="1" stroke-dasharray="2,2"/>

  <text x="150" y="100" text-anchor="middle" font-size="7.5" fill="#64748b">Q + T T T T: onda suave</text>
  <text x="150" y="112" text-anchor="middle" font-size="7" fill="#94a3b8">
    T calcula su CP como reflejo del anterior
  </text>
</svg>
```

---

## Cuándo usar `Q` vs `C`

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">¿Q o C?</text>

  <text x="10" y="38" font-size="8" fill="#f59e0b">Q (cuadrática)</text>
  <text x="10" y="50" font-size="7" fill="#64748b">• Curvas simétricas simples</text>
  <text x="10" y="62" font-size="7" fill="#64748b">• Arcos de burbuja de diálogo</text>
  <text x="10" y="74" font-size="7" fill="#64748b">• Texto curvo simple</text>

  <line x1="155" y1="22" x2="155" y2="90" stroke="#e2e8f0" stroke-width="1"/>

  <text x="165" y="38" font-size="8" fill="#3b82f6">C (cúbica)</text>
  <text x="165" y="50" font-size="7" fill="#64748b">• Control independiente en cada extremo</text>
  <text x="165" y="62" font-size="7" fill="#64748b">• Formas orgánicas y en S</text>
  <text x="165" y="74" font-size="7" fill="#64748b">• Letras y logotipos</text>
</svg>
```
