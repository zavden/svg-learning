# 4.4 Línea `<line>`

La línea conecta dos puntos. **Sin `stroke` es invisible** — no tiene relleno geométrico.

---

## Los cuatro atributos y el stroke obligatorio

Punto inicial (`x1`, `y1`) → punto final (`x2`, `y2`). El `stroke` es indispensable.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- La línea -->
  <line x1="40" y1="30" x2="260" y2="90"
        stroke="#3b82f6" stroke-width="3"/>

  <!-- Puntos de inicio y fin -->
  <circle cx="40"  cy="30" r="6" fill="#ef4444"/>
  <circle cx="260" cy="90" r="6" fill="#10b981"/>

  <!-- Anotaciones -->
  <text x="28"  y="22" font-size="9" fill="#ef4444" font-family="monospace">
    (x1=40, y1=30)
  </text>
  <text x="198" y="108" font-size="9" fill="#10b981" font-family="monospace">
    (x2=260, y2=90)
  </text>

  <!-- Código -->
  <rect x="40" y="110" width="220" height="18" rx="3" fill="#0f172a"/>
  <text x="150" y="123" text-anchor="middle"
        font-size="8" fill="#abb2bf" font-family="monospace">
    &lt;line x1="40" y1="30" x2="260" y2="90" stroke="#3b82f6"/&gt;
  </text>
</svg>
```

---

## Sin stroke → invisible

Esta es la trampa más común. `<line>` sin `stroke` no se ve.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Incorrecto: sin stroke -->
  <rect x="10" y="10" width="130" height="70" rx="6" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <line x1="30" y1="45" x2="120" y2="45"/>
  <!-- La línea existe pero es invisible: fill por defecto en línea = black pero NO afecta a line -->
  <text x="75" y="90" text-anchor="middle" font-size="8" fill="#ef4444">sin stroke → invisible</text>
  <text x="75" y="38" text-anchor="middle" font-size="8" fill="#94a3b8">(la línea está aquí pero no se ve)</text>

  <!-- Correcto: con stroke -->
  <rect x="160" y="10" width="130" height="70" rx="6" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <line x1="180" y1="45" x2="270" y2="45"
        stroke="#10b981" stroke-width="3"/>
  <text x="225" y="90" text-anchor="middle" font-size="8" fill="#10b981">stroke="#10b981" ✓</text>
</svg>
```

---

## Líneas horizontales, verticales y diagonales

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Horizontal: y1 = y2 -->
  <line x1="20" y1="35" x2="130" y2="35"
        stroke="#3b82f6" stroke-width="2.5"/>
  <text x="75" y="52" text-anchor="middle" font-size="8" fill="#64748b">horizontal</text>
  <text x="75" y="63" text-anchor="middle" font-size="7" fill="#94a3b8">y1=y2=35</text>

  <!-- Vertical: x1 = x2 -->
  <line x1="190" y1="15" x2="190" y2="100"
        stroke="#10b981" stroke-width="2.5"/>
  <text x="190" y="115" text-anchor="middle" font-size="8" fill="#64748b">vertical</text>
  <text x="190" y="126" text-anchor="middle" font-size="7" fill="#94a3b8">x1=x2=190</text>

  <!-- Diagonal -->
  <line x1="20" y1="90" x2="130" y2="145"
        stroke="#f59e0b" stroke-width="2.5"/>
  <text x="75" y="155" text-anchor="middle" font-size="8" fill="#64748b">diagonal</text>

  <!-- Diagonal opuesta -->
  <line x1="130" y1="90" x2="20" y2="145"
        stroke="#a855f7" stroke-width="2.5"/>

  <!-- Separador -->
  <line x1="155" y1="10" x2="155" y2="155"
        stroke="#e2e8f0" stroke-width="1"/>

  <!-- Código cada tipo -->
  <text x="75"  y="143" text-anchor="middle" font-size="7" fill="#64748b" font-family="monospace">
    cruzadas (X)
  </text>
</svg>
```

---

## `stroke-linecap` — extremos de la línea

La forma de los extremos del trazo se controla con `stroke-linecap`.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="18" text-anchor="middle"
        font-size="9" fill="#64748b">stroke-linecap</text>

  <!-- butt (default): corte recto en el punto exacto -->
  <line x1="40" y1="45" x2="140" y2="45"
        stroke="#3b82f6" stroke-width="14" stroke-linecap="butt"/>
  <!-- guías de los puntos exactos -->
  <line x1="40" y1="30" x2="40" y2="60"
        stroke="#ef4444" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="140" y1="30" x2="140" y2="60"
        stroke="#ef4444" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="90" y="68" text-anchor="middle" font-size="8" fill="#64748b">butt (defecto)</text>
  <text x="90" y="79" text-anchor="middle" font-size="7" fill="#94a3b8">corte recto</text>

  <!-- round: extremos redondeados (se extiende r más allá) -->
  <line x1="40" y1="100" x2="140" y2="100"
        stroke="#10b981" stroke-width="14" stroke-linecap="round"/>
  <line x1="40" y1="85" x2="40" y2="115"
        stroke="#ef4444" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="140" y1="85" x2="140" y2="115"
        stroke="#ef4444" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="90" y="123" text-anchor="middle" font-size="8" fill="#64748b">round</text>

  <!-- square: como butt pero se extiende r más allá -->
  <line x1="170" y1="45" x2="270" y2="45"
        stroke="#f59e0b" stroke-width="14" stroke-linecap="square"/>
  <line x1="170" y1="30" x2="170" y2="60"
        stroke="#ef4444" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="270" y1="30" x2="270" y2="60"
        stroke="#ef4444" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="220" y="68" text-anchor="middle" font-size="8" fill="#64748b">square</text>
  <text x="220" y="79" text-anchor="middle" font-size="7" fill="#94a3b8">extiende recta</text>

  <!-- Línea de punto (punto de longitud 0 con linecap round) -->
  <line x1="220" y1="100" x2="220" y2="100"
        stroke="#a855f7" stroke-width="14" stroke-linecap="round"/>
  <text x="220" y="123" text-anchor="middle" font-size="8" fill="#64748b">punto</text>
  <text x="220" y="112" text-anchor="middle" font-size="7" fill="#94a3b8">x1=x2, y1=y2</text>
</svg>
```

---

## `stroke-dasharray` — líneas discontinuas

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="18" text-anchor="middle"
        font-size="9" fill="#64748b">stroke-dasharray</text>

  <!-- Sólida (sin dasharray) -->
  <line x1="20" y1="36" x2="200" y2="36" stroke="#3b82f6" stroke-width="2"/>
  <text x="210" y="40" font-size="8" fill="#64748b">(sólida)</text>

  <!-- dasharray="8,4" -->
  <line x1="20" y1="60" x2="200" y2="60"
        stroke="#3b82f6" stroke-width="2" stroke-dasharray="8,4"/>
  <text x="210" y="64" font-size="8" fill="#64748b">"8,4"</text>

  <!-- dasharray="4,4" -->
  <line x1="20" y1="84" x2="200" y2="84"
        stroke="#10b981" stroke-width="2" stroke-dasharray="4,4"/>
  <text x="210" y="88" font-size="8" fill="#64748b">"4,4"</text>

  <!-- dasharray="2,6" (punteada) -->
  <line x1="20" y1="108" x2="200" y2="108"
        stroke="#f59e0b" stroke-width="2.5" stroke-dasharray="2,6" stroke-linecap="round"/>
  <text x="210" y="112" font-size="8" fill="#64748b">"2,6" + round</text>

  <!-- dasharray="12,4,2,4" (compleja) -->
  <line x1="20" y1="130" x2="200" y2="130"
        stroke="#a855f7" stroke-width="2" stroke-dasharray="12,4,2,4"/>
  <text x="210" y="134" font-size="8" fill="#64748b">"12,4,2,4"</text>
</svg>
```
